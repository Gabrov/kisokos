EF Core kódgenerálás létező adatbázisból:
A projekt .csproj fájljába be kell írni ezt, ha nem global tool-ként van telepítve a dotnet-ef:
```
<ItemGroup>
    <DotNetCliToolReference
        Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
        Version="1.0.0-msbuild3-final" />
</ItemGroup>
```

Majd kiadni az alábbi parancsokat (ha a provider megvan, akkor a középső nem kell):
```
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet ef dbcontext scaffold -o Models "Server=<szerver>;Database=<adatbázis>;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

```
dotnet add package Pomelo.EntityFrameworkCore.MySql
dotnet ef dbcontext scaffold "Server=localhost;Database=ef;User=root;Password=123456;TreatTinyAsBoolean=true;" "Pomelo.EntityFrameworkCore.MySql"
```

```
dotnet tool list -g
dotnet tool update dotnet-ef -g
```
