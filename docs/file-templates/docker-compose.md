# Docker Compose Template

```yaml
# Standard Docker Compose Template

version: "3.8"

services:
  # 1. Frontend (React/Vue/Next)
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: my_app
    restart: unless-stopped
    ports:
      - "127.0.0.1:3000:3000" # Localhost only (behind proxy)
    environment:
      - NODE_ENV=production
    networks:
      - internal_net

  # 2. Proxy (Nginx)
  nginx:
    image: nginx:alpine
    container_name: my_nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./certs:/etc/nginx/certs:ro
    depends_on:
      - app
    networks:
      - internal_net

  # 3. Database
  db:
    image: postgres:15-alpine
    container_name: my_db
    restart: always
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASS}
      POSTGRES_DB: ${DB_NAME}
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - internal_net

volumes:
  db_data:

networks:
  internal_net:
    driver: bridge
```
