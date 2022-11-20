---
date: 2021-07-20T13:00:00.000Z
layout: post
title: CyberTalents DFIR [RE] CTF
subtitle: [write-up]
description: >-
  This is my write-up for the DFIR Scholarship program ctf that hosted by CyberTalents
image: >-
  https://blogger.googleusercontent.com/img/a/AVvXsEg2KWlC-AfOZGv8IY1lA8XvszEEG8GDuUdwxX9A7IVgbe7TNaODau-PzGoo-DC5ZzH8BVCd3GLiWkm9FeMiPoAa4Dgx5d1znSCa_8ra3RkPRQLargJ0mLrtffeJ0DnM0kC6zE-LSLCGwi5fQOw6feVpBcH1I46zaYPE3CCjszS2lMIGWUCMTvrw6lECVg=s361
optimized_image: >-
  https://blogger.googleusercontent.com/img/a/AVvXsEg2KWlC-AfOZGv8IY1lA8XvszEEG8GDuUdwxX9A7IVgbe7TNaODau-PzGoo-DC5ZzH8BVCd3GLiWkm9FeMiPoAa4Dgx5d1znSCa_8ra3RkPRQLargJ0mLrtffeJ0DnM0kC6zE-LSLCGwi5fQOw6feVpBcH1I46zaYPE3CCjszS2lMIGWUCMTvrw6lECVg=s361
category: writeup
tags:
  - writeup
  - ctf
  - malware_reverse_engineering
  - cybertalents
author: Abdullah Aiman
paginate: true
---

# Description
This CTF is for DFIR Scholarship program to test the participant's technical skills. It will be in a Jeopardy Style where every player will have a list of challenges in Reverse Engineering category. For every challenge solved, the player will get a certain amount of points depending on the difficulty of the challenge.

# Challenges
- list
- r0t4t0r
- Assembly Master
- Dumper
- PE M0nSt3r
- ezez keygen2
- Kill Joy

**Note**: In this writeup I will explain the main steps to solve every challenge without writing the details to save the time...

## 1.List
Category: General Information
Level: Basic
Points: 25
Description: A security mechanism prohibiting the execution of those programs on a known malicious or undesired list of software.
> **Flag:** Google it xD

## 2.r0t4t0r
- Category: Malware Reverse Engineering
- Level: Easy
- Points: 50
- Description: One more rotation please.

### Solve:
Checking file type:

![](https://1.bp.blogspot.com/-seUjMSRGudg/YPZnC1PMX5I/AAAAAAAABfg/nU3spN4CfKsijSeeb4dhXY6kHuF4KPBcgCLcBGAsYHQ/s16000/Untitled.png)

Strings:

![](https://1.bp.blogspot.com/-7LdaZu4wUf0/YPZnmnaIOAI/AAAAAAAABfs/OuV1rh07V_YSfklJgnC-wDdAb4JtAUUXQCPcBGAYYCw/s16000/2.png)

well, I think it requires a password then it will encrypt or compare it with another value, and if it is correct it will print the flag..

So let's check the code in IDA :

![](https://lh3.googleusercontent.com/-KT2LWc_bliM/YPaEpDDBd6I/AAAAAAAABfw/PEVkmp7Q5lMR2q1oicFPGnN9mneTE6nvwCLcBGAsYHQ/s16000/image.png)


As I said before, it will take a password, encrypt every character and then compare it with a hard coded password, if they are equal to each other, then it will print the flag which stored in v7[8] 

checking sub_401550 function:

![](https://lh3.googleusercontent.com/-sQ_vtZdmCAo/YPaF2uOhZ1I/AAAAAAAABf4/1fug2LmfS7cBeBTSBz6aWWd6e5TALuwjQCLcBGAsYHQ/s16000/image.png)


the function use left rotation to encrypt the password.

from the main function we can see 6 hex values which the program will rotate them and then compare them with the password:

![](https://lh3.googleusercontent.com/-Oq44cduMOvo/YPaIjuOA60I/AAAAAAAABgA/hABh5kpHVaww_knvTi7eGLU3aVS-mrmTQCLcBGAsYHQ/s16000/image.png)

```
0xCDD0CCED9D85B199, 0xCCD1D0D1C0C97DE5, 0xCCD1D0D1C0C97DE5,
0xCCD1D0D1C0C97DE5, 0xF5D0, 0
```
**There are different methods to get the flag:**
1. writing a script to convert these values from hex and then rotate them to right.
2. bruteforcing password characters, encrypt them, and then compare it with the hard coded password.

So I used Cyberchef to convert each value from hex and then rotate it to right like that:

![](https://lh3.googleusercontent.com/-E4B4i8gj5OU/YPaMTPWx1UI/AAAAAAAABgI/Yhi4kQlpIycYumLpZQ5_w1KJdpJDwMxigCLcBGAsYHQ/s16000/image.png)

Nice ! we can reverse them now to get the right one like that:

![](https://lh3.googleusercontent.com/-2ex9aEuRf8A/YPaMfDBsbYI/AAAAAAAABgM/0JXxhFPOmkMFmcFcfMU82C2NQnpmqb8UgCLcBGAsYHQ/s16000/image.png)

we got the first one, you can repeat this step with each value to get the full flag and it will be:

>**Flag:** flag{34sy_r0t4t3_L3ft_4s_1234}

## 3. Assembly Master
- Category: Malware Reverse Engineering
- Level: Easy
- Points: 50
- Description: We all love assembly.

### Solve:
the file is the next assembly code:
```
x:
        .ascii  "v\200suizgahMsMa{\177d\200wMl}bMq|s\200\200w~uwo"
.LC0:
        .string "Wrong flag "
.LC1:
        .string "correct flag :D "
Secret_Checker:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 32
        mov     QWORD PTR [rbp-24], rdi
        mov     DWORD PTR [rbp-4], 0
        jmp     .L2
.L5:
        mov     eax, DWORD PTR [rbp-4]
        movsx   rdx, eax
        mov     rax, QWORD PTR [rbp-24]
        add     rax, rdx
        movzx   eax, BYTE PTR [rax]
        xor     eax, 19
        movsx   eax, al
        lea     edx, [rax+1]
        mov     eax, DWORD PTR [rbp-4]
        cdqe
        movzx   eax, BYTE PTR x[rax]
        movzx   eax, al
        cmp     edx, eax
        je      .L3
        mov     edi, OFFSET FLAT:.LC0
        call    puts
        mov     eax, 1
        jmp     .L4
.L3:
        add     DWORD PTR [rbp-4], 1
.L2:
        cmp     DWORD PTR [rbp-4], 32
        jle     .L5
        mov     edi, OFFSET FLAT:.LC1
        call    puts
        mov     eax, 1
.L4:
        leave
        ret
```
So, (x, .LC0, .LC1) are all strings

`Secret_Checker` jump to **.L2** so **.L2** maybe our entrypoint

checking **.L2**:

![](https://lh3.googleusercontent.com/-fPzv_OYBHcE/YPaVgU2mW8I/AAAAAAAABgY/QyMgUoY3KdAJ5iPFgFBZUkw9XDDROWDQQCLcBGAsYHQ/s16000/image.png)

after comparing the input with 32 characters it will jump to **.L5**

checking **.L5**:

![](https://lh3.googleusercontent.com/-1e46XOnm928/YPaV5fTOomI/AAAAAAAABgg/UvKG1UrZVL4NSmsQx7lFiAxNqXJ0BvqDACLcBGAsYHQ/s16000/image.png)

it takes each byte of the input, xor them with 19, jump to **.L3** which it will be increment with 1, then compare the result with the hardcoded string if all match it will print **.LC1**

the code will be like that:
```user_input[i]^19 + 1```

So we have to write a small script that decrement it with 1,xor with 19, and then print the flag.

This is the code i used:
```
x = "v\200suizgahMsMa{\177d\200wMl}bMq|s\200\200w~uwo"
flag=''
for i in x:
   flag += chr( (ord(i) - 1) ^ 19 )
print(flag)
```

>**Flag:** flag{just_a_simple_xor_challenge}

## 4. Dumper
- Category: Malware Reverse Engineering
- Level: Easy
- Points: 50
- Description: Another executable dumped from memory.

### Solve:
From the description of the challenge I think that the file is PE file and is dumped from a memory, so it may need a fix.

Using **PE-bear** to check it:

![](https://lh3.googleusercontent.com/-r0CPzHj6g1g/YPbBC46UgNI/AAAAAAAABgo/7dui1qfE6FoiyOF-CfqUUTYFHuSJI9_gACLcBGAsYHQ/w400-h224/image.png)

The file is unmapped and we should fix it by making it follow  the alignment of the partition not the alignment of the file, So we have to change the raw addresses to the virtual addresses and also the Raw Size...

The new one will be like that:

![](https://lh3.googleusercontent.com/-W7QVVsQWso4/YPbEya76ltI/AAAAAAAABgw/uwCD8taDaYs5yF7Kze4BkxRcVRMex1yqACLcBGAsYHQ/s16000/image.png)

Save and run the program to get the flag:

![](https://lh3.googleusercontent.com/-gCbGut-8rq8/YPbFPOtM1eI/AAAAAAAABg4/RiOFTv5UIk0iWRkX3klZorvew3QDYJVJQCLcBGAsYHQ/s16000/image.png)

>**Flag:** flag{Successfully_Done_123456}

## 5. PE M0nSt3r
- Category: Malware Reverse Engineering
- Level: Hard
- Points: 200
- Description: Better recognize art, cause I don’t forget names.

### Solve:
I got stuck after solving the entry point after 5 hours in this challenge so I went to sleep xD
This is a writeup for this challenge from our friend [Abdelrahman Nasr](https://t1m3m.github.io/tabs/about/) in his writeup for the Competition [here](https://t1m3m.github.io/posts/cybertalents-scholarship-re-ctf/).

## 6. ezez keygen2
- Category: Malware Reverse Engineering
- Level: Medium
- Points: 100
- Description: I want to play this game with the admin account, could you help me?

### Solve:
This challenge already solved in CyberTalents Cairo University CTF 2019 and you can find a writeup [here](https://t1m3m.github.io/posts/cybertalents-cairo-university-2019-ctf/#ezez-keygen-2)

## 7. Kill Joy
- Category: Malware Reverse Engineering
- Level: Medium
- Points: 100
- Description: Let's focus on the basics again.

### Solve:
#### Strings
![](https://lh3.googleusercontent.com/-s4YUTVtSQI8/YPbRWyVhA6I/AAAAAAAABhA/cEtiwrjeMlEd2pv3KcOc3mi20TbgBXfBACLcBGAsYHQ/s16000/image.png)

When using IDA I can't find the entry point, so I used a string "Not what i was searching for :(" to search in IDA and I found that:

![](https://lh3.googleusercontent.com/-WIW1ZwGE9i8/YPbeIut9EgI/AAAAAAAABhY/Yx88dPQUfVkALV5Wuklv2Rbc5p-r6aDwwCLcBGAsYHQ/s16000/image.png)

![](https://lh3.googleusercontent.com/-P82jCpTOZEg/YPbbGKaEycI/AAAAAAAABhQ/cgV9mIPhmdo69JJtzcdt_kNtso6yf7YFACLcBGAsYHQ/s16000/image.png)

The function firstly moves a values to the memory, then takes the process name which is the executable name 'Kill_Joy.exe' using szExeFile, and the for loop is shifting the name to be like that: lliK yoJ_ exe.

In the last line you can see that the flag will be the name of the executable and it will be 32 characters.

After some hours we got a hint which is :
>0x9E3779B9 ??? Google is your friend :D

When I googled it I noticed that it is **XTEA Algorithm**, well !

Now I need The algorithm, The key, and we have the encrypted data which the function moved to the memory before. So let's use a debugger to get the key:

Set a breakpoint to the function call which name is **sub_140011113** and start debugging:

![](https://lh3.googleusercontent.com/-4op-I4xHSjs/YPbt1j63FwI/AAAAAAAABhg/Yyjn22McVFAEcCiHlIkRBcn_oRZIBLNegCLcBGAsYHQ/s16000/image.png)

I got the argument values which is the key:

![](https://lh3.googleusercontent.com/-qu30AB2QumU/YPbwdw7GULI/AAAAAAAABho/XQyS_bKBW8ARSgO0ue9cYxa3idtfhUtbgCLcBGAsYHQ/s16000/image.png)

Checking function sub_7FF739C41850 I found that it is the encryption function:

![](https://lh3.googleusercontent.com/-LNu2OGFkyDY/YPbxY_Kg6YI/AAAAAAAABh4/Pg_o5yYnVcEVboHuXrIg-8gUeGxbZfT-ACLcBGAsYHQ/s16000/image.png)

The value `0x9E3779V9` seems to be the **Delta** for TEA algorithm but we are dealing with XTEA here (an updated version from it) .

Now I have the key, the encrypted data and the algorithm... So I can write a script to decrypt it:
```
#include <iostream>
using namespace std;
int main(){
    int count = 32;
    uint32_t enc_data[4][2] = {
            {0xD6F74320, 0x636A7B0A},
            {0xEEC58E45, 0x5F1E3AF5},
            {0x14D72088, 0x819BF516},
            {0x10A4D83A, 0x2C1001E7}
    };
    uint32_t key[4] = {
        0x34561234, 0x111F3423,
        0x01333337, 0x34D57910
    };
    for(int j=0; j < 4; j++){
        unsigned int i;
        uint32_t v0=enc_data[j][0], v1=enc_data[j][1],
            delta=0x9E3779B9, sum=delta*count;

        for (i=0; i < count; i++) {
            v1 -= (((v0 << 4) ^ (v0 >> 5)) + v0) ^ 
            (sum + key[(sum>>11) & 3]);
            sum -= delta;
            v0 -= (((v1 << 4) ^ (v1 >> 5)) + v1) ^
            (sum + key[sum & 3]);
        }
        enc_data[j][0]=v0;
        enc_data[j][1]=v1;
    }
    int j=0;
    while( j < 4 )
    {
        printf("%x%x", enc_data[j][0], enc_data[j][1]);
        j++;
    }   
    return 0;
}
```

I got this result which is a Hex value:

![](https://lh3.googleusercontent.com/-6-gLcX4-21U/YPb-ZKZBGwI/AAAAAAAABiA/XF8JK7LMDp8ec3EyQpRbnBJKI4eST6BKgCLcBGAsYHQ/s16000/image.png)

I used [CyberChef](https://gchq.github.io/CyberChef/) to convert it from Hex and this is the result:

![](https://lh3.googleusercontent.com/-Y6JF3V2TSyA/YPb-uEbR5WI/AAAAAAAABiI/Pd1nvOBabOgsp_Sg1_ds-3O53AG1AvlqQCLcBGAsYHQ/s16000/image.png)

>**Flag:** flag{th4ts_h0w_y0u_surv1v3_}

Thanks for reading 💙
