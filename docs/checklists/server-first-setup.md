# İlk 15 Dakika: Sunucu Hazırlık Kontrol Listesi

Yeni bir sunucu (VPS/Bare Metal) teslim aldığınızda, üzerine herhangi bir uygulama kurmadan önce bu listeyi tamamlayın. Bu adımlar, sunucunun internete açık olduğu ilk kritik anlarda güvenliği sağlar.

> [!IMPORTANT]
> Bu adımlar `root` yetkisi ile değil, `sudo` yetkisine sahip bir kullanıcı ile yapılmalıdır.

## 1. Erişim ve Kimlik

- [ ] **Sistemi Güncelle**: `apt update && apt upgrade -y`
- [ ] **Hostname Ayarla**: `hostnamectl set-hostname sunucu-adi`
- [ ] **Yeni Kullanıcı Oluştur**: `adduser deployer`
- [ ] **Sudo Yetkisi Ver**: `usermod -aG sudo deployer`
- [ ] **SSH Key Kopyala**: Local makinenizden `ssh-copy-id deployer@ip-adresi`

## 2. SSH Hardening (Kritik)

`/etc/ssh/sshd_config` dosyasında aşağıdaki değişiklikleri yapın ve servisi yeniden başlatın (`systemctl restart ssh`).

- [ ] **Root Login Kapat**: `PermitRootLogin no`
- [ ] **Parola Girişini Kapat**: `PasswordAuthentication no`
- [ ] **Boş Parolayı Engelle**: `PermitEmptyPasswords no`
- [ ] **Opsiyonel**: SSH Portunu değiştir (örn: 2222)

## 3. Firewall (UFW)

Asla tüm kapıları açık bırakmayın. "Default Deny" politikasını uygulayın.

- [ ] **UFW Kur**: `apt install ufw`
- [ ] **Varsayılanları Ayarla**: `ufw default deny incoming`, `ufw default allow outgoing`
- [ ] **SSH İzni Ver**: `ufw allow ssh` (veya `ufw allow 2222/tcp`)
- [ ] **Web İzni Ver**: `ufw allow 80/tcp`, `ufw allow 443/tcp`
- [ ] **UFW Aktifleştir**: `ufw enable`

## 4. Otomatik Korumalar

Siz uyurken sunucunuzu koruyacak servisler.

- [ ] **Fail2Ban Kur**: `apt install fail2ban`
- [ ] **Fail2Ban Aktifleştir**: `systemctl enable --now fail2ban`
- [ ] **Otomatik Güncelleme**: `apt install unattended-upgrades` ve `dpkg-reconfigure --priority=low unattended-upgrades`

## 5. Son Kontrol (Reboot)

- [ ] **Reboot At**: `reboot`
- [ ] **Yeni Kullanıcı ile Gir**: `ssh deployer@ip-adresi`
- [ ] **Sudo Dene**: `sudo whoami` (root dönmeli)
