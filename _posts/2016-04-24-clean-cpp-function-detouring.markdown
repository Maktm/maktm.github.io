---
layout: post
title:  "Clean C++ Member Function Detouring"
date:   2016-04-24
categories: jekyll update
---

Whenever I write detour code related to member functions things tend to get messy so today I decided not to be lazy and to do it properly. In order to write a detour for a member function cleanly you can do the following:

* Stop using __fastcall and use __thiscall as MSVC has support for declaring functions with __thiscall now
* Rebuild the entire class with its members, inheritance hierarchy and even the member functions just as if it were your code
* Use static pointers-to-member functions to manage your trampoline pointers
* Add support for member function detouring to your detour code
* Simple functions that return the address for each method

Example:

As an example I'll use a portion of Skype's Socket class. The first step is reversing a method (class function) that utilizes the data structure/class that is Socket so I found the Send/Receive methods which are Socket::Send and Socket::Receive.

Socket::Send Signature:

```c
signed int __thiscall Socket::Send(Socket *this, int Data, int Len);
```

Socket::Send Pattern:

```c
std::string const kSocketSendSig =     "8B 41 58 85 C0 74 0B 83 F8 04 74 06 83 C8 "
                                       "FF C2 08 00 8B 41 5C 53 FF 74 24 0C FF 74 "
                                       "24 0C 85 C0 74 20 50 E8 ? ? ? ? 8B D8 53 FF "
                                       "74 24 1C FF 74 24 1C 68 ? ? ? ? E8 ? ? ? ? 83 "
                                       "C4 1C EB 07 E8 ? ? ? ? 8B D8 85 DB 74 2B 83 FB FD";
```

Socket::Receive Signature:

```c
signed int __thiscall Socket::Receive(Socket *this, int Data, int Len);
```

Socket::Receive Pattern:

```c
std::string const kSocketReceivedSig =     "8B 41 58 85 C0 74 0B 83 F8 04 74 06 83 C8 "
                                           "FF C2 08 00 8B 41 5C 53 FF 74 24 0C FF 74 "
                                           "24 0C 85 C0 74 20 50 E8 ? ? ? ? 8B D8 53 FF "
                                           "74 24 1C FF 74 24 1C 68 ? ? ? ? E8 ? ? ? ? 83 "
                                           "C4 1C EB 07 E8 ? ? ? ? 8B D8 85 DB 74 2B 83 FB FE";
```

Inside of hex-rays right-click the signature of the pseudo-subroutine and click 'Create new struct type' and you have the members declared for you.

![Hex-rays pseudo code](https://puu.sh/ouB2k/c2cf9d7d23.png)

Hex-rays gave me this data structure:

```c
/* 288 */
struct __declspec(align(4)) Socket
{
  int gap0;
  int field_4;
  int field_8;
  int field_C;
  int field_10;
  int field_14;
  int field_18;
  int field_1C;
  int field_20;
  int field_24;
  int field_28;
  int field_2C;
  int field_30;
  int field_34;
  int field_38;
  int field_3C;
  int field_40;
  int field_44;
  int field_48;
  int field_4C;
  int field_50;
  int field_54;
  _DWORD dword58;
  _DWORD dword5C;
};
```

Now there's a start for creating the rest of the class. Now add the simple functions that return the address of the two methods after using a find pattern (or a hard-coded address depending on what you do).

```c++
std::uint8_t *GetSocketSendAddress()
{
    std::string const kSocketSendSig = "8B 41 58 85 C0 74 0B 83 F8 04 74 06 83 C8 "
                                       "FF C2 08 00 8B 41 5C 53 FF 74 24 0C FF 74 "
                                       "24 0C 85 C0 74 20 50 E8 ? ? ? ? 8B D8 53 FF "
                                       "74 24 1C FF 74 24 1C 68 ? ? ? ? E8 ? ? ? ? 83 "
                                       "C4 1C EB 07 E8 ? ? ? ? 8B D8 85 DB 74 2B 83 FB FD";
 
    std::uint8_t *result = Core::FindPattern(Core::SearchRegion::CodeSection,
                                             false,
                                             kSocketSendSig);
 
    if (result == nullptr)
    {
        CORE_THROW_EXCEPTION(Core::Error{} << "FindPattern failed - Socket::Send");
    }
 
    return result;
}
 
std::uint8_t *GetSocketReceiveAddress()
{
    std::string const kSocketReceivedSig = "8B 41 58 85 C0 74 0B 83 F8 04 74 06 83 C8 "
                                           "FF C2 08 00 8B 41 5C 53 FF 74 24 0C FF 74 "
                                           "24 0C 85 C0 74 20 50 E8 ? ? ? ? 8B D8 53 FF "
                                           "74 24 1C FF 74 24 1C 68 ? ? ? ? E8 ? ? ? ? 83 "
                                           "C4 1C EB 07 E8 ? ? ? ? 8B D8 85 DB 74 2B 83 FB FE";
 
    std::uint8_t *result = Core::FindPattern(Core::SearchRegion::CodeSection,
                                             false,
                                             kSocketReceivedSig);
 
    if (result == nullptr)
    {
        CORE_THROW_EXCEPTION(Core::Error{} << "FindPattern failed - Socket::Receive");
    }
 
    return result;
}
```

At this point you know the signature of the methods Socket::Send and Socket::Receive and you have the members too (probably not every member). At this point you can construct a portion of the class using what you have and come up with something like this:

```c++
class Socket
{
public:
    // Provides logging for data received by outputting
    // to a file from Socket::Receive.
    void Receive(std::uint8_t *data, std::size_t len);
 
    // The detour function for Socket::Send used
    // for logging the data being sent.
    void Send(std::uint8_t *data, std::size_t len);
 
private:
    int gap0;
    int field_4;
    int field_8;
    int field_C;
    int field_10;
    int field_14;
    int field_18;
    int field_1C;
    int field_20;
    int field_24;
    int field_28;
    int field_2C;
    int field_30;
    int field_34;
    int field_38;
    int field_3C;
    int field_40;
    int field_44;
    int field_48;
    int field_4C;
    int field_50;
    int field_54;
    unsigned long dword58;
    unsigned long dword5C;
};
```

My definition of Socket::Send and Socket::Receive can be used when I want to call the function manually but I can also use it as my detour function that replaces the real Socket::Send and Socket::Receive. In order detour a member function you have to be able to store a pointer to it and for this you need a special cast which kokole posted:

https://www.unknowncheats.me/forum/general-programming-and-reversing/175103-function-address.html

In your normal detour class if you already provide a templated constructor for accepting the target and detour then just create a specialization like I did:

```c++
template <typename T1, typename T2>
PatchDetour(T1 target, T2 detour, bool attach = true)
    :   DetourModel{reinterpret_cast<std::uint8_t*>(target),
                    reinterpret_cast<std::uint8_t*>(detour)},
        allocator_{kAllocatorSize}
{
    Generate();
    if (attach)
    {
        Attach();
    }
}

template <typename T1, typename T2, typename C, typename... A>
PatchDetour(T1 target, T2(C::* detour)(A...), bool attach = true)
    : DetourModel{reinterpret_cast<std::uint8_t*>(target),
                  static_cast<std::uint8_t*>((void*&)detour)},
      allocator_{kAllocatorSize}
{
    Generate();
    if (attach)
    {
        Attach();
    }
}
```

One final issue that you'll come across is declaring the trampoline. Normally I see people declaring trampolines as global variables that are pointer-to-functions but casting to a pointer-to-member function type isn't possible so you can't do:

```c++
std::uint8_t *ptr = nullptr;
    void(Skymod::Socket::*mptr)(std::uint8_t*, std::size_t) = magic_cast<void(Skymod::Socket::*)(std::uint8_t*, std::size_t)>(ptr);
```

But what you can do is use the __thiscall calling convention. Don't make the same mistake I did and assume __thiscall will do some magic that'll just pass on ecx to the function being called. The first parameter in a __thiscall pointer-to-member function declaration is a pointer to the class type. So what I did was:

```c++
class Socket
{
public:
    // Provides logging for data received by outputting
    // to a file from Socket::Receive.
    void Receive(std::uint8_t *data, std::size_t len);
 
    // The detour function for Socket::Send used
    // for logging the data being sent.
    void Send(std::uint8_t *data, std::size_t len);
 
public:
    using SocketSendT = void(__thiscall*)(Socket*, std::uint8_t*, std::size_t);
    using SocketReceiveT = void(__thiscall*)(Socket*, std::uint8_t*, std::size_t);
 
    static SocketSendT oSocketSend;
    static SocketReceiveT oSocketReceive;
 
private:
    int gap0;
    int field_4;
    int field_8;
    int field_C;
    int field_10;
    int field_14;
    int field_18;
    int field_1C;
    int field_20;
    int field_24;
    int field_28;
    int field_2C;
    int field_30;
    int field_34;
    int field_38;
    int field_3C;
    int field_40;
    int field_44;
    int field_48;
    int field_4C;
    int field_50;
    int field_54;
    unsigned long dword58;
    unsigned long dword5C;
};
```

I declared the trampolines oSocketSend and oSocketReceive as static objects variables inside of the class because it'll be easier to identify it later using Socket:Socket etc. Of course you'll have to define the variable in your implementation file (.cpp).

And yeah so you can finally write code like this:

```c++
void InitHooks()
{
    using Core::PatchDetour;
 
    static Core::PatchDetour socketsend(Skymod::GetSocketSendAddress(), &Skymod::Socket::Send, false);
    Skymod::Socket::oSocketSend = reinterpret_cast<Skymod::Socket::SocketSendT>(socketsend.GetTrampoline());
 
    static Core::PatchDetour socketreceive(Skymod::GetSocketReceiveAddress(), &Skymod::Socket::Receive, true);
    Skymod::Socket::oSocketReceive = reinterpret_cast<Skymod::Socket::SocketReceiveT>(socketreceive.GetTrampoline());
}
```

Result (some stuff I was working on today):

![Work from today](https://puu.sh/ouBXX/6bb951db39.png)