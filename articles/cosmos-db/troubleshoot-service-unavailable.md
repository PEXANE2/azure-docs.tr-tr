---
title: Azure Cosmos DB hizmeti kullanılamıyor özel durumuyla ilgili sorun giderme
description: Cosmos DB hizmet kullanılamıyor özel durumu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 12ecec7cf8f406ed53fb5e054fc304bf672cbbb0
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294519"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable"></a>Tanılama ve sorun giderme Azure Cosmos DB hizmeti kullanılamıyor
SDK Azure Cosmos DB hizmetine bağlanamadı.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, hizmet kullanılamaz özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="1-the-required-ports-are-not-enabled"></a>1. gerekli bağlantı noktaları etkin değil.
Tüm [gerekli bağlantı noktalarının](performance-tips-dotnet-sdk-v3-sql.md#networking) etkinleştirildiğini doğrulayın.

## <a name="if-an-existing-application-or-service-started-getting-503"></a>Mevcut bir uygulama veya hizmet 503 almaya başladıysa

### <a name="1-there-is-an-outage"></a>1. kesinti var
Devam eden bir sorun olup olmadığını görmek için [Azure durumunu](https://status.azure.com/status) denetleyin.

### <a name="2-snat-port-exhaustion"></a>2. SNAT bağlantı noktası tükenmesi
[SNAT bağlantı noktası Tükenme kılavuzunu](troubleshoot-dot-net-sdk.md#snat)izleyin.

### <a name="3-the-required-ports-are-being-blocked"></a>3. gerekli bağlantı noktaları engelleniyor
Tüm [gerekli bağlantı noktalarının](performance-tips-dotnet-sdk-v3-sql.md#networking) etkinleştirildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin