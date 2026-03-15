# Yougo.del v2 — دليل الإعداد الكامل 🚀

## الخطة الجديدة (مجانية 100% للأبد)

```
Cloudflare Pages  ← نشر الموقع
Supabase          ← Auth + قاعدة البيانات
Cloudflare Worker ← كاش يحمي Supabase
Cloudflare R2     ← صور المتاجر
MapLibre          ← خرائط مجانية
Firebase FCM      ← إشعارات
```

---

## خطوات الإعداد (بالترتيب)

### 1️⃣ Supabase (عندك أصلاً)
- ما في تغيير، استمر كما هو
- فقط أضف جدول `user_fcm_tokens` للإشعارات:
```sql
create table user_fcm_tokens (
  user_id uuid references auth.users(id),
  token   text not null,
  updated_at timestamptz default now(),
  primary key (user_id)
);
```

### 2️⃣ Cloudflare Worker (الكاش)
```bash
# 1. سجّل على cloudflare.com
# 2. اذهب لـ Workers & Pages
# 3. اضغط Create Worker
# 4. انسخ كود cloudflare-worker/worker.js
# 5. أضف Environment Variables:
#    SUPABASE_URL = رابط Supabase
#    SUPABASE_ANON_KEY = مفتاح Supabase
# 6. احفظ الرابط → VITE_WORKER_URL في .env
```

### 3️⃣ Cloudflare R2 (الصور)
```bash
# 1. في Cloudflare Dashboard → R2
# 2. Create Bucket: yougo-assets
# 3. فعّل Public Access
# 4. احفظ الرابط العام → VITE_R2_PUBLIC_URL
```

### 4️⃣ Firebase FCM (الإشعارات)
```bash
# 1. console.firebase.google.com
# 2. Create Project: yougo-del
# 3. Project Settings → Cloud Messaging
# 4. Generate VAPID Key
# 5. أضف كل القيم في .env
```

### 5️⃣ Cloudflare Pages (نشر الموقع)
```bash
# بدل Vercel، ننشر على Cloudflare Pages
# 1. Workers & Pages → Create Application → Pages
# 2. اربطه بـ GitHub repo
# 3. Build command: npm run build
# 4. Output directory: dist
# 5. أضف Environment Variables من .env
```

---

## ملفات جديدة في v2

| الملف | الوظيفة |
|---|---|
| `cloudflare-worker/worker.js` | الكاش الذكي |
| `src/lib/r2.js` | رفع الصور |
| `src/lib/notifications.js` | Firebase FCM |
| `public/_redirects` | Cloudflare Pages routing |
| `src/pages/MapPage.jsx` | MapLibre بدل Leaflet |

---

## ما تغيّر في الكود

- ❌ `leaflet` → ✅ `maplibre-gl` (أسرع + مجاني)
- ❌ `vercel.json` → ✅ `public/_redirects`
- ✅ إضافة Worker كاش للـ API
- ✅ إضافة R2 لرفع الصور
- ✅ إضافة FCM للإشعارات
