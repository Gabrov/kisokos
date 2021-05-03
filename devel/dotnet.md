.NET Core SPA sablonok telepítése/frissítése:
```
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Windows szolgáltatás (https://dotnetcoretutorials.com/2019/12/07/creating-windows-services-in-net-core-part-3-the-net-core-worker-way/):
```
dotnet publish -r win-x64 -c Release
sc create TestService BinPath=C:\full\path\to\publish\dir\WindowsServiceExample.exe
```

Telepített betűtípusok listázása (pl. Linqpad-ból):
```
foreach (System.Drawing.FontFamily font in System.Drawing.FontFamily.Families)
	Console.WriteLine(font.Name);
```
