---
title: Azure Service Fabric yapılandırma dosyalarını Parametreleştirme
description: Birden çok ortamı yönetirken yararlı bir tekniktir Service Fabric yapılandırma dosyalarını nasıl parametreleyeceğinizi öğrenin.
author: mikkelhegn
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: 4e96a732cffd70b0a5c24e7ebafe214297a72720
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644639"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Service Fabric 'de yapılandırma dosyalarını Parametreleştirme

Bu makalede, Service Fabric bir yapılandırma dosyasının nasıl parametreleştiriyapılacağı gösterilir.  Birden çok ortam için uygulamaları yönetmeye yönelik temel kavramlardan daha önce alışık değilseniz, [birden çok ortam için uygulamaları yönetme](service-fabric-manage-multiple-environment-app-configuration.md)makalesini okuyun.

## <a name="procedure-for-parameterizing-configuration-files"></a>Yapılandırma dosyalarını parametreize etme yordamı

Bu örnekte, uygulama dağıtımınızdaki parametreleri kullanarak bir yapılandırma değerini geçersiz kılarsınız.

1. Hizmet projenizde *\<hizmetim > \PackageRoot\Config\Settings.xml* dosyasını açın.
1. Aşağıdaki XML 'i ekleyerek bir yapılandırma parametresi adı ve değeri (örneğin, önbellek boyutu 25 ' e eşit) ayarlayın:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Dosyayı kaydedin ve kapatın.
1. *\<MyApplication > \ApplicationPackageRoot\ApplicationManifest.xml* dosyasını açın.
1. ApplicationManifest. xml dosyasında, `Parameters` öğesinde bir parametre ve varsayılan değer bildirin.  Parametre adının hizmetin adını içermesi önerilir (örneğin, "hizmetim").

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. ApplicationManifest. xml dosyasının `ServiceManifestImport` bölümünde yapılandırma paketine, bölümüne ve parametresine başvurarak bir `ConfigOverrides` ve `ConfigOverride` öğesi ekleyin.

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
> Bir ConfigOverride eklediğinizde Service Fabric her zaman uygulama bildiriminde belirtilen uygulama parametrelerini veya varsayılan değeri seçer.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio 'da kullanılabilen diğer uygulama yönetimi özellikleri hakkında daha fazla bilgi için bkz. [Visual Studio 'da Service Fabric uygulamalarınızı yönetme](service-fabric-manage-application-in-visual-studio.md).
