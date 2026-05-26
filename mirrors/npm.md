# میرور NPM / Yarn / PNPM – راهنمای تنظیم در ایران

## میرورهای ایرانی موجود

| میرور | آدرس | وضعیت |
|-------|------|-------|
| مخزن ملی منابع متن باز | `https://archive.ito.gov.ir/npm` | ✅ فعال |
| چابکان | `https://mirror2.chabokan.net/npm` | ✅ فعال |
| لیارا | `https://package-mirror.liara.ir/repository/npm`  | ✅ فعال |
| جمکو | `https://npm.jamko.ir/` | ✅ فعال |
| پردیسکو | `https://mirrors.pardisco.co/npm` | ✅ فعال |
| مگان | `https://hub.megan.ir/npm` | ✅ فعال |
| آتلانتیس کلود | `https://mirror.atlantiscloud.ir/npm` | ✅ فعال |
| کرگدن | `https://mirror.kargadan.ir/nexus/repository/npm-group` | ✅ فعال |
| پارس پک (ابرها)|`https://mirror.abrha.net/repository/npm/`| ✅ فعال |
| ایران سرور |`https://npm.iranserver.com/repository/npm/`| ✅ فعال |
| رانفلر | `https://npm.runflare.run` | ⚠️ محدودیت در پلن رایگان |

## روش ۱: نصب تکی با میرور (بدون تغییر تنظیمات)

اگر نمی‌خواهید تنظیمات سیستم را تغییر دهید، هنگام نصب هر پکیج آدرس میرور را مستقیماً مشخص کنید:

```bash
# نصب با npm
npm install --registry="https://mirror2.chabokan.net/npm" express

# نصب با npx
npx --registry="https://mirror2.chabokan.net/npm" create-react-app my-app
```

## روش ۲: تنظیم سراسری (Global) – پیشنهادی

با این روش، تمام دستورات `npm install` به‌صورت خودکار از میرور داخلی استفاده می‌کنند. این دستور در **تمام سیستم‌عامل‌ها** (لینوکس، مک، ویندوز) یکسان است:

### تنظیم برای npm

```bash
npm config set registry https://mirror2.chabokan.net/npm
```
##### روش دوم: فایل .npmrc
فایل `USER_HOME/.npmrc` را با محتویات زیر ایجاد کنید
```
registry=https://npm.jamko.ir/
chromedriver_cdnurl=https://download.jamko.ir/chromedriver/
sass-binary-site=https://download.jamko.ir/node-sass/
```
### تنظیم برای Yarn (v2+)

```bash
yarn config set npmRegistryServer https://mirror2.chabokan.net/npm
```
##### روش دوم: فایل .yarnrc
فایل `USER_HOME/.yarnrc` را با محتویات زیر ایجاد کنید.
```
"registry" "https://npm.jamko.ir/"
"chromedriver_cdnurl" "https://download.jamko.ir/chromedriver/"
"sass-binary-site" "https://download.jamko.ir/node-sass/"
```

### تنظیم برای pnpm

```bash
pnpm config set registry https://mirror2.chabokan.net/npm
```

### بررسی تنظیمات فعلی

```bash
# npm
npm config get registry

# yarn
yarn config get npmRegistryServer

# pnpm
pnpm config get registry
```

## روش ۳: تنظیم از طریق فایل `.npmrc`

می‌توانید در ریشه پروژه یا مسیر Home کاربر فایل `.npmrc` بسازید:

```ini
registry=https://mirror2.chabokan.net/npm
```

> **نکته:** فایل `.npmrc` در ریشه پروژه فقط روی همان پروژه اعمال می‌شود. فایل `.npmrc` در Home کاربر (`~/.npmrc`) به‌صورت سراسری اعمال می‌شود.

## بازگشت به مخزن اصلی

اگر به اینترنت بین‌المللی دسترسی پیدا کردید و می‌خواهید به مخزن اصلی npm برگردید:

```bash
npm config set registry https://registry.npmjs.org/
```

## عیب‌یابی

| مشکل | راه‌حل |
|------|--------|
| `ECONNREFUSED` هنگام `npm install` | میرور تنظیم نشده یا آدرس اشتباه است. `npm config get registry` را بررسی کنید |
| پکیج در میرور پیدا نمی‌شود (`404`) | پکیج هنوز سینک نشده؛ میرور دیگری امتحان کنید |
| سرعت پایین دانلود | میرور دیگری را تست کنید (مثلاً لیارا یا جمکو) |
| خطای SSL | از `--strict-ssl=false` استفاده کنید یا میرور را تغییر دهید |

