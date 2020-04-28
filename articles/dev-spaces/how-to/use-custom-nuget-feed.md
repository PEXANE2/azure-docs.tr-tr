---
title: Özel NuGet akışı kullanma
services: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/17/2019
ms.topic: conceptual
description: Azure dev alanında NuGet paketlerine erişmek ve bunları kullanmak için özel bir NuGet akışı kullanın.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
manager: gwallace
ms.openlocfilehash: 39984a3b3a1be64a497fb8088559ccfcdee4f1c6
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74325737"
---
# <a name="use-a-custom-nuget-feed-with-azure-dev-spaces"></a>Azure Dev Spaces ile özel bir NuGet akışı kullanma

Bir NuGet akışı, paket kaynaklarını bir projeye dahil etmek için uygun bir yol sağlar. Bağımlılıkların Docker kapsayıcısına düzgün şekilde yüklenmesi için Azure Dev Spaces bu akışa erişmesi gerekir.

## <a name="set-up-a-nuget-feed"></a>NuGet akışı ayarlama

Düğümün altındaki `*.csproj` dosyaya bağımlılığı için bir [paket başvurusu](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) ekleyin. `PackageReference` Örneğin:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

Proje klasöründe bir [NuGet. config](https://docs.microsoft.com/nuget/reference/nuget-config-file) dosyası oluşturun ve NuGet akışınız için `packageSources` ve `packageSourceCredentials` bölümlerini ayarlayın. `packageSources` Bölüm, aks kümenizdeki erişilebilir olması gereken akış URL 'nizi içerir. , `packageSourceCredentials` Akışa erişim için kimlik bilgileridir. Örneğin:

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

Dockerfiles 'ı, `NuGet.Config` dosyayı görüntüye kopyalamak için güncelleştirin. Örneğin:

```console
COPY ["<project folder>/NuGet.Config", "./NuGet.Config"]
```

> [!TIP]
> Windows, `NuGet.Config`, `Nuget.Config`, ve `nuget.config` hepsi geçerli dosya adları olarak çalışmaktadır. Linux 'ta, bu `NuGet.Config` dosya için yalnızca geçerli bir dosya adıdır. Azure Dev Spaces Docker ve Linux kullandığından, bu dosyanın adlandırılmış `NuGet.Config`olması gerekir. Adlandırmayı el ile veya çalıştırarak `dotnet restore --configfile nuget.config`giderebilirsiniz.


Git kullanıyorsanız, sürüm denetiminde NuGet akışınız için kimlik bilgilerine sahip olmanız gerekmez. Dosyanın sürüm denetimine `.gitignore` eklenmemesi için projenize öğesine ekleyin `NuGet.Config` `NuGet.Config` Azure Dev Spaces kapsayıcı görüntüsü oluşturma işlemi sırasında bu dosyaya ihtiyaç duyar, ancak varsayılan olarak, ' de `.gitignore` ve `.dockerignore` eşitleme sırasında tanımlanan kurallara uyar. Varsayılanı değiştirmek ve Azure Dev Spaces `NuGet.Config` dosyayı eşitlemesine izin vermek için, `azds.yaml` dosyayı güncelleştirin:

```yaml
build:
useGitIgnore: true
ignore:
- "!NuGet.Config"
```

Git kullanmıyorsanız, bu adımı atlayabilirsiniz.

Visual Studio Code veya Visual Studio `azds up` `F5` 'da bir sonraki sefer çalıştırdığınızda veya bir kez daha karşılaşışınızda, Azure dev Spaces `NuGet.Config` dosyayı eşitleyecek ve paket bağımlılıklarını yükleyecek şekilde kullanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[NuGet ve nasıl çalıştığı](https://docs.microsoft.com/nuget/what-is-nuget)hakkında daha fazla bilgi edinin.