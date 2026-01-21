# Port Stratejisi ve İzolasyon 🛡️

Karmaşık sunucu yapılarında hangi servisin hangi portta çalıştığını takip etmek zordur. Ayrıca standart portları (örn: 5432, 3306) kullanmak, bot saldırılarının ilk hedefi olmanıza neden olur.

Bu proje aşağıdaki **Port Konvansiyonu (Sözleşmesi)** üzerine kuruludur.

## 1. Port Blokları (Organizasyon)

Servisleri türlerine göre belirli bloklara ayırarak yönetimi kolaylaştırıyoruz.

| Blok Aralığı    | Servis Tipi       | Örnekler                                                    |
| :-------------- | :---------------- | :---------------------------------------------------------- |
| **3000 - 3999** | **Frontend (UI)** | Next.js (3000), React (3001), Admin Panel (3002)            |
| **4000 - 4999** | **Backend (API)** | Node.js API (4000), Go Service (4001), Python Worker (4002) |
| **5000 - 5999** | **Veritabanları** | PostgreSQL (5432 -> **54321**), Redis (6379 -> **63790**)   |
| **8000 - 8999** | **Ops & Admin**   | Traefik Dashboard (8080), Portainer (9000), Monitoring      |

## 2. Security by Obscurity (Gizlilik ile Güvenlik)

Standart portları değiştirmek (Obscurity) tek başına bir güvenlik önlemi değildir ancak **Gürültü Kirliliğini (Noise)** azaltır.

Otomatik tarayıcılar (botlar) genellikle varsayılan portları tarar:

- `22` (SSH)
- `5432` (PostgreSQL)
- `3306` (MySQL)

### Strateji: "Sonuna 0 Ekle" veya "Kaydır"

Veritabanlarını dış dünyaya açmanız gerekiyorsa (Geliştirme amacıyla), standart portu asla kullanmayın.

**Kötü:**

```yaml
ports:
  - "5432:5432" # Herkesin bildiği Postgre portu
```

**İyi (Bizim Standartımız):**

```yaml
ports:
  - "54321:5432" # 5432 -> 54321 (Daha az tahmin edilebilir)
```

> [!WARNING]
> Port numarasını değiştirmek sizi hedefli saldırılardan korumaz. Port taraması (nmap) ile açık portlar bulunabilir. Bu yüzden mutlaka **UFW (Firewall)** ile IP kısıtlaması yapın.

## 3. Örnek Uygulama

Bir e-ticaret projesi için örnek port dağılımı:

- **Frontend (Next.js):** `3000`
- **Admin Panel (React):** `3001`
- **Main API (.NET):** `4000`
- **Payment Service (Node):** `4001`
- **PostgreSQL (Global):** `54321` (Host Port) -> `5432` (Container Port)
- **Redis (Cache):** `63790` (Host) -> `6379` (Container)
