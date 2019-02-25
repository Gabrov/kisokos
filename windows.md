Másodpercek mutatásának bekapcsolása:
```
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced
```
Új kulcs: ShowSecondsInSystemClock DWORD (32 bit), az érték 1.  
  
VSS shadow-ok törlése:
```
vssadmin delete shadows /all
```

VSS terület átméretezése, hogy felszabduljon a hely (ha a shadow-ok törlését nem engedi):
```
vssadmin resize shadowstorage /for=D: /on=D: /maxsize=400MB
```

WinSxS mappa tisztítása:
```
@DISM.exe /online /Cleanup-Image /StartComponentCleanup
```
