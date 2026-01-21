# Kullanıcı Yönetimi

Sunucuda asla `root` kullanıcısı ile gündelik işlem yapmayın. Her yöneticinin kendi kullanıcı hesabı olmalıdır.

## 1. Kullanıcı İşlemleri (Lifecycle)

### Yeni Kullanıcı Ekleme

Sadece kullanıcı değil, home dizini ve varsayılan kabuğu (bash) ile birlikte oluşturur.

```bash
adduser yeni_kullanici
# Şifre soracaktır. Güçlü bir şifre belirleyin.
# Diğer soruları ENTER ile geçebilirsiniz.
```

### Kullanıcı Silme

Kullanıcıyı ve dosyalarını silmek için:

```bash
deluser --remove-home eski_kullanici
```

## 2. Yetkilendirme (Gruplar)

### Sudo (Yönetici) Yetkisi

Bir kullanıcıya `sudo` komutunu kullanma hakkı vermek için onu `sudo` grubuna ekleyin.

```bash
usermod -aG sudo yeni_kullanici
```

### Docker Yetkisi

Her seferinde `sudo docker` yazmamak için:

```bash
usermod -aG docker yeni_kullanici
# Etkili olması için kullanıcının çıkış yapıp tekrar girmesi gerekir.
```

## 3. Hesap Güvenliği

### Hesabı Kilitleme (Lock)

Bir personel işten ayrıldıysa veya şüpheli işlem varsa, hesabı silmeden kilitleyebilirsiniz.

```bash
# Kilitle (Giriş yapamaz)
passwd -l hedef_kullanici

# Kilidi Aç
passwd -u hedef_kullanici
```

### Parola Değişimi Zorlama

Kullanıcının bir sonraki girişinde parolasını değiştirmesini istiyorsanız:

```bash
chage -d 0 hedef_kullanici
```

### Sadece SSH Key (Parolasız)

Eğer SSH hardening yaptıysanız (`PasswordAuthentication no`), kullanıcı oluşturduktan sonra onun için bir SSH key tanımlamanız gerekir.

```bash
# Kullanıcı adına geçiş yap
su - yeni_kullanici

# .ssh klasörünü oluştur
mkdir .ssh
chmod 700 .ssh
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys

# Public key'i yapıştır
nano .ssh/authorized_keys
```

## 4. Kim Kimdir?

Sunucuda şu an kimlerin olduğunu veya geçmişte kimlerin girdiğini görmek için:

```bash
# Şu an aktif olanlar
w

# Son giriş yapanlar
last

# Başarısız giriş denemeleri
lastb
```

## 5. Varsayılan Cloud Kullanıcılarını (opc/ubuntu) Kapatma 🛡️

Oracle Cloud (`opc`), AWS (`ubuntu/ec2-user`) gibi sağlayıcılar size varsayılan bir kullanıcı verir. Güvenlik için bu kullanıcıyı devre dışı bırakıp kendi kullanıcınızı kullanmalısınız.

### Adım 1: Yeni Admin Oluşturun

Önce kendinize bir kullanıcı açın ve sudo verin (Bölüm 1 ve 2'deki gibi).

### Adım 2: Test Edin (Çok Önemli!)

Yeni bir terminal açıp **yeni kullanıcı ile** sunucuya giriş yapabildiğinizi ve `sudo` komutu çalıştırabildiğinizi doğrulayın. Asla test etmeden eski kullanıcıyı kapatmayın!

### Adım 3: SSH Erişimini Kısıtlayın (`AllowUsers`)

Bu en etkili yöntemdir. Sadece sizin kullanıcınızın SSH yapmasına izin verin.

`/etc/ssh/sshd_config` dosyasına ekleyin:

```bash
# Sadece bu kullanıcılara izin ver (Boşlukla ayırabilirsiniz)
AllowUsers yeni_kullanici baska_admin
```

SSH servisini yeniden başlatın: `sudo systemctl restart ssh`

### Adım 4: Varsayılan Kullanıcıyı Kilitleyin

Kullanıcıyı silmek (`deluser`) bazen risklidir (Cloud-init scriptleri bu kullanıcıya bağlı olabilir). Bunun yerine kilitlemek daha güvenlidir.

```bash
# 1. Şifresini kilitle
sudo usermod -L opc

# 2. Shell erişimini kapat (Giriş yapamaz)
sudo usermod -s /usr/sbin/nologin opc
```

Artık `opc` veya `ubuntu` kullanıcısı ile sisteme giriş yapılamaz.

## 6. Kısıtlı Kullanıcı Oluşturma (Geliştirici) 👨‍💻

Bazen ekibe yeni katılan bir geliştiriciye (junior) sunucu erişimi vermeniz gerekir ama **yönetici (sudo)** yetkisi olmasını istemezsiniz.

### Adım 1: Kullanıcıyı Oluşturun

```bash
# Sadece kullanıcı oluşturur. 'sudo' grubuna eklemediğiniz için YETKİSİZDİR.
sudo adduser mehmet
```

### Adım 2: SSH İzni Verin

Eğer `AllowUsers` kullanıyorsanız (ki kullanmalısınız), bu kullanıcıyı listeye ekleyin:

**`/etc/ssh/sshd_config`:**

```bash
# Hem admin hem de geliştiriciye izin ver
AllowUsers admin_user mehmet
```

Sonra servisi yeniden başlatın: `sudo systemctl restart ssh`

### Adım 3: SSH Anahtarını Ekleyin

Bölüm 3'teki "Sadece SSH Key" adımlarını uygulayın.

### Adım 4: Test (Doğrulama)

Mehmet kullanıcısı ile giriş yapın ve `sudo` komutunu deneyin.

```bash
ssh mehmet@sunucu
mehmet@server:~$ sudo apt update
[sudo] password for mehmet:
mehmet is not in the sudoers file. This incident will be reported. 🚫
```

Bu hatayı alıyorsanız işlem başarılıdır. Kullanıcı sadece kendi ev dizininde işlem yapabilir, sistem ayarlarını bozamaz.
