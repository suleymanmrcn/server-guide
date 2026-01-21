# Temel OS Kurulumu

Bu bolum, Ubuntu/Debian icin temel kurulum ve ilk ayarlari kapsar.

## Kurulum

- Minimal kurulum sec.
- Disk bolumleme planini belirle.
- Root yerine yonetici kullanici olustur.

## Ilk giris

```bash
ssh root@SUNUCU_IP
```

## Yonetici kullanici ve sudo

```bash
adduser deploy
usermod -aG sudo deploy
```

## Hostname ve /etc/hosts

```bash
hostnamectl set-hostname ornek-sunucu
```

`/etc/hosts` icine IP ve hostname eslemesini ekle.

## Zaman senkronizasyonu

```bash
timedatectl set-timezone Europe/Istanbul
systemctl enable --now systemd-timesyncd
```

## Temel guncellemeler

```bash
apt update
apt -y upgrade
```

## 5. Temel Araçlar (Essentials) 🧰

Temiz bir sunucu kurulumundan sonra, hayatınızı kolaylaştıracak şu araçları mutlaka kurmalısınız:

```bash
sudo apt install -y curl wget git htop btop jq dialog net-tools
```

### Neden Gerekli?

- **`dialog`:** Terminalde görsel menüler ve popup pencereler oluşturmak için (Kurulum scriptlerinin dostudur).
- **`jq`:** Komut satırında JSON verilerini okumak ve filtrelemek için (API testleri ve konfigürasyon için hayat kurtarır).
- **`htop` / `btop`:** Sistem kaynaklarını (CPU/RAM) görsel olarak izlemek için.
- **`git`:** Kod ve konfigürasyon yönetimi için.
- **`curl` / `wget`:** Dosya indirme ve API istekleri için.

## Dogrulama

- `deploy` kullanicisi ile SSH girisi yapilabiliyor mu?
- Sistem guncel mi?
- `hostnamectl` ve `timedatectl status` dogru mu?
