# Veritabani Down

Bu runbook, veritabani erisilemez oldugunda temel mudahale adimlarini icerir.

## Hazirlik
- Etkilenen servisleri ve bagimliliklari belirle.
- Son degisiklikleri not et.

## Mudahale adimlari
1. Servis durumu kontrol et.
2. Disk ve bellek kullanimini kontrol et.
3. Baglanti limitlerini ve loglari incele.
4. Gerekirse servisi yeniden baslat.

## Ornek komutlar
```bash
systemctl status postgresql
journalctl -u postgresql --since "30 min ago"
free -m
df -h
systemctl restart postgresql
```

## Dogrulama
- Uygulama DB baglantisi duzeldi mi?
- Hata oranlari normale dondu mu?

