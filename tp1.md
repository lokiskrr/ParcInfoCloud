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

```bash

``` 

🌞 **Call me** when it's done

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/13567e89-a8a7-417f-b41d-e5c10ad734a3" />
 

## 3. Make your own meow

🌞 **Vous devez :**

```bash

``` 

# Part IV : Docker security

## 1. Le groupe docker

🌞 **Prouvez que vous pouvez devenir `root`**

```bash

``` 

## 2. Scan de vuln

🌞 **Utilisez Trivy**

```bash

``` 

## 3. Petit benchmark secu

🌞 **Utilisez l'outil Docker Bench for Security**

```bash

``` 
