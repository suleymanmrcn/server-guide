# OOM Kill / Memory Leak

Bu runbook, bellek tukenmesi ve OOM kill durumlarini kapsar.

## Hazirlik
- Etkilenen servisleri belirle.
- Son degisiklikleri not et.

## Mudahale adimlari
1. Bellek kullanimini kontrol et.
2. OOM loglarini incele.
3. Gerekirse servisi yeniden baslat veya kaynak limit uygula.

## Ornek komutlar
```bash
free -m
journalctl -k | rg -i "oom|out of memory"
systemctl restart app
```

## Dogrulama
- Bellek kullanimi normale dondu mu?
- OOM kill tekrar etti mi?

