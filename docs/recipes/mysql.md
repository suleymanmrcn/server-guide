# MySQL / MariaDB

Production-ready MySQL or MariaDB setup using Docker.

## Configuration

Standard configuration includes:

- Secure root password
- Custom configuration via `my.cnf`
- Persistent storage for `/var/lib/mysql`

## Docker Compose Example

```yaml
version: "3.8"

services:
  db:
    image: mariadb:10.11
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASS}
      MYSQL_DATABASE: ${DB_NAME}
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASS}
    volumes:
      - db_data:/var/lib/mysql
      - ./my.cnf:/etc/mysql/conf.d/my.cnf:ro
    ports:
      - "127.0.0.1:3306:3306"
```
