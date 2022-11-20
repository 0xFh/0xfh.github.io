---
date: 2021-09-22T23:00:00.000Z
layout: post
title: TeasingButton challenge
subtitle: [write-up]
description: >-
  A write-up for Teasing Button ctf challenge from CyberTalents platform
image: >-
  https://blogger.googleusercontent.com/img/a/AVvXsEi0bGtafNVNekeFoqIXNWa1MZO3qXsfJ7FGJqDl7jQ3NBoYm8lGC5bLSeWeh8eC4HW0Vd74QljqJWzbCqzyS1KWgeToVMyqljbnKgR02GqN0_lhL6b_5rDz56Ib2U3WG6C_LxYhxdVYRsm1fLBFIVrJLBz6SzuSZ997g5zDTk1MPgNCeOQA6Wbv32Ijbw=w400-h226
optimized_image: >-
  https://blogger.googleusercontent.com/img/a/AVvXsEi0bGtafNVNekeFoqIXNWa1MZO3qXsfJ7FGJqDl7jQ3NBoYm8lGC5bLSeWeh8eC4HW0Vd74QljqJWzbCqzyS1KWgeToVMyqljbnKgR02GqN0_lhL6b_5rDz56Ib2U3WG6C_LxYhxdVYRsm1fLBFIVrJLBz6SzuSZ997g5zDTk1MPgNCeOQA6Wbv32Ijbw=w400-h226
category: writeup
tags:
  - writeup
  - ctf
  - malware_reverse_engineering
  - decompiling
author: Abdullah Aiman
paginate: true
---

> Level: Medium
> Points:100
> Description: Do you really need the Hash?

Given an executable, after check it's type I found that it is PE32 .Net GUI for windows and as we can see in the following image it seems to be packed or something:
![](https://1.bp.blogspot.com/-vmpbMvd9GKQ/YUsTfuPUioI/AAAAAAAABn0/mswXTAehW-gUIJwVs4uvu9i2IgoD8shGQCLcBGAsYHQ/s16000/1.png "image")
after running the executable we will see 2 buttons, Yes and No . When I click yes button it will just swap the two buttons and nothing happen:
![](https://1.bp.blogspot.com/-4wUOZ8y6D0M/YUsT9yxKs9I/AAAAAAAABn8/6XdL4qVOYOI0n_-HRE4_Q1kJ7jLJt35nwCLcBGAsYHQ/s16000/2.png "image")
Checking the code with dnSpy:
![](https://1.bp.blogspot.com/-BC42nEUHCmQ/YUsUHZ3okwI/AAAAAAAABoA/4FdELbc5jToA2zzSjipvfKb7uqHgb0RCwCLcBGAsYHQ/s16000/3.png "image")
as we can see it is too hard to understand the code because it is confused, so I tried to unpack it with de4dot tool..

checking the unpacked one with dnSpy again and now I can read it as we can see:
![](https://1.bp.blogspot.com/-0i-7Zht7jTg/YUsUVLlKnNI/AAAAAAAABoI/gxRnNW82zjYJpcAeoC7TBCHP6r5uKNj6gCLcBGAsYHQ/s16000/4.png "image")
going to btnYes_MouseHover and btnYes_MouseUp functions to check what is going on..
![](https://1.bp.blogspot.com/-8NkR2S6mt0E/YUsUeAySTRI/AAAAAAAABoQ/hT3PMsnDN7khInNm3NcY0E1QstNJxxa1ACLcBGAsYHQ/s16000/5.png "image")
As we can see in the image, there is MessageBox contains method_0 and method_2 which takes "68" as an argument so lets check method_2 :
![](https://1.bp.blogspot.com/-gaO-6eUKkoA/YUsUkF2015I/AAAAAAAABoU/bUDK_BlE-_sKpM7F6oi7B7v-QcBOvi_WACLcBGAsYHQ/s16000/6.png "image")
![](https://1.bp.blogspot.com/-jwTFhIL8Ffg/YUsUo7x3l9I/AAAAAAAABoc/w9Mlc1Ufs_Uyqe6mwxiYduwX4H7WHRLRACLcBGAsYHQ/s16000/7.png "image")
The function will return a value which is the hash, as we can see here:
`return str + string_0 + str2 + str3 + this.method_0(string) + this.method_1(list);`
So lets get the values of them:
`str = "79"` as this image:
![](https://1.bp.blogspot.com/-Z0rlJo90h4c/YUsUwZBf8-I/AAAAAAAABog/kIIONSn28JEWWpPeua3XvEuI03Zc4NQ8QCLcBGAsYHQ/s16000/8.png "image")
`string_0` is the argument that method_2 function takes before, so it is equal to `68` :
![](https://1.bp.blogspot.com/-oVS2fPyICPQ/YUsVcISrOMI/AAAAAAAABow/najiRMUSDQIIGpwPZwYlsmkY5rHRy2LNQCLcBGAsYHQ/s16000/9.png "image")
`str2 = "017"` from the first image contain str value
`str3 = "eeb"` as we see in this image:
![](https://1.bp.blogspot.com/-M24YiAo8Fag/YUsVk7WxISI/AAAAAAAABo0/b2ej37wmdloKmES8wkPDCwXb-tJLBdVfgCLcBGAsYHQ/s16000/10.png "image")
`method_0(string_)` will takes string_ values which is equal to "2708","d77",83f" and convert them to one string so it will equal to `"2708d7783f"` :
![](https://1.bp.blogspot.com/-SEwOUZ8H8bA/YUsVwkVnf0I/AAAAAAAABo8/MvyIKl8d28EuxGRXOzp_6xI31M2hrkGXACLcBGAsYHQ/s317/11.png "image")
![](https://1.bp.blogspot.com/-8R3X2eBd2Lg/YUsWNOLmreI/AAAAAAAABpI/AoAk-U-at3ME1SuoBQZg2icPXtpkjM9swCLcBGAsYHQ/s16000/12.png "image")
`method_1(list)` will takes the values of list as one string also, but we need to know how switch work:
![](https://1.bp.blogspot.com/-8rBQdSS6NpY/YUsWaxcQFWI/AAAAAAAABpM/bR-wTl-uQPgAHqPwgc8v1Kocef4JE-YpQCLcBGAsYHQ/s16000/13.png "image")
if you have a problem in reading the code you can write a code to follow the values of num2 so you can know which Case will happen next

This is example:
![](https://1.bp.blogspot.com/-ti67UgPZ0Q8/YUsWmoG_q4I/AAAAAAAABpU/hzw8L9fDaLwjtU3f7-qAGw74h2dOaSKSgCLcBGAsYHQ/s16000/14.png "image")
as we can see the code will start with Case number 5 ,then go to 7, after that it will go to 4,3,2,1 :
![](https://1.bp.blogspot.com/-PZgA6-p2lQg/YUsXO5IgtaI/AAAAAAAABpg/7HsDsnpEcz4hup3WbN9g-q7J-b2QnwGqgCLcBGAsYHQ/s16000/15.png "image")
So our order will start from case 3 then 2 then 1, So the values of list will be: `"f0bdf","5d1a","33b"`
Then `method_1(list)` will equal to `"f0bdf5d1a33b"`

now we have all values of the return:
`str = "79"`
`string_0 = "68"`
`str2 = "017"`
`str3 = "eeb"`
`method_0(string_) = "2708d7783f"`
`method_1(list) = "f0bdf5d1a33b"`

then hash will be `7968017eeb2708d7783ff0bdf5d1a33b` and this is the flag :)
