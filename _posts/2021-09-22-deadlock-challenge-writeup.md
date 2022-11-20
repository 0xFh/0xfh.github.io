---
date: 2021-09-22T23:00:00.000Z
layout: post
title: Deadlock challenge
subtitle: [write-up]
description: >-
  A write-up for Deadlock ctf challenge on CyberTalents platform
image: >-
  https://blogger.googleusercontent.com/img/a/AVvXsEjhKEmDrPn9ejgraKlWQujDfGAlhFuoF2pp3KN5cSS8uxIB-DmlADoDVUInmiLEPlijpHWlvExRiU_EFRzl_I_KD-rEfijOnvangcK2lLgHp0ppEzIKzSY3KZS0UtBjIoz35Hlw7v-5j9NkQsKrDsTeHZB1l4dHx0ov0mun9WQ8HXl53KJg6CWqrrKEfw=s960
optimized_image: >-
  https://blogger.googleusercontent.com/img/a/AVvXsEjhKEmDrPn9ejgraKlWQujDfGAlhFuoF2pp3KN5cSS8uxIB-DmlADoDVUInmiLEPlijpHWlvExRiU_EFRzl_I_KD-rEfijOnvangcK2lLgHp0ppEzIKzSY3KZS0UtBjIoz35Hlw7v-5j9NkQsKrDsTeHZB1l4dHx0ov0mun9WQ8HXl53KJg6CWqrrKEfw=s960
category: writeup
tags:
  - writeup
  - ctf
  - game_reverse_engineering
  - malware_reverse_engineering
author: Abdullah Aiman
paginate: true
---

> Level: Hard
> Points: 200
> Description: Can you play with me?

Given a deadl0ck.rar archive, after extracting it we got 3 files:
*Deadlock.exe
*data.win
*options.ini 
![](https://lh3.googleusercontent.com/-1DqjprHLM3M/YQJp-sDfGYI/AAAAAAAABkA/ufRJe_dGAqEiv66VDx6qjWjwwuoLdmVSgCLcBGAsYHQ/s16000/image.png "image")
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg4yclumB7MCFPi3dEupKM9BM3WWCim-wEhTTNGXsn3ornzpR80YtHbpeqCzLAWOLS8XJf-eQ6c03YOPAo7EgPet5jgxmsNXOD748jTik6UUstVlXWd3fwVLK-uJThY4m9EZxcg1c1uxlO4b0iKaFOqij4WzLxSww1s5vo5V7qgi4uQi34bZ8cvkEkmcg/s995/1.png "image")
Checking files type:
![](https://lh3.googleusercontent.com/-kmcjTFL8nlo/YQJpR9b3DFI/AAAAAAAABj4/L8sRQWirKEIH8UKWhQBDVpVJ1jpBBfuVACLcBGAsYHQ/s16000/image.png "image")
It seems to be a game, lets open it and see if there is something interesting...
![](https://lh3.googleusercontent.com/-lEGzwhkpji0/YQJqw-psECI/AAAAAAAABkI/I4dFncejXlwn5MMMMpJEcOrCB6imZ4UewCLcBGAsYHQ/s16000/image.png "image")
We got nothing, it just a game.

Trying Resource Hacker to get any interesting resource but still nothing. So let's check the data file which is data.win.

At this point I didn't know how can I edit it, so I should google it first xD

I found that there is a tool called 'UndertaleModTool' that can read and edit data.win file ..
![](https://lh3.googleusercontent.com/-R96wVWAvSxQ/YQJr5P4H7FI/AAAAAAAABkQ/7IKcJtlMH20s3ZMDvPFj_ocTIwQIZO5agCLcBGAsYHQ/s16000/image.png "image")
You can find the tool here: `https://github.com/krzys-h/UndertaleModTool/releases`

Let's open it and check the data file:
![](https://lh3.googleusercontent.com/-fRgY7xHLanQ/YQJ0LsTTRhI/AAAAAAAABkY/B2359gRip4UoCFme8fNTluuxZkCyH9CSwCLcBGAsYHQ/s16000/image.png "image")
From previous image I found that there are 2 rooms, room0 and Secret room,
So let's try to change their order to make Secret room open before room0...

We can drag the room from left and drop it in the list in right like that:
![](https://lh3.googleusercontent.com/-DJ2InKiCvuQ/YQJ1gHMmE4I/AAAAAAAABkg/z7t3r2LQqrwGKk_88CV2OQSDex3vgZLeACLcBGAsYHQ/s16000/image.png "image")

Now save the file, and then run the game to get the flag ;)
![](https://lh3.googleusercontent.com/--WaNKXkIj3Q/YQJ2GSf9PrI/AAAAAAAABko/zdrRFjy8UUsTHqEYWzIWVdY2ehAg2A2bQCLcBGAsYHQ/s16000/image.png "image")