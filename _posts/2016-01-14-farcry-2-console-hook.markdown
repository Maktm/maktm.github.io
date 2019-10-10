---
layout: post
title:  "FarCry 2 In-game Console Hook"
date:   2016-01-14
categories: jekyll update
---

I got bored again so I decided to come back to Far Cry 2 to see if it's any fun. As someone that doesn't have the energy to learn anything drawing related (dx/opengl) I decided that I'd go the usual route of CLI-based commands. The in-game console can be launched using the tilde (~) key and you can issue a limited set of commands. Using the data structures/functions below you can replace the default callback function with your custom one in order to add custom commands for your hax.

NOTE: Since there isn't any data on this game (SDK/PDB/etc.) I'm naming data structures with names I deem fit.

```c
struct __declspec(align(4)) IConsole

{

 __int32 N00000001[0x1A];

 char EnableConsoleWrite;

 __int16 mIsOpen;

 char Padding1;

 __int32 N0000001[0X90];

};
```

IConsole represents the in-game console. There's 1 instance of this class which is a global variable located at:

```c
.data:11555980
```

You can build a wrapper class that uses singleton pattern to properly rebuild this class or just use the pointer directly.

Every time a command is entered the game takes the input, formats it and sends it to the callback function for parsing, filtering etc. The address of this callback function is at:

```c
.code:0x102955E0
```

Pseudo-code for the callback function (Hexrays):

```
void __thiscall OnCommandEntered(IConsole *this, CommandDescriptor *commandDescriptor)

{

  IConsole *pConsole; // ebp@1

  unsigned int src_; // esi@8

  _BYTE *v4; // ecx@8

  unsigned int v5; // eax@11

  unsigned int v6; // esi@13

  unsigned int v7; // eax@13

  unsigned int v8; // eax@13

  int v9; // ST10_4@15

  unsigned int v10; // ST0C_4@15

  unsigned int srcLength; // eax@18

  const char *v12; // eax@18

  const char *v13; // esi@18

  int v14; // eax@20

  int v15; // edi@22

  int v16; // edi@24

  int v17; // eax@27

  int v18; // eax@27

  const char *i; // eax@27

  void *v20; // eax@32

  int v21; // ST10_4@34

  void *v22; // ST0C_4@34

  int v23; // esi@35

  char *v24; // eax@35

  CommandDescriptor *v25; // ST14_4@35

  wchar_t *v26; // eax@35

  int v27; // eax@38

  wchar_t *v28; // eax@39

  char *v29; // eax@41

  const char *v30; // eax@44

  const char *result; // eax@45

  const char *v32; // eax@46

  _BYTE *v33; // eax@46

  const char *v34; // eax@49

  int v35; // edi@49

  signed int v36; // ebp@51

  unsigned int v37; // eax@51

  const char **v38; // ebx@51

  const char *v39; // ecx@52

  const void *v40; // edx@57

  const char *v41; // ecx@60

  char *v42; // ecx@64

  int v43; // eax@67

  int v44; // ecx@70

  int v45; // eax@70

  wchar_t *v46; // eax@71

  char *v47; // eax@74

  char *commandStr; // eax@80

  char searchCharacter; // [sp+10h] [bp-150h]@1

  int v50; // [sp+14h] [bp-14Ch]@21

  bool v51; // [sp+1Bh] [bp-145h]@48

  int a1; // [sp+1Ch] [bp-144h]@1

  unsigned int src; // [sp+20h] [bp-140h]@5

  int v54; // [sp+30h] [bp-130h]@6

  unsigned int v55; // [sp+34h] [bp-12Ch]@4

  void *v56; // [sp+38h] [bp-128h]@1

  char *userCommandStr; // [sp+3Ch] [bp-124h]@1

  int v58; // [sp+4Ch] [bp-114h]@1

  unsigned int v59; // [sp+50h] [bp-110h]@1

  int v60; // [sp+54h] [bp-10Ch]@48

  unsigned int v61; // [sp+58h] [bp-108h]@48

  int v62; // [sp+5Ch] [bp-104h]@48

  char v63; // [sp+60h] [bp-100h]@26

  char *v64; // [sp+64h] [bp-FCh]@32

  int v65; // [sp+74h] [bp-ECh]@34

  unsigned int v66; // [sp+78h] [bp-E8h]@32

  int descriptor_1; // [sp+7Ch] [bp-E4h]@20

  IConsole *pConsole1; // [sp+98h] [bp-C8h]@1

  int (__stdcall **v69)(int, int); // [sp+9Ch] [bp-C4h]@35

  wchar_t *fmt; // [sp+A0h] [bp-C0h]@35

  int v71; // [sp+A4h] [bp-BCh]@78

  int *v72; // [sp+A8h] [bp-B8h]@78

  int v73; // [sp+ACh] [bp-B4h]@78

  unsigned int v74; // [sp+B4h] [bp-ACh]@35

  int descriptor; // [sp+B8h] [bp-A8h]@8

  char v76; // [sp+D4h] [bp-8Ch]@21

  char v77; // [sp+F0h] [bp-70h]@27

  int v78; // [sp+10Ch] [bp-54h]@27

  char v79; // [sp+128h] [bp-38h]@27

  char v80; // [sp+144h] [bp-1Ch]@27

 

  pConsole = this;

  pConsole1 = this;

  sub_100033D0(&a1, commandDescriptor);         // parses the descriptor?

  v56 = &unk_10F239D1;

  v59 = 15;

  searchCharacter = 0;

  v58 = 0;

  std::char_traits<char>::assign(&userCommandStr, &searchCharacter);// ZeroMemory

  if ( !sub_1028F9E0(pConsole, &a1) )

  {

    if ( !v54 )

    {

LABEL_85:

      sub_10096AC0(&v56);

      sub_10096AC0(&a1);

      return;

    }

    CommandDescriptor::CommandDescriptor(&descriptor, &version);// Constructor for a2

    sub_10002850(&descriptor, commandDescriptor, 0, 0xFFFFFFFF);

    sub_10292EE0(pConsole, &descriptor);

    src_ = src;

    v4 = src;

    if ( v55 < 0x10 )

      v4 = &src;

    if ( *v4 == 35 )

    {

      v5 = src;

      if ( v55 < 0x10 )

        v5 = &src;

      v6 = v5 + 1;

      v7 = std::char_traits<char>::length(v5 + 1);

      sub_10002D40(&a1, v6, v7);

      v8 = src;

      if ( v55 < 0x10 )

        v8 = &src;

      v9 = v54;

      v10 = v8;

      sub_102A7350();

      sub_102A7C90(v10, v9, 0);

      goto LABEL_84;

    }

    if ( v55 < 0x10 )

      src_ = &src;

    srcLength = std::char_traits<char>::length(src_);

    sub_10002D40(&a1, src_, srcLength);

    searchCharacter = 32;

    v12 = ParseCommand(&a1, &searchCharacter, 0x100000000ui64);

    v13 = v12;

    if ( v12 == -1 )

    {

      sub_10002B00(&v56, &a1, 0, 0xFFFFFFFF);

    }

    else

    {

      v14 = sub_10003110(&descriptor_1, 0, v12);

      sub_10002B00(&v56, v14, 0, 0xFFFFFFFF);

      sub_10096AC0(&descriptor_1);

    }

    sub_10742C00(&v56);

    sub_10002CC0(&v76);

    sub_10290810(&v50, &v76);

    if ( v50 != pConsole->N00000012 )

    {

      v15 = v50 + 40;

      if ( sub_1028F7D0(pConsole, v50 + 40) )

      {

        sub_10002B00(&a1, &a1, 0, 0xFFFFFFFF);

        sub_10294370(pConsole, v15, &a1);

        goto EXIT_FUNCTION;

      }

    }

    sub_10290190(&v50, &v56);

    v16 = v50;

    if ( v50 != pConsole->N00000018 )

    {

      v50 = *(v50 + 40);

      if ( sub_1028F7D0(pConsole, v50) )

      {

        sub_10047090(&v63);

        sub_100032C0(v16 + 12);

        if ( v13 != -1 )

        {

          v17 = sub_10003110(&descriptor_1, v13 + 1, -1);

          sub_100032C0(v17);

          sub_10096AC0(&descriptor_1);

          sub_10003300(&descriptor_1, filename);

          sub_10003300(&v78, "=\"");

          sub_100031F0(&v77, &v78);

          sub_100031F0(&v80, &a1);

          v18 = sub_100031F0(&v79, &descriptor_1);

          sub_100032C0(v18);

          sub_10096AC0(&v79);

          sub_10096AC0(&v80);

          sub_10096AC0(&v77);

          sub_10096AC0(&v78);

          sub_10096AC0(&descriptor_1);

          searchCharacter = 32;

          for ( i = ParseCommand(&a1, &searchCharacter, 0x100000000ui64);

                i != -1;

                i = ParseCommand(&a1, &searchCharacter, 0x100000000ui64) )

          {

            sub_100BC060(&a1, i, 1u);

            searchCharacter = 32;

          }

          if ( AreStringsEqual(&a1, "?") || AreStringsEqual(&a1, "help") )

          {

            v27 = sub_10290660(&v77);

            if ( *(v27 + 24) < 8u )

              v28 = (v27 + 4);

            else

              v28 = *(v27 + 4);

            CommandDescriptor::CommandDescriptor(&descriptor_1, v28);

            v29 = userCommandStr;

            if ( v59 < 0x10 )

              v29 = &userCommandStr;

            sub_10293700(pConsole, &descriptor_1, v29);

            sub_10002930(&descriptor_1);

            sub_10002930(&v77);

            sub_10096AC0(&v63);

            goto EXIT_FUNCTION;

          }

          if ( v54 )

          {

            v20 = v64;

            if ( v66 < 0x10 )

              v20 = &v64;

            v21 = v65;

            v22 = v20;

            sub_102A7350();

            sub_102A7C90(v22, v21, 0);

          }

        }

        v23 = v50;

        v24 = sub_102ADAC0(v50);

        sub_10003300(&v78, v24);

        CommandDescriptor::CommandDescriptor(&descriptor_1, L" = ");

        v25 = Descriptor::Descriptor(&v79, &v78);

        Descriptor::Descriptor(&v80, v23 + 4);

        sub_10191620(&v77, &descriptor_1);

        sub_10191620(&v69, v25);

        sub_10002930(&v77);

        sub_10002930(&v80);

        sub_10002930(&v79);

        sub_10002930(&descriptor_1);

        sub_10096AC0(&v78);

        v26 = fmt;

        if ( v74 < 8 )

          v26 = &fmt;

        WriteToConsole(pIConsole, 0, v26);

        sub_10002930(&v69);

        sub_10096AC0(&v63);

EXIT_FUNCTION:

        sub_10096AC0(&v76);

LABEL_84:

        sub_10002930(&descriptor);

        goto LABEL_85;

      }

    }

    searchCharacter = 63;

    v30 = ParseCommand(&a1, &searchCharacter, '\x01\0\0\0\0');

    searchCharacter = 0;

    if ( v30 + 1 == 0 )

    {

      searchCharacter = 33;

      result = ParseCommand(&a1, &searchCharacter, '\x01\0\0\0\0');

      searchCharacter = 1;

      if ( result + 1 == 0 )

      {

        if ( commandDescriptor->CopySize < 8u )

          commandStr = &commandDescriptor->UserCommand;

        else

          commandStr = commandDescriptor->UserCommand;

        WriteToConsole(pIConsole, '\0', L"Unknown command: %s", commandStr);

        goto EXIT_FUNCTION;

      }

    }

    v32 = sub_10AD7180(&a1, " ", '\0');

    sub_10003110(&v63, '\0', v32);

    v33 = src;

    if ( v55 < 0x10 )

      v33 = &src;

    v51 = *v33 == 63;

    v60 = '\0';

    v61 = '\0';

    v62 = 0;

    sub_10295300(&v60, 1);

    v50 = '\0';

    if ( v61 <= '\0' )

    {

LABEL_78:

      v72 = &v61;

      fmt = &v60;

      v71 = 0;

      v73 = 4;

      v69 = &off_10DC385C;

      sub_10225F00(&v69);

      sub_10096AC0(&v63);

      goto EXIT_FUNCTION;

    }

    while ( 1 )

    {

      v34 = v64;

      v35 = *(v60 + 4 * v50) + 4;

      if ( v66 < 0x10 )

        v34 = &v64;

      v36 = strlen(v34);

      v37 = *(*(v60 + 4 * v50) + 28);

      v38 = (*(v60 + 4 * v50) + 8);

      if ( v37 < 0x10 )

        v39 = (*(v60 + 4 * v50) + 8);

      else

        v39 = *v38;

      if ( strlen(v39) < v36 )

        goto LABEL_77;

      if ( !v51 )

      {

        if ( searchCharacter )

        {

          v42 = v64;

          if ( v66 < 0x10 )

            v42 = &v64;

          if ( v37 < 0x10 )

            v43 = *(v60 + 4 * v50) + 8;

          else

            v43 = *v38;

          if ( !sub_1018DAD0(v43, v42) )

            goto LABEL_77;

        }

        else

        {

          v40 = v64;

          if ( v66 < 0x10 )

            v40 = &v64;

          if ( v37 < 0x10 )

            v41 = (*(v60 + 4 * v50) + 8);

          else

            v41 = *v38;

          if ( memicmp(v41, v40, v36) )

            goto LABEL_77;

        }

      }

      v44 = *(v60 + 4 * v50);

      v45 = sub_10290660(&v77);

      if ( *(v45 + 24) < 8u )

        v46 = (v45 + 4);

      else

        v46 = *(v45 + 4);

      CommandDescriptor::CommandDescriptor(&descriptor_1, v46);

      if ( *(v35 + 24) < 0x10u )

        v47 = (v35 + 4);

      else

        v47 = *v38;

      sub_10293700(pConsole1, &descriptor_1, v47);

      sub_10002930(&descriptor_1);

      sub_10002930(&v77);

LABEL_77:

      if ( ++v50 >= v61 )

        goto LABEL_78;

    }

  }

  if ( v59 >= 0x10 )

    sub_10419130(userCommandStr);

  v59 = 15;

  searchCharacter = 0;

  v58 = 0;

  std::char_traits<char>::assign(&userCommandStr, &searchCharacter);

  if ( v55 >= 0x10 )

    sub_10419130(src);

  v55 = 15;

  searchCharacter = 0;

  v54 = 0;

  std::char_traits<char>::assign(&src, &searchCharacter);

}
```

The function takes a pointer to a data structure I named CommandDescriptor. This contains information such as the actual string, length of the string and some other stuff that I'll act like I understand. CommandDescriptor:

```c
struct CommandDescriptor

{

 int field_0;

 char *UserCommand;

 int field_8;

 int field_C;

 int field_10;

 int CommandLength;

 int CopySize;

};
```

If you want to implement your own commands then just hook/redirect the callback function and add in your own parsing, filtering code. A nice way to implement this would be to use an std::map that maps strings (the command) to a second callback function to be called for that specific command.

Enjoy (:

Bonus:

The version of the console is printed in a different position inside of the console. In order to print out in any position on the screen you can reverse the way it's printed. This is at:

```c
.text:104C262F
```