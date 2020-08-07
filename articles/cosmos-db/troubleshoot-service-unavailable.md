---
title: Azure Cosmos DB hizmeti kullanılamıyor özel durumuyla ilgili sorun giderme
description: Cosmos DB hizmet kullanılamıyor özel durumu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: b9c24ee3b94be86ccf9d27b928c42fc194800a3b
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87987384"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable"></a>Tanılama ve sorun giderme Azure Cosmos DB hizmeti kullanılamıyor
SDK Azure Cosmos DB hizmetine bağlanamadı.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, hizmet kullanılamaz özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="1-the-required-ports-are-being-blocked"></a>1. gerekli bağlantı noktaları engelleniyor
Tüm [gerekli bağlantı noktalarının](performance-tips-dotnet-sdk-v3-sql.md#networking) etkinleştirildiğini doğrulayın.

### <a name="2-client-side-transient-connectivity-issues"></a>2. istemci tarafı geçici bağlantı sorunları
Hizmet kullanılamıyor özel durumlar yüzeyi, zaman aşımına neden olan geçici bağlantı sorunları olduğunda yüzeyi taşıyabilir. Genellikle bu senaryoya ilişkin yığın izlemesi bir içerir `TransportException` , örneğin:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Sorunu çözmek için [istek zaman aşımı sorunlarını](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps) giderin.

### <a name="3-service-outage"></a>3. hizmet kesintisi
Devam eden bir sorun olup olmadığını görmek için [Azure durumunu](https://status.azure.com/status) denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin