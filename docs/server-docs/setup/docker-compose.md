---
title: "Using Docker Compose"
sidebar_position: 2
---

[Docker](https://www.docker.com) is a platform for developing, shipping, and running applications in containers, and [Docker Compose](https://docs.docker.com/compose) is a tool for defining and managing multi-container Docker applications.

In this guide, you will learn how to set up GameVault using Docker and Docker Compose.

## Prerequisites

- [Docker Engine](https://docs.docker.com/engine/install/) is installed on your system. ([Docker Desktop](https://docs.docker.com/get-docker/) should work too, but this guide is tailored for Docker Engine. Steps might look different on Desktop, but the gist is the same.)

## Step 1: Creating a Docker Compose file

Create a new file named `docker-compose.yml` in a directory of your choice and copy the following code:

```yaml
version: "3.8"
services:
  gamevault-backend:
    image: phalcode/gamevault-backend:latest
    restart: unless-stopped
    environment:
      DB_HOST: db
      DB_USERNAME: gamevault
      DB_PASSWORD: YOURPASSWORDHERE
      # The following line grants Admin Role to the account with this username upon registration.
      SERVER_ADMIN_USERNAME: admin
      # Uncomment and insert your RAWG API Key here if you have one (http://rawg.io/login?forward=developer)
      # RAWG_API_KEY: YOURAPIKEYHERE
    volumes:
      # Mount the folder where your games are
      - /your/games/folder:/files
      # Mount the folder where GameVault should store its images
      - /your/images/folder:/images
    ports:
      - 8080:8080
  db:
    image: postgres:15
    restart: unless-stopped
    environment:
      POSTGRES_USER: gamevault
      POSTGRES_PASSWORD: YOURPASSWORDHERE
      POSTGRES_DB: gamevault
    volumes:
      # Mount the folder where your PostgreSQL database files should land
      - /your/database/folder:/var/lib/postgresql/data
```

:::note
Replace the the variables (`YOURPASSWORDHERE`, `YOURAPIKEYHERE`, `etc.`), as well as the folder paths with what suits you, of course.
:::

### Alternative Step 1: Running without a PostgreSQL Database

We don't recommend it, but you can run GameVault without a PostgreSQL Database too using the following configuration:

```yaml
version: "3.8"
services:
  gamevault-backend:
    image: phalcode/gamevault-backend:latest
    restart: unless-stopped
    environment:
      DB_SYSTEM: "SQLITE"
      # The following line grants Admin Role to the account with this username upon registration.
      SERVER_ADMIN_USERNAME: admin
      # Uncomment and insert your RAWG API Key here if you have one (https://gamevau.lt/docs/server-docs/indexing-and-metadata#rawg-api-key)
      # RAWG_API_KEY: YOURAPIKEYHERE
    volumes:
      - /your/games/folder:/files
      - /your/images/folder:/images
      - /your/sqlite/database/folder:/db
```

:::note
Replace the variables, as well as the folder paths with what suits you, of course.
:::

## Step 2: Start the Service

Open a terminal, navigate to the directory where the `docker-compose.yml` file is located, and run the following command:

```bash
docker compose up -d
```

This will start the GameVault server and PostgreSQL server in the background. The `-d` parameter detaches the process from the terminal.

## Conclusion

You have now successfully set up your GameVault Server using Docker and Docker Compose.

[Click here to continue.](setup.md#what-next)

## Additional Info

### Stopping the server

Open a terminal, navigate to the directory where the `docker-compose.yml` file is located, and run the following command:

```bash
docker compose down
```

### Reading the logs

Open a terminal and run the following command:

```bash
docker logs gamevault-backend
```
