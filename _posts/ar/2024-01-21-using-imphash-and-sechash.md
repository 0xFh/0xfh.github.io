---
date: 2024-01-21T13:00:00.000Z
layout: post
title: imphash & sechash (Ar)
subtitle: using imphash & sechash
description: >-
  A small article about linking malware to their adversaries using imphash and sechash
image: >-
  /assets/img/uploads/ar/ishash-ar.jpg
optimized_image: >-
  /assets/img/uploads/ar/ishash-ar.jpg
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
من ضمن المؤشرات اللي بتستخدمها الشركات في تتبع Threat Actor أو Adversary معين أثناء عمليات ال Static Analysis هي ال imphash و sechash عشان كدة هتلاقي موقع زي VirusTotal مثلاً بيعرضلك معلوماتهم في تفاصيل الفحص.. ولكن إزاي بيتم إستخدامهم في ال Static Analysis و إيه هم أصلاً؟
</span>
</p>

<p dir="rtl">
<span>
مبدئياً الThreat Actors احيانا بيستخدمو نفس الكود والfunctions مع أكتر من Malware بدل ما يعملو مالوير جديد من الصفر ولكن مع تغييرات بسيطة فيه عشان يظهر كأنه مالوير جديد ويتخطى الحلول الأمنية اللي كشفته قبل كدة... 
</span>
</p>

<p dir="rtl">
<span>
أولاً imphash موجود في python module اسمه pefile من تنفيذ شركة FireEye وهو اختصار ل Import Hash ودا عبارة عن hash خاص بال IAT أو Import Address Table، لو متعرفش الIAT فـ دا جدول فيه أسماء ال DLLs والFunctions اللي المالوير بيعملها import عشان يستخدمها.. الIAT ده بيتعمل تلقائياً من ال Linker أثناء عملية ال Compilation للكود بعد ما الLinker يشوف ترتيب الFunctions المكتوبة الأول.. وال imphash بيتحسب بعد كدة بناءً على محتويات الجدول اللي اتعمل.
</span>
</p>

<p dir="rtl">
<span>
اللي بيحصل هنا ان الMalware Author لو استخدم نفس الكود في مالوير أو نسخة جديدة منه هيكون ال imphash هو نفسه الموجود في النسخة القديمة متغيرش ودا في حالة انه مغيرش في ترتيب الFunctions مثلاً... بالتالي هيبقى سهل علينا نحدد إن العينة الجديدة دي متطابقة مع واحدة قديمة ومرتبطة بنفس ال Threat Actor أو نفس ال Malware Family بتاعت المالوير القديم .. ودا هيسهل شوية عملية تحليل أو تصنيف المالوير بالنسبة للمؤسسة.
</span>
</p>

<p dir="rtl">
<span>
بالعكس بردو لو ترتيب الFunctions جوا المالوير اتغير مع الحفاظ على نفس الكود بالتالي هتلاقي الimphash اتغير واختلف.. فوقتها مش شرط يكون المالوير جديد او تبع threat actor مختلف لكن بشوية تحليل أكتر هنقدر نوصل للإختلاف في الIAT ونعرف ان المالوير دا هو نفس المالوير السابق لكن مع بعض التعديلات اللي أدت ان الهاش يكون مختلف.
</span>
</p>

<p dir="rtl">
<span>
فيه ملاحظة مهمة هنا: الPackers ممكن تستخدم نفس الIAT للبرمجيات اللي بتعملها Packing بالتالي البرمجيات دي ممكن يكون ليها نفس الimphash بسبب استخدام نفس ال packer بالتالي دا مش معناه انهم تابعين لنفس ال Adversary ووقتها ممكن نحاول نعمل Unpacking الأول.
</span>
</p>

<p dir="rtl">
<span>
ثانياً بالنسبة لل sechash دا اختصار لـ Section Hash و زي ماحنا عارفين أي PE File بيحتوي على Sections بيكون فيها الكود التنفيذي للملف والداتا والموارد بتاعته زي سكاشن .text و .data و .rsrc وغيرهم، الsechash عبارة عن الهاش الخاص بكل سكشن لوحده... وهنا بيتم التحقق من ال hashes دي ومطابقتها ب sechashes لmalware samples تانية بحيث لو فيه عينتين متطابقتين هيبقى فيه احتمال لتبعيتهم لنفس الThreat Actor.
</span>
</p>

<p dir="rtl">
<span>
عشان تشوف ال imphash لأي PE File ممكن تستخدم VT علطول، وممكن تستخدم اداة pehash أو تستخدم الpefile module الخاص ببايثون، اما بالنسبة لل section hashs فـ دي هتلاقيها في VT وبرامج تانية كتير زي PeStudio وغيره.
</span>
</p>

<p dir="rtl">
<span>
وفي النهاية هنقدر نربط بين كل العينات المتطابقة وال malware families او ال threat actors اللي عملوها !
</span>
</p>

### Resources

- [PE File Format - EN](https://0xfh.github.io/pe-file-format/)

- [PE File Format - AR | Part 1](https://0xbatx.blogspot.com/2022/06/PEFile1.html)

- [PE File Format - AR | Part 2](https://0xbatx.blogspot.com/2022/06/PEFile2.html)

- [Packing & Unpacking - AR](https://0xbatx.blogspot.com/2022/06/Packing.html)