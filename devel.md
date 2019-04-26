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

EF Core kódgenerálás létező adatbázisból:
A projekt .csproj fájljába be kell írni ezt:
```
<ItemGroup>
    <DotNetCliToolReference
        Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
        Version="1.0.0-msbuild3-final" />
</ItemGroup>
```

Majd kiadni az alábbi parancsokat (ha a provider megvan, akkor a középső nem kell):
```
dotnet restore
dotnet add Microsoft.EntityFrameworkCore.SqlServer
dotnet ef dbcontext scaffold -o Models "Server=<szerver>;Database=<adatbázis>;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```
