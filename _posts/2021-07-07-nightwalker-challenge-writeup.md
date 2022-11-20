---
date: 2021-07-07T13:00:00.000Z
layout: post
title: NightWalker challenge
subtitle: [write-up]
description: >-
  NightWalker is a CTF challenge on CyberTalents platform and this is a -report like- writeup for it
image: >-
  /assets/img/uploads/nightwalker.jpg
optimized_image: >-
  /assets/img/uploads/nightwalker.jpg
category: writeup
tags:
  - writeup
  - ctf
  - malware_reverse_engineering
  - cybertalents
author: Abdullah Aiman
paginate: true
---

# Challenge Info
- CTF Platform: CyberTalents
- Challenge Name: Night Walker
- Category: Malware Reverse Engineering
- Level: Hard
- Points: 200

## Summary
Running the executable will drop 1 file called ‘Secret_log.txt’ and then it will run in the background to get 40 keystrokes, once the executable get them it will encrypt them with AES CBC Mode algorithm and save the encrypted text to ‘Secret_log.txt’ file, then it will be terminated from the background.

So we should identify the Algorithm, Key & IV and write a small script to brute force the last 2 bytes of the key to get the final decrypted text.

## Methodology
1. Basic Static Analysis
2. Basic Dynamic Analysis
3. Advanced Static Analysis

After downloading ‘N1ght_walker.rar’ and extracting it, we can see there are 2 files, ‘N1ght_Walker.exe’ and ‘flag.txt’ .

## 1. Basic Static Analysis
By identifying the type of the 2 files I got that: ‘N1ght_Walker.exe’ is PE32 executable for windows and ‘flag.txt’ seems to be encrypted text file as we can see in this image:

![image](https://lh3.googleusercontent.com/-NrbrYli2UI8/YOL6FxGwsFI/AAAAAAAABVA/0OQWtQ00Hbox8ueXeMOI3nN6yzbrf0MzQCLcBGAsYHQ/image.png)

So let’s extracting the metadata from N1ght_Walker.exe using [Strings – Resource Hacker – PeStudio – Capa - ExeinfoPE]

### Strings
I found some interesting strings in these following images:

![image](https://lh3.googleusercontent.com/-f47n6uYQ-JE/YOL6NMsbeTI/AAAAAAAABVE/y--j5V3t8EwtaCA1uabEM0mPachIOOTEQCLcBGAsYHQ/s16000/image.png)
![image](https://1.bp.blogspot.com/-GaEOaXEECBQ/YOMCZJQ6cCI/AAAAAAAABVg/8Q8QI5AhiewUTIXJH2UdQ0xra3BD0XrBQCPcBGAYYCw/s16000/image.png)

>Secret_log.txt   -> Interesting File Name
>
>bcrypt.dll       -> Windows Cryptographic API

Now we know that this executable may encrypt something in the Space xD

I tried to see any resources into the file by Resource Hacker but “nothing” So i went for PeStudio to get more details about it...

### PeStudio
As we can see in the next image, the file is not packed because there is no a big difference between raw-size and virtual-size...

![image](https://1.bp.blogspot.com/-J8ZoYLKULFo/YOMEhoJDqmI/AAAAAAAABVw/7EH0t2AwtfoRHZc7ryGhkJY68q5ii4FxACPcBGAYYCw/s16000/image.png)

Checking imports and libraries but nothing new, it just uses the bcrypt API to encrypt something as we said before…

![image](https://1.bp.blogspot.com/-KhZLbM-mYlg/YOMFH6j49KI/AAAAAAAABV4/gGtt9bWpOQE_WNnUjhTJELZN6F4gv3LcQCPcBGAYYCw/s16000/image.png)

### Capa
By identifying the capabilities in our executable using Capa to know more about its behavior, we got something interesting...

![image](https://lh3.googleusercontent.com/-z7HS4FF5X3Q/YOMF60XarWI/AAAAAAAABWA/i1qQmHby8j0L65vNQQ7qwLrbzK-K0UslACLcBGAsYHQ/s16000/image.png)

The executable seems to be a Keylogger !

### ExeinfoPE
Trying to know more about the signature of the executable using ExeinfoPE...

![image](https://lh3.googleusercontent.com/-tkJzJd-pUcA/YOMGAMKcHuI/AAAAAAAABWE/e_yFv9J_mG0Yi6WO5E1Ms2OuK-Ja0VQNQCLcBGAsYHQ/s16000/image.png)

Ok it is a 64bit executable and as we said in the Step2 the file is not packed.

> Before going to Advanced Static Analysis I need to know more about its behavior so let’s go for Basic Dynamic Analysis...

## 2. Basic Dynamic Analysis
### Procmon
Process Monitor show that the executable dropped a file called Secret_log.txt

![image](https://lh3.googleusercontent.com/-zAjwVL4mwD0/YOMGTuQkJ_I/AAAAAAAABWQ/9_sE9jLR10sjHAXtWVHZ5-JU67-F1XmfACLcBGAsYHQ/s16000/image.png)

The executable console disappeared so I checked the new file ‘Secret_log.txt’ and I noticed that it is empty.

I used Process Hacker to sure that the executable is not running in the background but guess what? It is running :D

![image](https://lh3.googleusercontent.com/-FzZenY3FA-w/YOMGZg7YciI/AAAAAAAABWU/g0MY2u6D8YUxcZO-ZfkHn-Z-b488dl7mwCLcBGAsYHQ/s16000/image.png)

> So I should go for Advanced Static Analysis now to know what happened...

## 3. Advanced Static Analysis
Using IDA to check the instructions and search for anything interesting...

Ok I took 5 minutes to get the main function from the entry point after searching for the file name ‘Secret_log.txt’ which the executable dropped it before and I got this function in the image so I follow its xrefand I found that the executable uses ShowWindows API to hide itself and call the function sub_140001104 using the file name ‘Secret_log.txt’.

![image](https://lh3.googleusercontent.com/-ivm-vLCy3No/YOMGdjv6hQI/AAAAAAAABWY/YEK_D76NoUAtWgSF97GikkoC6GQayR_1wCLcBGAsYHQ/s16000/image.png)

So let’s follow sub_140001226 function...

![image](https://lh3.googleusercontent.com/-DdEga2c84Bc/YOMGm_amBsI/AAAAAAAABWg/JiugEcywOnI40hXmcd4zhpwteYuZthWegCLcBGAsYHQ/s16000/image.png)

Then go sub_140003E90 and I found this function is very interesting because there is a Windows API called `SetWindowsHookExW` so have to search about it on Google...

![image](https://lh3.googleusercontent.com/-JSGpnT4gncE/YOMGsyfGmBI/AAAAAAAABWo/dPeirrA0QX8d24pCT_Ac4B_ZGy7UvDAcQCLcBGAsYHQ/s16000/image.png)

Here is the syntax of this API, `(idHook, lpfn, hmod, dwThreadId)`

![image](https://lh3.googleusercontent.com/-rdOWwkQOLnY/YOMGwDgjM7I/AAAAAAAABW0/ArhJsqredGQ_H_d5lCCVnMwm9FSB9au5ACLcBGAsYHQ/s16000/image.png)

According to the last function which has: `result = SetWindowsHookExW(13, fn, 0i64, 0);`

I found idHook parameters value in the same website so 13 represents to a Keylogger;

![image](https://lh3.googleusercontent.com/-UC38HsQL1LY/YOMGzQdC_PI/AAAAAAAABW4/8vDAziLswuIUBBNxb6jTGt15xc_OEvX7QCLcBGAsYHQ/s16000/image.png)

We can identify that fn is a function, so by following it we will see that it calling another function called `sub_140003E20`

![image](https://lh3.googleusercontent.com/-ZTozNP-o6WA/YOMG3HZ9NVI/AAAAAAAABXA/iMrCR_FD-hIp2M8hCwiwMnT1daQm_N8tACLcBGAsYHQ/s16000/image.png)

After following it we get this:

![image](https://lh3.googleusercontent.com/-9h4KdtTkyIE/YOMG6rBX77I/AAAAAAAABXE/3VvEJEg7JMUuWFqY8ytSv7zmMMneBN-9wCLcBGAsYHQ/s16000/image.png)

Ok it calls sub_140001339 function and if you follow it you will reach this function which is for key logging:

![image](https://lh3.googleusercontent.com/-e6r2-RPkK0w/YOMG-au1kYI/AAAAAAAABXM/7TOMRk_o1iwRqd1pA6ENTKmXl51Frg3fwCLcBGAsYHQ/s16000/image.png)

After checking this function I found that the function will take only **40 keystrokes** and after that generate a number between **0 to 120** in the loop and then put them on `pbSecret[i]` so I think this is a part of key so we can jump to its xrefto see which functions used it:

![image](https://lh3.googleusercontent.com/-EfrltJXZlp4/YOMHCde9KHI/AAAAAAAABXQ/Z1DAu3jaLkU1ZsP5SsKk2cyRZsi47s5mACLcBGAsYHQ/s16000/image.png)

Wait, it means that the executable which is running in the background logged the keystrokes I did and then terminated automatically, so I back to the file ‘Secret_log.txt’ and I found it contains encrypted text !

![image](https://lh3.googleusercontent.com/-3EfvaunEvDs/YOMHEoZ6q3I/AAAAAAAABXU/-CuFAmQdeCYFJ2hx5FyI0M-oc-AHsikFgCLcBGAsYHQ/s16000/image.png)

Let us back to IDAagain check the functions that we jumped into it:

![image](https://lh3.googleusercontent.com/-8DfDGwFbyzA/YOMHFwMVy2I/AAAAAAAABXY/6IE_lxvEwioFS7x5RGv0iqC2uYPdxKtogCLcBGAsYHQ/s16000/image.png)

By checking this function I found an interesting and special something:

![image](https://lh3.googleusercontent.com/-XQ8GDTBxJ0w/YOMHH_z4WsI/AAAAAAAABXg/jPmQs5YqFwo1cPQWhIWkSW_TYYCo8nHngCLcBGAsYHQ/s16000/image.png)

There is BCrypt Windows API which uses AESCipher for the encryption!

From the last 2 images we can notice that the cipher is AES with CBC (Cipher Block Chaining) mode.

![image](https://lh3.googleusercontent.com/-xlJ_N5-C4ic/YOMHLg_zZ9I/AAAAAAAABXk/Tw2cU9MhlbkJQSfQgjBYDfEih6ypcdbXACLcBGAsYHQ/s16000/image.png)

Following unk_140011DE8 to check the data and we got that:

![image](https://lh3.googleusercontent.com/-_WBHuSP7haY/YOMHOHruNxI/AAAAAAAABXs/f6jz_I7LjcQh3QVvtZlRRlRoR0AJPOghACLcBGAsYHQ/s16000/image.png)

Notice that the last 2 bytes of the key are random and iv is from 0 to F so we can brute force the key for all possible values and try to search the word ‘Window:’ in the result.

So I wrote this simple script to get the clear flagwithout any useless strings:


```javascript

from Crypto.Cipher import AES

if __name__ == '__main__':

    with open('flag.txt','rb') as flagFile:
        x=flagFile.read()

#initializing key and iv
    key = b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D'
    pad = b'\x0E\x0F'
    iv = key + pad	#This is 'Initialization Vector' and should be 16 bytes (in binary)
    flag = str("")

    print('\nDecrypting flag.txt , Please wait ...')

    for i in range(120):
        for j in range(120):
            tmp = key+chr(i).encode()+chr(j).encode()
            cipher = AES.new(tmp, AES.MODE_CBC, iv)
            plaintext = cipher.decrypt(x)
            if b'[Window:' in plaintext:
                flag = flag + str((plaintext.decode()))


#Convert flag to List for deleting the useless strings

flag = flag.split('\n')

#This is the length of the useless string maybe different from date to date so change it if necessary
len1 = len("[Window: flag.txt - WordPad - at Mon Aug  3 04:23:16 2020] ")

for i in range(len(flag)):
    flag[i] = flag[i][len1:]

flag = flag[0:len(flag)-6]


#Convert flag to string for replacing characters "[SHIFT] [ ] -" with "{ } -"
def listToString(str1): 
    str1 = ""
    return(str1.join(flag))
flag = listToString(flag)

#replacing
flag = flag.replace("[SHIFT][","{")
flag = flag.replace("[SHIFT]]","}")
flag = flag.replace("[SHIFT]-","_")


#Print The Final Result
print("\n------------------Flag--------------------\n")
print(flag)
print("\n------------------------------------------\n")
```
Running it...

![image](https://lh3.googleusercontent.com/-JLyIQtlkRbk/YOWCR-IT7sI/AAAAAAAABdw/EbkdsGvV9qozyqwxgr4vOpN-9nCtM8xiQCLcBGAsYHQ/s16000/image.png)

And we got the flag.

You can find the script [Here](https://github.com/0xFh/AES.Decrytion-Nightwalker)
