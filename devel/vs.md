## Visual Studio

Bár nem javasolt, de a csomag cache kikapcsolható (mindent netről tölt le és utána törli):
```
"%ProgramFiles(x86)%\Microsoft Visual Studio\Installer\vs_installer.exe" --nocache
```

Ha törölve lett, a javításhoz a visszaállítása:
```
"%ProgramFiles(x86)%\Microsoft Visual Studio\Installer\vs_installer.exe" repair --passive --norestart --cache
```

Hard link-kel (junction) át lehet tenni máshova:
```
mklink /J oldpath newpath
```
