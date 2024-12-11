# TP1 : Premiers pas Docker

## I. Init

### 3. sudo c pa bo

🌞 Ajouter votre utilisateur au groupe docker

```
[hugo@tp1Linux ~]$
sudo usermod -aG docker $USER
```

### 4. Un premier conteneur en vif

🌞 Lancer un conteneur NGINX

```
[hugo@tp1Linux ~]$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
bc0965b23a04: Pull complete
650ee30bbe5e: Pull complete
8cc1569e58f5: Pull complete
362f35df001b: Pull complete
13e320bf29cd: Pull complete
7b50399908e1: Pull complete
57b64962dd94: Pull complete
Digest: sha256:fb197595ebe76b9c0c14ab68159fd3c08bd067ec62300583543f0ebda353b5be
Status: Downloaded newer image for nginx:latest
00382d52846f7c1474a1b3d9ad74c57ad6c4aacaf10dae0466062cb35b2ca39a    
```

🌞 Visitons

- vérifier que le conteneur est actif avec une commande qui liste les conteneurs en cours de fonctionnement

```
[hugo@tp1Linux ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
00382d52846f   nginx     "/docker-entrypoint.…"   43 seconds ago   Up 41 seconds   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   intelligent_fermi
```

- afficher les logs du conteneur

```
[hugo@tp1Linux ~]$ docker logs 00
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/12/11 14:41:20 [notice] 1#1: using the "epoll" event method
2024/12/11 14:41:20 [notice] 1#1: nginx/1.27.3
2024/12/11 14:41:20 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2024/12/11 14:41:20 [notice] 1#1: OS: Linux 5.14.0-427.37.1.el9_4.x86_64
2024/12/11 14:41:20 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1073741816:1073741816
2024/12/11 14:41:20 [notice] 1#1: start worker processes
2024/12/11 14:41:20 [notice] 1#1: start worker process 29
```

- afficher le port en écoute sur la VM avec un sudo ss -lnpt

```
[hugo@tp1Linux ~]$ sudo ss -lnpt
[sudo] password for hugo:
State         Recv-Q        Send-Q               Local Address:Port                 Peer Address:Port        Process
LISTEN        0             4096                       0.0.0.0:9999                      0.0.0.0:*            users:(("docker-proxy",pid=3776,fd=4))
LISTEN        0             128                        0.0.0.0:22                        0.0.0.0:*            users:(("sshd",pid=682,fd=3))
LISTEN        0             4096                          [::]:9999                         [::]:*            users:(("docker-proxy",pid=3781,fd=4))
LISTEN        0             128                           [::]:22                           [::]:*            users:(("sshd",pid=682,fd=4))
```

🌞 On va ajouter un site Web au conteneur NGINX

🌞 Visitons

- vérifier que le conteneur est actif

```
[hugo@tp1Linux nginx]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                                 NAMES
2366bd76845b   nginx     "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   80/tcp, 0.0.0.0:9999->8080/tcp, [::]:9999->8080/tcp   reverent_brattain
```

### 5. Un deuxième conteneur en vif

🌞 Lance un conteneur Python, avec un shell

```
[hugo@tp1Linux nginx]$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
fdf894e782a2: Pull complete
5bd71677db44: Pull complete
551df7f94f9c: Pull complete
ce82e98d553d: Pull complete
5f0e19c475d6: Pull complete
abab87fa45d0: Pull complete
2ac2596c631f: Pull complete
Digest: sha256:220d07595f288567bbf07883576f6591dad77d824dce74f0c73850e129fa1f46
Status: Downloaded newer image for python:latest
```

🌞 Installe des libs Python

- installez deux libs, elles ont été choisies complètement au hasard (avec la commande pip install)

```
root@f0b73b8eacd9:/# pip install aiohttp
root@f0b73b8eacd9:/# pip install aioconsole
```

- tapez la commande python pour ouvrir un interpréteur Python

```
root@f0b73b8eacd9:/# python
Python 3.13.1 (main, Dec  4 2024, 20:40:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
```

## II. Images

### 1. Images publiques

🌞 Récupérez des images

- listez les images que vous avez sur la machine avec une commande docker

```
[hugo@tp1Linux nginx]$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED       SIZE
linuxserver/wikijs   latest    863e49d2e56c   5 days ago    465MB
python               latest    3ca4060004b1   7 days ago    1.02GB
nginx                latest    66f8bdd3810c   2 weeks ago   192MB
wordpress            latest    c89b40a25cd1   2 weeks ago   700MB
mysql                latest    56a8c14e1404   8 weeks ago   603MB
```

🌞 Lancez un conteneur à partir de l'image Python

- lancez un terminal bash ou sh

```
[hugo@tp1Linux nginx]$ docker run -it python bash
```

- vérifiez que la commande python est installée dans la bonne version

```
root@c2b766773029:/# python -V
Python 3.13.1
```

### 2. Construire une image

🌞 Ecrire un Dockerfile pour une image qui héberge une application Python

```
[hugo@tp1Linux python_app_biuld]$ cat Dockerfile
FROM debian

RUN apt update

RUN apt install -y python3

RUN apt install -y python3-emoji

COPY app.py /tmp/

ENTRYPOINT ["python3", "/tmp/app.py"]
```

🌞 Build l'image

```
[hugo@tp1Linux python_app_biuld]$ docker build . -t python_app:version_de_ouf
[+] Building 48.2s (10/10) FINISHED                          docker:default
 => [internal] load build definition from Dockerfile                   0.2s
 => => transferring dockerfile: 248B                                   0.0s
 => [internal] load metadata for docker.io/library/debian:latest       0.0s
 => [internal] load .dockerignore                                      0.1s
 => => transferring context: 2B                                        0.0s
 => [1/5] FROM docker.io/library/debian:latest                         0.1s
 => [internal] load build context                                      0.3s
 => => transferring context: 188B                                      0.0s
 => [2/5] RUN apt update                                               8.7s
 => [3/5] RUN apt install -y python3                                  28.0s
 => [4/5] RUN apt install -y python3-emoji                             5.6s
 => [5/5] COPY app.py /tmp/                                            1.0s
 => exporting to image                                                 3.9s
 => => exporting layers                                                3.8s
 => => writing image sha256:10be384e22fc95ae3e9687ed6dbb9c94cba9024c7  0.0s
 => => naming to docker.io/library/python_app:version_de_ouf           0.0s
```

🌞 Lancer l'image

```
[hugo@tp1Linux python_app_biuld]$ docker run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```

## III. Docker compose

🌞 Créez un fichier docker-compose.yml

- dans un nouveau dossier dédié /home/<USER>/compose_test

```
[hugo@tp1Linux ~]$ mkdir compose_test
```

🌞 Lancez les deux conteneurs avec docker compose

- go exécuter docker compose up -d

```
[hugo@tp1Linux compose_test]$ docker compose up -d
WARN[0000] /home/hugo/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 3/3
 ✔ Network compose_test_default                  Created               1.1s
 ✔ Container compose_test-conteneur_flopesque-1  Started               2.8s
 ✔ Container compose_test-conteneur_nul-1        Started               2.8s
```

🌞 Vérifier que les deux conteneurs tournent

```
[hugo@tp1Linux compose_test]$ docker compose ps
WARN[0000] /home/hugo/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
NAME                                 IMAGE     COMMAND        SERVICE               CREATED         STATUS         PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   2 minutes ago   Up 7 seconds
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         2 minutes ago   Up 7 seconds
```

🌞 Pop un shell dans le conteneur conteneur_nul

```
[hugo@tp1Linux compose_test]$ docker exec -it 32 bash
```

```
root@32965062e900:/# ping conteneur_flopesque
PING conteneur_flopesque (172.18.0.3) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=1 ttl=64 time=0.080 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=2 ttl=64 time=0.124 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=3 ttl=64 time=0.088 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.3): icmp_seq=4 ttl=64 time=0.208 ms
^C
--- conteneur_flopesque ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3067ms
rtt min/avg/max/mdev = 0.080/0.125/0.208/0.050 ms
```