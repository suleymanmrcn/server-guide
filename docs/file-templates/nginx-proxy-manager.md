# Nginx Proxy Manager (Production Ready) 🌐

**Nginx Proxy Manager (NPM)**, SSL sertifikalarını otomatik yöneten (Let's Encrypt) ve web arayüzü (UI) üzerinden host eklemenizi sağlayan popüler bir araçtır.

Prodüksiyon ortamları için **MariaDB** önerilir ancak düşük kaynaklı sunucular için **SQLite** da yeterlidir.

## ⚖️ Hangi Veritabanını Seçmeliyim?

| Özellik            | MariaDB Option 🏆           | SQLite Option 🍃            |
| :----------------- | :-------------------------- | :-------------------------- |
| **Kullanım Alanı** | Prodüksiyon, Yoğun siteler  | Kişisel projeler, Küçük VPS |
| **RAM Tüketimi**   | ~200MB+                     | ~10MB                       |
| **Performans**     | Yüksek (Concurrent access)  | Orta (Dosya tabanlı)        |
| **Kurulum**        | Ekstra container gerektirir | Tek dosya, kurulum yok      |

---

## 🏗️ Seçenek 1: MariaDB (Önerilen)

Eğer sunucunuzda en az 2GB RAM varsa bunu kullanın.

`/opt/npm` dizininde şu yapıyı kurun:

```text
npm/
├── docker-compose.yml
├── .env
├── data/                 # NPM Konfigürasyonları
├── letsencrypt/          # SSL Sertifikaları
└── mysql/                # MariaDB Veritabanı Dosyaları
```

> [!WARNING] > **SQLite vs MariaDB:** İnternetteki standart örneklerde `mysql` klasörü veya servisi göremezsiniz çünkü onlar basit SQLite kullanır. Biz prodüksiyon performansı için **MariaDB** kullanıyoruz. Eğer `mysql` klasörü oluşturduysanız aşağıdaki **prodüksiyon** compose dosyasını kullanmalısınız.

---

## 🐳 1. MariaDB Docker Compose

`/opt/npm` dizininde şu yapıyı kurun:

```text
npm/
├── docker-compose.yml
├── .env
├── data/
├── letsencrypt/
└── mysql/  <-- MariaDB kullanacaksanız bu klasör şart!
```

`docker-compose.yml` içeriği:

`docker-compose.yml` içeriği:

```yaml
version: "3.8"

services:
  app:
    image: "jc21/nginx-proxy-manager:latest"
    container_name: npm_app
    restart: always
    ports:
      # HTTP ve HTTPS Portları (Dünyaya Açık)
      - "80:80"
      - "81:81" # Admin UI (DİKKAT: Firewall ile Kısıtlayın!)
      - "443:443"
    environment:
      # MariaDB Bağlantı Bilgileri
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm_password" # .env'den almak daha güvenlidir
      DB_MYSQL_NAME: "npm"
      # IPv6'yı kapat (Bazen sorun çıkarır)
      DISABLE_IPV6: "true"
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db
    networks:
      - npm_public # Uygulamalarla konuşacağı ağ
      - npm_internal # Veritabanı ile konuşacağı ağ

  db:
    image: "jc21/mariadb-aria:latest" # NPM için optimize edilmiş imaj
    container_name: npm_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: "npm_root_password"
      MYSQL_DATABASE: "npm"
      MYSQL_USER: "npm"
      MYSQL_PASSWORD: "npm_password"
    volumes:
      - ./mysql:/var/lib/mysql
    networks:
      - npm_internal

networks:
  npm_public:
    external: true # Önceden yaratılmış olmalı ki diğer app'ler bağlansın
  npm_internal:
    internal: true # Dışarıya kapalı
```

---

## 🍃 Seçenek 2: SQLite (Hafif Sürüm)

Eğer ekstra veritabanı container'ı ile uğraşmak istemiyorsanız (veya RAM kısıtlıysa) bunu kullanın.

**Klasör Yapısı:**

```text
npm/
├── docker-compose.yml
├── data/
└── letsencrypt/
# mysql klasörüne gerek yok!
```

**`docker-compose.yml` (SQLite):**

```yaml
version: "3.8"

services:
  app:
    image: "jc21/nginx-proxy-manager:latest"
    container_name: npm_app
    restart: always
    ports:
      - "80:80"
      - "81:81"
      - "443:443"
    environment:
      # Veritabanı ayarı YOK (Otomatik SQLite kullanır)
      DISABLE_IPV6: "true"
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    networks:
      - npm_public

networks:
  npm_public:
    external: true
```

````

---

## 🐘 Seçenek 3: PostgreSQL (Unified Stack)

Eğer sisteminizdeki diğer uygulamalar zaten PostgreSQL kullanıyorsa, NPM için de Postgres kullanarak tek bir veritabanı teknolojisine odaklanabilirsiniz.

**Klasör Yapısı:**
```text
npm/
├── docker-compose.yml
├── data/
├── letsencrypt/
└── postgres/
````

**`docker-compose.yml` (PostgreSQL):**

```yaml
version: "3.8"

services:
  app:
    image: "jc21/nginx-proxy-manager:latest"
    container_name: npm_app
    restart: always
    ports:
      - "80:80"
      - "81:81"
      - "443:443"
    environment:
      # Postgres Bağlantı Bilgileri
      DB_POSTGRES_HOST: "db"
      DB_POSTGRES_PORT: 5432
      DB_POSTGRES_USER: "npm"
      DB_POSTGRES_PASSWORD: "npm_password"
      DB_POSTGRES_NAME: "npm"
      DISABLE_IPV6: "true"
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db
    networks:
      - npm_public
      - npm_internal

  db:
    image: postgres:15-alpine
    container_name: npm_db
    restart: always
    environment:
      POSTGRES_USER: "npm"
      POSTGRES_PASSWORD: "npm_password"
      POSTGRES_DB: "npm"
    volumes:
      - ./postgres:/var/lib/postgresql/data
    networks:
      - npm_internal

networks:
  npm_public:
    external: true
  npm_internal:
    internal: true
```

---

## 🔌 Ağ (Network) Yapılandırması 🕸️

Kullanıcı sordu: _"Niye 2 tane network var? Hepsine aynı networkü versek olmaz mı?"_

**Cevap:** Olur ama **güvenli olmaz**. İki farklı ağ kullanmamızın sebebi "Sorumluluk Ayrımıdır":

1.  **`npm_public` (Trafik Polisi Ağı):**
    - Bu ağ "Halka Açık" kapıdır. NPM, ziyaretçileri karşılar ve arkadaki Web Sitesine yönlendirir.
    - Web sitenizi bu ağa bağlamanız şarttır.
2.  **`npm_internal` (Özel Kasa Dairesi):**
    - Bu ağ NPM ile kendi veritabanı (MariaDB) arasındadır.
    - Bunu ayırmazsak, web sitenizdeki bir açıktan sızan hacker, NPM'in veritabanına doğrudan erişebilir. Biz bunu engelliyoruz.

### Nasıl Bağlanır?

1.  Önce genel amacı ağı oluşturun:

NPM'in diğer container'lara (örneğin az önceki Redis veya Postgres UI gibi) ulaşabilmesi için **ortak bir ağa** ihtiyacı vardır.

1.  Önce ağı oluşturun:

    ```bash
    docker network create npm_public
    ```

2.  Kendi uygulamanızı (örneğin bir Web Sitesi) bu ağa dahil edin:

    ```yaml
    # Web Sitesi Compose Dosyası
    services:
      my-website:
        image: nginx
        networks:
          - default
          - npm_public # <-- Buraya dahil oldu

    networks:
      npm_public:
        external: true
    ```

3.  NPM Arayüzü'nde "Forward Hostname / IP" kısmına container adını yazın: `my-website` (Port: 80).

---

## 🛡️ Güvenlik Uyarısı: Port 81

`81` portu NPM'in yönetim panelidir. Varsayılan kullanıcı şifresi: `admin@example.com` / `changeme`.

**Kesinlikle Yapılması Gerekenler:**

1.  İlk girişte şifreyi değiştirin.
2.  Mümkünse `81` portunu **Firewall (UFW)** ile dış dünyaya kapatın, sadece kendi IP'nize veya VPN IP'nize açın.

```bash
# Sadece yönetim IP'sine izin ver
sudo ufw allow form 1.2.3.4 to any port 81
sudo ufw deny 81
```
