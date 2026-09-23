# Stage 82 — جداسازی واقعی کد Admin از APK Viewer

این مرحله فقط مخفی‌کردن منو نیست. ساختار Gradle و source-setها اصلاح شده‌اند تا کدهای مدیریتی در زمان کامپایل Viewer اصلاً وارد APK نشوند.

## تغییرات

- `AppNavigation.kt` به دو نسخه Flavor-specific تقسیم شد:
  - `src/admin/java/.../ui/AppNavigation.kt`
  - `src/viewer/java/.../ui/AppNavigation.kt`
- صفحات مدیریتی فقط در `src/admin` قرار دارند:
  - ContentManagementScreen
  - ContentEditorScreen
  - ViewerAccessManagementScreen
  - SpecialUsersManagementScreen
  - AccessAuditLogScreen
  - DialogueBuilderScreen
- ابزارهای مدیریتی مرتبط نیز به `src/admin` منتقل شدند:
  - WordContentImport
  - ContentHealthHelper
  - ContentTransferHelper
  - AccessAuditLog
  - PolicyAppliedReceiver
- `PolicyChangedReceiver` فقط در Viewer قرار دارد.
- Manifest مربوط به Policy Applied فقط در Admin قرار دارد.
- Viewer دیگر کد routeهای مدیریت محتوا، مدیریت کاربران و مدیریت دسترسی را کامپایل نمی‌کند.
- Workflow علاوه بر بررسی assetهای Viewer، APK Viewer را برای نشت کلاس‌های Admin بررسی می‌کند.

## نتیجه مورد انتظار

در `assembleViewerDebug`، کلاس‌های مدیریتی فهرست‌شده در Workflow نباید داخل APK وجود داشته باشند.

این جداسازی از UI-level permission قوی‌تر است، اما مانند هر APK آفلاین، جلوی مهندسی معکوس سایر کدهای باقی‌مانده یا استخراج محتوای موردنیاز اجرای Viewer را به‌صورت مطلق نمی‌گیرد.
