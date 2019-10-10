---
layout: post
title:  "Writing IDA Plugins in C/C++"
date:   2015-03-02
categories: jekyll update
---

Hey,

so this is just a small tutorial to get your environment all set up to write some neat IDA plugins. I've read in a lot of places that you have to move to VS2010 in order to write plugins but the error was that the Windows header file was included twice. I fixed it a while back with one of the IDA SDK header files and can't which header file needed the fix but I've provided the same ones I use so it will most likely work for you.

## Setting up your environment
Fire up Visual Studio (I'm using 2013 because it's the best, no flame pls) and CTRL + SHIFT + N for a new project. Once you have the window open select 'Win32 Project' and click 'Ok' (After you name it of course).

![New Project in Visual Studio](https://i.imgur.com/vMfp97N.png)

Next up press 'Next' and choose the 'DLL' radio button, select 'Empty Project' and I disable SDL checks but keep it if you so please. Window should look something like this:

![Win32 Application Wizard](https://i.imgur.com/aXzYjTQ.png)

Click 'Finish' and you then have your project. To set up your complete environment just follow these steps carefully :

Open up your project's settings page by right-clicking your project and going to 'Properties'. Once you have that dialog follow these steps.

1. Click 'Configuration Manager' on the top left and in the Configuration panel change it from 'Debug' to 'Release'

2. Go to Configuration Properties -> General -> Output Directory and in the input box change it to your ida plugins directory. For example mine is `C:\Users\Mike\Desktop\Storage\IDA_v6.1\IDA_v6.1\plugins`

Then go to Linker -> General: Change 'Output File' to $(OutDir)nameofyourplugin.plw


3. Go to C/C++ -> General -> Additional Include Directories: Put in your sdk's include path. Mine is `
C:\Users\Mike\Desktop\Idasdk61\idasdk61\include`

4. Go to C/C++ -> Preprocessor: Add __NT__;__IDP__; to your preprocessor definitions

5. Go to C/C++ -> Code Generation: Set Basic Runtime Checks to 'Default' and disable 'Security Checks'

6. Go to C/C++ -> Advanced: Change your 'Calling Convention' to __stdcall

7. Go to Linker -> General -> Additional Library Directories: Add your lib file directory. Mine is `C:\Users\Mike\Desktop\Idasdk61\idasdk61\lib\x86_win_vc_32`

8. Go to Linker -> Input: Add one more lib file. It is the ida.lib file located in the same directory as the instruction before this (no. 7). Add the full path and don't forget the ; at the end. Mine looks like this `kernel32.lib;user32.lib;gdi32.lib;winspool.lib;comdlg32.lib;advapi32.lib;shell32.lib;ole32.lib;oleaut32.lib;uuid.lib;odbc32.lib;odbccp32.lib;C:\Users\Mike\Desktop\Idasdk61\idasdk61\lib\x86_win_vc_32\ida.lib;%(AdditionalDependencies)`
9. Go to Linker -> Debugging: Set 'Generate Debug Info' to 'No'.

10. Since we want IDA to open up as soon as we build our plugin everytime (to see the changes and quickly test) we have to do this :
Go to Build Events -> Post-Build Event: Set it to your ida executable directory. Mine is `C:\Users\Mike\Desktop\Storage\IDA_v6.1\IDA_v6.1\idaq.exe`

## Template

```c++
#include <Windows.h>
#include <ida.hpp>
#include <idp.hpp>
#include <loader.hpp>

int __stdcall IDAP_init(void)
{
 // Do checks here to ensure your plug-in is being used within
 // an environment it was written for. Return PLUGIN_SKIP if the
 // checks fail, otherwise return PLUGIN_KEEP.
 msg("[+] I am now running");

 return (PLUGIN.flags & PLUGIN_UNL) ? PLUGIN_OK : PLUGIN_KEEP;
}

void __stdcall IDAP_term(void)
{
 // Stuff to do when exiting, generally you'd put any sort
 // of clean-up jobs here.
 return;
}

void __stdcall IDAP_run(int arg);
// There isn't much use for these yet, but I set them anyway.

char IDAP_comment[] = "IDA Plugin by ____";
char IDAP_help[] = "ida plug-in template";

// The name of the plug-in displayed in the Edit->Plugins menu. It can
// be overridden in the user's plugins.cfg file.

char IDAP_name[] = "IDA Plugin by _____";

// The hot-key the user can use to run your plug-in.
char IDAP_hotkey[] = "Ctrl-Alt-X";

 

// The all-important exported PLUGIN object

plugin_t PLUGIN =
{
 IDP_INTERFACE_VERSION,  // IDA version plug-in is written for
 PLUGIN_UNL,     // Flags (see below)
 IDAP_init,      // Initialisation function
 IDAP_term,      // Clean-up function
 IDAP_run,       // Main plug-in body
 IDAP_comment,   // Comment unused
 IDAP_help,      // As above unused
 IDAP_name,      // Plug-in name shown in
 IDAP_hotkey     // Hot key to run the plug-in
};

BOOL CALLBACK EnumIdaMainWindow(HWND hwnd, LPARAM lParam)
{
 WINDOWINFO winInfo;
 DWORD dwIdaProcessId = *((DWORD*)lParam);
 DWORD dwProcessId;
 GetWindowThreadProcessId(hwnd, &dwProcessId);
 winInfo.cbSize = sizeof (WINDOWINFO);
 GetWindowInfo(hwnd, &winInfo);

 if (dwProcessId == dwIdaProcessId && GetParent(hwnd) == NULL
  && winInfo.dwStyle & WS_VISIBLE)
 {
  *((HWND *)lParam) = hwnd;

  return FALSE; // stop EnumWindow()
 }

 return TRUE;
}

HWND GetIdaMainWindow(void)
{
 DWORD dwIdaProcessId = GetCurrentProcessId();

 if (!EnumWindows(EnumIdaMainWindow, (LPARAM)&dwIdaProcessId))
 {
  return (HWND)dwIdaProcessId;

 }

 return NULL;
}

HWND GetIdaMainWindow(void);

static void __stdcall AskUsingForm(void);

// The plugin can be passed an integer argument from the plugins.cfg
// file. This can be useful when you want the one plug-in to do
// something different depending on the hot-key pressed or menu
// item selected.

void __stdcall IDAP_run(int arg)
{
 // The "meat" of your plug-in
 msg("ida plug-in run!\n");
 HWND hIdaMainWindow = GetIdaMainWindow();
 if (hIdaMainWindow == NULL)
  return;
}

static const char *dialog1 = //
"This is the title\n\n"// dialog title
"<##Radio Buttons##Radio 1:R>\n"
"<Radio 2:R>>\n"//ushort* number of selected radio
"<##Radio Buttons##Radio 1:R>\n"
"<Radio 2:R>>\n"//ushort* number of selected radio
"<##Check Boxes##Check 1:C>\n"
"<Check 2:C>>\n"//ushort* bitmask of checks
"<##Check Boxes##Check 1:C>\n"
"<Check 2:C>>\n";//ushort* bitmask of checks

static void __stdcall AskUsingForm(void)
{
 ushort bitMask, bitMask1 = 0;
 ushort btnIndex, bitIndex1;

 int ok = AskUsingForm_c(dialog1, &btnIndex, &bitIndex1, &bitMask, &bitMask1);
}
```

## Resources
Let me clarify that I learned how to do this from the PDF, 'IDA Plug-In Writing in C/C++' and I just changed up some of the instruction since it didn't work for me the first time. The PDF contains documented functions for IDA and it is what you should refer to. The SDK folder is also provided here.

https://www.unknowncheats.me/forum/downloads.php?do=file&id=13932

## Feedback

Some of you may have noticed that I am not an expert at using the different settings Visual Studio provides and if you see any problems I'd be happy to correct them. As for errors in the article please respond and I'll make the necessary amendments.