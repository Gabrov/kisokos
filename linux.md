## SSH törési próbálkozások ellenőrzése

#### Brute force:
```
grep sshd.\*Failed /var/log/auth.log | less
```

#### Tanúsítvány:
```
grep sshd.\*Unable /var/log/auth.log | less
```

#### Csatlakozási kísérlet autentikáció nélkül (pl. port scan):
```
grep sshd.*Did /var/log/auth.log | less
```

## Proxy beállítása
#### /etc/profile.d/proxy.sh  
```bash
export http_proxy=http://192.168.0.1:3128
```

#### /etc/apt/apt.conf.d/99HttpProxy
```bash
Acquire::http::Proxy "http://192.168.0.1:3128";
```

#### /etc/wgetrc
```bash
http_proxy = http://192.168.0.1:3128
```

#### Git
```bash
git config --global http.proxy http://username:password@host:port
git config --global https.proxy http://username:password@host:port
```

~/.gitconfig
```bash
[http]
        proxy = http://username:password@host:port
[https]
        proxy = http://username:password@host:port
```

#### NPM
```bash
npm config set proxy http://proxy.company.com:8080
npm config set https-proxy http://proxy.company.com:8080
```

~/.npmrc
```bash
proxy=http://username:password@host:port
https-proxy=http://username:password@host:port
https_proxy=http://username:password@host:port
```

#### Yarn
```bash
yarn config set proxy http://username:password@host:port
yarn config set https-proxy http://username:password@host:port
```

#### Docker
```bash
$ sudo mkdir -p /etc/systemd/system/docker.service.d
```
/etc/systemd/system/docker.service.d/http-proxy.conf  
```bash
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80/" "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```

/etc/systemd/system/docker.service.d/https-proxy.conf  
```bash
[Service]
Environment="HTTPS_PROXY=https://proxy.example.com:80/" "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker

$ systemctl show --property=Environment docker
Environment=HTTP_PROXY=http://proxy.example.com:80/

$ systemctl show --property=Environment docker
Environment=HTTPS_PROXY=https://proxy.example.com:443/
```

docker-machine (lehet szerkeszteni a ~/.docker/machine/machines/default/config.json fájlt is)
```bash
docker-machine create -d virtualbox \
    --engine-env HTTP_PROXY=http://username:password@host:port \
    --engine-env HTTPS_PROXY=http://username:password@host:port \
    default
```


### ext4 root tárhely beállítása
A Linux alapból 5% tárhelyet lefoglal a root felhasználó és a rendszer szolgáltatások
számára, így tele lemeznél is azok még tudnak csináni valamit.

A foglalás kikapcsolása:
```bash
sudo tune2fs -m 0 /dev/sdb1
```

Ellenőrzés:
```bash
sudo tune2fs -l /dev/sdb1 | grep ‘Reserved block count’
```
