# Redis

High-performance in-memory data store for caching and session management.

## Setup

Use Redis for:

- Application Cache (e.g. Laravel, Rails, Node.js)
- Queue Management (e.g. Sidekiq, Bull)
- Session Storage

## Docker Compose

```yaml
version: "3.8"

services:
  redis:
    image: redis:7-alpine
    restart: always
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data
    ports:
      - "127.0.0.1:6379:6379"

volumes:
  redis_data:
```
