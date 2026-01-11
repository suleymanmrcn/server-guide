# CPU Spike

Bu runbook, CPU kullanimi aniden arttiginda teshis ve stabilizasyonu kapsar.

## Hazirlik
- Etkilenen host ve servisleri belirle.
- Zaman araligini not et.

## Mudahale adimlari
1. CPU kullanimini ve en cok kaynak tuketen surecleri belirle.
2. Son degisiklikleri kontrol et.
3. Gerekirse servisleri yeniden baslat veya limit uygula.

## Ornek komutlar
```bash
top -o %CPU
ps -eo pid,comm,%cpu --sort=-%cpu | head
systemctl restart app
```

## Dogrulama
- CPU kullanimi normale dondu mu?
- Alarm kapanis oldu mu?

