# Paket Yönetimi ve Docker

Sunucu kararlılığı için en önemli kural: **"Paketleri kaynağından ve kontrollü yükle."**

## 1. APT (Advanced Package Tool)

Ubuntu/Debian sistemlerde varsayılan paket yöneticisidir.

### Temel Komutlar (Doğru Kullanım)

```bash
# Sadece paket listesini güncelle (Yükleme yapmaz)
apt update

# Güvenli güncelleme (Mevcut konfigürasyonu bozmadan)
apt upgrade -y

# Temizlik (Gereksiz bağımlılıkları siler - ÖNEMLİ)
apt autoremove --purge
apt clean
```

### Versiyon Sabitleme (Holding)

Bir paketin (örn. Nginx veya MySQL) kazara güncellenmesini istemiyorsanız:

```bash
# Güncellemeyi engelle
apt-mark hold nginx

# Engeli kaldır
apt-mark unhold nginx
```

## 2. Docker Kurulumu (Resmi Yöntem)

⚠️ **UYARI:** Asla `apt install docker.io` komutunu kullanmayın! Bu komut Ubuntu deposundaki (genellikle çok eski)
sürümü kurar. Her zaman resmi Docker reposunu kullanın.

### Kurulum Scripti

```bash
# 1. Eski sürümleri temizle
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

# 2. Keyring ve Repo ayarla
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 3. Repoyu listeye ekle
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 4. Kur
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 3. PPA ve Harici Repolar

**Prensip:** "Mümkün olan en az harici repo."

Google'da bulduğunuz rastgele PPA'leri eklemeyin. Sadece üreticinin (Vendor) resmi dokümanındaki repoları kullanın (Örn: Postgres.org, Redis.io, Nginx.org).

## 4. Snap ve Flatpak

Sunucularda (Server Environment) **kullanılması önerilmez**.

- **Neden?**: Snap arka planda sanal loop cihazları (mount points) oluşturur. `df -h` çıktısını kirletir ve bazen disk yönetimi sorunlarına yol açar.
- **İstisna**: Sadece başka alternatifi yoksa (Örn: Bazı durumlarda Certbot) kullanılabilir ama `apt` veya `docker` her zaman önceliklidir.
