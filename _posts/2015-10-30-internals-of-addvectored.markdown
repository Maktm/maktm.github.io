---
layout: post
title:  "Internals of AddVectoredExceptionHandler"
date:   2015-10-30
categories: jekyll update
---

NOTE: This code is not portable whatsoever. It's just to show some reversing I've been doing which might help some people. It probably doesn't have much use to you but you might learn something.

I decided to find out how vectored exceptions worked inside of Windows and after searching all around the web I couldn't find any information regarding the internals of Windows vectored exception handling. So I reversed the AddVectoredExceptionHandler function which led me down this path:

```
AddVectoredExceptionHandler -> ntdll!RtlAddVectoredExceptionHandler -> sub_xxx
```

Since the final function wasn't named by IDA I just named it 'ntdll_RtlAddVectoredExceptionHandler_Impl'. After reversing the structures I found out that Windows keeps a string of doubly linked list nodes that link a circle (last node points to the first node). The structure for a single node looks like this:

```c
typedef struct _VECTORED_NODE
{
    _VECTORED_NODE *NextNode;
    _VECTORED_NODE *PrevNode;
    BOOL            IsAllocated;
    PVOID           EncodedHandler;
}VECTORED_NODE, *PVECTORED_NODE;
```

Windows uses the EncodePointer function to encode the handler that you pass through AddVectoredExceptionHandler and stores that inside the last member, EncodedHandler. The IsAllocated member is set to 1 once the function is sure that a node can be added and decremented when the node is removed (after calling RemoveVectoredExceptionHandler).

To get to the list of structures you have to access the exception list using this structure:

```c
typedef struct _RTL_BLOCK
{
    PVOID   Unknown;
    PVECTORED_NODE ExceptionList;
}RTL_BLOCK, *PRTL_BLOCK;
```

I don't know what this function is for but I know the exception list is stored. This address of this function is static and sadly I can only share the address for my version of ntdll (6.1.7601.19018), which is at address 0x7DF74744.

So to access the nodes you would do something like this:

```c
// C++ Implementation of the Windows
// vectored exception handling mechanism
 
#include <iostream>
#include <string>
#include <windows.h>
 
#define WINDOWS_7_BLOCK 0x7DF74744
 
typedef struct _VECTORED_NODE
{
    _VECTORED_NODE *NextNode;
    _VECTORED_NODE *PrevNode;
    BOOL            IsAllocated;
    PVOID           EncodedHandler;
}VECTORED_NODE, *PVECTORED_NODE;
 
typedef struct _RTL_BLOCK
{
    PVOID   Unknown;
    PVECTORED_NODE ExceptionList;
}RTL_BLOCK, *PRTL_BLOCK;
 
LONG CALLBACK TopLevelHandler(EXCEPTION_POINTERS *info)
{
    if(info->ExceptionRecord->ExceptionCode == EXCEPTION_ACCESS_VIOLATION)
    {
        std::cout << "Yep, caught" << std::endl;
    }
 
    return EXCEPTION_CONTINUE_SEARCH;
}
 
int main(int, char**)
{
    std::cout << std::hex << TopLevelHandler << std::endl;
    AddVectoredExceptionHandler(TRUE, TopLevelHandler);
 
    PRTL_BLOCK block = reinterpret_cast<PRTL_BLOCK>(WINDOWS_7_BLOCK);
    PVECTORED_NODE firstNode = block->ExceptionList;
 
    std::cout << DecodePointer(firstNode->EncodedHandler) << std::endl;
 
    return 0;
}
```

That accesses the first node. In that case I added a top-most handler manually thus it should print the address of my handler.

But anyways without further a due here's the C++ equivalent of AddVectoredExceptionHandler:

```c
/*
Used to add a new handler to
the beginning of the linked
list as the top-most node/handler
*/
void add_handler(void)
{
    // Allocate new memory for node
    PRTL_BLOCK block = reinterpret_cast<PRTL_BLOCK>(WINDOWS_7_BLOCK);
    PVECTORED_NODE edi = block->ExceptionList->NextNode;
    PVECTORED_NODE eax = edi->NextNode;
    PVECTORED_NODE allocNode = new VECTORED_NODE;
 
    // Do some interlocked magic
    if(edi->PrevNode == edi)
    {
        _interlockedbittestandset((volatile long*) (*(DWORD *) (__readfsdword(24) + 48) + 40), 0 + 2);
    }
 
    // Set up the new node
    allocNode->EncodedHandler = EncodePointer(TopLevelHandler);
    allocNode->IsAllocated = TRUE;
 
    // Link up the new node
    allocNode->NextNode = eax;
    allocNode->PrevNode = edi;
 
    // Redirect old nodes
    eax->PrevNode = allocNode;
    edi->NextNode = allocNode;
}
```

I didn't change variable names but you can figure it out (hopefully).