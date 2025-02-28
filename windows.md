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

Ellenőrzés:

```
dism /Online /Cleanup-Image /AnalyzeComponentStore
Dism /Online /Cleanup-Image /ScanHealth
```

WinSxS mappa tisztítása:

```
DISM.exe /online /Cleanup-Image /StartComponentCleanup
sfc /scannow
```

## Windows Update

Ha valamelyik frissítés hibára fut (legtöbbször 0x80248007), akkor ki kell kapcsolni az update-t, törölni a cache-t, majd visszakapcsolni és újra letölteni a frissítés(eke)t.

Cache törlés:

```shell
net stop wuauserv
net stop cryptSvc
net stop bits
net stop msiserver
ren C:\Windows\SoftwareDistribution SoftwareDistribution.old
ren C:\Windows\System32\catroot2 catroot2.old
net start wuauserv
net start cryptSvc
net start bits
net start msiserver
```

Windows Update komponensek visszaállítása:

```shell
net stop bits
net stop cryptsvc
net stop wuauserv
net stop appidsvc
net stop cryptsvc
net stop wscsvc
ren C:\Windows\System32\catroot2 catroot2.old
ren C:\Windows\System32\SoftwareDistribution SoftwareDistribution.old
ren C:\Windows\System32\appmgmt appmgmt.old
net start bits
net start cryptsvc
net start wuauserv
net start appidsvc
net start cryptsvc
net start wscsvc
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

Nem elérhető hálózati meghajtók csatolásának törlése (ha intézőből/parancssorból nem megy):

```
HKEY_CURRENT_USER\Network
```

Itt kell a megfelelő kulcso(ka)t törölni.

Foglalt/fentartott portok listája:

```
netsh int ipv4 show excludedportrange protocol=tcp
```

Portok felszabadítása (nem engedi a Win):

```
netsh int ipv4 delete excludedportrange protocol=tcp startport=1540 numberofports=100
```

Windows Search adatbázis töredezettség mentesítése (shrinkelése):

```
Sc config wsearch start=disabled
Net stop wsearch

EsentUtl.exe /d %AllUsersProfile%\Microsoft\Search\Data\Applications\Windows\Windows.edb

Sc config wsearch start=delayed-auto

Net start wsearch
```

PowerShell scriptek futtatásának engedélyezése (pl. NodeJS globális parancsokhoz):

```
Get-ExecutionPolicy -List
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Házirendek: AllSigned, Bypass, Default (Restricted a Windows ügyfelekhez, RemoteSigned a Windows kiszolgálókhoz), RemoteSigned, Restricted, Undefined, Unrestricted

Külső meghajtó mountolása WSL2 alá (rendszergaza jog kell hozzá):

```
wmic diskdrive list brief
wsl --mount \\.\PHYSICALDRIVE10 --partition 0
wsl --unmount \\.\PHYSICALDRIVE10
```

FortiClient (vagy bármely más program törlése, ha a programok eltávolítása menüből nem hagyja magát):

```
wmic product where "name like 'Forti%%'" call uninstall /nointeractive
```

Windows Terminal indítása osztott panelekkel:

```
wt split-pane -V; move-focus left; split-pane -H; move-focus right; split-pane -H
```
