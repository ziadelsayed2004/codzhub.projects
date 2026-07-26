# Market Home — Production Provider Keys Checklist / قائمة مفاتيح ومزودي الإنتاج

> **Scope update / تحديث النطاق:**
> Hosting, domain, database, and media storage are **not subscriptions to buy separately in this checklist** because production will run on **Hostinger VPS**, with the domain/DNS/database/media uploads handled there by the project owner. Online payment is also **out of scope** for this release.
>
> **مهم:** هذا الملف لا يحتوي على اشتراك هوست أو دومين أو قاعدة بيانات أو بوابة دفع. التشغيل النهائي سيكون على **Hostinger VPS**، والميديا سترفع على نفس بيئة Hostinger، والدفع الإلكتروني غير مطلوب حاليًا.

---

## English summary

This checklist keeps only the external providers that still need accounts, keys, templates, or billing activation before production:

1. **WhySMS SMS OTP** — customer SMS OTP only.
2. **Meta WhatsApp Business Platform / Cloud API** — WhatsApp OTP for the three mobile user roles: customer, merchant, and driver.
3. **Firebase Cloud Messaging** — push notifications for customer, merchant, and driver apps.
4. **Google Maps Platform** — maps, pins, geocoding/places if used, routes, distance, ETA, and driver navigation contracts.
5. **Apple Push Notification service setup** — required only for iOS push delivery through Firebase.
6. **Production monitoring/logging/backup operational setup on Hostinger VPS** — not a subscription item here, but a required deployment task.

Explicitly excluded:

- No Vercel production hosting subscription.
- No MongoDB Atlas subscription.
- No separate domain/DNS subscription in this checklist.
- No Cloudinary / Cloudflare R2 media storage subscription.
- No Paymob / online payment gateway subscription.

---

## الملخص العربي

القائمة دي فيها فقط الخدمات الخارجية اللي محتاجة حسابات أو مفاتيح أو قوالب أو تفعيل قبل الإنتاج:

1. **WhySMS SMS OTP** — رسائل SMS OTP للعميل فقط.
2. **Meta WhatsApp Business Platform / Cloud API** — WhatsApp OTP للثلاث مستخدمين: العميل، التاجر، والدليفري.
3. **Firebase Cloud Messaging** — إشعارات الموبايل لتطبيق العميل والتاجر والدليفري.
4. **Google Maps Platform** — الخرائط، تحديد المواقع، المسافات، ETA، Routes، وملاحة الدليفري.
5. **Apple Push Notification service setup** — مطلوب فقط لتوصيل إشعارات iOS عن طريق Firebase.
6. **تشغيل ومراقبة ونسخ احتياطي على Hostinger VPS** — مهمة تشغيلية مطلوبة، لكنها ليست اشتراكًا ضمن هذا الملف.

المستبعد صراحة من هذه القائمة:

- لا يوجد اشتراك Vercel Production.
- لا يوجد اشتراك MongoDB Atlas.
- لا يوجد بند دومين/DNS هنا.
- لا يوجد Cloudinary / Cloudflare R2 لأن الميديا على Hostinger.
- لا يوجد Paymob أو بوابة دفع إلكتروني.

---

## Provider matrix / جدول المزودين

| Priority | Area | Provider | Required for | Official link | Pricing / cost signal | Env / setup keys | Status |
|---:|---|---|---|---|---|---|---|
| 1 | Customer SMS OTP | WhySMS | Customer SMS OTP only; can be fallback beside WhatsApp for customer if product enables both. | https://whysms.com/ and https://whysms.com/faq | Public packages show per-SMS pricing; API access is free with purchased SMS credits; VAT may apply. | `WHYSMS_API_URL`, `WHYSMS_API_KEY`, `WHYSMS_SENDER_ID`, `WHYSMS_MESSAGE_TEMPLATE`, `WHYSMS_TIMEOUT_MS` | Required |
| 2 | WhatsApp OTP | Meta WhatsApp Business Platform / Cloud API | WhatsApp OTP for customer, merchant, and driver. Also future lifecycle messages if approved. | https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing and https://whatsappbusiness.com/products/platform-pricing/ | Meta charges per delivered message by market and category. OTP templates normally use authentication category. | `WHATSAPP_API_URL`, `WHATSAPP_API_TOKEN`, `WHATSAPP_TEMPLATE_NAME`, `WHATSAPP_TEMPLATE_LANGUAGE`, webhook verify token/app secret if webhooks are enabled | Required |
| 3 | Push notifications | Firebase Cloud Messaging | Push notifications for customer, merchant, and driver apps. | https://firebase.google.com/products/cloud-messaging | FCM itself has no cost; other Firebase products can have separate billing if used. | `FCM_ENABLED=true`, `FCM_PROVIDER=http_v1`, `FIREBASE_PROJECT_ID`, `FIREBASE_SERVICE_ACCOUNT_JSON`, app sender IDs / config files | Required |
| 4 | iOS push bridge | Apple Developer / APNs | Required only if iOS apps receive push notifications through Firebase. | https://developer.apple.com/programs/ | Apple Developer Program is normally required for production iOS app distribution and APNs credentials. | APNs key/cert uploaded/configured in Firebase console | Required for iOS production |
| 5 | Maps / Routing / ETA | Google Maps Platform | Maps, pinned locations, distance, ETA, routes, driver navigation, and delivery fee distance calculations. | https://mapsplatform.google.com/pricing/ and https://developers.google.com/maps/billing-and-pricing/pricing | Google Maps Platform uses pay-as-you-go and/or subscription plans depending selected products/usage. | `GOOGLE_MAPS_API_KEY`, `GOOGLE_MAPS_ANDROID_API_KEY`, `GOOGLE_MAPS_IOS_API_KEY`, `GOOGLE_MAPS_WEB_API_KEY`, `GOOGLE_ROUTES_API_KEY`, map IDs if used | Required |
| 6 | Media uploads | Hostinger VPS filesystem / mounted storage | Product images, merchant documents, driver documents, invoice receipts, logos/covers. | Hostinger VPS panel / server path | Covered by Hostinger VPS storage; no Cloudinary/R2 in current scope. | `UPLOADS_ROOT_DIR=/var/www/markethome/uploads` or a secure VPS path served by Nginx | Required as server setup |
| 7 | Logs / backups / monitoring | Hostinger VPS + PM2/Nginx/system tools | Production health, crash visibility, access logs, backups. | Hostinger VPS panel + Linux tooling | Covered by server operation; may add external monitoring later if client wants. | PM2 logs, Nginx logs, Mongo backup cron, disk alerts | Required as deployment task |

---

## Arabic provider notes / ملاحظات تنفيذ عربية

### 1. WhySMS — SMS OTP للعميل فقط

- الاستخدام الحالي: **العميل فقط**.
- لا يستخدم كـOTP أساسي للتاجر أو الدليفري في القرار الحالي.
- لازم يتعمل حساب WhySMS، شراء credits، ضبط Sender ID، وأخذ API URL/Key.
- يجب اختبار إرسال OTP حقيقي قبل إعلان production.

### 2. Meta WhatsApp Cloud API — WhatsApp OTP للثلاث مستخدمين

- الاستخدام الحالي: العميل + التاجر + الدليفري.
- مطلوب Meta Business، WhatsApp Business Account، رقم واتساب رسمي، template من نوع OTP/authentication، وتفعيل API token.
- لازم يتم اعتماد template قبل التشغيل الحقيقي.
- لا يتم الادعاء بأن WhatsApp OTP live إلا بعد smoke test فعلي على أرقام تجربة.

### 3. Firebase Cloud Messaging — الإشعارات

- مطلوب لكل تطبيقات الموبايل.
- Android يحتاج `google-services.json`.
- iOS يحتاج `GoogleService-Info.plist` + APNs داخل Firebase.
- السيرفر يحتاج `FIREBASE_PROJECT_ID` و `FIREBASE_SERVICE_ACCOUNT_JSON`.

### 4. Google Maps Platform — الخرائط والمسافات

- مطلوب لتحديد المواقع، Routes، ETA، navigation، وتكلفة التوصيل المعتمدة على المسافة.
- يفضل فصل مفاتيح Android/iOS/Web وتقييد كل مفتاح حسب package/bundle/domain.
- يجب تفعيل billing والـAPIs المطلوبة فقط.

### 5. Media on Hostinger VPS — الميديا على هوستنجر

- الميديا لن تستخدم Cloudinary/R2 في النطاق الحالي.
- الإنتاج يجب ألا يستخدم `/tmp` الخاص بـVercel.
- على Hostinger VPS يفضل مسار ثابت مثل:

```bash
/var/www/markethome/uploads
```

- يجب ضبط Nginx لخدمة الملفات static مع حماية مناسبة للملفات الحساسة.
- مستندات التجار/الدليفري والإيصالات يجب ألا تكون public بدون authorization إذا كانت حساسة.

---

## English implementation notes

### 1. WhySMS — customer SMS OTP only

- Current scope: **customer only**.
- Merchant and driver OTP must not depend on WhySMS in the current product decision.
- Buy WhySMS credits, configure Sender ID, and set API URL/key.
- Run a real OTP smoke test before production claim.

### 2. Meta WhatsApp Cloud API — WhatsApp OTP for three user roles

- Current scope: customer, merchant, and driver.
- Requires Meta Business, WhatsApp Business Account, approved phone number, OTP/authentication template, and API token.
- Templates must be approved before production use.
- Do not claim live WhatsApp OTP until a real smoke test passes.

### 3. Firebase Cloud Messaging

- Required for all mobile apps.
- Android needs `google-services.json`.
- iOS needs `GoogleService-Info.plist` and APNs configured in Firebase.
- Backend needs `FIREBASE_PROJECT_ID` and `FIREBASE_SERVICE_ACCOUNT_JSON`.

### 4. Google Maps Platform

- Required for maps, pins, Routes, ETA, navigation, and distance-based delivery fee calculations.
- Use restricted per-platform keys: Android, iOS, Web, and server Routes key.
- Enable only the APIs actually used by the apps/backend.

### 5. Media on Hostinger VPS

- Cloudinary/R2 is out of scope.
- Production must not use Vercel `/tmp`.
- Use a persistent VPS directory such as:

```bash
/var/www/markethome/uploads
```

- Configure Nginx/static serving carefully; sensitive documents and receipts should not be publicly readable without authorization.

---

## Production env checklist / قائمة ENV للإنتاج

```env
# Runtime
NODE_ENV=production
DEPLOYMENT_TEST_MODE=false

# Media on Hostinger VPS
UPLOADS_ROOT_DIR=/var/www/markethome/uploads

# WhatsApp OTP for customer, merchant, driver
WHATSAPP_API_URL=https://graph.facebook.com/v18.0/<PHONE_NUMBER_ID>/messages
WHATSAPP_API_TOKEN=<META_WHATSAPP_CLOUD_API_TOKEN>
WHATSAPP_TEMPLATE_NAME=otp_verification
WHATSAPP_TEMPLATE_LANGUAGE=en
WHATSAPP_AVAILABILITY_CHECK_ENABLED=true

# WhySMS SMS OTP for customer only
WHYSMS_API_URL=<WHYSMS_API_URL>
WHYSMS_API_KEY=<WHYSMS_API_KEY>
WHYSMS_SENDER_ID=<APPROVED_SENDER_ID>
WHYSMS_MESSAGE_TEMPLATE=Your Market Home verification code is {{code}}
WHYSMS_TIMEOUT_MS=5000

# Firebase Cloud Messaging
FCM_ENABLED=true
FCM_PROVIDER=http_v1
FIREBASE_PROJECT_ID=<FIREBASE_PROJECT_ID>
FIREBASE_SERVICE_ACCOUNT_JSON=<MINIFIED_FIREBASE_SERVICE_ACCOUNT_JSON>

# Google Maps / Routes
GOOGLE_MAPS_API_KEY=<SERVER_OR_GENERAL_MAPS_KEY_IF_USED>
GOOGLE_MAPS_ANDROID_API_KEY=<ANDROID_RESTRICTED_KEY>
GOOGLE_MAPS_IOS_API_KEY=<IOS_RESTRICTED_KEY>
GOOGLE_MAPS_WEB_API_KEY=<WEB_RESTRICTED_KEY>
GOOGLE_ROUTES_API_KEY=<SERVER_RESTRICTED_ROUTES_KEY>
GOOGLE_ROUTES_BASE_URL=https://routes.googleapis.com
GOOGLE_ROUTES_TIMEOUT_MS=5000
GOOGLE_ROUTES_TRAFFIC_AWARE=true
```

---

## Excluded production items / بنود مستبعدة من هذه القائمة

| Item | English decision | القرار العربي |
|---|---|---|
| Hosting | Hostinger VPS is already the chosen target; no Vercel/hosting subscription is listed here. | التشغيل على Hostinger VPS، لذلك لا يوجد بند اشتراك هوست هنا. |
| Domain/DNS | Managed with the client/project Hostinger/domain setup, not in this provider list. | الدومين والـDNS خارج هذه القائمة ويتم ضبطهم من جهة هوستنجر/مالك المشروع. |
| Database | Runs on the production server/owner-managed DB plan; no MongoDB Atlas subscription item here. | قاعدة البيانات ضمن إعدادات السيرفر/المالك وليست اشتراكًا في هذه القائمة. |
| Media storage | Hostinger VPS storage; no Cloudinary/R2 in current scope. | الميديا على هوستنجر، ولا يوجد Cloudinary/R2 حاليًا. |
| Online payments | No online payment gateway in current release. | لا يوجد دفع إلكتروني في الإصدار الحالي. |
| Paymob | Explicitly excluded until the client requests online collection. | Paymob مستبعد حتى يطلب العميل دفع إلكتروني. |

---

## Production blockers / موانع إعلان الإنتاج

- Do not commit real `.env` secrets.
- Do not claim production while `DEPLOYMENT_TEST_MODE=true`.
- Do not claim live OTP until real WhySMS and WhatsApp smoke tests pass.
- Do not claim live push notifications until Firebase device-token smoke tests pass for Android and iOS.
- Do not claim accurate Routes/ETA/pricing until Google Maps/Routes keys pass real route smoke tests.
- Do not expose uploaded sensitive documents publicly from Hostinger without authorization controls.
- لا يتم إعلان الإنتاج قبل اختبار OTP الحقيقي، الإشعارات، الخرائط، ورفع/عرض الميديا على مسار Hostinger الدائم.
