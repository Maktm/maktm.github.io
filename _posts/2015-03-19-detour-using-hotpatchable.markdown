---
layout: post
title:  "Detour using Hotpatchable Functions (MSVC Feature)"
date:   2015-03-19
categories: jekyll update
---

So when you detour normal functions you just have to write the JMP instruction (1 byte) and the relative offset (4 bytes) to the specified location and it will work while the minimum number of bytes is available. Another thing that's available is the hooking of Windows API functions which have hotpatching enabled (i'm not sure if it's all of them). Functions that have hotpatching enabled look like this :

```assembly
0x100 NOP
0x101 NOP
0x102 NOP
0x103 NOP
0x104 NOP 			; 5 bytes in total
0x105 MOV EDI,EDI	; 2 bytes
0x107 PUSH EBP		; Function prologue (starting)
0x108 MOV EBP,ESP
```

This was initially created by Microsoft in order to make quick changes when source isn't available and as the name implies quickly patch shit up. So basically what you want to do when hooking these functions is place a short JMP to the start of MOV EDI,EDI since when this function is called that's where it start from. The short Jump instruction should redirect to the start of the NOP/INT3 instructions and since the size of all the NOP/INT3 instructions added up is 5 you can place an unconditional jump to the code you want.

In the end it will look something like this :

```assembly
0x100 JMP App.Location 	  ;Jump to your required section
0x105 JMP SHORT App.0x100 ;Short jump to 0x100
```

Now know that you can use a normal detour. I just wrote this up since I never knew about hotpatching and made a function for it. If you find it fun then cool

```c++
// Windows API functions have slots for hotpatching
// in the format { mov edi,edi } with NOP prepending it
// so this writes to the { mov edi,edi } to jump
// to the NOPs to jump to the relative jump
void DetourAPI(__int32 start, __int32 end)
{
#define JMP	0xE9
#define NOP	0x90	
#define JMP_SHORT	0xF9EB
    
    unsigned long protection;
    VirtualProtect(reinterpret_cast<void*>(start - 5), 7, PAGE_EXECUTE_READWRITE, &protection);
    
    // Set up the unconditional relative jump
    byte * relativeLoc = (byte*)(start - 5);
    *relativeLoc = JMP;
    *(relativeLoc + 1) = (end - start) - 5;
    
    // Set up the jump to the to the relative jump
    *(SHORT*)start =			JMP_SHORT;
    
    VirtualProtect(reinterpret_cast<void*>(start - 5), 7, protection, &protection);
}
```

