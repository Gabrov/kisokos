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
