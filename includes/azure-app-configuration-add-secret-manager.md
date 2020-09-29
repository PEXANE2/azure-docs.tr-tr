---
ms.openlocfilehash: c3c5b8ef94b507cad433e587c9ebfc2ec16c0ff9
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91440398"
---
## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Gizli dizi Yöneticisi olarak adlandırılan bir araç, geliştirme çalışması için hassas verileri proje ağacınızdaki dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur. ASP.NET Core projesinde gizli yöneticinin kullanımını etkinleştirmek için aşağıdaki adımları izleyin:

#### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

Projenin kök dizinine gidin ve projede gizli dizileri etkinleştirmek için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet user-secrets init
```

`UserSecretsId`GUID içeren bir öğe *. csproj* dosyasına eklenir:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    
    <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

</Project>
```

#### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

1. *. Csproj* dosyasını açın.

1. `UserSecretsId`Burada gösterildiği gibi *. csproj* dosyasına bir öğesi ekleyin. Aynı GUID 'i kullanabilir veya bu değeri kendi kendinizinkini kullanarak değiştirebilirsiniz.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    
        <PropertyGroup>
            <TargetFramework>netcoreapp2.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
        <ItemGroup>
            <PackageReference Include="Microsoft.AspNetCore.App" />
            <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
        </ItemGroup>
    
    </Project>
    ```
    
1. *. Csproj* dosyasını kaydedin.

---

> [!TIP]
> Gizli yönetici hakkında daha fazla bilgi edinmek için bkz. [ASP.NET Core geliştirme sırasında uygulama gizli dizileri Için güvenli depolama](/aspnet/core/security/app-secrets).
