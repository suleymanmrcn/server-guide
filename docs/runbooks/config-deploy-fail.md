# Config Deploy Hatasi

Bu runbook, config veya env hatasindan kaynakli deploy sorunlarini kapsar.

## Hazirlik
- Son degisiklikleri not et.
- Hangi config'in deploy edildigini belirle.

## Mudahale adimlari
1. Config diff kontrol et.
2. Eksik env degiskenlerini tamamla.
3. Gerekirse onceki config'e don.

## Ornek komutlar
```bash
env | rg "APP_"
```

## Dogrulama
- Uygulama dogru config ile ayakta mi?

