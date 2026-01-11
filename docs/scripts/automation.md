# Otomasyon Scripti: Server Init

Bu script, "İlk 15 Dakika" rehberindeki tüm adımları (User, SSH, UFW, Fail2ban, Updates) tek seferde ve standartlara uygun şekilde yapar.

> [!WARNING]
> Bu script **yeni kurulmuş (fresh)** Ubuntu/Debian sunucular içindir. Halihazırda çalışan bir sunucuda denemeyin, konfigürasyonlarınızı ezebilir.

## Kullanım

Sunucunuza `root` olarak giriş yapın ve aşağıdaki komutu çalıştırın:

```bash
# Scripti indir ve çalıştır
curl -O https://raw.githubusercontent.com/your-repo/handbook/main/scripts/server-init.sh
chmod +x server-init.sh
./server-init.sh
```

## Script İçeriği (`server-init.sh`)

Kendi reponuzda saklayabileceğiniz kaynak kod:

```bash
#!/bin/bash
set -e

# --- AYARLAR ---
NEW_USER="deployer"
SSH_PORT="2222"
# ----------------

echo "🚀 Sunucu Kurulumu Basliyor..."

# 1. Sistemi Guncelle
echo "📦 Paketler guncelleniyor..."
apt update && apt upgrade -y
apt install -y ufw fail2ban curl git unattended-upgrades

# 2. Yeni Kullanici
echo "👤 Kullanici olusturuluyor: $NEW_USER"
if id "$NEW_USER" &>/dev/null; then
    echo "   Kullanici zaten var, atlaniyor."
else
    adduser --disabled-password --gecos "" $NEW_USER
    usermod -aG sudo $NEW_USER

    # SSH Key klasoru
    mkdir -p /home/$NEW_USER/.ssh
    if [ -f /root/.ssh/authorized_keys ]; then
        echo "   Root keyleri kopyalaniyor..."
        cp /root/.ssh/authorized_keys /home/$NEW_USER/.ssh/
        chown -R $NEW_USER:$NEW_USER /home/$NEW_USER/.ssh
        chmod 700 /home/$NEW_USER/.ssh
        chmod 600 /home/$NEW_USER/.ssh/authorized_keys
    else
        echo "⚠️ DIKKAT: /root/.ssh/authorized_keys bulunamadi. Lutfen elle key ekleyin!"
    fi
fi

# 3. SSH Hardening
echo "🔒 SSH sertlestiriliyor (Port: $SSH_PORT)..."
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sed -i "s/#Port 22/Port $SSH_PORT/" /etc/ssh/sshd_config
sed -i "s/Port 22/Port $SSH_PORT/" /etc/ssh/sshd_config
sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# 4. Firewall (UFW)
echo "🛡️ Firewall (UFW) ayarlaniyor..."
ufw default deny incoming
ufw default allow outgoing
ufw allow $SSH_PORT/tcp comment 'SSH Port'
ufw allow 80/tcp comment 'HTTP'
ufw allow 443/tcp comment 'HTTPS'
# ufw enable komutunu script içinde calistirmak bazen baglantiyi kesebilir
# o yuzden sona sakliyoruz veya kullaniciya birakiyoruz.
echo "⚠️ Firewall kurallari eklendi. Aktiflestirmek icin: 'ufw enable'"

# 5. Fail2Ban
echo "🚫 Fail2Ban aktif..."
systemctl enable --now fail2ban

echo "✅ Kurulum Tamamlandi!"
echo "👉 Lutfen 'ufw enable' komutunu calistirin ve $SSH_PORT portundan baglanmayi deneyin."
```
