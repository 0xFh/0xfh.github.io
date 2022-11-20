---
date: 2021-07-07T23:00:00.000Z
layout: post
title: YouBeeEx challenge
subtitle: [write-up]
description: >-
  A write-up for you_bee_ex ctf challenge from CyberTalents platform
image: >-
  https://1.bp.blogspot.com/-0JOgq4o_Xic/YOVplMh0_zI/AAAAAAAABdc/0OGE3HdPMDob3ZbFq17ORWp945OsfWMwACPcBGAYYCw/s1300/Bee.png
optimized_image: >-
  https://1.bp.blogspot.com/-0JOgq4o_Xic/YOVplMh0_zI/AAAAAAAABdc/0OGE3HdPMDob3ZbFq17ORWp945OsfWMwACPcBGAYYCw/s1300/Bee.png
category: writeup
tags:
  - writeup
  - ctf
  - malware_reverse_engineering
  - upx
  - ida
  - scylla
author: Abdullah Aiman
paginate: true
---

> Level: Hard
> Points: 200

#Summary
The executable is packed and after analysis we can identify that it requires 101~104 arguments to return something, it uses GetModuleFileNameAwindows API to get the pass of the file then xor all characters from the path with a constant value and finally print the text.
So we should unpack it, but there is an error from the unpacking process and we should unpack it manually or use a debugger to follow the process and patch the pass of the file then execute it till return the flag in the cmd.

First, by identifying the type of the file we get that 'you_bee_ex.exe' is PE32 executable for Windows and it may be packed with UPX as we can see in this image:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

So let’s extract the metadata from it using [Strings – Resource Hacker – PeStudio – Capa] …

> Strings
Using Strings to see any important or interesting string but nothing important I think...
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

> Resource Hacker
Trying Resource Hacker but the same as strings, still nothing...
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

> PeStudio
As we can see in the next image, sections show that there is a big difference between raw-size and virtual-size which means the executable is packed...
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Checking imports and libraries but nothing new:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

> Capa
By identifying the capabilities in our executable using Capa to know more about its behavior:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")
I didn’t get anything new but now I know the executable is packed, Nice!
What a new and interesting something -_-
Let’s go for Basic Dynamic Analysis to know more about its behavior

Run the executable and check Procmon but there is nothing..
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

So I should go for Advanced Static Analysis now...

> IDA
Using IDA to check the instructions and search for anything interesting but before this we should unpack the executable
We can unpack it by **UPX** tool, check the next image:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")
Let’s Open IDA...
By following the main function we got that:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")
The function takes some arguments and then checks if they are between 100 and 105 and if true then it will get the pass of the file by `GetModuleFileNameA` API ..
After that it will xor all characters from the path with a constant value then print the encoded strings.
Let’s try to pass 101 arguments to the packed and unpacked files to see what happens:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")
we got different results from the 2 files.

As we know the GetModuleFileNameA API get the pass of the executable so we can debug the packed file, follow the API, get the pass, and then patch it with the original pass.

We can get the original path from the unpacked file by Strings :
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

This is the original pass:
`C:\Users\11x256\documents\visual studio 2015\Projects\ConsoleApplication1\Debug\ConsoleApplication1.pdb`

> x32dbg

Let’s debug the packed one with **x32dbg** and follow the API...
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

We should add the arguments to the command line of the program first, you can do it from `File > Change Command Line`

And then add the arguments as the next image:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Now set the API as a breakpoint by adding `bp GetModuleFileNameA` to command here:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

After running, it will break at the API as the next image:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

We don’t need to know what happened in the API so just click on `execute till return` and we will get that:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Step over one time to get this:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Let’s understand it now,
If u back to IDA you will see the same cmp instruction here:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

And we can get that `[ebp-14]` before it in the last image in **x32dbg** will be the pass of the file in our system, so we can get the value of the pass by follow its value in the dump like the next image:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

And we got it:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Now we can patch the pass and add the original pass that we get from strings before, just right click on the pass > Binary > Edit :
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

We will get this window:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Now just change the pass value:
`C:\Users\10x64\Desktop\you_bee_ex.exe`
With the original one:
`C:\Users\11x256\documents\visual studio 2015\Projects\ConsoleApplication1\Debug\ConsoleApplication1.pdb`
And press OK :
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Notice the change here:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Ok if we just scroll back some lines we will get the xor instruction:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

So we can click on `execute till return` to see what will return:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Check the program console and...
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")

Yes we got the **** flag! x_x

OK there are another methods you can solve the challenge with them like getting the tail jump and get the **OEP** (Original Entry Point) in x32dbg, then by using **Scylla** plugin to fix the **OEP** and check the executable in IDA but it will take more time than the debugging method here, So you can solve it by different ways .

Sorry for my English btw xD

Good Luck <3
