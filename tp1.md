# Part I : Docker basics

## 1. Install

🌞 **Installer Docker votre machine Azure**

```bash
fatma@ubuntu:~$ sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)

fatma@ubuntu:~$ sudo apt update
fatma@ubuntu:~$ sudo apt install ca-certificates curl
fatma@ubuntu:~$ sudo install -m 0755 -d /etc/apt/keyrings
fatma@ubuntu:~$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
fatma@ubuntu:~$ sudo chmod a+r /etc/apt/keyrings/docker.asc

fatma@ubuntu:~$ sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

fatma@ubuntu:~$ sudo apt update

fatma@ubuntu:~$ sudo systemctl start docker

fatma@ubuntu:~$ sudo usermod -aG docker $(whoami)
```

## 2. Vérifier l'install

## 3. Lancement de conteneurs


🌞 **Utiliser la commande `docker run`**

```bash
fatma@ubuntu:~$ docker run -d --name web -p 9999:80 nginx
3d106d5b28c4a3e84dbfe135267d3524262088473101f9cecd0b0a770cbee32a
```

🌞 **Rendre le service dispo sur internet**

```bash
fatma@ubuntu:~$ sudo ufw allow 9999/tcp
[sudo] Mot de passe de fatma : 
Les règles ont été mises à jour
Les règles ont été mises à jour (IPv6)
fatma@ubuntu:~$ sudo ufw status
État : inactif

fatma@ubuntu:~$ sudo ufw enable
Le pare-feu est actif et lancé au démarrage du système
fatma@ubuntu:~$ sudo ufw status
État : actif

Vers                       Action      De
----                       ------      --
9999/tcp                   ALLOW       Anywhere                  
9999/tcp (v6)              ALLOW       Anywhere (v6)             


fatma@ubuntu:~$ hostname -I
10.100.0.18 172.17.0.1 
fatma@ubuntu:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 24:b2:b9:7d:b1:2d brd ff:ff:ff:ff:ff:ff
    inet 10.100.0.18/19 brd 10.100.31.255 scope global dynamic noprefixroute wlp3s0
       valid_lft 602919sec preferred_lft 602919sec
    inet6 fe80::bae5:f7b6:c5e8:a33a/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 66:29:73:b6:6d:ce brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::6429:73ff:feb6:6dce/64 scope link 
       valid_lft forever preferred_lft forever
8: veth84aeb8a@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether aa:3c:08:e0:9d:54 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::a83c:8ff:fee0:9d54/64 scope link 
       valid_lft forever preferred_lft forever


fatma@ubuntu:~$ curl -I http://10.100.0.18:9999
HTTP/1.1 200 OK
Server: nginx/1.29.6
Date: Thu, 19 Mar 2026 10:05:17 GMT
Content-Type: text/html
Content-Length: 896
Last-Modified: Tue, 10 Mar 2026 15:29:07 GMT
Connection: keep-alive
ETag: "69b038c3-380"
Accept-Ranges: bytes
```

🌞 **Custom un peu le lancement du conteneur**

```bash
fatma@ubuntu:~$ mkdir -p ~/tp_docker
fatma@ubuntu:~$ cd ~/tp_docker

fatma@ubuntu:~/tp_docker$ nano meow.conf
server {
    listen 7777;
    root /var/www/tp_docker;
    index index.html;
}

fatma@ubuntu:~/tp_docker$ nano index.html                               
 <h1>OTTER SUPREMACY</h1>
 <img src="loutre.jpg">

fatma@ubuntu:~/tp_docker$ docker run -d \
  --name meow \
  --memory="512m" \
  -p 7777:7777 \
  -v $(pwd)/meow.conf:/etc/nginx/conf.d/meow.conf \
  -v $(pwd):/var/www/tp_docker \
  nginx
2f14991d08faf4def2b75309706d776c1c3caf184a60fef5fb12212b704687e5

fatma@ubuntu:~/tp_docker$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                                 NAMES
2f14991d08fa   nginx     "/docker-entrypoint.…"   45 seconds ago   Up 45 seconds   80/tcp, 0.0.0.0:7777->7777/tcp, [::]:7777->7777/tcp   meow
3d106d5b28c4   nginx     "/docker-entrypoint.…"   54 minutes ago   Up 54 minutes   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp               web
``` 

🌞 **Call me**

```bash
ENJOYYYY :)  http://10.100.0.18:7777/
``` 

# Part II : Images

## Construisez votre propre Dockerfile

🌞 **Construire votre propre image**

🌞 **OU ALORS**

🌞 **Dans les deux cas, j'attends juste votre `Dockerfile` dans le compte-rendu**

```bash
FROM ubuntu

RUN apt update -y && apt install -y apache2

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

COPY index.html /var/www/html/index.html

EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"]
``` 

# Part III : `docker-compose`

## 1. Intro

## 2. WikiJS

🌞 **Installez un WikiJS** en utilisant Docker

j'ai oublié de prendre les commandes sur le terminal donc les voici
```bash 
mkdir wikijs-project
cd wikijs-project
nano docker-compose.yml
docker compose up -d
docker compose ps
docker compose logs -f wiki
``` 

🌞 **Call me** when it's done

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/13567e89-a8a7-417f-b41d-e5c10ad734a3" />
 

## 3. Make your own meow

🌞 **Vous devez :**

```bash Dockerfile 
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8888
CMD ["python", "app.py"]
``` 

```bash dockercompose.yml                  
services:
  web:
    build: .
    ports:
      - "8888:8888"
    depends_on:
      - db
    environment:
      - REDIS_HOST=db

  db:
    image: redis:alpine
    expose:
      - "6379"
``` 

# Part IV : Docker security

## 1. Le groupe docker

🌞 **Prouvez que vous pouvez devenir `root`**

```bash
fatma@ubuntu:~$ docker run --rm -v /:/host_root alpine cat /host_root/etc/shadow
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
9e595aac14e0: Download complete 
caa817ad3aea: Download complete 
Digest: sha256:25109184c71bdad752c8312a8623239686a9a2071e8825f20acb8f2198c3f659
Status: Downloaded newer image for alpine:latest
root:*:20494:0:99999:7:::
daemon:*:20494:0:99999:7:::
bin:*:20494:0:99999:7:::
sys:*:20494:0:99999:7:::
sync:*:20494:0:99999:7:::
games:*:20494:0:99999:7:::
man:*:20494:0:99999:7:::
lp:*:20494:0:99999:7:::
mail:*:20494:0:99999:7:::
news:*:20494:0:99999:7:::
uucp:*:20494:0:99999:7:::
proxy:*:20494:0:99999:7:::
www-data:*:20494:0:99999:7:::
backup:*:20494:0:99999:7:::
list:*:20494:0:99999:7:::
irc:*:20494:0:99999:7:::
_apt:*:20494:0:99999:7:::
nobody:*:20494:0:99999:7:::
systemd-network:!*:20494::::::
systemd-timesync:!*:20494::::::
dhcpcd:!:20494::::::
messagebus:!:20494::::::
syslog:!:20494::::::
systemd-resolve:!*:20494::::::
uuidd:!:20494::::::
usbmux:!:20494::::::
tss:!:20494::::::
systemd-oom:!*:20494::::::
kernoops:!:20494::::::
whoopsie:!:20494::::::
dnsmasq:!:20494::::::
avahi:!:20494::::::
tcpdump:!:20494::::::
sssd:!:20494::::::
speech-dispatcher:!:20494::::::
cups-pk-helper:!:20494::::::
fwupd-refresh:!*:20494::::::
saned:!:20494::::::
geoclue:!:20494::::::
cups-browsed:!:20494::::::
hplip:!:20494::::::
gnome-remote-desktop:!*:20494::::::
polkitd:!*:20494::::::
rtkit:!:20494::::::
colord:!:20494::::::
gnome-initial-setup:!:20494::::::
gdm:!:20494::::::
nm-openvpn:!:20494::::::
fatma:$6$eiXKEoMiktH[...]2G10:20509:0:99999:7:::
``` 

## 2. Scan de vuln

🌞 **Utilisez Trivy**

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:0.49.1 image requarks/wiki:2

docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:0.49.1 image postgres:15-alpine

docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:0.49.1 image apache2

docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:0.49.1 image nginx:latest
``` 

## 3. Petit benchmark secu

🌞 **Utilisez l'outil Docker Bench for Security**

```bash
done :)
``` 
