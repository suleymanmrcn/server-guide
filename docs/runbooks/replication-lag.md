# Replication Lag

Bu runbook, veritabani replikasyon gecikmesi durumunu kapsar.

## Hazirlik
- Primary/replica rollerini belirle.

## Mudahale adimlari
1. Replikasyon durumunu kontrol et.
2. Disk ve IO durumunu incele.
3. Gerekirse replica'yi yeniden senkronize et.

## Ornek komutlar
```bash
# Postgres ornegi
psql -c "select now() - pg_last_xact_replay_timestamp();"
```

## Dogrulama
- Lag kabul edilebilir seviyede mi?

