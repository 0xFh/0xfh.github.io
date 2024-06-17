---
date: 2022-08-27T13:00:00.000Z
layout: post
title: Persistence [RegKeys & StartupFolders] (Ar)
subtitle: Persistence via Registry Keys & Startup Folders
description: >-
  An article about persistence technique through reg keys and startup folders
image: >-
  /assets/img/uploads/ar/persistence-regkeys.png
optimized_image: >-
  /assets/img/uploads/ar/persistence-regkeys.png
category: article
tags:
  - arabic
  - article
  - malware_analysis
  - blue_teaming
author: Abdullah Aiman
paginate: true
---
<p dir="rtl" style="font-weight:600">
<span>
الـ Persistence من أهم المراحل للMalware عشان يحافظ على وجوده أو وصوله للنظام حتى لو حصل للنظام ده reboot او اتغيرت الصلاحيات او حصلت اي مشاكل تمنع المالوير من الوصول للنظام بشكل عام، وفي المقال دا كتبت معظم الـ Reg Keys اللي ممكن يستخدمها المالوير في تثبيت وجوده ✍️
</span>
</p>

<p dir="rtl">
<span>
من ضمن طرق الPersistence اللي بتستخدمها الMalware حاجة اسمها <b> Boot/Logon Autostart Execution </b> ودي طريقة منتشرة ممكن تتم بطرق كتير، وهنتكلم عن واحدة منهم حالياً وهي عن طريق ال Registry Keys & Startup Folder. فعلى سبيل المثال فيه Registry Keys و Startup Folder في الويندوز مسؤولين عن تشغيل البرامج والخدمات بشكل تلقائي لما المُستخدم يعمل Login او لما الجهاز يعمل Reboot (إعادة تشغيل).
<br>
<b>دي مسارات الstartup folder في الويندوز:</b><br>
<code>C:\Users\[Username]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup</code><br>
<code>C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp</code><br>
<br>
<b>و دي Registry Keys موجودة على الويندوز بشكل إفتراضي ومسؤولة عن تشغيل البرامج تلقائيا:</b><br>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run</code>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce</code>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run</code>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce</code><br>
<br>
<b>بعض الRegistry Keys ممكن يتم استخدامها في التحكم في الStartup Folder زي:</b><br>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders</code><br>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders</code><br>
<code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders</code><br>
<code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders</code><br>
<br>
<b>وبعضها ممكن يتحكم في الServices اللي بتشتغل أثناء عملية الboot زي:</b><br>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce</code><br>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce</code><br>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices</code><br>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServices</code><br>
<br>
<b>وبعضها بيستخدم الPolicy settings زي:</b><br>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run</code><br>
<code>HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run</code><br>
<br>
<b>وفيه keys تانية زي:</b><br>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit</code><br>
<code>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell</code><br>
 بس دول أشهر الkeys المُستخدمة.<br>
<br>
ممكن تستخدم اداة <b>Autoruns</b> عشان تشوف الstartup entries الموجودة في السيستم، وممكن تستخدم ادوات <b>ProcMon</b> او <b>RegShot</b> لمراقبة الRegistry Activity بشكل عام. ويستحسن يتعمل Monitoring على الFile Modifications و المسارات والkeys المهمة في السيستم.
</span>
</p>