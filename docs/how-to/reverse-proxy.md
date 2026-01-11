# Reverse Proxy

Bu rehber, Nginx uzerinden uygulama proxy'lemeyi kapsar.

## Gereksinimler
- Nginx kurulu ve calisir durumda
- Uygulama portu lokalda dinliyor
  - Nginx kurulumu icin: [Nginx Kurulumu](nginx.md)

## Ornek konfigurasyon
`/etc/nginx/sites-available/app.conf`:
```
server {
  listen 80;
  server_name example.com;

  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
  }
}
```

```bash
ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

## Dogrulama
- `nginx -t` basarili mi?
- Uygulama endpointi yanit veriyor mu?
