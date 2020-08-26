---
title: Azure Cosmos DB hizmeti kullanılamayan özel durumların sorunlarını giderme
description: Azure Cosmos DB hizmeti kullanılamayan özel durumları tanılama ve çözme hakkında bilgi edinin.
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 749592b778612c6903c9c15e336de3fb00978199
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88870876"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable-exceptions"></a>Hizmet kullanılamayan özel durumları tanılama ve sorun giderme Azure Cosmos DB
SDK Azure Cosmos DB bağlanamadık.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, hizmet kullanılamaz özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="the-required-ports-are-being-blocked"></a>Gerekli bağlantı noktaları engelleniyor
Tüm [gerekli bağlantı noktalarının](performance-tips-dotnet-sdk-v3-sql.md#networking) etkinleştirildiğini doğrulayın.

### <a name="client-side-transient-connectivity-issues"></a>İstemci tarafı geçici bağlantı sorunları
Hizmet kullanılamıyor özel durumlar, zaman aşımına neden olan geçici bağlantı sorunları olduğunda yüzeysel olabilir. Genellikle, bu senaryoya ilişkin yığın izlemesi bir `TransportException` hata içerir. Örneğin:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Sorunu çözmek için [istek zaman aşımı giderme adımlarını](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps) izleyin.

### <a name="service-outage"></a>Hizmet kesintisi
Devam eden bir sorun olup olmadığını görmek için [Azure durumunu](https://status.azure.com/status) kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-dot-net-sdk.md) .
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md)için performans yönergeleri hakkında bilgi edinin.