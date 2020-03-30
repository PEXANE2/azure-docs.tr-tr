---
title: Azure Media Services v3 ile akış uç noktalarını yönetme
description: Bu makalede, Azure Media Services v3 ile akış uç noktalarının nasıl yönetilen gösterildiği gösterilmiştir.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2020
ms.author: juliako
ms.openlocfilehash: 75ba2ad87eabd7ff6b0625ad95ab24a8ae58dd0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79461053"
---
# <a name="manage-streaming-endpoints-with--media-services-v3"></a>Medya Hizmetleri v3 ile akış uç noktalarını yönetme

Medya Hizmetleri hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan Bir Akış [Bitiş Noktası](streaming-endpoint-concept.md) eklenir. İçeriğinizi akışa başlamak ve [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik [şifrelemeden](content-protection-overview.md)yararlanmak için, içeriği aktarmak istediğiniz akış bitiş **noktasının Çalışan** durumunda olması gerekir.

Bu makalede, farklı teknolojileri kullanarak akış bitiş noktanızda [başlangıç](https://docs.microsoft.com/rest/api/media/streamingendpoints/start) komutunu nasıl çalıştırabileceğinizgösterilmektedir. 
 
> [!NOTE]
> Yalnızca Akış Bitiş Noktanız çalışırken faturalandırılırsınız.
    
## <a name="prerequisites"></a>Ön koşullar

Inceleme: 

* [Medya Hizmetleri kavramları](concepts-overview.md)
* [Akış Bitiş Noktası kavramı](streaming-endpoint-concept.md)
* [Dinamik paketleme](dynamic-packaging-overview.md)

## <a name="use-rest"></a>REST kullanma

```rest
POST https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/slitestmedia10/streamingEndpoints/myStreamingEndpoint1/start?api-version=2018-07-01
```

Daha fazla bilgi için bkz. 

* [Bir StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints/start) başvuru belgesi başlatın.
* Akış bitiş noktasını başlatmak eşzamanlı bir işlemdir. 

    Uzun süren işlemleri izleme hakkında bilgi [için, uzun süren işlemlere](media-services-apis-overview.md)bakın.
* Bu [Postacı koleksiyonu,](https://github.com/Azure-Samples/media-services-v3-rest-postman/blob/master/Postman/Media%20Services%20v3.postman_collection.json) akış bitiş noktasının nasıl başlatılalabildiğini de içeren birden çok REST işleminin örneklerini içerir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma 
 
1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure Medya Hizmetleri hesabınıza gidin.
1. Sol **bölmede, Akış Uç Noktaları'nı**seçin.
1. Başlatmak istediğiniz akış bitiş noktasını seçin ve ardından **Başlat'ı**seçin.

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

```cli
az ams streaming-endpoint start [--account-name]
                                [--ids]
                                [--name]
                                [--no-wait]
                                [--resource-group]
                                [--subscription]
```

Daha fazla bilgi için [az ams streaming-endpoint start'a](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest#az-ams-streaming-endpoint-start)bakın.

## <a name="use-sdks"></a>SDK’ları kullanma

### <a name="java"></a>Java
    
```java
if (streamingEndpoint != null) {
// Start The Streaming Endpoint if it is not running.
if (streamingEndpoint.resourceState() != StreamingEndpointResourceState.RUNNING) {
    manager.streamingEndpoints().startAsync(config.getResourceGroup(), config.getAccountName(), STREAMING_ENDPOINT_NAME).await();
}
```

[Java kod örneğinin](https://github.com/Azure-Samples/media-services-v3-java/blob/master/DynamicPackagingVODContent/StreamHLSAndDASH/src/main/java/sample/StreamHLSAndDASH.java#L128)tamamına bakın.

### <a name="net"></a>.NET

```csharp
StreamingEndpoint streamingEndpoint = await client.StreamingEndpoints.GetAsync(config.ResourceGroup, config.AccountName, DefaultStreamingEndpointName);

if (streamingEndpoint != null)
{
    if (streamingEndpoint.ResourceState != StreamingEndpointResourceState.Running)
    {
        await client.StreamingEndpoints.StartAsync(config.ResourceGroup, config.AccountName, DefaultStreamingEndpointName);
    }
```

[.NET kod örneğinin](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/master/DynamicPackagingVODContent/StreamHLSAndDASH/Program.cs#L112)tamamına bakın.

---

## <a name="next-steps"></a>Sonraki adımlar

* [Medya Hizmetleri v3 OpenAPI Belirtimi (Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01)
* [Akış Bitiş Noktası işlemleri](https://docs.microsoft.com/rest/api/media/streamingendpoints)
