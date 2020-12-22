## VSS
VSS shadow-ok törlése:
```
vssadmin delete shadows /all
```

VSS terület átméretezése, hogy felszabduljon a hely (ha a shadow-ok törlését nem engedi):
```
vssadmin resize shadowstorage /for=D: /on=D: /maxsize=400MB
```

## WinSxS
WinSxS mappa tisztítása:
```
DISM.exe /online /Cleanup-Image /StartComponentCleanup
sfc /scannow 
```

Így nem módosít semmit, csak ellenőriz:
```
Dism /Online /Cleanup-Image /ScanHealth
```

## Vegyes
Termék kulcs megnézése:
```
wmic path SoftwareLicensingService get OA3xOriginalProductKey
```

Windows Update engedélyezése:
```
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\WindowsUpdate
If value for DisableWindowsUpdateAccess is 1, modify it to 0.
```

Másodpercek mutatásának bekapcsolása:
```
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced
```  
Új kulcs: ShowSecondsInSystemClock DWORD (32 bit), az érték 1.  

Remote Desktop hozzáférés jelszó nélkül:
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa]
"LimitBlankPasswordUse"=dword:00000000
```
0 érték engedi, 1 tiltja.
