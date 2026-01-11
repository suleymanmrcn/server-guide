# Firewall (UFW) Yönetimi

Linux çekirdeğindeki (Kernel) paket filtresini yönetmek için UFW (Uncomplicated Firewall) kullanıyoruz.

**Altın Kural:** "Her şeyi kapat, sadece ihtiyacı aç." (Default Deny)

## 1. Temel Kurulum

Eğer UFW kurulu değilse veya ayarları karışmışsa:

```bash
apt install ufw
ufw disable
ufw reset
```

## 2. Politikalar (Giriş ve Çıkış)

Çoğu rehber sadece girişi kapatır. Biz **çıkışı da** kapatacağız. Bu, sunucunuz hacklenirse saldırganın dışarıya veri kaçırmasını veya sunucunuzu bir botnet parçası olarak kullanmasını zorlaştırır.

```bash
# Giren her şeyi engelle
ufw default deny incoming

# Çıkan her şeyi engelle (Bu risklidir, aşağıda izinleri vermeyi unutma!)
ufw default deny outgoing
```

## 3. Temel Kurallar (İzinler)

### Gelen Trafik (Inbound)

```bash
# SSH (Port numaranız 2222 ise onu yazın)
ufw allow veriden 2222/tcp comment 'SSH Port'

# Web Sunucusu
ufw allow 80/tcp comment 'HTTP'
ufw allow 443/tcp comment 'HTTPS'
```

### Çıkan Trafik (Outbound) - KRİTİK

Dışarı çıkışı kapattığımız için, sunucunun çalışması için gereken temel portlara izin vermeliyiz.

```bash
# DNS Sorguları (Alan adı çözümlemek için şart)
ufw allow out 53 comment 'DNS'

# HTTP/HTTPS (Update yapmak, curl ile dosya çekmek için)
ufw allow out 80/tcp comment 'HTTP Out'
ufw allow out 443/tcp comment 'HTTPS Out'

# NTP (Saat senkronizasyonu)
ufw allow out 123/udp comment 'NTP'
```

## 4. İleri Seviye Kurallar

### IP Banlama (Kara Liste)

Spesifik bir saldırganı engellemek için:

```bash
# Tek bir IP'yi engelle
ufw deny from 203.0.113.4

# Tüm bir subnet'i engelle (Örn: Çin/Rusya bloğu gibi)
ufw deny from 203.0.113.0/24
```

### IP İzin Verme (Beyaz Liste)

SSH portunu tüm dünyaya kapatıp, sadece ofis IP'nize açmak en güvenli yöntemdir.

```bash
# Önce genel SSH kuralını silin
ufw delete allow 2222/tcp

# Sadece ofis IP'sine izin ver
ufw allow from 198.51.100.4 to any port 2222 proto tcp
```

### Kural Silme

Hatalı bir kural girdiniz, silmek istiyorsunuz ama komutu hatırlamıyorsunuz. Numaralı liste kullanın:

```bash
ufw status numbered
# Çıktı:
# [ 1] 2222/tcp ALLOW IN ...
# [ 2] 80/tcp ALLOW IN ...

# 2 numaralı kuralı sil
ufw delete 2
```

## 5. Aktifleştirme

**DİKKAT:** SSH kuralının (ve outbound izinlerinin) ekli olduğundan %100 emin olun.

```bash
ufw enable
```

Kontrol:

```bash
ufw status verbose
```
