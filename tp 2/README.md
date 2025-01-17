# TP2 : Utilisation courante de Docker

🌞 docker-compose.yml

[Docker-compose](TP_Linux_tp2/docker-compose.yml)

# TP2 admins : Web stack

## I. Good practices

🌞 Limiter l'accès aux ressources

- limiter la RAM que peut utiliser chaque conteneur à 1G

```
docker run --memory="1G"
```

- limiter à 1CPU chaque conteneur

```
docker run --cpus 1.0
```

- avec le compose

```yml
deploy:
  resources:
    limits:
      cpus: '1.0'
      memory: 1G
```

🌞 Mode read-only

```
docker run -d -p 9999:80 --read-only site_web:version1
```

## II. Reverse proxy buddy

### A. Simple HTTP setup

🌞 Adaptez le docker-compose.yml

[docker-compose](TP_Linux_tp2/docker-compose.yml)

### B. HTTPS auto-signé

🌞 HTTPS auto-signé