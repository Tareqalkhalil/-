# قارئ القرآن الكريم — أندرويد و iOS

المشروع يغلّف تطبيق الويب (مجلد `www`) داخل تطبيق أصلي باستخدام Capacitor.

## المتطلبات
- Node.js (إصدار حديث)
- أندرويد: Android Studio
- iOS: جهاز Mac مع Xcode (لا يمكن بناء iOS على ويندوز أو لينكس)

## الإعداد (مرة واحدة)
```bash
npm install
npm install @capacitor/core @capacitor/cli @capacitor/android @capacitor/ios
npx cap add android
npx cap add ios        # على Mac فقط
```

قبل النشر غيّر `appId` في `capacitor.config.json` إلى معرّف خاص بك، مثل `com.yourname.quran`.

## التشغيل والبناء
```bash
npx cap sync
npx cap open android   # يفتح Android Studio ثم Run أو Build > Build APK
npx cap open ios       # يفتح Xcode ثم Run
```

بعد أي تعديل على `www/index.html` نفّذ `npx cap sync` مرة أخرى.

## ملاحظات
- عند أول تشغيل يطلب التطبيق استيراد ملف Tanzil (صيغة Pipe-separated، النص العثماني) من tanzil.net/download. يُحفظ في التطبيق ولا يتكرر الطلب.
- يشترط ترخيص Tanzil عدم تعديل النص وذكر المصدر.
- النشر على Google Play وApp Store يحتاج حساب مطوّر (Google ≈ 25$ مرة واحدة، Apple ≈ 99$ سنوياً)، ويُنصح بمراجعة النص من جهة مختصة قبل النشر.
- الخط يُحمَّل من Google Fonts، ولتشغيله دون إنترنت نزّل خط Amiri Quran وضعه في www وغيّر رابطه في index.html.

## بناء APK بدون Android Studio (عبر GitHub)
1. أنشئ مستودعاً جديداً على github.com وارفع محتويات هذا المجلد (بما فيه `.github`).
2. افتح تبويب Actions ثم Build APK ثم Run workflow (أو يعمل تلقائياً عند الرفع على main).
3. بعد دقائق افتح التشغيل المكتمل ونزّل ملف `quran-app-apk` من قسم Artifacts، وبداخله `app-debug.apk`.
4. انقله إلى هاتفك وثبّته (فعّل "التثبيت من مصادر غير معروفة").
ملاحظة: هذا APK تجريبي (debug) مناسب للتجربة والاستخدام الشخصي، أما النشر على Google Play فيحتاج ملف AAB موقّعاً.

## بناء AAB موقّع للنشر على Google Play
1. أنشئ مفتاح التوقيع على جهازك (مرة واحدة، واحتفظ بنسخة احتياطية منه وبكلمات المرور في مكان آمن):
   ```bash
   keytool -genkey -v -keystore release.jks -keyalg RSA -keysize 2048 -validity 10000 -alias quran
   ```
2. حوّله إلى نص: على Mac/لينكس `base64 -i release.jks | tr -d '\n'`، وعلى ويندوز `certutil -encode release.jks out.txt` (انسخ ما بين السطرين).
3. في المستودع: Settings ثم Secrets and variables ثم Actions، وأضف أربعة أسرار:
   `KEYSTORE_BASE64` و `KEYSTORE_PASSWORD` و `KEY_ALIAS` (مثل quran) و `KEY_PASSWORD`.
4. افتح Actions ثم Build AAB (signed) ثم Run workflow، ونزّل `app-release.aab` من Artifacts.
5. ارفعه في Google Play Console عند إنشاء التطبيق (يُنصح بتفعيل Play App Signing).
لا ترفع ملف `release.jks` إلى المستودع أبداً.
