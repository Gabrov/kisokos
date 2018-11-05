LXC lemez méretének csökkentése (csak backup/restore-ral lehet jelenleg):

```
pct stop <id>
vzdump <id> -storage local -compress lzo
pct destroy <id>
pct restore <id> /var/lib/lxc/vzdump-lxc-<id>-....tar.lzo --rootfs local:<newsize>
```
