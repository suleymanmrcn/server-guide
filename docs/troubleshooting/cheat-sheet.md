# Hızlı Teşhis (Cheat Sheet)

Sunucu yavaşladığında veya çöktüğünde paniklemeden kullanabileceğiniz komutlar rehberi.

## 🚨 Yüksek Yük (CPU/RAM)

Sunucu çok yavaşsa:

| Komut                | Açıklama                                              |
| :------------------- | :---------------------------------------------------- | ------------------------------------------------------------------ |
| `htop`               | CPU ve RAM kullanımını renkli ve interaktif gösterir. |
| `uptime`             | Load average değerlerini gösterir (1, 5, 15 dk).      |
| `dmesg               | tail`                                                 | Kernel hatalarını (OOM Kill, disk hatası) son satırlarda gösterir. |
| `free -h`            | RAM kullanımını insan okunabilir formatta gösterir.   |
| `ps aux --sort=-%mem | head -10`                                             | En çok RAM yiyen 10 işlemi listeler.                               |

## 🌐 Ağ ve Bağlantı (Network)

Bağlantı sorunları veya saldırı şüphesinde:

| Komut               | Açıklama                                                     |
| :------------------ | :----------------------------------------------------------- |
| `ss -tulpn`         | Hangi portların dinlendiğini (listening) gösterir.           |
| `uallow` (ufw)      | `ufw status verbose` ile firewall kurallarını kontrol et.    |
| `iftop`             | Anlık ağ trafiğini (kim kime ne kadar veri atıyor) gösterir. |
| `curl -I localhost` | Yerel web sunucusunun yanıt verip vermediğini test eder.     |
| `ping 8.8.8.8`      | Sunucunun internete çıkışı var mı?                           |

## 📝 Disk ve Dosya Sistemi

"No space left on device" hatası alıyorsanız:

| Komut      | Açıklama                                                         |
| :--------- | :--------------------------------------------------------------- | ----- | ----------------------------------------------------------------- |
| `df -h`    | Disk doluluk oranlarını gösterir.                                |
| `df -i`    | Inode doluluk oranlarını gösterir (Çok küçük dosya varsa dolar). |
| `du -sh \* | sort -hr                                                         | head` | Klasör boyutlarını büyükten küçüğe sıralar (Suçluyu bulmak için). |

## 📜 Loglar (Günlükler)

Son 1 saatte ne oldu?

```bash
# Nginx Hataları
tail -f /var/log/nginx/error.log

# Sistem Logları (Systemd) - Son 1 saat, Kırmızı hatalar
journalctl -p 3 -xb --since "1 hour ago"

# SSH Giriş Denemeleri
grep "Failed password" /var/log/auth.log | tail -n 20
```

## 🔁 Servis Yönetimi

```bash
systemctl status servis-adi   # Durum
systemctl restart servis-adi  # Yeniden Başlat
systemctl stop servis-adi     # Durdur
systemctl enable servis-adi   # Açılışta Başlat
```
