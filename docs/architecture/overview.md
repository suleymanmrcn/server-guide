# Mimari Genel Bakış

Bu bölüm, "Production-Ready" bir sunucunun nasıl görüneceğini görselleştirir. Rastgele servis kurulumu yerine, katmanlı bir savunma ve operasyon modeli izliyoruz.

## Standart İstek Akışı (Request Flow)

Bir HTTP isteğinin son kullanıcıdan uygulamanıza ulaşırken geçtiği güvenlik ve işlem katmanları:

```mermaid
graph TD
    User[🌍 Kullanıcı] -->|HTTPS/443| CF[☁️ Cloudflare / CDN]
    CF -->|Filtrelenmiş Trafik| FW[🛡️ Sunucu Firewall (UFW)]

    subgraph "Sunucu İç Katmanları"
        FW -->|Port 80/443| Nginx[🚀 Nginx Reverse Proxy]

        subgraph "Uygulama Alanı"
            Nginx -->|Proxy Pass| App[⚙️ Uygulama (Node/Python/Go)]
            App -->|Query| DB[(🗄️ Veritabanı)]
            App -->|Cache| Redis[(⚡ Redis)]
        end

        subgraph "Yönetim & İzleme"
            SSH[🔑 SSH (Port 2222)] --> FW
            Agent[👀 Monitoring Agent] .->|Metrics| Cloud[Ext. Monitoring]
        end
    end
```

## Katmanlar Detayı

Her katman bir **savunma hattı** ve **sorumluluk alanı** olarak tasarlanmıştır.

### 1. Ağ Katmanı (The Moat)

Dış dünya ile sunucu arasındaki ilk temas noktası.

- **Firewall (UFW):** `Default Deny`. Sadece 80, 443 ve özel SSH portuna izin verilir.
- **Fail2Ban/CrowdSec:** Brute-force deneyen IP'leri otomatik banlar.
- **SSH Hardening:** Asla varsayılan port (22) kullanılmaz, asla parola ile girilmez.

### 2. Yayın Katmanı (The Gatekeeper)

Trafiği karşılayan ve dağıtan katman.

- **Nginx / Caddy:** SSL sonlandırma (Termination) burada yapılır.
- **Security Headers:** XSS, Clickjacking gibi saldırılar burada engellenir.
- **Rate Limiting:** Tek bir IP'den gelen aşırı istekler burada frenlenir.

### 3. Uygulama ve Veri (The Core)

İş mantığının çalıştığı yer.

- **Systemd:** Uygulama servis olarak çalışır, öldüğünde otomatik yeniden başlar.
- **Least Privilege:** Uygulama asla `root` yetkisiyle çalışmaz.
- **Local Only:** Veritabanı portları (örn. 5432, 6379) dış dünyaya asla açılmaz, sadece `localhost` dinler.

### 4. Operasyon (The Watchtower)

Sistemin sağlığını izleyen mekanizmalar.

- **Log Rotation:** Loglar diski doldurmasın diye düzenli sıkıştırılır/silinir.
- **Backups:** Şifreli ve sunucu dışında (off-site) yedeklenir.
