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
