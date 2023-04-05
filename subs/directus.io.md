# directus.io
turns any SQL database into an API and beautiful no-code app.
A perfect layer to put in front of a database. User's can manage data immediatly. The API enables quick development.

 - [https://directus.io](https://directus.io)

## Quickstart with docker-compose
`docker-compose.yaml`
```yaml
version: '3.7'
services:

  postgres:
    image: postgres:${POSTGRES_VERSION}
    container_name: postgres
    restart: unless-stopped
    environment:
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_DB=${POSTGRES_DB}
    volumes:
      - ${POSTGRES_DEV_DATA_DIR:-./postgres-data}/postgres:/var/lib/postgresql/data
    ports:
      - "127.0.0.1:5432:5432"

  directus:
    container_name: directus
    image: directus/directus:${DIRECTUS_VERSION}
    restart: unless-stopped
    ports:
      - 127.0.0.1:8055:8055
    volumes:
      # By default, uploads are stored in /directus/uploads
      # Always make sure your volumes matches the storage root when using
      # local driver
      - ./uploads:/directus/uploads
      # Make sure to also mount the volume when using SQLite
      # - ./database:/directus/database
      # If you want to load extensions from the host
      # - ./extensions:/directus/extensions
    depends_on:
      - postgres
    environment:
      KEY: ${DIRECTUS_KEY}
      SECRET: ${DIRECTUS_SECRET}

      DB_CLIENT: 'pg'
      DB_HOST: 'postgres'
      DB_PORT: '5432'
      DB_DATABASE: ${POSTGRES_DB}
      DB_USER: ${POSTGRES_USER}
      DB_PASSWORD: ${POSTGRES_PASSWORD}

      CACHE_ENABLED: 'false'

      ADMIN_EMAIL: ${DIRECTUS_ADMIN_EMAIL}
      ADMIN_PASSWORD: ${DIRECTUS_ADMIN_PASSWORD}

      # Make sure to set this in production
      # (see https://docs.directus.io/self-hosted/config-options#general)
      # only used for e-mails (not needed internaly)
      PUBLIC_URL: ${DIRECTUS_PUBLIC_URL}
```
`.env`
```
POSTGRES_VERSION=15.1
DIRECTUS_VERSION=9.20

POSTGRES_USER=admin
POSTGRES_PASSWORD=
POSTGRES_DB=
POSTGRES_DEV_DATA_DIR=./postgres-data

DIRECTUS_KEY=
DIRECTUS_SECRET=
DIRECTUS_ADMIN_EMAIL=
DIRECTUS_ADMIN_PASSWORD=
DIRECTUS_PUBLIC_URL=
```

## Quicklearn 
 - [https://www.youtube.com/watch?v=h-6DrDjAm_c](https://www.youtube.com/watch?v=h-6DrDjAm_c) Build a CRM and Project Tracker with Directus No-code App Data Studio
