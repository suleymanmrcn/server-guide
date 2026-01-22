# Systemd Service Template (Production Ready) ⚙️

Systemd, modern Linux dağıtımlarında servisleri yöneten ana sistemdir. Bu rehber, production-grade systemd service dosyaları oluşturmanız için gereken her şeyi içerir.

---

## 📋 İçindekiler

1. [Temel Yapı](#temel-yapi)
2. [Unit Section](#unit-section)
3. [Service Section](#service-section)
4. [Install Section](#install-section)
5. [Gelişmiş Örnekler](#gelismis-ornekler)
6. [Troubleshooting](#troubleshooting)

---

## 🏗️ Temel Yapı {#temel-yapi}

```ini
[Unit]
Description=My Awesome Node.js App
Documentation=https://example.com
After=network.target

[Service]
User=deployer
Group=deployer
Environment=NODE_ENV=production
Environment=PORT=3000
WorkingDirectory=/home/deployer/app
ExecStart=/usr/bin/node /home/deployer/app/server.js
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### Hızlı Başlangıç Komutları

```bash
# Dosyayı oluştur
sudo nano /etc/systemd/system/myapp.service

# Systemd'yi yeniden yükle
sudo systemctl daemon-reload

# Servisi aktif et ve başlat
sudo systemctl enable --now myapp

# Durum kontrolü
sudo systemctl status myapp
```

---

## 📦 Unit Section {#unit-section}

### Temel Direktifler

```ini
[Unit]
# Servis açıklaması (systemctl status'da görünür)
Description=My Awesome Node.js App

# Dokümantasyon linkleri
Documentation=https://example.com
Documentation=man:myapp(8)

# Bu servisler başladıktan SONRA başla
After=network.target postgresql.service redis.service

# Bu servisler başlamadan ÖNCE başla
Before=nginx.service

# Zorunlu bağımlılıklar (bunlar başarısız olursa bu da başlamaz)
Requires=postgresql.service

# Opsiyonel bağımlılıklar (bunlar başarısız olsa da başlar)
Wants=redis.service

# Çakışan servisler (aynı anda çalışamaz)
Conflicts=apache2.service

# Servis koşulu (dosya varsa başla)
ConditionPathExists=/home/deployer/app/server.js

# Servis koşulu (dizin varsa başla)
ConditionDirectoryNotEmpty=/home/deployer/app
```

### Dependency Örnekleri

**Veritabanı gerektiren uygulama:**

```ini
[Unit]
Description=Backend API Server
After=network.target postgresql.service
Requires=postgresql.service
Wants=redis.service
```

**Ağ servisi bekleyen uygulama:**

```ini
[Unit]
Description=Web Application
After=network-online.target
Wants=network-online.target
```

---

## ⚙️ Service Section {#service-section}

### Type Direktifi

```ini
[Service]
# simple (varsayılan): ExecStart hemen başlar
Type=simple

# forking: Daemon gibi fork yapan uygulamalar için
Type=forking
PIDFile=/var/run/myapp.pid

# oneshot: Bir kez çalışıp biten scriptler için
Type=oneshot
RemainAfterExit=yes

# notify: Hazır olduğunda systemd'ye sinyal gönderen uygulamalar
Type=notify

# exec: ExecStart binary'si çalıştırıldığında başarılı sayılır
Type=exec
```

### Kullanıcı ve Grup

```ini
[Service]
# Çalıştıracak kullanıcı
User=deployer
Group=deployer

# Dinamik kullanıcı (geçici, izole)
DynamicUser=yes

# Ek gruplar
SupplementaryGroups=docker ssl-cert
```

### Çevresel Değişkenler

```ini
[Service]
# Tek tek tanımlama
Environment=NODE_ENV=production
Environment=PORT=3000
Environment=DB_HOST=localhost

# Dosyadan okuma
EnvironmentFile=/etc/myapp/environment
EnvironmentFile=-/etc/myapp/environment.local  # "-" = opsiyonel

# Tüm environment'ı temizle
UnsetEnvironment=HOME
```

**Environment dosyası örneği** (`/etc/myapp/environment`):

```bash
NODE_ENV=production
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=myapp_user
DB_PASS=supersecret
JWT_SECRET=my-jwt-secret
```

### Çalışma Dizini ve Komutlar

```ini
[Service]
# Çalışma dizini
WorkingDirectory=/home/deployer/app

# Ana komut
ExecStart=/usr/bin/node server.js

# Başlamadan önce çalışacak komutlar
ExecStartPre=/usr/bin/npm install
ExecStartPre=/bin/mkdir -p /var/log/myapp

# Başladıktan sonra çalışacak komutlar
ExecStartPost=/bin/echo "Service started" >> /var/log/myapp/events.log

# Yeniden yükleme komutu (SIGHUP yerine)
ExecReload=/bin/kill -HUP $MAINPID

# Durma komutu (özel graceful shutdown)
ExecStop=/usr/bin/node /home/deployer/app/scripts/shutdown.js

# Durduktan sonra temizlik
ExecStopPost=/bin/rm -f /var/run/myapp.pid
```

### Restart Politikaları

```ini
[Service]
# Restart seçenekleri:
# no          : Asla restart etme
# always      : Her zaman restart et
# on-success  : Sadece başarılı çıkışta (exit code 0)
# on-failure  : Sadece başarısız çıkışta
# on-abnormal : Sinyal veya timeout durumunda
# on-abort    : Yakalanmamış sinyal durumunda
# on-watchdog : Watchdog timeout durumunda

Restart=on-failure

# Restart öncesi bekleme süresi
RestartSec=10

# Maksimum restart denemesi (30 saniye içinde 5 deneme)
StartLimitBurst=5
StartLimitIntervalSec=30

# Başarılı sayılması için gereken çalışma süresi
RestartPreventExitStatus=0
SuccessExitStatus=143
```

### Timeout Ayarları

```ini
[Service]
# Başlama timeout'u
TimeoutStartSec=90

# Durma timeout'u
TimeoutStopSec=30

# Her ikisi için
TimeoutSec=60

# Watchdog (uygulama periyodik sinyal göndermeli)
WatchdogSec=30
```

### Kaynak Limitleri

```ini
[Service]
# CPU limiti (%100 = 1 core)
CPUQuota=200%

# Memory limiti
MemoryMax=512M
MemoryHigh=400M

# Dosya limitleri
LimitNOFILE=65535
LimitNPROC=4096

# Nice değeri (-20 ile 19 arası)
Nice=-5

# I/O önceliği
IOSchedulingClass=best-effort
IOSchedulingPriority=4
```

### Güvenlik Ayarları

```ini
[Service]
# Filesystem koruması
ProtectSystem=strict          # /usr, /boot, /efi read-only
ProtectHome=read-only         # /home, /root, /run/user read-only
PrivateTmp=yes                # İzole /tmp

# Sadece bu dizinlere yazabilir
ReadWritePaths=/var/lib/myapp /var/log/myapp

# Network koruması
PrivateNetwork=no             # yes = ağ erişimi yok
RestrictAddressFamilies=AF_INET AF_INET6 AF_UNIX

# Kernel koruması
ProtectKernelTunables=yes
ProtectKernelModules=yes
ProtectControlGroups=yes

# Capability kısıtlaması
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE

# Syscall filtreleme
SystemCallFilter=@system-service
SystemCallFilter=~@privileged @resources

# Diğer güvenlik
NoNewPrivileges=yes
PrivateDevices=yes
ProtectClock=yes
ProtectHostname=yes
```

### Logging

```ini
[Service]
# Stdout/Stderr yönlendirmesi
StandardOutput=journal
StandardError=journal

# Veya dosyaya
StandardOutput=append:/var/log/myapp/stdout.log
StandardError=append:/var/log/myapp/stderr.log

# Syslog identifier
SyslogIdentifier=myapp

# Log seviyesi
SyslogLevel=info
```

---

## 📥 Install Section {#install-section}

```ini
[Install]
# Multi-user seviyesinde başlat (genellikle bu kullanılır)
WantedBy=multi-user.target

# Grafik arayüzü ile başlat
WantedBy=graphical.target

# Alias tanımla
Alias=mywebapp.service

# Bu servis enable edilince bunları da enable et
Also=myapp-worker.service myapp-scheduler.service
```

---

## 🚀 Gelişmiş Örnekler {#gelismis-ornekler}

### 1. Production Node.js Uygulaması

```ini
[Unit]
Description=Production Node.js API Server
Documentation=https://docs.myapp.com
After=network-online.target postgresql.service redis.service
Wants=network-online.target
Requires=postgresql.service
Wants=redis.service

[Service]
Type=simple
User=nodeapp
Group=nodeapp

# Environment
Environment=NODE_ENV=production
EnvironmentFile=/etc/myapp/production.env
EnvironmentFile=-/etc/myapp/secrets.env

# Çalışma
WorkingDirectory=/opt/myapp
ExecStartPre=/usr/bin/npm run db:migrate
ExecStart=/usr/bin/node --max-old-space-size=4096 dist/server.js
ExecReload=/bin/kill -USR2 $MAINPID

# Restart
Restart=always
RestartSec=10
StartLimitBurst=5
StartLimitIntervalSec=60

# Timeouts
TimeoutStartSec=120
TimeoutStopSec=30

# Kaynaklar
MemoryMax=4G
CPUQuota=300%
LimitNOFILE=65535

# Güvenlik
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/opt/myapp/uploads /var/log/myapp

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=myapp-api

[Install]
WantedBy=multi-user.target
```

### 2. Python Django/Gunicorn Uygulaması

```ini
[Unit]
Description=Django Application with Gunicorn
After=network.target postgresql.service

[Service]
Type=notify
User=django
Group=www-data

WorkingDirectory=/var/www/mydjango
Environment=DJANGO_SETTINGS_MODULE=myproject.settings.production
EnvironmentFile=/etc/mydjango/env

ExecStart=/var/www/mydjango/venv/bin/gunicorn \
    --workers 4 \
    --worker-class uvicorn.workers.UvicornWorker \
    --bind unix:/run/mydjango/gunicorn.sock \
    --access-logfile /var/log/mydjango/access.log \
    --error-logfile /var/log/mydjango/error.log \
    --capture-output \
    myproject.asgi:application

ExecReload=/bin/kill -s HUP $MAINPID

Restart=on-failure
RestartSec=5
KillMode=mixed
TimeoutStopSec=30

PrivateTmp=yes
ProtectSystem=strict
ReadWritePaths=/var/www/mydjango/media /var/log/mydjango /run/mydjango

[Install]
[Install]
WantedBy=multi-user.target
```

### 3. .NET Core / ASP.NET Uygulaması

```ini
[Unit]
Description=ASP.NET Core Web API
After=network.target

[Service]
Type=notify
User=dotnet
Group=dotnet

WorkingDirectory=/opt/myapp
EnvironmentFile=/etc/myapp/appsettings.env

# Kestrel web server
ExecStart=/usr/bin/dotnet /opt/myapp/MyApp.dll

# Graceful shutdown
ExecStop=/bin/kill -SIGTERM $MAINPID
KillSignal=SIGTERM
TimeoutStopSec=30

# .NET için özel ayarlar
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=ASPNETCORE_URLS=http://localhost:5000

Restart=on-failure
RestartSec=10
TimeoutStartSec=90

# Memory limiti (.NET için)
MemoryMax=1G
Environment=DOTNET_GCHeapHardLimit=0x40000000

# Güvenlik
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/var/log/myapp

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=myapp-dotnet

[Install]
WantedBy=multi-user.target
```

### 4. PHP-FPM Uygulaması (Laravel/Symfony)

```ini
[Unit]
Description=Laravel Application (PHP-FPM)
After=network.target php8.2-fpm.service mysql.service
Requires=php8.2-fpm.service

[Service]
Type=oneshot
RemainAfterExit=yes
User=www-data
Group=www-data

WorkingDirectory=/var/www/laravel
EnvironmentFile=/var/www/laravel/.env

# Laravel için gerekli izinler
ExecStartPre=/bin/chown -R www-data:www-data /var/www/laravel/storage
ExecStartPre=/bin/chown -R www-data:www-data /var/www/laravel/bootstrap/cache

# Cache temizleme ve optimize etme
ExecStart=/usr/bin/php /var/www/laravel/artisan config:cache
ExecStart=/usr/bin/php /var/www/laravel/artisan route:cache
ExecStart=/usr/bin/php /var/www/laravel/artisan view:cache

# Queue worker için ayrı servis gerekir (laravel-queue.service)

[Install]
WantedBy=multi-user.target
```

**Laravel Queue Worker** (`laravel-queue.service`):

```ini
[Unit]
Description=Laravel Queue Worker
After=network.target mysql.service redis.service

[Service]
Type=simple
User=www-data
Group=www-data

WorkingDirectory=/var/www/laravel
EnvironmentFile=/var/www/laravel/.env

ExecStart=/usr/bin/php /var/www/laravel/artisan queue:work \
    --sleep=3 \
    --tries=3 \
    --max-time=3600 \
    --queue=default,emails,notifications

Restart=always
RestartSec=10

# Memory leak önleme
ExecReload=/bin/kill -USR1 $MAINPID
TimeoutStopSec=60

[Install]
WantedBy=multi-user.target
```

### 5. React/Vue/Angular (Production Build Serve)

**Option A: serve paketi ile:**

```ini
[Unit]
Description=React Production App (serve)
After=network.target

[Service]
Type=simple
User=webapp
Group=webapp

WorkingDirectory=/var/www/react-app
Environment=NODE_ENV=production
Environment=PORT=3000

# serve paketi ile static dosyaları servis et
ExecStart=/usr/bin/npx serve -s build -l 3000

Restart=always
RestartSec=5

# Güvenlik
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ReadWritePaths=/var/log/react-app

[Install]
WantedBy=multi-user.target
```

**Option B: http-server ile:**

```ini
[Unit]
Description=React App with http-server
After=network.target

[Service]
Type=simple
User=webapp
Group=webapp

WorkingDirectory=/var/www/react-app/build

ExecStart=/usr/bin/npx http-server \
    -p 3000 \
    -c-1 \
    --gzip \
    --cors

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

> [!TIP] > **Production'da React/Vue/Angular için en iyi yöntem:**
> Systemd ile serve etmek yerine **Nginx** ile static dosyaları servis edin. Çok daha hızlı ve güvenlidir:
>
> ```nginx
> server {
>     listen 80;
>     server_name myapp.com;
>     root /var/www/react-app/build;
>     index index.html;
>     location / {
>         try_files $uri $uri/ /index.html;
>     }
> }
> ```

### 6. Go Binary Uygulaması

```ini
[Unit]
Description=Go Microservice
After=network.target

[Service]
Type=exec
User=goapp
Group=goapp

EnvironmentFile=/etc/goapp/config.env

ExecStart=/opt/goapp/bin/myservice
Restart=always
RestartSec=5

# Go uygulamaları için memory limiti
MemoryMax=256M
Environment=GOMEMLIMIT=200MiB

# Güvenlik (Go binary için sıkı kısıtlamalar)
NoNewPrivileges=yes
PrivateTmp=yes
PrivateDevices=yes
ProtectSystem=strict
ProtectHome=yes
ProtectKernelTunables=yes
ProtectKernelModules=yes
ProtectControlGroups=yes
RestrictNamespaces=yes
RestrictRealtime=yes
RestrictSUIDSGID=yes
SystemCallFilter=@system-service
SystemCallErrorNumber=EPERM

[Install]
WantedBy=multi-user.target
```

### 4. Java Spring Boot Uygulaması

```ini
[Unit]
Description=Spring Boot Application
After=network.target

[Service]
Type=simple
User=spring
Group=spring

WorkingDirectory=/opt/springapp
EnvironmentFile=/etc/springapp/application.env

ExecStart=/usr/bin/java \
    -Xms512m \
    -Xmx2g \
    -XX:+UseG1GC \
    -XX:MaxGCPauseMillis=200 \
    -Djava.security.egd=file:/dev/./urandom \
    -Dspring.profiles.active=production \
    -jar /opt/springapp/app.jar

ExecStop=/bin/kill -TERM $MAINPID
SuccessExitStatus=143

Restart=on-failure
RestartSec=15
TimeoutStartSec=180
TimeoutStopSec=30

MemoryMax=2500M

[Install]
WantedBy=multi-user.target
```

### 5. Worker/Background Job Servisi

```ini
[Unit]
Description=Background Job Worker
After=network.target redis.service
Requires=redis.service
PartOf=myapp.service

[Service]
Type=simple
User=worker
Group=worker

WorkingDirectory=/opt/myapp
EnvironmentFile=/etc/myapp/worker.env

ExecStart=/opt/myapp/venv/bin/celery \
    -A myapp worker \
    --loglevel=info \
    --concurrency=4 \
    --queues=default,emails,reports

# Graceful shutdown için
ExecStop=/bin/kill -TERM $MAINPID
KillSignal=SIGTERM
TimeoutStopSec=60

Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### 6. Scheduled Task (Timer ile)

**Service dosyası** (`/etc/systemd/system/backup.service`):

```ini
[Unit]
Description=Daily Database Backup

[Service]
Type=oneshot
User=backup
Group=backup

ExecStart=/opt/scripts/backup.sh
StandardOutput=journal
StandardError=journal

# Oneshot için gerekirse
RemainAfterExit=no
```

**Timer dosyası** (`/etc/systemd/system/backup.timer`):

```ini
[Unit]
Description=Run backup daily at 2 AM

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true
RandomizedDelaySec=300

[Install]
WantedBy=timers.target
```

**Kullanım:**

```bash
sudo systemctl enable --now backup.timer
sudo systemctl list-timers
```

### 7. Socket Activation

**Socket dosyası** (`/etc/systemd/system/myapp.socket`):

```ini
[Unit]
Description=MyApp Socket

[Socket]
ListenStream=8080
Accept=no
ReusePort=yes

[Install]
WantedBy=sockets.target
```

**Service dosyası** (`/etc/systemd/system/myapp.service`):

```ini
[Unit]
Description=MyApp Service
Requires=myapp.socket
After=myapp.socket

[Service]
Type=simple
User=myapp

ExecStart=/opt/myapp/bin/server
StandardInput=socket
StandardOutput=journal
StandardError=journal

NonBlocking=yes

[Install]
WantedBy=multi-user.target
```

### 8. Template Service (Birden Fazla Instance)

**Template dosyası** (`/etc/systemd/system/myapp@.service`):

```ini
[Unit]
Description=MyApp Instance %i
After=network.target

[Service]
Type=simple
User=myapp
Group=myapp

Environment=INSTANCE=%i
EnvironmentFile=/etc/myapp/%i.env

WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/bin/server --port=%i

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Kullanım:**

```bash
# Port 3001'de instance başlat
sudo systemctl enable --now myapp@3001

# Port 3002'de instance başlat
sudo systemctl enable --now myapp@3002

# Tüm instance'ları listele
sudo systemctl list-units 'myapp@*'
```

---

## 🔧 Troubleshooting {#troubleshooting}

### Sık Kullanılan Komutlar

```bash
# Servis durumu
sudo systemctl status myapp

# Tüm logları görüntüle
sudo journalctl -u myapp

# Son 100 satır log
sudo journalctl -u myapp -n 100

# Canlı log takibi
sudo journalctl -u myapp -f

# Belirli zaman aralığı
sudo journalctl -u myapp --since "2024-01-01" --until "2024-01-02"

# Sadece hatalar
sudo journalctl -u myapp -p err

# Servis dosyasını kontrol et
sudo systemd-analyze verify /etc/systemd/system/myapp.service

# Servis bağımlılıklarını göster
sudo systemctl list-dependencies myapp

# Servis özelliklerini göster
sudo systemctl show myapp

# Failed servisleri listele
sudo systemctl list-units --state=failed

# Servisi resetle (restart limiti aşıldıysa)
sudo systemctl reset-failed myapp
```

### Yaygın Hatalar ve Çözümleri

| Hata Kodu           | Sebep                 | Çözüm                           |
| :------------------ | :-------------------- | :------------------------------ |
| **203/EXEC**        | ExecStart yolu yanlış | Binary yolunu kontrol et        |
| **217/USER**        | Kullanıcı yok         | `useradd` ile kullanıcı oluştur |
| **226/NAMESPACE**   | Namespace hatası      | Güvenlik ayarlarını gevşet      |
| **200/CHDIR**       | WorkingDirectory yok  | Dizini oluştur                  |
| **Start limit hit** | Çok fazla restart     | `reset-failed` ve sorunu çöz    |

### Debug Mode

```ini
[Service]
# Ekstra debug bilgisi için
Environment=DEBUG=*
Environment=NODE_DEBUG=*

# Veya systemd debug
StandardOutput=journal+console
StandardError=journal+console
```

---

## 📚 Referanslar

- [systemd.service Manual](https://www.freedesktop.org/software/systemd/man/systemd.service.html)
- [systemd.exec Manual](https://www.freedesktop.org/software/systemd/man/systemd.exec.html)
- [systemd.unit Manual](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)
