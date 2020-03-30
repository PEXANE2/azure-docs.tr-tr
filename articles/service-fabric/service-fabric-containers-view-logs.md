---
title: Azure Hizmet Kumaşı'ndaki kapsayıcı günlüklerini görüntüleme
description: Service Fabric Explorer'ı kullanarak çalışan Servis Kumaşı konteyner hizmetleri için kapsayıcı günlüklerinin nasıl görüntülenebildiğini açıklar.
ms.topic: conceptual
ms.date: 05/15/2018
ms.openlocfilehash: c47a408b272f95dbfcf3d791c644bfeb52254a72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458192"
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Service Fabric konteyner hizmeti günlüklerini görüntüleme
Azure Service Fabric bir konteyner orkestratör ve linux [ve Windows kapsayıcıları](service-fabric-containers-overview.md)hem destekler.  Bu makalede, sorunları tanılamak ve sorun gidermek için çalışan bir kapsayıcı hizmetinin veya ölü bir kapsayıcının kapsayıcı günlüklerinin nasıl görüntülenebileceği açıklanmaktadır.

## <a name="access-the-logs-of-a-running-container"></a>Çalışan bir kapsayıcının günlüklerine erişin
Konteyner günlüklerine [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)kullanılarak erişilebilir.  Bir web tarayıcısında, cluster'ın yönetim bitiş noktasından Hizmet `http://mycluster.region.cloudapp.azure.com:19080/Explorer`Kumaş Explorer'ı aç.  

Kapsayıcı günlükleri, kapsayıcı hizmet örneğinin üzerinde çalışan küme düğümünde bulunur. Örnek olarak, [Linux Oylama örnek uygulamanın](service-fabric-quickstart-containers-linux.md)web ön uç konteyner günlükleri olsun. Ağaç görünümünde, **Küme**>**Uygulamaları**>**VotingType**>**kumaş:/Voting/azurevotefront'u**genişletin.  Sonra bölümü genişletin (d1aa737e-f22a-e347-be16-eec90be24bc1, bu örnekte) ve kapsayıcı küme düğümü *üzerinde*_lnxvm_0 çalıştığını görmek .

Ağaç görünümünde, **düğümleri**>**_lnxvm_0**>kumaşı genişleterek *_lnxvm_0* düğümündeki kod paketini**bulun:/Oylama**>**azurevotfrontPkg**>**Kod Paketleri**>**kodu**.  Ardından, **kapsayıcı günlüklerini** görüntülemek için Kapsayıcı Günlükleri seçeneğini belirleyin.

![Service Fabric platformu][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Ölü veya kilitlenen bir konteynerin günlüklerine erişin
v6.2'den başlayarak, [REST API'leri](/rest/api/servicefabric/sfclient-index) veya [Service Fabric CLI (SFCTL)](service-fabric-cli.md) komutlarını kullanarak ölü veya kilitlenmiş bir konteynerin günlüklerini de getirebilirsiniz.

### <a name="set-container-retention-policy"></a>Kapsayıcı bekletme ilkesi ayarlama
Service Fabric (6.1 veya üzeri sürümler), kapsayıcı başlatma hatalarının tanılanmasına yardımcı olmak için sonlandırılan veya başlatılamayan kapsayıcıların bekletilmesini destekler. Bu ilke, aşağıdaki kod parçacığında gösterildiği gibi **ApplicationManifest.xml** dosyasında ayarlanabilir:
```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
 ```

**ContainersRetentionCount** ayarı, başarısız olduğunda bekletilecek kapsayıcı sayısını belirtir. Negatif bir değer belirtilirse başarısız olan tüm kapsayıcılar bekletilir. **ContainersRetentionCount** özniteliği belirtilmediğinde, hiçbir kapsayıcı tutulmaz. **ContainersRetentionCount** özniteliği, kullanıcıların test ve üretim kümeleri için farklı değerler belirtebilmesi amacıyla Uygulama Parametrelerini destekler. Kapsayıcı hizmetinin diğer düğümlere taşınmasını önlemek için bu özellikler kullanılırken kapsayıcı hizmetinin belirli bir düğümü hedeflemesini sağlamak için yerleştirme kısıtlamaları kullanın. Bu özellik kullanılarak bekletilen tüm kapsayıcılar el ile kaldırılmalıdır.

**RunInteractive** ayarı Docker's `--interactive` ve `tty` [bayraklara](https://docs.docker.com/engine/reference/commandline/run/#options)karşılık gelir. Bu ayar bildirim dosyasında doğru olarak ayarlandığında, bu bayraklar kapsayıcıyı başlatmak için kullanılır.  

### <a name="rest"></a>REST
Çöken bir kapsayıcının günlüklerini almak için [Düğümde Dağıtılan Konteyner Günlüklerini Al](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) işlemini kullanın. Kapsayıcının üzerinde çalışan düğümün adını, uygulama adını, hizmet bildirimi adını ve kod paketi adını belirtin.  `&Previous=true` belirtin. Yanıt, kod paketi örneğinin ölü kapsayıcısının kapsayıcı günlüklerini içerir.

Uri isteği aşağıdaki formu vardır:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Örnek istek:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

200 Yanıt gövdesi:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Servis Kumaşı (SFCTL)
Kilitlenen bir kapsayıcının günlüklerini almak için [sfctl servis get-container-logs](service-fabric-sfctl-service.md) komutunu kullanın.  Kapsayıcının üzerinde çalışan düğümün adını, uygulama adını, hizmet bildirimi adını ve kod paketi adını belirtin. Bayrağı `--previous` belirtin.  Yanıt, kod paketi örneğinin ölü kapsayıcısının kapsayıcı günlüklerini içerir.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –-previous
```
Yanıt:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Linux konteyner uygulama öğretici oluştur](service-fabric-tutorial-create-container-images.md)üzerinden çalışın.
- [Servis Kumaşı ve konteynerler](service-fabric-containers-overview.md) hakkında daha fazla bilgi edinin

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
