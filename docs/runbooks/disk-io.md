# Disk IO Krizi

Bu runbook, disk IO saturation durumunda teshis ve azaltma adimlarini kapsar.

## Hazirlik
- IO darbogazini dogrula.
- Etkilenen servisleri listele.

## Mudahale adimlari
1. IO kullanimini kontrol et.
2. En cok IO yapan surecleri belirle.
3. Gerekirse servisi gecici durdur veya limit uygula.

## Ornek komutlar
```bash
iostat -xz 1 5
pidstat -d 1 5
systemctl stop batch-job
```

## Dogrulama
- IO bekleme sureleri dustu mu?
- Servisler stabil mi?

