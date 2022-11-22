---
date: 2022-06-20T13:00:00.000Z
layout: post
title: PE File Format
subtitle: [Malware Basics]
description: >-
  A brief article about PE File and its structure
image: >-
  /assets/img/uploads/malware/pefile.png
optimized_image: >-
  /assets/img/uploads/malware/pefile.png
category: article
tags:
  - article
  - basics
  - PE
  - malware_analysis
author: Abdullah Aiman
paginate: true
---
## What is PE ?
Most of malware comes in the form of PE format, PE stands for **Portable Executable** which means that windows can run and execute the code inside it directly such as exe, dll, sys, fon, drv files
## PE Structure
PE file consists of 2 parts:

The first part is Headers such as DOS Header, PE Header, Optional Header, Data Directories, Sections Table.

The second part is PE Sections such as Code, Imports, Data of the file

## 1. Headers

### DOS MZ Header
It is located at the beginning of any PE File and it is **64 bytes** long.

The first 2 bytes are `MZ` and you can see them if you open any PE File in any Hex Editor.

### DOS Stub
Appears after the DOS header, and there is nothing to say except that it tells you; `This program cannot be run in DOS mode....` etc

### PE Header
It has a start signature, which is the first 4 bytes starts with 2 bytes which are `PE` and their Hex is `50 45`, then the last 2 bytes are `00` and `00`

### File Header
It is the **first 20 bytes** that come after the first 4bytes, and it contains information about **Number of Sections** in the file, **TimeDate Stamp**, and **Characteristic** that defines whether this file is exe or dll and whether it is 32 or 64 bits..etc.

### Optional Header
It comes after that, and It is not optional because It is the most important header that contains many important information such as **Address of Entry Point** (which starts the execution of the code), **Image Base** which helps us in the dynamic analysis, and **Size of Image** which is the size of the malware when running in the memory.

**Note**:
We do not need to count the number of bytes and extract information from them, because we can use automated tools such as **CFF-Explorer** and **PEstudio** to analyze the PE headers and extract information from them, these information such as:
`Entry Point, Image Base, Raw Size & Virtual Size, Characteristic Sections, Import Table, ASLR...` etc

### Data Directory
This refers to important things such as **Import & Export Table** which contains the functions and libraries that malware uses to encrypt or delete files or connect to external websites or download/upload files.. etc

**Note**: Since these libraries are for Windows such as dll files, the code that Malware will use will be inside the dll files not inside the malware itself, so it can show you that is not malicious !

**DLL** stands for **Dynamic Link Library** which is a library with a fixed name and contains fixed functions such as:
User32.dll - WSock32.dll - Wininet.dll - Advapi32.dll - Kernel32.dll

Each of them contains a set of common functions that can be used by malware.

**Kernel32.dll** contains functions such as:
CreateDirectoryW - CreateFileW - ReadFile - WriteFile

**Advapi32.dll** contains:
RegCloseKey - RegDeleteValueW - RegOpenKeyEx

**User32.dll** contains:
LoadIconW - LoadMenueW - MessageBoxW - SetClipboardData

And so on.

The **exported functions** are opposite of the **imported functions** so when We see a **DLL file**, We will find exported functions not imported one because another program uses them.

On the other hand, when We see an **executable file** we will find imported functions because the program will import them from another dlls

**Note**: This doesn't prevent that dlls contain both import & export together because the code may need functions from other place, so it must import them. and because there are functions that are used by other programs, you must export them.

### Why PE Headers are important?
It helps us in the Static Analysis stage because we know from them the names of the functions that the malware imports from Windows, so that if a function is used, it encrypts or deletes files, or communicates with websites on the Internet.. Thus, we know the capabilities of the malware and determine its type as well.

## Section
Sections contain the code that will be executed and the data of the program.
- **.text** contains the executable code to be executed
- **.data** contains the global data used by the program
- **.rdata** contains the read-only data, and this differs from .data in that it is readable only, so it cannot be modified.
- **.rsrc** contains the resources needed by the program such as icons, menus, dialogs, version & font information.  This section may contain any random data, and therefore the malware can hide data inside it that will be used in its malicious action, so it is important that we see its contents, and this can be done by using **Resource Hacker** and **CFF Explorer** tools to see the resources and contents.

Of course, there are other sections like .idata, .edata, .pdata, and .reloc

### Why Sections are important?
It helps us in the dynamic analysis stage when we run the malware and see the data inside it. and it helps in static analysis to get the hidden resources or hidden data from inside.
