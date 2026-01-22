# Temel OS Kurulumu 🖥️

Yeni bir Ubuntu/Debian sunucusuna ilk giriş yaptıktan sonra yapılması gereken **temel ayarlar ve güvenlik önlemleri**.

---

## 1. İlk Bağlantı (SSH)

### Root ile İlk Giriş

```bash
# Varsayılan SSH portu (22)
ssh root@SUNUCU_IP

# Özel port kullanıyorsanız
ssh -p 2222 root@SUNUCU_IP

# SSH key ile bağlanma
ssh -i ~/.ssh/id_rsa root@SUNUCU_IP
```

> [!WARNING] > **Güvenlik:** Root ile SSH bağlantısını mümkün olan en kısa sürede kapatmalısınız. Önce sudo yetkili kullanıcı oluşturun.

---

## 2. Sistem Güncellemeleri

```bash
# Paket listesini güncelle
apt update

# Tüm paketleri yükselt
apt upgrade -y

# Tam yükseltme (kernel dahil)
apt full-upgrade -y

# Gereksiz paketleri temizle
apt autoremove -y
apt autoclean
```

---

## 3. Yönetici Kullanıcı Oluşturma

Root yerine sudo yetkili kullanıcı kullanın:

```bash
# Yeni kullanıcı oluştur
adduser deploy

# Sudo grubuna ekle
usermod -aG sudo deploy

# Kullanıcıyı kontrol et
id deploy
groups deploy
```

### SSH Key Kopyalama (Önemli!)

Root'tan yeni kullanıcıya SSH key'i kopyalayın:

```bash
# Root'un SSH key'ini kopyala
mkdir -p /home/deploy/.ssh
cp /root/.ssh/authorized_keys /home/deploy/.ssh/
chown -R deploy:deploy /home/deploy/.ssh
chmod 700 /home/deploy/.ssh
chmod 600 /home/deploy/.ssh/authorized_keys
```

**Test Edin:**

```bash
# Yeni terminalde test et (root oturumunu KAPATMAYIN!)
ssh deploy@SUNUCU_IP
sudo whoami  # "root" çıktısı vermeli
```

---

## 4. SSH Port Değiştirme (Güvenlik)

Varsayılan 22 portu yerine özel port kullanın (bot saldırılarını azaltır):

```bash
# SSH config dosyasını düzenle
sudo nano /etc/ssh/sshd_config
```

**Değiştirilecek satırlar:**

```bash
# Port değiştir (örn: 2222)
Port 2222

# Root girişini kapat
PermitRootLogin no

# Şifre ile girişi kapat (sadece SSH key)
PasswordAuthentication no

# Boş şifreleri engelle
PermitEmptyPasswords no
```

**SSH'yi yeniden başlat:**

```bash
sudo systemctl restart sshd

# Yeni port dinliyor mu kontrol et
sudo ss -tlnp | grep 2222
```

**Firewall'da portu aç:**

```bash
# UFW kullanıyorsanız
sudo ufw allow 2222/tcp
sudo ufw delete allow 22/tcp  # Eski portu kapat
```

> [!CAUTION] > **Dikkat:** Yeni port ile bağlantıyı test etmeden eski oturumu KAPATMAYIN! Aksi halde sunucuya erişiminizi kaybedebilirsiniz.

**Yeni port ile bağlanma:**

```bash
ssh -p 2222 deploy@SUNUCU_IP
```

---

## 5. Hostname ve Timezone Ayarları

### Hostname Değiştirme

```bash
# Hostname ayarla
sudo hostnamectl set-hostname production-server

# Kontrol et
hostnamectl

# /etc/hosts dosyasını güncelle
sudo nano /etc/hosts
```

`/etc/hosts` içeriği:

```
127.0.0.1 localhost
127.0.1.1 production-server

# Sunucu IP'si
YOUR_SERVER_IP production-server
```

### Timezone Ayarlama

```bash
# Mevcut timezone
timedatectl

# Timezone listesi
timedatectl list-timezones | grep Istanbul

# Timezone ayarla
sudo timedatectl set-timezone Europe/Istanbul

# Zaman senkronizasyonu aktif et
sudo systemctl enable --now systemd-timesyncd

# Kontrol et
timedatectl status
```

---

## 6. Temel Araçlar Kurulumu 🧰

```bash
sudo apt install -y \
  curl \
  wget \
  git \
  htop \
  btop \
  jq \
  dialog \
  net-tools \
  vim \
  nano \
  unzip \
  zip \
  tree \
  ncdu \
  tmux \
  screen
```

### Araçların Kullanım Alanları

| Araç             | Kullanım                              |
| :--------------- | :------------------------------------ |
| `curl`, `wget`   | Dosya indirme, API testleri           |
| `git`            | Kod ve config yönetimi                |
| `htop`, `btop`   | Sistem kaynak izleme (CPU/RAM)        |
| `jq`             | JSON parse etme                       |
| `dialog`         | Terminal UI scriptleri                |
| `net-tools`      | Network komutları (ifconfig, netstat) |
| `vim`, `nano`    | Metin editörleri                      |
| `tree`           | Dizin yapısını görselleştirme         |
| `ncdu`           | Disk kullanımı analizi                |
| `tmux`, `screen` | Terminal multiplexer                  |

---

## 7. Düşük Kaynaklı Sistemler İçin Optimizasyon 🐌

### A. Swappiness Ayarı

Düşük RAM'li sistemlerde swap kullanımını optimize edin:

```bash
# Mevcut değer (varsayılan: 60)
cat /proc/sys/vm/swappiness

# Düşük RAM için önerilen: 10
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### B. Gereksiz Servisleri Devre Dışı Bırakma

```bash
# Çalışan servisleri listele
systemctl list-unit-files --state=enabled

# Gereksiz servisleri durdur (örnekler)
sudo systemctl disable --now bluetooth.service
sudo systemctl disable --now cups.service
sudo systemctl disable --now avahi-daemon.service
```

### C. Minimal Kernel Parametreleri

`/etc/sysctl.conf` dosyasına ekleyin:

```bash
# Düşük RAM için
vm.vfs_cache_pressure=50
vm.dirty_ratio=10
vm.dirty_background_ratio=5

# Network buffer'ları küçült
net.core.rmem_max=8388608
net.core.wmem_max=8388608
```

Uygula:

```bash
sudo sysctl -p
```

### D. Hafif Alternatifler

| Standart           | Hafif Alternatif     |
| :----------------- | :------------------- |
| `htop`             | `btop` (daha hafif)  |
| `systemd-journald` | Log boyutunu sınırla |
| `snapd`            | Kaldır (gereksizse)  |

**snapd kaldırma (isteğe bağlı):**

```bash
sudo apt purge snapd -y
sudo apt autoremove -y
```

**journald log boyutu sınırlama:**

```bash
sudo nano /etc/systemd/journald.conf
```

```ini
[Journal]
SystemMaxUse=100M
SystemMaxFileSize=10M
```

```bash
sudo systemctl restart systemd-journald
```

---

## 8. Otomatik Güvenlik Güncellemeleri

```bash
# Unattended-upgrades kur
sudo apt install unattended-upgrades -y

# Aktif et
sudo dpkg-reconfigure --priority=low unattended-upgrades

# Konfigürasyon
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

**Önerilen ayarlar:**

```
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
    "${distro_id}ESMApps:${distro_codename}-apps-security";
};

Unattended-Upgrade::AutoFixInterruptedDpkg "true";
Unattended-Upgrade::MinimalSteps "true";
Unattended-Upgrade::Remove-Unused-Dependencies "true";
Unattended-Upgrade::Automatic-Reboot "false";
```

---

## 9. Sistem Bilgilerini Görüntüleme

```bash
# OS versiyonu
lsb_release -a

# Kernel versiyonu
uname -r

# CPU bilgisi
lscpu

# RAM bilgisi
free -h

# Disk bilgisi
df -h

# Çalışma süresi
uptime

# Sistem yükü
top
htop
```

---

## 10. Doğrulama Checklist ✅

Kurulumu tamamladıktan sonra kontrol edin:

- [ ] Sistem güncel (`apt update && apt upgrade`)
- [ ] Sudo yetkili kullanıcı oluşturuldu (`deploy`)
- [ ] SSH key ile bağlanılabiliyor
- [ ] SSH portu değiştirildi (varsayılan 22 değil)
- [ ] Root SSH girişi kapatıldı (`PermitRootLogin no`)
- [ ] Hostname ayarlandı (`hostnamectl`)
- [ ] Timezone ayarlandı (`timedatectl`)
- [ ] Temel araçlar kuruldu (`htop`, `git`, `curl`)
- [ ] Otomatik güvenlik güncellemeleri aktif
- [ ] Firewall yapılandırıldı (UFW)

---

## 🔗 Sonraki Adımlar

1. **[Kullanıcı Yönetimi](user-management.md)** - Ek kullanıcılar ve SSH key yönetimi
2. **[Güvenlik - SSH Hardening](../security/ssh.md)** - SSH'yi daha da güvenli hale getirin
3. **[Güvenlik - Firewall](../security/firewall.md)** - UFW ile firewall kurulumu
4. **[Docker Kurulumu](docker.md)** - Container altyapısını kurun

---

## 📚 Referanslar

- [Ubuntu Server Guide](https://ubuntu.com/server/docs)
- [Debian Administrator's Handbook](https://debian-handbook.info/)
- [SSH Hardening Guide](https://www.ssh.com/academy/ssh/sshd_config)
