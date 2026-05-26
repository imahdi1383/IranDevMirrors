# مستندات تنظیم میرور NuGet

تنظیم و استفاده از میرورهای ایرانی NuGet برای `.NET CLI`، `nuget.exe`، Visual Studio، Rider، Docker و CI/CD.

## میرورها

| نام | آدرس | پروتکل | توضیح |
| --- | --- | --- | --- |
| کرگدن | `https://mirror.kargadan.ir/repository/nuget-group/index.json` | HTTPS | پیشنهادشده، NuGet v3 |
| کرگدن | `http://mirror.kargadan.ir/repository/nuget-group/index.json` | HTTP | فقط در صورت نیاز |
| رانفلر | `https://mirror-nuget.runflare.com/v3/index.json` | HTTPS | محدودیت در پلن رایگان |
| ایران‌سرور | `https://nuget.iranserver.com/repository/nuget/` | HTTPS | فعال |
| جمکو | `https://nuget.chrepo.ir` | HTTPS | فعال |
| پردیسکو | `https://mirrors.pardisco.co/nugetv3/index.json` | HTTPS | فعال |
| چابکان | `https://mirror2.chabokan.net/nuget/v3/index.json` | HTTPS | فعال |
| مگان | `https://hub.megan.ir/nuget/index.json` | HTTPS | فعال |

## روش‌های تنظیم

### 1) تنظیم با `.NET CLI` (پیشنهادی)

#### افزودن میرور

```bash
# کـرگدن
dotnet nuget add source https://mirror.kargadan.ir/repository/nuget-group/index.json -n kargadan

# رانفلر
dotnet nuget add source https://mirror-nuget.runflare.com/v3/index.json -n runflare

# ایران‌سرور
dotnet nuget add source https://nuget.iranserver.com/repository/nuget/ -n iranserver

# جمکو
dotnet nuget add source https://nuget.chrepo.ir -n jamko

# پردیسکو
dotnet nuget add source https://mirrors.pardisco.co/nugetv3/index.json -n pardisco

# چابکان
dotnet nuget add source https://mirror2.chabokan.net/nuget/v3/index.json -n chabokan

# مگان
dotnet nuget add source https://hub.megan.ir/nuget/index.json -n megan
```

#### حذف یا غیرفعال کردن منبع پیش‌فرض

```bash
dotnet nuget remove source nuget.org
```

یا:

```bash
dotnet nuget disable source nuget.org
```

#### مشاهده منابع ثبت‌شده

```bash
dotnet nuget list source
```

#### بازیابی پکیج‌ها با یک منبع خاص

```bash
dotnet restore --source https://mirror.kargadan.ir/repository/nuget-group/index.json
```

#### پاک کردن کش محلی

```bash
dotnet nuget locals all --clear
```

---

### 2) تنظیم با `nuget.exe`

#### افزودن منبع

```bash
nuget sources add -name KargadanMirror -source https://mirror.kargadan.ir/repository/nuget-group/index.json
```

نمونه برای چابکان:

```bash
nuget sources add -name ChabokMirror -source https://mirror2.chabokan.net/nuget/v3/index.json
```

#### حذف منبع پیش‌فرض

```bash
nuget sources remove -name nuget.org
```

#### مشاهده منابع

```bash
nuget sources list
```

#### بازیابی پکیج‌ها

```bash
nuget restore packages.config -Source https://mirror-nuget.runflare.com/v3/index.json -PackagesDirectory ./packages
```

---

### 3) تنظیم با فایل `NuGet.Config`

#### مسیر فایل

- **Windows:** `%APPDATA%\NuGet\NuGet.Config`
- **Linux/macOS:** `~/.nuget/NuGet/NuGet.Config`
- **Linux/macOS` (جایگزین رایج):** `~/.config/NuGet/NuGet.Config`
- **سطح پروژه:** کنار فایل `.sln` یا `.csproj`

#### نمونه: فقط یک میرور

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="kargadan" value="https://mirror.kargadan.ir/repository/nuget-group/index.json" />
  </packageSources>
</configuration>
```

#### نمونه: کرگدن + `nuget.org`

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="kargadan" value="https://mirror.kargadan.ir/repository/nuget-group/index.json" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

#### نمونه: با `packageSourceMapping`

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="kargadan" value="https://mirror.kargadan.ir/repository/nuget-group/index.json" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>

  <packageSourceMapping>
    <packageSource key="kargadan">
      <package pattern="*" />
    </packageSource>
  </packageSourceMapping>
</configuration>
```

#### نمونه: پردیسکو

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="PardisMirror" value="https://mirrors.pardisco.co/nugetv3/index.json" />
  </packageSources>
</configuration>
```

#### نمونه: جمکو

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="chrepo" value="https://nuget.chrepo.ir" />
  </packageSources>
</configuration>
```

#### نمونه: مگان

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="megan" value="https://hub.megan.ir/nuget/index.json" />
  </packageSources>
</configuration>
```

---

### 4) تنظیم در Visual Studio

1. وارد **Tools → NuGet Package Manager → Package Manager Settings** شوید.
2. **Package Sources** را انتخاب کنید.
3. روی **+** بزنید.
4. مقادیر را وارد کنید:
   - **Name:** `Kargadan Mirror`
   - **Source:** `https://mirror.kargadan.ir/repository/nuget-group/index.json`
5. در صورت نیاز منبع را به بالای لیست ببرید.
6. **Update** و سپس **OK** را بزنید.

برای مگان:
- **Name:** `Megan`
- **Source:** `https://hub.megan.ir/nuget/index.json`

---

### 5) تنظیم در Rider و Visual Studio for Mac

- از فایل `NuGet.Config` استفاده کنید
- یا از مسیر **Settings → Build, Execution, Deployment → NuGet** منبع را اضافه کنید

---

## بررسی تنظیمات

### بررسی منابع ثبت‌شده

```bash
dotnet nuget list source
```

یا:

```bash
nuget sources list
```

### تست با ساخت پروژه نمونه

```bash
dotnet new console -n TestApp
cd TestApp
dotnet add package Newtonsoft.Json --source https://mirrors.pardisco.co/nugetv3/index.json -v 13.0.3
```

اگر پکیج با موفقیت دریافت شد، تنظیمات درست است.

---

## بازگشت به تنظیمات اصلی

### حذف یک میرور

```bash
dotnet nuget remove source kargadan
```

یا:

```bash
nuget sources remove -name KargadanMirror
```

### بازگرداندن `nuget.org`

```bash
dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org
```

### استفاده موقت از `nuget.org`

```bash
dotnet add package <package-name> --source https://api.nuget.org/v3/index.json
```

---

## استفاده در Docker

اگر پروژه شما از imageهای `.NET` استفاده می‌کند، می‌توانید هم‌زمان از رجیستری Docker و میرور NuGet استفاده کنید.

### نمونه Dockerfile با کرگدن

```dockerfile
FROM docker-mcr-mirror.kargadan.ir/dotnet/sdk:9.0 AS build
WORKDIR /src

COPY NuGet.Config ./
COPY *.csproj ./
RUN dotnet restore

COPY . .
RUN dotnet publish -c Release -o /app

FROM docker-mcr-mirror.kargadan.ir/dotnet/aspnet:9.0
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

---

## استفاده در CI/CD

### GitHub Actions

```yaml
name: .NET Build
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '9.0.x'
      - run: dotnet nuget add source https://mirror.kargadan.ir/repository/nuget-group/index.json -n kargadan
      - run: dotnet restore
      - run: dotnet build --no-restore
      - run: dotnet test --no-build
```

### Azure DevOps

```yaml
trigger: [main]
pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: UseDotNet@2
    inputs:
      version: '9.0.x'
  - script: dotnet nuget add source https://mirror.kargadan.ir/repository/nuget-group/index.json -n kargadan
    displayName: 'Configure NuGet mirror'
  - script: dotnet restore
  - script: dotnet build --configuration Release
```

---

## اطلاعات میرور کرگدن

| مورد | مقدار |
| --- | --- |
| نوع میرور | Group Aggregated |
| منابع بالادستی | Chabokan، Pardisco، Jamko، Runflare، NuGet.org |
| نسخه Feed | NuGet v3 |
| پروتکل‌ها | HTTPS / HTTP |
| احراز هویت | نیاز ندارد |

---

## نکات

- برای پروژه‌های تیمی، بهتر است فایل `NuGet.Config` را در ریشه solution commit کنید تا همه توسعه‌دهندگان و CI از یک تنظیم یکسان استفاده کنند.
- اگر می‌خواهید فقط از یک میرور استفاده شود، از `<clear />` در `NuGet.Config` استفاده کنید.
- رانفلر روی پلن رایگان محدودیت دارد.
- کرگدن از چند upstream استفاده می‌کند و گزینه مناسبی برای استفاده عمومی است.
- اگر یک پکیج در میرور cache نشده باشد، بعضی میرورها آن را از upstream دریافت می‌کنند.
- برای بهترین سرعت، از تنظیم دائمی به‌جای `--source` موقت استفاده کنید.

## عیب‌یابی

### پکیج‌ها هنوز از `nuget.org` دانلود می‌شوند

فایل `NuGet.Config` را بررسی کنید و مطمئن شوید:

```xml
<packageSources>
  <clear />
  <add key="kargadan" value="https://mirror.kargadan.ir/repository/nuget-group/index.json" />
</packageSources>
```

### تنظیمات اعمال نمی‌شود

کش را پاک کنید:

```bash
dotnet nuget locals all --clear
```

سپس دوباره restore بزنید:

```bash
dotnet restore
```

### در Visual Studio منبع دیده نمی‌شود

Visual Studio را ببندید و دوباره باز کنید، یا از بخش Package Sources دوباره منبع را اضافه کنید.

### در CI/CD خطای دسترسی دارید

بهتر است `NuGet.Config` را داخل repository قرار دهید تا runner دقیقاً همان تنظیمات را استفاده کند.