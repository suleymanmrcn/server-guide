# DDoS / Rate Limit

Bu runbook, ani trafik artisi ve rate limit ihtiyacini kapsar.

## Hazirlik
- Trafik kaynagini ve hedef endpointleri belirle.

## Mudahale adimlari
1. Trafik kaynagini analiz et.
2. Rate limit veya WAF kurallarini uygula.
3. Gerekirse gecici IP bloklama yap.

## Ornek komutlar
```bash
sudo ufw limit ssh
sudo ufw deny from 203.0.113.0/24
```

## Dogrulama
- Trafik stabil mi?
- Legit istekler etkilenmedi mi?

