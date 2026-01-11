# Network Latency Spike

Bu runbook, ag gecikmesi artisinda teshis ve azaltma adimlarini kapsar.

## Hazirlik
- Etkilenen route ve servisleri belirle.

## Mudahale adimlari
1. Temel latency olcumu yap.
2. Packet loss kontrol et.
3. Kritik servislere oncelik ver.

## Ornek komutlar
```bash
ping -c 20 8.8.8.8
mtr -rw example.com
```

## Dogrulama
- Latency normale dondu mu?
- Packet loss azaldi mi?

