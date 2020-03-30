---
title: Özel NuGet akışı kullanma
services: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/17/2019
ms.topic: conceptual
description: Azure Geliştirme Alanı'nda NuGet paketlerine erişmek ve bunları kullanmak için özel bir NuGet akışı kullanın.
keywords: Docker, Kubernetes, Azure, AKS, Azure Konteyner Hizmeti, konteynerler
manager: gwallace
ms.openlocfilehash: 39984a3b3a1be64a497fb8088559ccfcdee4f1c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325737"
---
# <a name="use-a-custom-nuget-feed-with-azure-dev-spaces"></a>Azure Dev Spaces ile özel bir NuGet akışı kullanma

NuGet beslemesi, paket kaynaklarını projeye dahil etmek için kullanışlı bir yol sağlar. Bağımlılıkların Docker kapsayıcısına düzgün şekilde yüklenmesi için Azure Geliştirme Alanları'nın bu akışa erişmesi gerekir.

## <a name="set-up-a-nuget-feed"></a>NuGet akışı ayarlama

Düğüm [package reference](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) altında `*.csproj` dosyada bağımlılığıiçin bir paket başvurusu ekleyin. `PackageReference` Örnek:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

Proje klasöründe bir [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file) dosyası `packageSources` oluşturun `packageSourceCredentials` ve NuGet akışınızın bölümlerini ayarlayın. Bölüm, `packageSources` AKS kümenizden erişilebilen özet url'nizi içerir. Özet `packageSourceCredentials` akışına erişmek için kimlik bilgileri vardır. Örnek:

```xml
<packageSources>
    <add key="Contoso" value="https://contoso.com/packages/" />
</packageSources>

<packageSourceCredentials>
    <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="ClearTextPassword" value="33f!!lloppa" />
    </Contoso>
</packageSourceCredentials>
```

Dosyayı görüntüye kopyalamak için Dockerdosyalarınızı güncelleştirin. `NuGet.Config` Örnek:

```console
COPY ["<project folder>/NuGet.Config", "./NuGet.Config"]
```

> [!TIP]
> Windows'da `NuGet.Config` `Nuget.Config`ve `nuget.config` tüm geçerli dosya adları olarak çalışır. Linux'ta `NuGet.Config` yalnızca bu dosya için geçerli bir dosya adı verilir. Azure Dev Spaces Docker ve Linux kullandığından, bu dosyanın adı `NuGet.Config`verilmelidir. Adlandırmayı el ile veya çalıştırarak `dotnet restore --configfile nuget.config`düzeltebilirsiniz.


Git kullanıyorsanız, sürüm denetiminde NuGet akışınızın kimlik bilgilerine sahip olmamalıdır. Dosyanın `.gitignore` sürüm denetimine eklenmemesi için projeniz için ekleyin. `NuGet.Config` `NuGet.Config` Azure Dev Spaces kapsayıcı görüntü oluşturma işlemi sırasında bu dosyaya ihtiyaç duyar, `.gitignore` `.dockerignore` ancak varsayılan olarak eşitleme sırasında ve sırasında tanımlanan kurallara uyar. Varsayılanı değiştirmek ve Azure Dev Spaces'in `NuGet.Config` dosyayı `azds.yaml` eşitlemesine izin vermek için dosyayı güncelleştirin:

```yaml
build:
useGitIgnore: true
ignore:
- "!NuGet.Config"
```

Git kullanmıyorsanız, bu adımı atlayabilirsiniz.

Visual Studio Code `azds up` veya `F5` Visual Studio'da bir sonraki kez çalıştırdığınızda veya vurduğunuzda, Azure Dev Spaces dosyayı `NuGet.Config` paket bağımlılıklarını yüklemek için kullanır.

## <a name="next-steps"></a>Sonraki adımlar

[NuGet ve nasıl çalıştığı](https://docs.microsoft.com/nuget/what-is-nuget)hakkında daha fazla bilgi edinin.