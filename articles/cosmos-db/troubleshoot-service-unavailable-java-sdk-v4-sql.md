---
title: Java v4 SDK ile Azure Cosmos DB hizmeti kullanılamayan özel durumların sorunlarını giderme
description: Java v4 SDK ile Azure Cosmos DB hizmeti kullanılamayan özel durumları tanılamayı ve gidermeyi öğrenin.
author: kushagrathapar
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.date: 10/28/2020
ms.author: kuthapar
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 03c899307b00010ca87b279ed720b92946d3673c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93340099"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-java-v4-sdk-service-unavailable-exceptions"></a>Java v4 SDK hizmeti kullanılamayan özel durumları tanılama ve sorun giderme Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]
Java v4 SDK Azure Cosmos DB bağlanamadık.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, hizmet kullanılamaz özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="the-required-ports-are-being-blocked"></a>Gerekli bağlantı noktaları engelleniyor
Tüm [gerekli bağlantı noktalarının](sql-sdk-connection-modes.md#service-port-ranges) etkinleştirildiğini doğrulayın.

### <a name="client-side-transient-connectivity-issues"></a>İstemci tarafı geçici bağlantı sorunları
Hizmet kullanılamıyor özel durumlar, zaman aşımına neden olan geçici bağlantı sorunları olduğunda yüzeysel olabilir. Genellikle, bu senaryoya ilişkin yığın izleme, `ServiceUnavailableException` Tanılama ayrıntılarıyla bir hata içerecektir. Örnek:

```java
Exception in thread "main" ServiceUnavailableException{userAgent=azsdk-java-cosmos/4.6.0 Linux/4.15.0-1096-azure JRE/11.0.8, error=null, resourceAddress='null', requestUri='null', statusCode=503, message=Service is currently unavailable, please retry after a while. If this problem persists please contact support.: Message: "" {"diagnostics"}
```

Sorunu çözmek için [istek zaman aşımı giderme adımlarını](troubleshoot-request-timeout-java-sdk-v4-sql.md#troubleshooting-steps) izleyin.

### <a name="service-outage"></a>Hizmet kesintisi
Devam eden bir sorun olup olmadığını görmek için [Azure durumunu](https://status.azure.com/status) kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
* Java v4 SDK Azure Cosmos DB kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-java-sdk-v4-sql.md) .
* [Java v4 SDK](performance-tips-java-sdk-v4-sql.md)için performans yönergeleri hakkında bilgi edinin.