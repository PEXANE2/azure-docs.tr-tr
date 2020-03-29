---
title: Azure Hizmet Kumaşı'nda config dosyalarını parametreize
description: Birden çok ortamı yönetirken yararlı bir teknik olan Service Fabric'te yapılandırma dosyalarını nasıl parametreleştirebilirsiniz öğrenin.
author: mikkelhegn
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: 4e96a732cffd70b0a5c24e7ebafe214297a72720
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644639"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric'te yapılandırma dosyaları nasıl parametreye kullanılır?

Bu makalede, Hizmet Kumaşı'nda bir yapılandırma dosyasını nasıl parametrenize aktarabileceğiniz gösterilmektedir.  Birden çok ortam için uygulamaları yönetmetemel kavramlarına zaten aşina değilseniz, [birden çok ortam için uygulamaları yönet'i](service-fabric-manage-multiple-environment-app-configuration.md)okuyun.

## <a name="procedure-for-parameterizing-configuration-files"></a>Yapılandırma dosyalarını parametreleme yordamı

Bu örnekte, uygulama dağıtımınızdaki parametreleri kullanarak bir yapılandırma değerini geçersiz kılarsınız.

1. Hizmet projenizde * \<MyService>\PackageRoot\Config\Settings.xml* dosyasını açın.
1. Aşağıdaki XML'yi ekleyerek, örneğin önbellek boyutu 25'e eşit bir yapılandırma parametre adı ve değeri ayarlayın:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Dosyayı kaydedin ve kapatın.
1. * \<MyApplication>\ApplicationPackageRoot\ApplicationManifest.xml* dosyasını açın.
1. ApplicationManifest.xml dosyasında, öğedeki bir parametre `Parameters` ve varsayılan değeri bildirin.  Parametre adının hizmetin adını içermesi önerilir (örneğin, "MyService").

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. ApplicationManifest.xml dosyasının `ServiceManifestImport` bölümünde, yapılandırma `ConfigOverrides` paketine, bölüme ve parametreye atıfta bulunarak a ve `ConfigOverride` öğe ekleyin.

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> ConfigOverride eklediğinizde, Service Fabric her zaman uygulama parametrelerini veya uygulama bildiriminde belirtilen varsayılan değeri seçer.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio'da bulunan diğer uygulama yönetimi özellikleri hakkında daha fazla bilgi için Visual [Studio'daki Hizmet Kumaşı uygulamalarınızı yönet'e](service-fabric-manage-application-in-visual-studio.md)bakın.
