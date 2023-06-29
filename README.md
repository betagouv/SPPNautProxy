# SPPNautProxy

Ce dépôt contient un fichier de configuration docker compose, illustrant l'utiliastion du [reverse proxy Traefik](https://doc.traefik.io/traefik/) pour une utilisation des projets [SPPNaut Carting](/SPPNautCarting) et [SPPNaut SPO](/SPPNautSPO) dans un environnement de production.

## Prérequis

1. [Docker et docker compose](https://docs.docker.com) sont requis pour démarrer la stack.
2. Suivez les instructions des projets SPPNaut Carting et SPO pour créer les fichiers d'environnements pour chaque projet (`.env`).

## Récupération du code source

Les projets SPO et Carting sont inclus comme [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) à titre d'illustration, mais il est fortement recommandé de se référer aux documentations de ces deux projets avant d'essayer de les éxécuter.

```
git clone --recurse-submodules git@github.com:betagouv/SPPNautProxy.git
```

## Lancement de la stack

```sh
docker compose up --detach
```

## Logs et debug

Les logs de chaque service peuvent être inspectés

```sh
docker compose logs --follow spo
docker compose logs --follow carting

# Pour débuguer d'eventuels problème avec le reverse proxy
docker compose logs --follow proxy
```

## Ports

La stack démarre par défaut sur le port 80. S'il est occupé sur la machine hôte, il peut être modifié

```yml
# docker-compose.yml
services:
  # ...
  proxy:
    # ...
    ports:
      # On a ici remplacé 80 par 8000 pour exposer le reverse proxy sur le port  8000 de la machine hôte.
      - "8000:80"
```

Chaque port exposé dans les `Dockerfile` via l'instruction `EXPOSE` sera le port atteint par défaut par Traefik au sein du conteneur cible.
