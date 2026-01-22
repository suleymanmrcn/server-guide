# Sistem Yönetimi - Kurulum & Konfigürasyon ⚙️

Bu bölüm, Linux sunucunuzu sıfırdan production-ready hale getirmek için gereken **adım adım kurulum rehberlerini** içerir.

---

## 🎯 Bu Bölümde Neler Var?

### 🖥️ Temel Sistem

Sunucunuzun temelini oluşturan core ayarlar:

- **[Temel OS](base-os.md)** - İlk kurulum sonrası yapılması gerekenler
- **[Kullanıcı Yönetimi](user-management.md)** - Kullanıcı oluşturma, sudo, SSH key
- **[Shell Konfigürasyonu](shell-config.md)** - `.bashrc`, alias, PATH yönetimi
- **[Dosya İzinleri](file-permissions.md)** - chmod, chown, umask, ACL
- **[Paket Yönetimi](package-management.md)** - apt, yum, snap kullanımı
- **[Swap Bellek](swap.md)** - Swap alanı oluşturma ve optimizasyon

### 🏗️ Altyapı Servisleri

Production ortamı için gerekli servisler:

- **[Docker & Storage](docker.md)** - Docker kurulumu ve disk yönetimi
- **[Nginx](nginx.md)** - Web sunucusu kurulumu
- **[Reverse Proxy](reverse-proxy.md)** - Nginx ile reverse proxy yapılandırması
- **[TLS](tls.md)** - SSL/TLS sertifika yönetimi (Let's Encrypt)

### ⚙️ Otomasyon

Zamanlanmış görevler ve servis yönetimi:

- **[Systemd Servis](systemd-service.md)** - Systemd ile servis tanımlama
- **[Zamanlanmış Görevler (Cron)](cron.md)** - Cron, systemd timers, anacron, at

### 🔧 Bakım & İzleme

Sistem sağlığı ve veri güvenliği:

- **[Monitoring](monitoring.md)** - Sistem izleme araçları
- **[Monitoring Stack](monitoring-stack.md)** - Prometheus + Grafana kurulumu
- **[Logrotate](logrotate.md)** - Log dosyası rotasyonu
- **[Yedekleme (Backup)](backups.md)** - Yedekleme stratejileri
- **[Sistem Snapshot (Timeshift)](snapshots.md)** - Sistem anlık görüntüleri
- **[Sistem Klonlama (Tar)](system-clone.md)** - Sunucu klonlama

### 🌐 Network

Ağ ve port yönetimi:

- **[Ağ/Port Kontrolleri](port-checks.md)** - Port dinleme, network troubleshooting

---

## 📖 Nasıl Kullanılır?

1. **Yeni Sunucu:** Yukarıdan aşağıya sırayla ilerleyin (Temel Sistem → Altyapı → Otomasyon → Bakım)
2. **Belirli Bir Konu:** Sol menüden ilgili sayfaya direkt gidin
3. **Hızlı Referans:** Her sayfada kopyala-yapıştır hazır komutlar bulacaksınız

---

## 🚀 Hızlı Başlangıç

Yeni bir sunucu kuruyorsanız, şu sırayı takip edin:

1. ✅ [Temel OS](base-os.md) - Sistem güncellemeleri, timezone, hostname
2. ✅ [Kullanıcı Yönetimi](user-management.md) - Sudo kullanıcısı oluştur
3. ✅ [Shell Konfigürasyonu](shell-config.md) - Alias ve PATH ayarla
4. ✅ [Dosya İzinleri](file-permissions.md) - İzin yönetimini öğren
5. ✅ [Docker](docker.md) - Container altyapısını kur
6. ✅ [Nginx](nginx.md) - Web sunucusunu kur
7. ✅ [TLS](tls.md) - SSL sertifikası al
8. ✅ [Monitoring Stack](monitoring-stack.md) - İzleme sistemini kur
9. ✅ [Cron](cron.md) - Otomasyonu ayarla
10. ✅ [Yedekleme](backups.md) - Backup stratejisi oluştur

> [!TIP] > **Güvenlik:** Kurulum tamamlandıktan sonra [Güvenlik](../security/index.md) bölümüne geçerek sunucunuzu sertleştirin!

---

## 🔗 İlgili Bölümler

- **[Güvenlik](../security/index.md)** - Sunucunuzu koruma altına alın
- **[Şablonlar](../file-templates/postgres.md)** - Hazır konfigürasyon dosyaları
- **[Scriptler](../scripts/index.md)** - Otomasyon scriptleri
- **[Kontrol Listeleri](../checklists/server-first-setup.md)** - İlk kurulum checklist
