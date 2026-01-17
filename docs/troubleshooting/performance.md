# Performans Forensics 🔬

Sistem yavaş ama neden? CPU mu, Disk mi, yoksa Kernel kilitlenmesi mi? Ezbere konuşmayalım, kanıtlara bakalım.

## 1. Load Average Yalanı

`uptime` yazdığınızda çıkan `load average: 4.00, ...` ne anlama gelir?

- **Linux'ta Load:** Sadece CPU bekleyenler DEĞİL, Disk (IO) bekleyenler de dahildir!
- Yani Load yüksekse, CPU boşta olabilir (%0 idle), ama disk darboğazdadır (%100 busy).

**Teşhis:**
`vmstat 1` çalıştırın ve `procs` sütununa bakın:

- `r` (runnable): CPU bekleyen işlem sayısı. -> **CPU Sorunu**
- `b` (blocked): Disk/Network IO bekleyen (uyuyan) işlem sayısı. -> **Disk/IO Sorunu**

## 2. Process State "D" (Uninterruptible Sleep)

`top` veya `htop` açtığınızda `S` (State) sütununda **"D"** harfi görüyorsanız korkun.

- **R (Running):** Çalışıyor.
- **S (Sleep):** Uyuyor (Normal).
- **D (Disk Sleep):** Kernel seviyesinde donanım (Disk/NFS) cevabı bekliyor. **ÖLDÜRÜLEMEZ (Kill -9 işlemez).**
- **Z (Zombie):** Ölmüş ama babası (parent) haberini almamış. Kaynak yemez ama process tablosunu kirletir.

> [!WARNING]
> Çok fazla "D" state process varsa, diskiniz bozuk olabilir veya NFS sunucusu cevap vermiyordur. Sunucuyu reboot etmek zorunda kalabilirsiniz.

## 3. OOM Killer (Out of Memory) 💀

Linux, RAM biterse sistemi kurtarmak için "en az önemli ama en çok RAM yiyen" işlemi vurur.

**Belirtiler:**

- MySQL/Java servisi aniden kapanıyor ama hata logu yok.
- SSH bağlantısı kopuyor.

**Kanıt:**

```bash
dmesg -T | grep -i "killed process"
# Çıktı: "Out of memory: Kill process 1234 (java) score 800 or sacrifice child"
```

**Çözüm:**

- Swap alanı ekleyin (Acil durum freni).
- Systemd servisine `Restart=always` ekleyin.
- Uygulamanın memory leak yapıp yapmadığını `valgrind` veya monitoring ile inceleyin.
