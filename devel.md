.NET Core SPA sablonok telepítése/frissítése:
```
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Git almodul klónozás:
```
git clone --recurse-submodules -j8 git://github.com/foo/bar.git
```
(-j8 opcionális, 2.8-tól, párhuzamosan 8 almodult tölt le)
```
git clone git://github.com/foo/bar.git
cd bar
git submodule update --init --recursive
```
