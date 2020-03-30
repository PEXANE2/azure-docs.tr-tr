---
title: Parametreleri kullanarak bir hizmetin bağlantı noktası numarasını belirtin
description: Hizmet Kumaşı'ndaki bir uygulamanın bağlantı noktasını belirtmek için parametrelerin nasıl kullanılacağını gösterir
author: mikkelhegn
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: a53626b8fd362397ba89df30b099fa3c9ff7b0a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609868"
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Hizmet Kumaşı'ndaki parametreleri kullanarak bir hizmetin bağlantı noktası numarası nasıl belirtilir?

Bu makalede, Visual Studio kullanarak Hizmet Kumaşparametreleri kullanarak bir hizmetin bağlantı noktası numarasını nasıl belirtin gösterir.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Parametreleri kullanarak bir hizmetin bağlantı noktası numarasını belirtme yordamı

Bu örnekte, bir parametre kullanarak asp.net çekirdek web API'nizin bağlantı noktası numarasını ayarlarsınız.

1. Visual Studio'yu açın ve yeni bir Service Fabric uygulaması oluşturun.
1. Stateless ASP.NET Core şablonunu seçin.
1. Web API'sini seçin.
1. ServiceManifest.xml dosyasını açın.
1. Hizmetiniz için belirtilen bitiş noktasının adını not edin. `ServiceEndpoint` varsayılan değerdir.
1. ApplicationManifest.xml dosyasını açın
1. Öğede `ServiceManifestImport` ServiceManifest.xml dosyanızdaki bitiş noktasına atıfta bulunarak yeni `RessourceOverrides` bir öğe ekleyin.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. Öğede, `Endpoint` artık bir parametre kullanarak herhangi bir öznitelik geçersiz kılabilirsiniz. Bu örnekte, `Port` kare ayraçkullanarak bir parametre adı belirtir ve ayarlarsınız - örneğin,`[MyWebAPI_PortNumber]`

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint" Port="[MyWebAPI_PortNumber]"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. ApplicationManifest.xml dosyasında ise, öğedeki parametreyi `Parameters` belirtirsiniz

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" />
      </Parameters>
    ```

1. Ve bir tanımlamak`DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="8080" />
      </Parameters>
    ```

1. ApplicationParameters klasörünü ve `Cloud.xml` dosyayı açma
1. Uzak bir kümeye yayımlanırken kullanılacak farklı bir bağlantı noktası belirtmek için, bağlantı noktası numarasını içeren parametreyi bu dosyaya ekleyin.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" Value="80" />
      </Parameters>
    ```

Cloud.xml yayımlama profilini kullanarak uygulamanızı Visual Studio'dan yayınlarken, hizmetiniz 80 no'lu bağlantı noktasını kullanacak şekilde yapılandırılır. Uygulamayı MyWebAPI_PortNumber parametresini belirtmeden dağılarsanız, hizmet 8080 portu kullanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede tartışılan temel kavramlardan bazıları hakkında daha fazla bilgi edinmek [için, birden çok ortam makalesi için uygulamaları yönet'e](service-fabric-manage-multiple-environment-app-configuration.md)bakın.

Visual Studio'da bulunan diğer uygulama yönetimi özellikleri hakkında daha fazla bilgi için Visual [Studio'daki Hizmet Kumaşı uygulamalarınızı yönet'e](service-fabric-manage-application-in-visual-studio.md)bakın.
