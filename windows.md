VSS shadow-ok törlése:
```
vssadmin delete shadows /all
```

VSS terület átméretezése, hogy felszabduljon a hely (ha a shadow-ok törlését nem engedi):
```
vssadmin resize shadowstorage /for=D: /on=D: /maxsize=400MB
```