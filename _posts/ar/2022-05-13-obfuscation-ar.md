---
date: 2022-05-13T13:00:00.000Z
layout: post
title: Obfuscation Overview (Ar)
subtitle: Obfuscation (in Arabic)
description: >-
  An article about Obfuscation in Arabic Language
image: >-
  /assets/img/uploads/ar/obfuscation-ar.png
optimized_image: >-
  /assets/img/uploads/ar/obfuscation-ar.png
category: article
tags:
  - arabic
  - article
  - basics
  - malware_analysis
author: Abdullah Aiman
paginate: true
---
<p dir="rtl">
<span>
الObfuscation من ضمن الطرق اللي بيستخدمها الThreat Actors أو الMalware Authors عشان يصعبو عمليات الDetection ويخلو الAnalysis ياخد وقت أطول واحيانا تكلفة أكبر
</span>
</p>

<h2 dir="rtl">
<span>
ايه هو الObfuscation و ازاي بيحصل؟
</span>
</h2>

<p dir="rtl">
<span>
الObfuscation عبارة عن تعديل على الكود بحيث يبقى صعب القراءة والفهم وممكن يتم بأشكال مختلفة زي الطريقة اللي بيتكتب بيها الكود (ودي ليها طرق كتير)، أو عن طريق الPacking أو الEncoding أو الEncryption أو الCompression ..الخ
</span>
</p>

<h2 dir="rtl">
<span>
أنواع الObfuscation:
</span>
</h2>

<p dir="rtl">
<span>
فيه انواع للcode obfuscation بتُستخدم في الAnti-Reversing Techniques عشان تصعب قراءة وفهم الكود على الRE Engineer او الMalware Analyst حتى لو كانو بيستخدمو Tools جاهزة زي الDisassemblers.. الانواع دي زي:
</span>
</p>

<h3 dir="rtl">
<span>
1-Logic/Control Flow Obfuscation:
</span>
</h3>
<p dir="rtl">
<span>
وهنا الكود بيتحول ل(بطاطس مهروسة) بحيث متفهمش الlogic او المنطق اللي شغال بيه ولا تعرف منين بيودي علي فين..
</span>
</p>

<h3 dir="rtl">
<span>
2-NOP Obfuscation:
</span>
</h3>
<p dir="rtl">
<span>
احنا عارفين ان الNOP instruction ملوش تأثير في الexecution وممكن أثناء الMalware Analysis نشوف اجزاء من الكود فيها تعليمات NOP كتيرة، لكن الفكرة هنا اننا مش هنستخدم تعليمات NOP نفسها ولكن هنستخدم مجموعة instructions تانية بحيث لو جمعناهم مع بعض مش هيكون ليهم اي تاثير حقيقي في الexecution يعني مثلا ممكن يزود 3 انواع مختلفة من الregister ويعمل عليهم بعض العمليات وبعدين يرجعهم لاصلهم تاني فمفيش تغيير هيحصل غير انه حيّر الMalware Analyst وخلاه يشتغل وياخد وقت اكتر على الفاضي..
</span>
</p>

<h3 dir="rtl">
<span>
3-Anti-Disassemblers Obfuscation:
</span>
</h3>
<p dir="rtl">
<span>
الطريقة دي بتبقى فعّالة جدا و سهلة التنفيذ و بتعتمد على إرباك الdisassemblers نفسها، ممكن تتم من خلال إضافة junk bytes (بايتات مش مهمة) بين الinstructions بحيث تخلي الdisassemblers تاخد الjunk bytes دي في الاعتبار وتعرض كود او تعليمات غلط مش صحيحة
  </span>
</p>
<p dir="rtl">
<span>
وممكن تتم بإستخدام fake return addresses يعني بتحط الjunk bytes بعد الcall instruction وبتغير الreturn address اللي جوا الcalled function تخليه يعمل skip للjunk bytes اللي حطيناه بعدها في الاول بحيث تخدع الdisassembler
</span>
</p>

<h3 dir="rtl">
<span>
4-Trampolines Obfuscation:
</span>
</h3>
<p dir="rtl">
<span>
احنا عارفين ان التعليمات البرمجية في الوضع الطبيعي بتبقى في سطور تحت بعضها، لكن الطريقة دي بتعتمد على وضع الinstructions في اماكن مختلفة وعشوائية وبتستخدم الjumps عشان توجّه الexecution flow بعد كل تعليمة للتعليمة الصح اللي بعدها ودا بيخلي الكود صعب الفهم وصعب التتبع ودا مش بيأثر في مرحلة الstatic analysis بس، دا بيأثر في الdynamic analysis كمان وبيخلي الAnalyst يعيد ترتيب الinstructions تحت بعضها عشان يفهم الAlgorithm أو الcode
</span>
</p>

<h3 dir="rtl">
<span>
5-Instruction Permutations:
</span>
</h3>
<p dir="rtl">
<span>
الطريقة بتتم من خلال استبدال بعض التعليمات او الinstructions البسيطة بمجموعة تعليمات ليها نفس النتيجة فبتخلي الAnalyst يضيع وقت أكبر في فك وتحليل التعليمات الكتيرة دي بدل ما يركز على التعليمات الحقيقية.
</span>
</p>
<p dir="rtl">
<span>
وطبعا مش دي بس الطرق لكن فيه طرق تانية ممكن نتعرضلها بعدين..
</span>
</p>

<h2 dir="rtl">
<span>
إزاي نتأكد من وجود الObfuscation؟
</span>
</h2>
<p dir="rtl">
<span>
بعض الطرق اللي بنتأكد بيها من وجوده:
</span>
</p>
<p dir="rtl">
<span>
1-عن طريق الAbnormal PE Sections وهنا بنلاحظ وجود تغييرات غير طبيعية في شكل الsections الموجودة في المالوير.. على سبيل المثال لو سكشن .text حجمه 0 على الdisk لكن ليه حجم أكبر في الmemory دا بيبقى مؤشر ان فيه عملية Packing حصلت عليه لانه في هيروح يستدعي الكود وينفذه في الميموري
</span>
</p>
<p dir="rtl">
<span>
2-عن طريق الAPIs المستخدمة في المالوير يعني لو لاحظنا انه بيستخدم APIs خاصة بالPackers او فيه Functions بتعمل encryption او encoding ففيه إحتمال انه obfuscated او packed والنقطة دي ممكن نستخدم فيها ادوات زي Detect it Easy و PEiD عشان نشوف الFunctions والAPIs المستخدمة
</span>
</p>
<p dir="rtl">
<span>
3-عن طريق الEntropy ودا عبارة عن مقياس للعشوائية (كل ما كان مستواه عالي كل ما كانت نسبة وجود الobfuscation عالية) ودا نقدر نشوفه باستخدام اداة Detect it Easy ونتاكد من الsections من خلالها..الخ
</span>
</p>
<p dir="rtl">
<span>
في الاخر لاحظ ان الobfuscation مش شرط يُستخدم لأغراض سيئة، ممكن يُستخدم من بعض المبرمجين لحماية حقوق الطبع والنشر وما إلى ذلك.
</span>
</p>
