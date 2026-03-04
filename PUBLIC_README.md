# فكرة — Fekra | Architectural Planning Platform

<div dir="rtl">

## نظرة عامة

**فكرة** منصة ويب تربط **العملاء** بالمهندسين المعماريين لإدارة مشاريع التخطيط المعماري. يختار العميل باقة خدمة، يملأ بيانات الأرض والاحتياجات، والمهندس يرفع المخططات ويتابع التعديلات والمحادثة حتى التسليم. تدعم المنصة ثلاثة أدوار: عميل، مهندس، وإدارة.

</div>

---

## Overview

**Fekra** (فكرة — "Idea") is a full‑stack web platform that connects **clients** with architectural engineers to manage **architectural planning** projects. Clients choose a service package, submit land and requirements, and engineers upload plans, handle revision requests, and communicate via in-app chat until delivery. The platform supports three roles: **Client**, **Engineer**, and **Admin**.

---

<div dir="rtl">

## المكدس التقني | Tech Stack

| الطبقة | التقنيات |
|--------|----------|
| الواجهة | Next.js 14 (App Router), React 18, TypeScript, Tailwind CSS |
| الخلفية | Next.js API Routes, Prisma ORM |
| قاعدة البيانات | PostgreSQL (hosted) |
| المصادقة والتخزين | Auth & Storage services (cloud) |
| النشر | Vercel |

أدوات مساندة: react-hook-form, zod, SWR, lucide-react. الخط العربي: **Tajawal** (Google Fonts) مع دعم RTL والوضع النهاري/الليلي.

</div>

| Layer | Technologies |
|-------|--------------|
| Frontend | Next.js 14 (App Router), React 18, TypeScript, Tailwind CSS |
| Backend | Next.js API Routes, Prisma ORM |
| Database | PostgreSQL (hosted) |
| Auth & Storage | Cloud auth and object storage |
| Deployment | Vercel |

Supporting tools: react-hook-form, zod, SWR, lucide-react. Arabic typography: **Tajawal** (Google Fonts) with RTL and light/dark theme support.

---

<div dir="rtl">

## المميزات التقنية | Technical Highlights

### 1. نظام الصلاحيات (Role-Based Access)

- ثلاثة أدوار: **عميل (CLIENT)**، **مهندس (ENGINEER)**، **إدارة (ADMIN)**.
- التحقق من الهوية والصلاحية يتم على الخادم في كل طلب API؛ لا يعتمد الوصول على الواجهة فقط.
- حماية المسارات: لوحة العميل، لوحة المهندس، ولوحة الإدارة معزولة حسب الدور.
- دعم مصادقة مرن (يمكن التبديل بين مزودي مصادقة حسب بيئة التشغيل).

### 2. إدارة الطلبات والملفات

- دورة حياة الطلب: من الإنشاء إلى الدفع، تعيين المهندس، رفع المخططات، طلبات التعديل (مراجعات)، والمحادثة.
- رفع وتخزين المخططات مع صلاحيات وصول مرتبطة بالطلب والمدة؛ انتهاء صلاحية التحميل بعد مدة محددة.
- محادثة داخل الطلب مع تخزين الرسائل وربطها بالمستخدم والطلب.

### 3. لوحات التحكم

- **لوحة العميل:** متابعة الطلبات، المحادثة، تحميل المخططات، وشراء مراجعات إضافية عند الحاجة.
- **لوحة المهندس:** قائمة الطلبات المعينة، رفع المخططات، إدارة المراجعات والمحادثة.
- **لوحة الإدارة:** إحصائيات، إدارة الباقات والمستخدمين والطلبات، محتوى الصفحة الرئيسية، وإعدادات إضافية (مثل شراء المراجعات). تقارير الطلبات وتصدير بيانات.

### 4. تجربة المستخدم

- واجهة متجاوبة (responsive) لجميع الصفحات مع breakpoints مناسبة للجوال والتابلت وسطح المكتب.
- وضع نهاري/ليلي مع شعار مزدوج (لوجو مختلف حسب الثيم).
- نموذج إنشاء طلب متعدد الخطوات مع التحقق من صحة الحقول (zod).

</div>

### 1. Role-Based Access

- Three roles: **CLIENT**, **ENGINEER**, **ADMIN**. Authorization is enforced on the server for every API call; the UI alone does not grant access.
- Route protection for client dashboard, engineer dashboard, and admin panel.
- Auth implementation supports switching providers per environment.

### 2. Orders & File Management

- Full order lifecycle: creation, payment, engineer assignment, plan uploads, revision requests, and in-order chat.
- Plan uploads with access scoped to the order and a configurable download validity period; automatic cleanup of expired files.
- In-order messaging with persisted messages linked to user and order.

### 3. Dashboards

- **Client:** Order list, chat, plan downloads, and optional purchase of extra revisions.
- **Engineer:** Assigned orders, plan uploads, revision handling, and chat.
- **Admin:** Stats, package/user/order management, homepage content, and settings (e.g. revision and pin-pack pricing); order reports and export.

### 4. UX

- Responsive layout across all pages with mobile/tablet/desktop breakpoints.
- Light/dark theme with dual logo (theme-specific assets).
- Multi-step order creation form with client- and server-side validation (zod).

---

<div dir="rtl">

## هيكل المشروع | Project Structure

```
├── app/
│   ├── (auth)/          # تسجيل الدخول، التسجيل، نسيت كلمة المرور، إلخ
│   ├── (client)/        # لوحة العميل، الطلبات، المحادثة، الدفع، المراجعات
│   ├── admin/           # لوحة الإدارة، الباقات، المستخدمين، التقارير، الإعدادات
│   ├── engineer/        # لوحة المهندس، الطلبات، التعديلات، التقديم برابط دعوة
│   ├── api/             # API Routes (طلبات، رسائل، مخططات، دفعات، cron، إلخ)
│   ├── layout.tsx
│   ├── page.tsx         # الصفحة الرئيسية
│   └── globals.css
├── components/
│   ├── layout/          # Header, ThemeToggle, UserMenu
│   ├── shared/          # Button, Card
│   ├── notifications/
│   └── providers/
├── hooks/               # useAuth, useOrderChat, useMyOrders, إلخ
├── lib/                 # API client, auth helpers, Prisma, upload utilities
├── prisma/
│   ├── schema.prisma    # نماذج المستخدم، الطلب، الباقة، المخطط، الرسائل، إلخ
│   └── migrations/
├── public/              # أصول ثابتة (شعارات، أيقونات)
├── schemas/             # تحقق النماذج (zod)
└── types/               # TypeScript types
```

- **app/(auth):** Login, register, forgot password/email, reset password.
- **app/(client):** Client dashboard, orders, order detail, chat, payment, revisions, select package, profile, policy.
- **app/admin:** Dashboard, packages, orders, engineers, applications, users, homepage content, settings, reports.
- **app/engineer:** Dashboard, order detail, chat, revision editor, apply-by-token.
- **app/api:** REST-style routes for orders, messages, plans, payments, admin, cron jobs; all protected by server-side auth/role checks.

</div>

---

<div dir="rtl">

## لقطات الواجهة (Placeholders) | UI Screenshots

الواجهة تستخدم خط **Tajawal** وألوان الهوية المعمارية (وضع نهاري/ليلي). يمكن إضافة لقطات شاشة هنا بعد التقاطها من المشروع.

</div>

The UI uses **Tajawal** and a consistent light/dark palette.

**Live project links (to be updated by you):**

- Production (Vercel): `https://your-fekra-app.vercel.app`
- Source code (GitHub): `https://github.com/your-username/your-fekra-repo`

Replace the placeholder URLs above with your real deployment and repository links.

---

<div dir="rtl">

## ملاحظة أمنية | Security Note

- لا يُفترض أن يحتوي هذا الملف على مفاتيح API أو روابط قواعد بيانات أو أي متغيرات بيئية. إعداد التشغيل يتم عبر ملفات بيئة محلية غير مرفوعة.
- التحقق من الهوية والصلاحيات يتم دائماً على الخادم؛ لا يتم الاعتماد على دور المستخدم من الواجهة فقط.
- عمليات حساسة (مثل الدفع، تعيين المهندس، حذف الملفات المنتهية) محمية بتحقق من الدور والـ session أو token.
- تفاصيل تنفيذ المصادقة والصلاحيات (دوال التحقق، التخزين المؤقت للدور، ربط الحسابات) لا تُكشف هنا للحفاظ على أمان المنصة.

</div>

- This README does **not** include API keys, database URLs, or environment variables. Runtime configuration is done via local env files that are not committed.
- Authentication and authorization are enforced on the server; the UI does not alone determine access.
- Sensitive operations (e.g. payment, engineer assignment, file cleanup) are guarded by role and session/token checks.
- Implementation details of auth and role resolution are intentionally omitted to keep the production system secure.

---

<div dir="rtl">

## الترخيص | License

هذا المشروع لأغراض العرض Portfolio. الاستخدام التجاري أو إعادة النشر قد يتطلب التنسيق مع صاحب المشروع.

</div>

This project is for **portfolio** demonstration. Commercial use or redistribution may require permission from the project owner.
