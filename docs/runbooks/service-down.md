# Servis Kesintisi

Bu runbook, servis down durumunda temel teshis ve geri getirme adimlarini icerir.

## Hazirlik
- Etkilenen servis ve bagimli servisleri belirle.
- Son degisiklikleri not et.

## Mudahale adimlari
1. Servis durumunu kontrol et.
2. Loglari incele.
3. Kaynak kullanimini kontrol et.
4. Gerekirse servisi yeniden baslat.

## Ornek komutlar
```bash
systemctl status app
journalctl -u app --since "30 min ago"
free -m
systemctl restart app
```

## Dogrulama
- Saglik kontrolu basarili mi?
- Hata oranlari normale dondu mu?

