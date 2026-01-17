# Network Forensics 🌐

Ağ sorunları genellikle "yavaşlık" olarak rapor edilir. Sorunun sunucuda mı, arada (ISP) mı yoksa karşı tarafta mı olduğunu anlamak sanattır.

## 1. Latency vs Bandwidth 🐢 vs 🏎️

- **Bandwidth (Bant Genişliği):** Borunun genişliği. (1 Gbps)
- **Latency (Gecikme):** Verinin borudan geçiş süresi. (20ms)

> **Kural:** Web sitelerinin yavaş açılmasının sebebi %90 **Latency** sorunudur, Bandwidth değil.

**Teşhis:**
`mtr` (My Traceroute) kullanın. Ping ve Traceroute'un birleşimidir.

```bash
mtr -zbwc 100 8.8.8.8
```

- Paket kaybı (Loss%) nerede başlıyor? İlk hopta ise sorun sizde/kablonuzda. Ortada ise ISP sorunu.

## 2. DNS Çözümleme Sorunları 🗺️

"Siteye girilmiyor" demeden önce "Adres çözülüyor mu?" diye bakın.

**Basit Test:**

```bash
host google.com
```

**Detaylı Analiz (Trace):**
DNS sorgusunun kök sunuculardan (Root Servers) başlayarak adım adım nasıl çözüldüğünü izleyin:

```bash
dig +trace google.com
```

**Systemd-resolve Sorunları:**
Ubuntu/Debian'da DNS cache sorunları için:

```bash
resolvectl status
resolvectl flush-caches
```

## 3. Tcpdump ile Paket Yakalama 🎣

Sorunu loglarda göremiyorsanız, kabloyu dinleyin.

**Örnek: HTTP 500 dönüyor ama log yok**
80 portunu dinle, ASCII formatında (-A) bas, paketleri kesme (-s0):

```bash
tcpdump -i eth0 port 80 -A -s0
```

**Örnek: Kim bana SSH brute-force yapıyor?**
Sadece SYN paketlerini (bağlantı kurma isteği) yakala:

```bash
tcpdump -i eth0 port 22 "tcp[tcpflags] & tcp-syn != 0"
```

> [!TIP] > `tcpdump` çıktısını okumak zordur. `-w capture.pcap` ile dosyaya kaydedip, dosyayı bilgisayarınıza indirip **Wireshark** ile görsel olarak inceleyebilirsiniz.
