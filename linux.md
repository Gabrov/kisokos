SSH törési próbálkozások ellenőrzése

Brute force:
```
grep sshd.\*Failed /var/log/auth.log | less
```

Tanúsítvány:
```
grep sshd.\*Unable /var/log/auth.log | less
```

Csatlakozási kísérlet autentikáció nélkül (pl. port scan):
```
grep sshd.*Did /var/log/auth.log | less
```
