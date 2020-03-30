---
title: Hizmetler için ortam değişkenlerini belirtin
description: Service Fabric'deki uygulamalar için ortam değişkenlerinin nasıl kullanılacağını gösterir
author: mikkelhegn
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f4c4f2a1c140e3d0f181c4fd55482056f9f91b62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614324"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Service Fabric'teki hizmetler için ortam değişkenleri nasıl belirtilir?

Bu makalede, Hizmet Kumaşı'ndaki bir hizmet veya kapsayıcı için ortam değişkenlerinin nasıl belirtilen bir şekilde belirtilen.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Hizmetler için ortam değişkenlerini belirtme yordamı

Bu örnekte, bir kapsayıcı için bir ortam değişkeni ayarlarsınız. Makalede, zaten bir uygulama ve hizmet bildirimi var varsayar.

1. ServiceManifest.xml dosyasını açın.
2. Öğede, her ortam değişkeni `CodePackage` için yeni `EnvironmentVariables` bir öğe ve öğe `EnvironmentVariable` ekleyin.

    ```xml
    <CodePackage Name="MyCode" Version="CodeVersion1">
            ...
            <EnvironmentVariables>
                  <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
                  <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentVariables>
    </CodePackage>
    ```

   Ortam değişkenleri uygulama bildiriminde geçersiz kılınabilir.

3. Uygulama bildirimindeki ortam değişkenlerini geçersiz kılmak `EnvironmentOverrides` için öğeyi kullanın.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="specifying-environment-variables-dynamically-using-docker-compose"></a>Docker Compose kullanarak ortam değişkenlerini dinamik olarak belirtme

Service Fabric Dağıtım [için Docker Compose kullanma](service-fabric-docker-compose.md#supported-compose-directives)yeteneğini destekler. Dosya oluşturma, ortam değişkenlerini kabuktan kaynaklayabilir. Bu davranış, istenen ortam değerlerini dinamik olarak değiştirmek için kullanılabilir:

```yml
environment:
  - "hostname:${hostname}"
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede tartışılan temel kavramlardan bazıları hakkında daha fazla bilgi edinmek [için, birden çok ortam makalesi için uygulamaları yönet'e](service-fabric-manage-multiple-environment-app-configuration.md)bakın.

Visual Studio'da bulunan diğer uygulama yönetimi özellikleri hakkında daha fazla bilgi için Visual [Studio'daki Hizmet Kumaşı uygulamalarınızı yönet'e](service-fabric-manage-application-in-visual-studio.md)bakın.
