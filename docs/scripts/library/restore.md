# Database Restore

Veritabanı yedeklerinden geri yükleme işlemi.

## Kullanım

```bash
./scripts/restore-db.sh <backup_file.sql.gz>
```

## Prosedür

1. **Yedek Doğrulama:** Dosya bütünlüğü kontrol edilir.
2. **Servis Durdurma:** Veri tutarlılığı için uygulama durdurulur (opsiyonel).
3. **Restore:**
   - Mevcut veritabanı silinir (`DROP DATABASE`)
   - Yeni veritabanı oluşturulur
   - Dump import edilir
4. **Kontrol:** Tablo sayıları ve boyutlar raporlanır.

> [!WARNING]
> Bu işlem mevcut verileri silecektir!

```bash
# Örnek
./scripts/restore-db.sh backups/daily-2024-01-01.sql.gz
```
