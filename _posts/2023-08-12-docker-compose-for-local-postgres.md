---
title: "Docker-compose for local PostgreSQL"
date: 2023-08-12
---

Example related to service named `srh`:

```
version: '3.8'

services:
  srh-db-service:
    image: postgres:15.3
    restart: on-failure
    container_name: srh-db-container-name
    environment:
      - POSTGRES_USER=srh-user
      - POSTGRES_PASSWORD=srh-pwd
      - POSTGRES_DB=srh-db
    ports:
      - '54321:5432'
    volumes:
      - pg-data:/var/lib/postgresql/data

volumes:
  pg-data:
```
To connect via some DB client (DBeaver, for example) - specify port 54321 (first one)
