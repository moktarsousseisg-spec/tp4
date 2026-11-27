# TP4 : Docker et Docker Compose (application MERN)

ISG de Sousse - Introduction au DevOps - 3ème année LIG

## Structure du projet

- `Client/` : application React (Vite), servie par `serve` sur le port 3000
- `Server/` : API Express, port 9000
- `docker-compose.yml` : orchestration des 3 services (mongodb, server, client)

## Étapes réalisées

### 1. Clonage du projet

    git clone https://github.com/wiem-kb/MyAppReact.git
    cd MyAppReact

### 2. Dockerfiles

**Serveur** (`Server/Dockerfile`) : image `node:lts-alpine`, copie de `package*.json`, `npm install`, copie du code, port 9000, démarrage avec `npm start`. La variable `MONGO_URI` est définie dans Docker Compose.

**Client** (`Client/Dockerfile`) : image `node:lts-alpine`, `npm install`, `npm run build` (Vite génère le dossier `dist`), installation de `serve`, port 3000.

Un fichier `.dockerignore` dans chaque dossier évite de copier `node_modules`.

Construction des images :

    docker build -t mern-server ./server
    docker build -t mern-client ./client

### 3. Réseau Docker

    docker network create mern-network

### 4. MongoDB

    docker run -d --name mongodb --network mern-network mongo

### 5. Serveur et client

    docker run -d --name server --network mern-network -p 9000:9000 mern-server
    docker run -d --name client --network mern-network -p 3000:3000 mern-client

Vérification avec `docker ps` et `docker logs server`.

### 6. Docker Compose

Le fichier `docker-compose.yml` définit :

- `mongodb` : image `mongo`, réseau `mern-network`
- `server` : build depuis `./server`, port 9000, variable `MONGO_URI`, dépend de `mongodb`
- `client` : build depuis `./client`, port 3000, dépend de `server`
- réseau `mern-network` avec le driver `bridge`

Lancement :

    docker compose up --build

Arrêt :

    docker compose down

## Accès à l'application

- Client : http://localhost:3000
- API : http://localhost:9000/api/health

## Remarque

Le code du serveur fourni n'utilise pas MongoDB pour le moment. La variable `MONGO_URI` est quand même définie dans Docker Compose (`mongodb://mongodb:27017/mydb`), conformément à l'énoncé.
