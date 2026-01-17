# Traefik Proxy

Modern HTTP reverse proxy and load balancer intended to deploy microservices with ease.

## Overview

Traefik natively supports Docker and can automatically discover new services.

## Basic Configuration

`docker-compose.yml`:

```yaml
version: "3"

services:
  traefik:
    image: traefik:v2.9
    command:
      - "--api.insecure=false"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
```
