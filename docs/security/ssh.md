# SSH Hardening (Sertleştirme)

SSH servisi, sunucuların en çok saldırı alan kapısıdır. Varsayılan ayarlar güvensizdir.

## 1. Kritik Konfigürasyon (`sshd_config`)

Dosya: `/etc/ssh/sshd_config`

Aşağıdaki ayarları bulun ve değiştirin. Eğer yoksa dosyanın sonuna ekleyin.

```ssh
# 1. Varsayılan Portu Değiştir (Scannerlardan kaçınmak için)
Port 2222

# 2. Root Girişini Kesinlikle Kapat
PermitRootLogin no

# 3. Parola Girişini Kapat (Sadece SSH Key)
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no  # Bazı sistemlerde PAM parolayı zorlayabilir, kapatın.

# 4. Boş Parolaları Engelle
PermitEmptyPasswords no

# 5. Bağlantı Sürelerini Kısıtla (Askıda kalan oturumları öldür)
ClientAliveInterval 300
ClientAliveCountMax 2

# 6. Sadece Belirli Kullanıcıya İzin Ver
AllowUsers deployer
```

Değişikliklerden sonra sözdizimini kontrol et ve yeniden başlat:

```bash
sshd -t  # Hata varsa gösterir
systemctl restart ssh
```

## 2. SSH Anahtarı Oluşturma (Client Tarafı)

Eğer hala parolanız varsa, bilgisayarınızda yeni bir anahtar oluşturun:

```bash
ssh-keygen -t ed25519 -C "admin@sunucum"
ssh-copy-id -p 2222 deployer@sunucu-ip-adresi
```
