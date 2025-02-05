Docker for Windows futtatásakor a docker konténerek belső Hyper-V-s hálózata nem érhető el alapból. Be kell állítani a route-olást
a Hyper-V alapértelmezett és privát adaptere között:

```PowerShell
route /P add 172.0.0.0 MASK 255.0.0.0 10.0.75.2
```

Docker konténer IP-jének lekérdezése:

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```

Hasznos aliasok:

```
alias docker-upgrade="docker images|cut -f1 -d' '|sort -u|grep -v elbandi|xargs --no-run-if-empty -n1 docker pull"
alias docker-remove="docker images -q --filter 'dangling=true'|xargs --no-run-if-empty docker rmi"
alias docker-clean="docker ps -a | egrep \"Exited|Dead\" | awk '{print \$1}'|xargs --no-run-if-empty docker rm"
```

Ha a konténer indításakor ez az üzenet jön:

```
bind: A szoftvercsatornához a hozzáférési engedélyeket megsértő módon történt hozzáférés.
```

A *Host Network Service* újraindítása segíthet.

```
net stop hns
net start hns
```

Konténer indítási parancsok:

Transmission

```
docker run -d \
  --restart always \
  --name=transmission \
  -e PUID=1000 \
  -e PGID=1000 \
  -e UMASK=077 \
  -e TZ=Europe/Budapest \
  -e USER=<felhasználó> \
  -e PASS=<jelszó> \
  -e HOST_WHITELIST=<IP lista> \
  -p 9091:9091 \
  -p 51413:51413 \
  -p 51413:51413/udp \
  -v ~/docker/torrent/config:/config \
  -v ~/docker/torrent/downloads:/downloads \
  -v ~/docker/torrent/watch:/watch \
  lscr.io/linuxserver/transmission:latest
```

MS SQL

```
docker run -d \
  --name mssql \
  --hostname mssql \
  -p 1433:1433 \
  -e PUID=1000 \
  -e PGID=1000 \
  -e "ACCEPT_EULA=Y" \
  -e "MSSQL_SA_PASSWORD=SaPassword.123" \
  -v ~/docker/mssql/data:/var/opt/mssql/data \
  -v ~/docker/mssql/log:/var/opt/mssql/log \
  -v ~/docker/mssql/secrets:/var/opt/mssql/secrets \
  --restart unless-stopped \
  mcr.microsoft.com/mssql/server:latest
```

Portainer

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

```
docker run --rm --pull=always -v portainer_data:/data portainer/helper-reset-password
```
