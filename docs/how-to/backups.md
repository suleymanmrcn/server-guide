# Yedekleme Stratejisi ve Otomasyonu

"Yedek yoksa, veri yoktur."

## 1. 3-2-1 Kuralı

Basit ama hayat kurtarır:

- **3** kopya veriniz olsun (1 canlı, 2 yedek).
- **2** farklı medya kullanın (Sunucu diski + Harici Depolama).
- **1** kopya mutlaka "Off-site" (Sunucu dışında) olsun.

## 2. Otomatik Yedekleme Scripti

Aşağıdaki script, belirlediğiniz klasörleri ve MySQL veritabanını sıkıştırıp tarihli olarak `/backup` dizinine atar. Ayrıca eski yedekleri (7 günden yaşlı) otomatik temizler.

Dosya konumu: `/root/scripts/daily-backup.sh`

```bash
#!/bin/bash
set -e

# --- AYARLAR ---
BACKUP_DIR="/backups"
DIRS_TO_BACKUP="/etc /var/www /home/deployer"
DB_NAME="myapp_db"
DB_USER="backup_user"
DB_PASS="gizli_sifre"
DATE=$(date +%F_%H-%M)
# ---------------

mkdir -p $BACKUP_DIR

echo "📦 [$DATE] Dosya yedegi aliniyor..."
tar -czf "$BACKUP_DIR/files_$DATE.tar.gz" $DIRS_TO_BACKUP

echo "🗄️ [$DATE] Veritabani yedegi aliniyor..."
mysqldump -u $DB_USER -p"$DB_PASS" $DB_NAME | gzip > "$BACKUP_DIR/db_${DB_NAME}_$DATE.sql.gz"

echo "🧹 Eski yedekler temizleniyor (7 gunden eski)..."
find $BACKUP_DIR -type f -name "*.gz" -mtime +7 -delete

echo "✅ Yedekleme tamamlandi: $BACKUP_DIR"
# Buraya rclone sync veya s3 upload komutu eklenebilir.
```

## 3. Zamanlama (Cron)

Scripti her gece 03:00'da çalıştırmak için:

```bash
crontab -e
```

Eklenecek satır:

```cron
0 3 * * * /bin/bash /root/scripts/daily-backup.sh >> /var/log/backup.log 2>&1
```

## 4. Off-Site Transfer (Rclone)

Yedekleri sunucuda tutmak yetmez (Sunucu yanarsa yedekler de yanar). `rclone` kullanarak S3, Google Drive veya Dropbox'a atın.

```bash
# Rclone kurulumu ve konfigürasyonu
apt install rclone
rclone config # (Sihirbazı takip edin)

# Scriptin sonuna eklenecek komut:
rclone copy $BACKUP_DIR/ remote:my-server-backups/ --progress
```
