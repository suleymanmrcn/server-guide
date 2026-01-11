# Guvenlik Guncellemeleri

Bu bolum, otomatik guvenlik guncellemelerini etkinlestirir.

## Gereksinimler
- Root veya sudo yetkisi

## Kurulum
```bash
apt update
apt install -y unattended-upgrades
```

## Konfigurasyon
```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

## Bakim notlari
- Ayda bir planli guncelleme penceresi belirle.
- Kritik yamalar icin acil prosedur tanimla.
- Eski paketleri periyodik temizle:
```bash
apt autoremove -y
```

## Dogrulama
- `/etc/apt/apt.conf.d/20auto-upgrades` dosyasi olustu mu?
- Guncelleme zamanlayicisi aktif mi?
