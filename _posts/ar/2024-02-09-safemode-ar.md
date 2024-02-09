---
date: 2024-02-09T13:00:00.000Z
layout: post
title: Evasion using Safe Mode (Ar)
subtitle: Defense Evasion using Windows Safe Mode
description: >-
  An overview about Defense Evasion using Safe Mode
image: >-
  /assets/img/uploads/ar/safemode-ar.png
optimized_image: >-
  /assets/img/uploads/ar/safemode-ar.png
category: article
tags:
  - arabic
  - article
  - malware_analysis
  - threat_hunting
author: Abdullah Aiman
paginate: true
---
<p dir="rtl" style="font-weight:600">
<span>
من ضمن الـ Evasion Techniques اللي بتستخدمها البرمجيات الخبيثة للتهرّب من الدفاعات الأمنية على الـ Endpoints واللي استخدمتها Malware مشهورة زي REvil و AvosLocker و BlackBasta هي استعمال الـ Safe Mode.. <br>
لكن إزاي بيحصل و إيه هو الـ Safe Mode Boot ؟ <br>
إيه الإجراءات اللي ممكن يعملها الـ Defenders لكشف التقنية دي أو التخفيف من آثارها ؟
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
مبدئياً إيه هو الـ Safe Mode ؟
</span>
</p>
<p dir="rtl">
<span>
الويندوز ليه كذا وضع ممكن يشتغل بيهم من ضمنهم الوضع الآمن أو Safe Mode ودا وضع بيستخدم في استكشاف الأخطاء وإصلاحها من خلال تشغيل الويندوز في حالة محدودة يعني بيقفل أغلب التعريفات والبرامج والخدمات الغير ضرورية واللي بتشتغل أثناء الـ Startup لوحدها، فيه أنواع للـ Safe Mode هتلاقي فيها Minimal اللي بيشتغل بأقل إمكانيات مع واجهة مستخدم قياسية، أو Alternate Shell ودا بيشتغل بالـ Command Prompt من غير واجهة مستخدم، أو Network ودا زي الـ minimal بس بيستخدم بعض الخدمات والتعريفات الخاصة بالشبكة عشان لو هتستخدم الانترنت أثناء الـ troubleshooting وفيه Modes غيرهم.
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
ليه الـ Malware بتستغل الـ Safe Mode ؟
</span>
</p>

<p dir="rtl">
<span>
لأن زي ما قلت النظام بيشتغل وقتها بحالة محدودة من ناحية الخدمات والبرامج والتعريفات فالدفاعات الأمنية وخصوصا برامج الـ EDR ممكن متشتغلش على الوضع دا بالتالي الـ Malware هيقدر يشتغل وياخد راحته.
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
إزاي البرمجيات الخبيثة بتستغل الـ Safe Mode بقى ؟
</span>
</p>

<p dir="rtl">
<span>
0- قبل أي حاجة الـ Malware لازم يبقى معاه صلاحيات Administrator عشان يقدر يعمل التغييرات الجاية و دي بيكون حصل عليها في خطوة سابقة من الهجوم وممكن يعمل User جديد عشان يحافظ على الـ Persistence ويعمل Auto Login بعد عملية الريستارت (مش موضوعنا حالياً). <br>
  <br>
1- بعدين الـ Malware بيجبر النظام إنه يدخل في الـ Safe Mode بعد إعادة تشغيله من خلال تعديل ملفات خاصة بالـ Boot بيُطلق عليها BCD أو Boot Configuration Data واللي بدورها مسؤولة عن إعدادات الـ Boot Application؛ فمثلاً ممكن يستخدم command زي دا عشان يخلي النظام لما يشتغل مرة تانية يعمل boot في الـ Safe Mode مش في الـ Normal Mode: <br>
bcdedit.exe /set {current} safeboot minimal <br>
  <br>
وممكن يستخدم shutdown /r /f /t 00 عشان يجبر النظام على إعادة التشغيل في الحال وميستناش حد يعيد تشغيله. <br>
  <br>
2- بعد التعديل اللي فات بيروح الـ Malware يضيف نفسه للـ minimal services اللي بتشتغل في الـ Safe Mode ودا ممكن يتم من خلال التعديل على بعض الـ Registry values الخاصة بالـ Startup في الوضع دا أو من خلال Malicious COM object واللي ممكن تتسجل وتشتغل في الـ Safe Mode كذلك؛ فمثلاً ممكن يستخدم الـ Reg Key دا لإضافة نفسه ك Startup Service في الـ Safe Mode: <br>
HKLM\SYSTEM\CurrentControlSet\Control\SafeBoot\Minimal <br>
  <br>
وبكدة هيكون المالوير ضمن إن النظام هيعمل boot من الـ Safe Mode بعد إعادة تشغيله المرة الجاية وضمن كمان إنه هيشتغل علطول زي الـ startup services. <br>
<br>
ملحوظة هنا: COM Object هحاول أشرحه في مقال تاني لانه مش موضوعنا حالياً بس خليك فاكر ان ليه Reg Keys ودا مكانها: <br>
HKEY_LOCAL_MACHINE\Software\Classes\CLSID <br>
HKEY_CURRENT_USER\Software\Classes\CLSID
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
إزاي نخفف من آثار استخدام التقنية دي او نمنعها؟
</span>
</p>

<p dir="rtl">
<span>
1- نتأكد إن الدفاعات الأمنية على الـ Endpoints بتشتغل في الـ Safe Mode بشكل كامل وسليم بحيث لو المالوير اشتغل في الوضع دا يكون متراقب ويتكشف بسهولة. <br>
2- نقلل حسابات المسؤولين أو الـ Administrators لأقل عدد ممكن مع إتّباع الـ least privilege principles بحيث المالوير ميقدرش يستغلها ويجبر النظام على الدخول في الـ Safe Mode.
</span>
</p>

<p dir="rtl" style="font-weight:550">
<span>
إزاي نعمل Detection لإستغلال الـ Safe Mode في الجزئية دي؟
</span>
</p>

<p dir="rtl">
<span>
1- مراقبة الأوامر اللي تم تنفيذها واللي مرتبطة بالإعدادات الخاصة بالـ Boot زي bcdedit.exe و bootcfg.exe. <br>
2- مراقبة الـ Processes اللي تم تنفيذها مؤخراً والتحقق من أي محاولات لاستخدام صلاحيات المسؤولين أو لتغيير الـ Boot Options..الخ. <br>
3- مراقبة عمليات إنشاء وتعديل قيم الـ Registry الخاصة بتشغيل الـ Services في الـ Safe Mode، على سبيل المثال؛ <br>
HKLM\Software\Microsoft\Windows\CurrentVersion\Run["*Startup"="{Path}"]HKLM\SYSTEM\CurrentControlSet\Control\SafeBoot\Minimal <br>
  <br>
لاحظ ان الـ Key دا هو المسؤول عن الـ startup أثناء الـ Safe Mode boot: <br>
HKLM\SYSTEM\CurrentControlSet\Control\SafeBoot\Minimal
</span>
</p>

<p dir="rtl">
<span>
فيه تقرير لـ Sophos عن Snatch Ransomware اللي استخدم نفس التقنية لتخطي الدفاعات قبل ما يبدأ عملية تشفير الملفات، وتقرير تاني عن AvosLocker واللي استخدم نفس التقنية مع شوية إضافات تانية، وتقرير عن BlackBasta اللي استخدم الـ Safe Mode كذلك، وطبعا مننساش REvil اللي عامل قلق ممكن تشوف تقارير كل دول من المصادر تحت.
</span>
</p>

### Resources

- [Snatch Ransomware - Sophos](https://news.sophos.com/en-us/2019/12/09/snatch-ransomware-reboots-pcs-into-safe-mode-to-bypass-protection)

- [AvosLocker - Trendmicro](https://www.trendmicro.com/vinfo/us/security/news/ransomware-spotlight/ransomware-spotlight-avoslocker)

- [BlackBasta - Trendmicro](https://www.trendmicro.com/en_us/research/22/e/examining-the-black-basta-ransomwares-infection-routine.html)

- [REvil - bleepingcomputer](https://www.bleepingcomputer.com/news/security/revil-ransomware-has-a-new-windows-safe-mode-encryption-mode)
