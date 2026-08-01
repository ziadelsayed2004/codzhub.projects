<div dir="rtl" align="right">

# ملاحظات تعديل UI/UX لتطبيقي التاجر والدليفري

**تاريخ المراجعة:** ٢ أغسطس ٢٠٢٦  
**مصادر الحقيقة:** `PRD_AR.md` و`PRD.md` وPostman النشط والمخطط  
**ملفات الواجهة المراجعة:** `Merchant.rar` و`Delivery.rar`

## نتيجة المراجعة

- تمت مراجعة **٤٧ شاشة PDF للتاجر**: الملفات `0–43` و`45–47`؛ الملف `44` غير موجود.
- تمت مراجعة **٣٤ شاشة PDF للدليفري**: الملفات `0–32` و`43`؛ الملفات `33–42` غير موجودة.
- الملفات تصميمات فقط وليست Source قابلًا للتشغيل؛ لذلك كل مهام الواجهة تبقى Open حتى استلام الكود وبناء التطبيق واختباره.
- لا توجد في التصميمات الحالية شاشات للبوت أو لوحة المراكز أو مكافآت الأنشطة الاجتماعية. هذه الإضافات تخص تطبيق العميل ولوحة الأدمن، ولا تُضاف لتطبيق التاجر أو الدليفري بلا تغيير نطاق جديد.
- الإضافة الجديدة التي تغيّر تطبيق الدليفري مباشرة هي شاشة الطوارئ المتقدمة.
- عجلة الحظ مستبعدة بالكامل: لا شاشة، ولا مكان محجوز، ولا إعداد، ولا نص دعائي، ولا حركة نقاط.

## درجات الأولوية

- **P0:** خطأ عقد/ماليات/أمان/صلاحية يمنع التسليم.
- **P1:** فلو أساسي ناقص أو حالة Backend غير ممثلة.
- **P2:** Copy، اتساق بصري، Accessibility، أو تحسين تجربة لا يغير الحقيقة التشغيلية.

## قواعد مشتركة للتطبيقين

١. استبدال كل ظهور لـ`Market Home` أو `Market Office` أو `Market Studio` باسم **يلا حالًا** أو اسم التاجر الحقيقي القادم من API. لا تستخدم قيمًا ثابتة في الشاشة.  
٢. العربية RTL بالكامل والإنجليزية LTR، بما في ذلك الأيقونات الاتجاهية والحقول والأرقام ورسائل الخطأ.  
٣. كل شاشة بيانات تحتاج حالات: تحميل، فارغ، Offline، Retry، Session expired، `403`، `404`، `409`، `422`، `429`، وتعذر المزود الخارجي.  
٤. لا تحسب الواجهة سعرًا أو عمولة أو رصيدًا أو استحقاقًا أو حالة طلب من نفسها؛ تعرض Snapshot السيرفر.  
٥. لا توجد مدفوعات إلكترونية للعميل في النطاق الحالي. Wallet/InstaPay داخل التصميمات تخص **إثبات تسوية التاجر/الدليفري للمنصة** فقط.  
٦. أرقام الهاتف، تعليمات الدفع، حساب InstaPay، عنوان المقر، الحدود الزمنية، وحدود التكرار تأتي من API ولا تُكتب داخل التصميم.  
٧. إضافة تأكيدات للإجراءات غير القابلة للتراجع، ومساحة لمس مناسبة، ودعم قارئ الشاشة، وContrast واضح، وعدم الاعتماد على اللون وحده.  
٨. استخدام رقم طلب ظاهر للمستخدم مع الاحتفاظ بالـID الداخلي عند الحاجة للدعم، ومنع عرض بيانات تخص تاجر/سائق/عميل آخر.  

---

# أولًا: تطبيق التاجر

## خريطة الشاشات والتعديلات

<table dir="rtl">
  <thead dir="rtl">
    <tr dir="rtl">
      <th><div dir="rtl" align="right">الشاشات</div></th>
      <th><div dir="rtl" align="right">المحتوى الحالي</div></th>
      <th><div dir="rtl" align="right">التعديلات المطلوبة</div></th>
      <th><div dir="rtl" align="right">الأولوية</div></th>
    </tr>
  </thead>
  <tbody dir="rtl">
    <tr dir="rtl"><td align="right">0–3</td><td align="right">Splash، اللغة، إنشاء الحساب، OTP</td><td align="right">توحيد البراند، رسائل OTP والقفل والـCooldown من السيرفر، منع كشف وجود الحساب، وإضافة كل حالات الفشل وإعادة الإرسال.</td><td align="right">P1</td></tr>
    <tr dir="rtl"><td align="right">4–6</td><td align="right">مستندات، موقع، انتظار الموافقة</td><td align="right">التصنيف يطلب أثناء التسجيل فقط وتثبته الإدارة بعد الموافقة. توضيح المستندات المطلوبة/الاختيارية، حد وجهي المستند، Progress/Retry، وخريطة Google بدبوس ومراجعة عنوان.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">7–11</td><td align="right">Login وReset وحساب محظور</td><td align="right">تصحيح “Rest Password” إلى “Reset Password” و“Account Block” إلى “Account Blocked”، وإظهار سبب/قناة التواصل المسموحة بدون تسريب بيانات أو تخطي حالة الحساب.</td><td align="right">P2/P1</td></tr>
    <tr dir="rtl"><td align="right">12–17</td><td align="right">طلبات جديدة/تجهيز/تاريخ وتفاصيل</td><td align="right">ثلاث Tabs صريحة: New، Preparing، History؛ Counts وحالات فارغة؛ سبب الرفض إجباري؛ قبول الطلب يدعم ±5 دقائق وحد القدرة للطلب الكبير؛ حالات Timeout/إلغاء جزء تاجر/إعادة التسعير؛ لا شات مع العميل أو الدليفري.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">18–29</td><td align="right">المنتجات والإضافة والتعديل والخيارات والعروض</td><td align="right">التاجر يختار فقط من التصنيفات المعينة من الإدارة، مع subcategory وStandard/S/M/L والمنتجات المتعلقة من sibling subcategories. إضافة Validation، حالة المخزون، Progress الصور، تأكيد الحذف، وعدم استخدام “Present” بدل Percentage.</td><td align="right">P0/P1</td></tr>
    <tr dir="rtl"><td align="right">30–32</td><td align="right">KPIs المنتج</td><td align="right">ربط الفترات والـTotals مباشرة بعقد Analytics، توحيد المقاييس، Empty/error، وتصحيح “Analyzes” إلى “Analytics”.</td><td align="right">P1/P2</td></tr>
    <tr dir="rtl"><td align="right">33–36</td><td align="right">Flash Sales</td><td align="right">حذف أي حقيقة UI غير موجودة بالعقد: Pay-now، تسعير بالساعة، Total Cost عشوائي، أو Banner/عنوان غير مدعوم. عرض المجاني من إعداد الأدمن، وعدد/مدة التشغيل، والخطة الأسبوعية المدفوعة بعد المجاني وحالتها على الفاتورة.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">37–40</td><td align="right">Dashboard، تقييم، تجهيز، KPIs الطلبات</td><td align="right">عرض Revenue/Commission/Grace progress/Flash cost/Points cost/discount funding من Snapshots منفصلة. تقييم التاجر مشتق من تقييم تجربة منتجات الأجزاء المسلمة. نص Open/Busy يجب أن يطابق حالة التشغيل الفعلية.</td><td align="right">P0/P1</td></tr>
    <tr dir="rtl"><td align="right">41–43</td><td align="right">إشعارات، إعدادات، Help Center</td><td align="right">إزالة نصوص homeowner/booking/e-payment القديمة، تحديث البراند، دعم Deep links الصحيحة، وفصل FAQ عن تذكرة الدعم البشري. البوت الذكي المعتمد للعميل فقط ولا يضاف هنا.</td><td align="right">P1/P2</td></tr>
    <tr dir="rtl"><td align="right">44</td><td align="right">غير موجود</td><td align="right">تسجيله كصفحة مفقودة في تسليم UI. لا يخترع الفريق شاشة أو رقمًا بديلًا؛ يحدد المصمم هل هي Bridge للفواتير أم خطأ ترقيم.</td><td align="right">P1</td></tr>
    <tr dir="rtl"><td align="right">45–47</td><td align="right">تسوية الفاتورة وإثبات الدفع</td><td align="right">فاتورة التاجر أسبوعية، وفترة السداد/Grace هي عقد السيرفر وليست “خلال 24 ساعة”. Wallet وInstaPay يحتاجان إثباتًا خاصًا؛ الحالات Pending/Approved/Rejected/Re-upload. لا تكرار تكلفة خصم أو كوبون داخل العمولة.</td><td align="right">P0</td></tr>
  </tbody>
</table>

## فلو الطلب المطلوب للتاجر

١. **New:** Card لكل Portion تخص التاجر فقط، مع العناصر/الاختيارات/الملاحظات/السعر Snapshot ووقت الرد.  
٢. **Accept:** تحديد وقت التجهيز ضمن العقد، إقرار القدرة على الطلب الكبير عند ظهوره، ثم Update من السيرفر.  
٣. **Reject:** سبب إجباري من قائمة + ملاحظة اختيارية، ثم تحديث الحالة والتسعير للعميل.  
٤. **Preparing:** تعديل وقت التجهيز في الحدود المسموحة، ثم Ready.  
٥. **Pickup:** تأكيد تسليم الجزء للدليفري ومبلغ التاجر المحسوب من السيرفر؛ لا يكتب التاجر مبلغًا يدويًا.  
٦. **History/Receipt:** الجزء الخاص بالتاجر فقط، مع Receipt صالح للطباعة لا يظهر عمولة المنصة أو بيانات تسوية داخلية للعميل.  

## الفواتير والماليات — حالات يجب تصميمها

- لا توجد فاتورة بعد، فاتورة مفتوحة، داخل المهلة، مستحقة، إثبات قيد المراجعة، مقبول، مرفوض مع سبب وإعادة رفع، متأخر/تقييد الحساب.
- بنود مستقلة: عمولة الطلبات، خطة Flash الأسبوعية، تسوية نقاط التاجر، مساهمة التاجر في كوبون/عيد ميلاد، Credits/Corrections، الإجمالي.
- عرض تقدم الإعفاء من العمولة حسب عدد الطلبات المؤهلة، بدون إعادة حساب من الواجهة.
- Free delivery ممول من المنصة ولا يتحول إلى تكلفة على التاجر.

---

# ثانيًا: تطبيق الدليفري

## خريطة الشاشات والتعديلات

<table dir="rtl">
  <thead dir="rtl">
    <tr dir="rtl">
      <th><div dir="rtl" align="right">الشاشات</div></th>
      <th><div dir="rtl" align="right">المحتوى الحالي</div></th>
      <th><div dir="rtl" align="right">التعديلات المطلوبة</div></th>
      <th><div dir="rtl" align="right">الأولوية</div></th>
    </tr>
  </thead>
  <tbody dir="rtl">
    <tr dir="rtl"><td align="right">0–3</td><td align="right">Splash، اللغة، التسجيل، OTP</td><td align="right">توحيد البراند وحالات OTP والقفل والـCooldown، وتصحيح نصوص Reset.</td><td align="right">P1/P2</td></tr>
    <tr dir="rtl"><td align="right">4–6</td><td align="right">المركبة، المستندات، انتظار الموافقة</td><td align="right">الهوية والصورة الشخصية وصور المركبة مطلوبة؛ الرخصة وتسجيل المركبة مطلوبان للسيارة/الموتوسيكل فقط. لا تجعل Police clearance حقيقة عامة إن لم يطلبها إعداد التسجيل. دعم التروسيكل والعجلة وحدود كل مركبة.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">7–11</td><td align="right">Login وReset وحظر الحساب</td><td align="right">نفس تصحيحات النصوص والحماية، مع حالة suspended المرتبطة بالفاتورة اليومية أو قرار الإدارة.</td><td align="right">P1</td></tr>
    <tr dir="rtl"><td align="right">12–16</td><td align="right">طلبات متاحة، تفاصيل، ملاحة، استلام، تسليم</td><td align="right">الأهلية من السيرفر حسب المنطقة/Online/المركبة/الجاهزية/المسافة/الحجم. ترتيب Google Routes، قبول يدوي أو Auto-nearest اختياري، مسار Multi-stop، تأكيد مبلغ التاجر، واختيار `door` أو `building_entrance` عند التسليم.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">17–19</td><td align="right">شات العميل وتنبيه Alert/Ring</td><td align="right">شات الدليفري مع العميل فقط. الرسالة العادية منفصلة عن Alert/Ring. زمن التكرار والحدود من API ولا يُثبت “3 دقائق/دقيقة”. الهاتف المخفي يمنع الاتصال فقط ولا يمنع الشات/التنبيه.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">20–21</td><td align="right">Offline وSuspended</td><td align="right">توضيح سبب عدم استقبال الطلبات، CTA للاستعادة/الدعم/سداد الفاتورة، ومنع أي قبول طلب عند عدم الأهلية.</td><td align="right">P1</td></tr>
    <tr dir="rtl"><td align="right">22–26</td><td align="right">إشعارات، إعدادات، بروفايل، دعم، Dashboard</td><td align="right">الإشعارات مرتبطة بالـDeep link؛ DND لا يخفي الطوارئ أو حالات الطلب/الفاتورة الإلزامية؛ البيانات والـKPIs من API؛ الدعم بشري. البوت المعتمد ليس للدليفري.</td><td align="right">P1</td></tr>
    <tr dir="rtl"><td align="right">27</td><td align="right">Driver Rating</td><td align="right">حذف الشاشة والويدجت وأي Positive/Negative rating؛ تقييم الدليفري في الاتجاهين متقاعد في PRD والـRuntime الحالي.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">28–29</td><td align="right">KPIs الطلبات والأرباح</td><td align="right">تمييز Customer collection وMerchant payout وDriver earning وPlatform credit. لا تعرض “Total Times” أو قيم تقييم متقاعدة، ولا تجمع قيمًا من الواجهة.</td><td align="right">P0/P1</td></tr>
    <tr dir="rtl"><td align="right">30–32</td><td align="right">الفاتورة اليومية وطرق الدفع</td><td align="right">الفاتورة إلزامية يومية وموعدها 24 ساعة. Cash at HQ يؤكده الأدمن بلا Receipt؛ Wallet وInstaPay يحتاجان Receipt. الحالات Pending/Approved/Rejected/Expired/Suspended. Platform credits تخصم من المستحق ولا تظهر Coupon/Free Delivery كرسوم مزدوجة.</td><td align="right">P0</td></tr>
    <tr dir="rtl"><td align="right">33–42</td><td align="right">غير موجودة</td><td align="right">تسجيلها كملفات مفقودة. لا يفترض أنها حالات فاتورة أو طوارئ دون ملف تصميم/قرار من UI/UX.</td><td align="right">P1</td></tr>
    <tr dir="rtl"><td align="right">43</td><td align="right">Help Center</td><td align="right">استبدال محتوى Market Home/homeowner/booking/e-payment بمساعدة تشغيل الدليفري: الطلبات، الملاحة، الاستلام، التسليم، الفاتورة اليومية، الطوارئ، والدعم.</td><td align="right">P1/P2</td></tr>
  </tbody>
</table>

## شاشة الطوارئ المتقدمة — تصميم جديد إلزامي

يظهر زر واضح أعلى خريطة الطلب النشط، ويفتح Bottom Sheet/Modal يحتوي على ثلاثة إجراءات منفصلة:

١. **إرسال SOS للمنصة:** ينشئ تنبيهًا داخليًا حرجًا، ويمكن إرفاق آخر موقع مصرح به.  
٢. **اتصال بطوارئ يلا حالًا:** يظهر فقط إذا كانت الوجهة مفعلة ورقمها صالح للمنطقة.  
٣. **اتصال بالإسعاف:** يظهر فقط إذا كانت وجهة الإسعاف مفعلة ورقمها صالح للمنطقة.  

قواعد التصميم:

- لا يبدأ الاتصال عند فتح الشاشة أو الضغط الأول؛ يلزم اختيار ثم Confirmation يعرض اسم الجهة والرقم.
- التطبيق يفتح Dialer فقط ولا يجري مكالمة في الخلفية.
- بعد العودة من Dialer يرسل نتيجة محاولة الفتح، لا نتيجة المكالمة ولا صوتها.
- فشل Dialer لا يلغي SOS سبق إرساله، ويظهر Retry أو اختيار الجهة الأخرى.
- DND لا يخفي الزر أو الخيارات، ولا تتبع الطوارئ Cooldown تنبيه شات العميل.
- عند عدم وجود رقم صالح يظهر الإجراء Disabled مع رسالة واضحة أو يخفى حسب عقد API، مع CTA للدعم؛ لا يعرض رقمًا قديمًا Cache بدون صلاحية.
- الأدمن يرى Audit: الجهة، وقت المحاولة، الدليفري، الطلب إن وجد، Snapshot الرقم، ونتيجة فتح Dialer.

## فلو الفاتورة اليومية للدليفري

- توضيح تاريخ اليوم/الفترة، المبلغ المحصل من العملاء، مدفوعات التجار، عمولة/مستحق المنصة، Credits التي تتحملها المنصة، والتصحيحات.
- لا تعرض كوبون أو عيد ميلاد أو توصيل مجاني كتكلفة على الدليفري؛ تعرض فقط أثر Snapshot الصحيح على التسوية.
- Cash at HQ: طلب تسجيل الدفع ثم انتظار تأكيد الأدمن؛ لا يرفع Receipt.
- Wallet/InstaPay: تعليمات من السيرفر + رفع Receipt خاص + حالات المراجعة.
- عند الرفض يظهر السبب وإعادة الرفع. عند التأخر تظهر قيود الحساب بدون السماح بتجاوزها محليًا.

---

# ثالثًا: الإضافات التي تحتاج شاشات خارج ملفي RAR

## تطبيق العميل

- شاشة مساعد خدمة العملاء: جلسة، مصادر مبسطة عند الملاءمة، “لا توجد إجابة موثوقة”، تحويل لموظف، وحالات Limits/Provider unavailable.
- شاشة Leaderboard: Top 10، وصاحب الحساب مرة واحدة في مركزه الحقيقي أو صف منفصل بعدهم إذا كان خارجهم.
- ملف عام محدود لأول 10 وSelf فقط: اسم/صورة مسموحة، المركز، XP، أكثر تاجر، وأكثر منتج من الطلبات المسلمة.
- شاشة الأنشطة الاجتماعية بأربع Cards مستقلة: Facebook Follow، Instagram Follow، Facebook Post، Instagram Post؛ كل Card تعرض النقاط والحملة والحد والحالة وطريقة التحقق.
- الضغط على Follow/Share لا يظهر “تمت إضافة النقاط”. الحالات الصحيحة: Link required، Verification pending، Verified/Awarded، Manual proof allowed، Rejected، Policy disabled، Limit reached، Provider unavailable.

## لوحة الأدمن

- إدارة قاعدة المعرفة: Draft/Published/Unpublished/Ingestion pending/Ready/Failed، Version، Audit، وتجربة استرجاع بدون نشر تلقائي.
- عمليات البوت: تشغيل، Limits/Cost controls، نصوص التعذر والتحويل، Unanswered analytics، وقائمة Handoffs.
- إعداد كل نشاط اجتماعي مستقلًا: التفعيل، النقاط، البداية/النهاية، Cooldown، حدود الحساب/اليوم/الشهر، Capability/Policy mode.
- قائمة مراجعة تظهر فقط للاستثناءات المسموحة غير القابلة للتحقق تلقائيًا، مع دليل وسبب قرار وسجل تدقيق.
- إعداد وجهات الطوارئ حسب المنطقة: النوع، الاسم، الرقم، التفعيل، Validation، ومعاينة ما سيراه الدليفري.
- سجل SOS ومحاولات فتح Dialer منفصلان، ولا يظهر محتوى مكالمة.

---

# ربط Postman بالواجهات

## أصول Runtime الحالية

- التاجر يعتمد على `YallaHalan_01_Merchant_UI` و`YallaHalan_05_Merchant_Order_Tabs_UAT`، مع Auth/Uploads المشتركة.
- الدليفري يعتمد على `YallaHalan_02_Driver_UI`، مع Auth/Uploads المشتركة.
- `YallaHalan_00_All_UI_Ordered` هو الترتيب الشامل، وليس بديلًا عن Collections الدور عند التطوير اليومي.
- لا يعاد استخدام أي Collection مؤرشفة أو باسم MarketHome.

## الإضافات الجديدة

`YallaHalan_07_Approved_New_Additions_PLANNED.postman_collection.json` مرجع مخطط فقط وكل طلباته متوقفة تلقائيًا. لا يربط UI فعليًا بها قبل ترقية كل Endpoint بعد تنفيذ الكود والاختبارات والصلاحيات.

عند التنفيذ يكون ترتيب الترقية:

١. Admin knowledge/settings.  
٢. Customer AI session/messages/handoff.  
٣. XP/Leaderboard/Profile.  
٤. Social activities/provider/campaign/claims.  
٥. Driver emergency destinations/call-attempt audit.  
٦. تحديث Collections الدور ثم `00_All_UI_Ordered` واختبارات Missing-token/Wrong-role/Cross-owner.  

## تعريف اكتمال شاشة UI

لا تعتبر الشاشة مكتملة إلا عند وجود:

- Source قابل للبناء ومطابق للتصميم المعدل.
- Endpoint نشط في Runtime inventory وليس داخل `_planned`.
- Request/response/error موثق ومختبر في Postman.
- Loading/empty/offline/permission/validation/rate-limit/provider-failure states.
- اختبار صلاحيات وOwnership وعدم كشف بيانات.
- RTL/LTR وAccessibility وResponsive QA.
- UAT دليل حي على بيئة مصرح بها.

</div>
