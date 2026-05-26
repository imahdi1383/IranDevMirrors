# میرور Ubuntu – راهنمای تنظیم در ایران

## میرورهای ایرانی موجود

| میرور | آدرس | وضعیت |
|-------|------|-------|
| **کرگدن** | `https://mirror.kargadan.ir/repository/ubuntu-proxy/` | ✅ فعال |
| **رانفلر** | `http://mirror-linux.runflare.com/ubuntu` | ⚠️ محدودیت در پلن رایگان |
| **آروان کلاد** | `http://mirror.arvancloud.ir/ubuntu` | ✅ فعال |
| **پارس‌پک** | `http://mirror.parspack.net/ubuntu` | ✅ فعال |

---

## روش ۱: تنظیم خودکار (توصیه می‌شود)

### کرگدن (Kargadan)

```bash
sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-proxy $(lsb_release -cs) main restricted universe multiverse
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-proxy $(lsb_release -cs)-updates main restricted universe multiverse
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-proxy $(lsb_release -cs)-backports main restricted universe multiverse
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-security-proxy $(lsb_release -cs)-security main restricted universe multiverse
EOF
sudo apt update
```
### رانفلر (Runflare)

```bash
UBUNTU_CODENAME=$(lsb_release -cs) && sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror-linux.runflare.com/ubuntu $UBUNTU_CODENAME main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu $UBUNTU_CODENAME-updates main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu $UBUNTU_CODENAME-backports main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu $UBUNTU_CODENAME-security main restricted universe multiverse
EOF
sudo apt update
```
### آروان کلاد (ArvanCloud)

```bash
UBUNTU_CODENAME=$(lsb_release -cs) && sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror.arvancloud.ir/ubuntu $UBUNTU_CODENAME main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu $UBUNTU_CODENAME-updates main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu $UBUNTU_CODENAME-backports main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu $UBUNTU_CODENAME-security main restricted universe multiverse
EOF
sudo apt update
```
### پارس‌پک (ParsPack)

```bash
UBUNTU_CODENAME=$(lsb_release -cs) && sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://mirror.parspack.net/ubuntu $UBUNTU_CODENAME main restricted universe multiverse
deb http://mirror.parspack.net/ubuntu $UBUNTU_CODENAME-updates main restricted universe multiverse
deb http://mirror.parspack.net/ubuntu $UBUNTU_CODENAME-backports main restricted universe multiverse
deb http://mirror.parspack.net/ubuntu $UBUNTU_CODENAME-security main restricted universe multiverse
EOF
sudo apt update
```
---

## روش ۲: تنظیم دستی از طریق فایل

### ۱. ویرایش فایل sources.list

```bash
sudo nano /etc/apt/sources.list
```
### ۲. جایگزینی محتوا
```
**برای کرگدن:**

```bash
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-proxy noble main restricted universe multiverse
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-proxy noble-updates main restricted universe multiverse
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-proxy noble-backports main restricted universe multiverse
deb [trusted=yes] https://mirror.kargadan.ir/repository/ubuntu-security-proxy noble-security main restricted universe multiverse
```
**برای رانفلر:**

```bash
deb http://mirror-linux.runflare.com/ubuntu noble main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu noble-updates main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu noble-backports main restricted universe multiverse
deb http://mirror-linux.runflare.com/ubuntu noble-security main restricted universe multiverse
```
**برای آروان کلاد:**

```bash
deb http://mirror.arvancloud.ir/ubuntu noble main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu noble-updates main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu noble-backports main restricted universe multiverse
deb http://mirror.arvancloud.ir/ubuntu noble-security main restricted universe multiverse
```
**برای پارس‌پک:**

```bash
deb http://mirror.parspack.net/ubuntu noble main restricted universe multiverse
deb http://mirror.parspack.net/ubuntu noble-updates main restricted universe multiverse
deb http://mirror.parspack.net/ubuntu noble-backports main restricted universe multiverse
deb http://mirror.parspack.net/ubuntu noble-security main restricted universe multiverse
```
> **نکته:** `noble` نام کدی Ubuntu 24.04 LTS است. برای نسخه‌های دیگر از `jammy` (22.04)، `focal` (20.04) یا خروجی دستور `lsb_release -cs` استفاده کنید.

### ۳. ذخیره و خروج

- در nano: `Ctrl+O` → `Enter` → `Ctrl+X`

### ۴. به‌روزرسانی

```bash
sudo apt update
```
---

## بررسی تنظیمات

```bash
apt-cache policy
```
خروجی باید میرور ایرانی انتخابی شما را نشان دهد.

---

## بازگشت به تنظیمات اصلی

```bash
sudo tee /etc/apt/sources.list > /dev/null <<EOF
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs) main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu $(lsb_release -cs)-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu $(lsb_release -cs)-security main restricted universe multiverse
EOF
sudo apt update
```
---

## عیب‌یابی

### خطای GPG/Key

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <KEY_ID>
```
یا برای کرگدن که از `[trusted=yes]` استفاده می‌کند، نیازی به کلید نیست.

### خطای 404 Not Found

- نسخه Ubuntu خود را بررسی کنید: `lsb_release -a`
- مطمئن شوید میرور از نسخه شما پشتیبانی می‌کند
- نام کدی صحیح را جایگزین کنید

### سرعت پایین

میرور دیگری را امتحان کنید یا از چند میرور به صورت ترکیبی استفاده کنید.
