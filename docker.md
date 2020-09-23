Docker for Windows futtatásakor a docker konténerek belső Hyper-V-s hálózata nem érhető el alapból. Be kell állítani a route-olást
a Hyper-V alapértelmezett és privát adaptere között:

```PowerShell
route /P add 172.0.0.0 MASK 255.0.0.0 10.0.75.2
```

Docker konténer IP-jének lekérdezése:

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```