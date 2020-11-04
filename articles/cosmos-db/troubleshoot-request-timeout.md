---
title: Azure Cosmos DB hizmet isteği zaman aşımı özel durumları sorunlarını giderme
description: Azure Cosmos DB hizmeti istek zaman aşımı özel durumlarını tanılamayı ve gidermeyi öğrenin.
author: j82w
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 9e52acd2f7f594523059b22fefd121314bd77921
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93340084"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-timeout-exceptions"></a>Tanılama ve sorun giderme Azure Cosmos DB istek zaman aşımı özel durumları
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB bir HTTP 408 istek zaman aşımı döndürdü.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, istek zaman aşımı özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="check-the-sla"></a>SLA 'yı denetleyin
408 özel durum sayısının Azure Cosmos DB SLA 'sını ihlal ettiğini görmek için [Azure Cosmos DB izlemeyi](monitor-cosmos-db.md) denetleyin.

#### <a name="solution-1-it-didnt-violate-the-azure-cosmos-db-sla"></a>Çözüm 1: Azure Cosmos DB SLA 'sını ihlal etmedi
Uygulamanın bu senaryoyu işlemesi ve bu geçici hatalarda yeniden denemesi gerekir.

#### <a name="solution-2-it-did-violate-the-azure-cosmos-db-sla"></a>Çözüm 2: Azure Cosmos DB SLA 'sını ihlal ediyor
[Azure desteği](https://aka.ms/azure-support)'ne başvurun.
 
### <a name="hot-partition-key"></a>Sık erişimli bölüm anahtarı
Azure Cosmos DB, genel olarak sağlanan üretilen işi fiziksel bölümler arasında eşit olarak dağıtır. Bir sıcak bölüm olduğunda, fiziksel bir bölümdeki bir veya daha fazla mantıksal bölüm anahtarı, tüm fiziksel bölümün Istek birimi (RU/s) olarak tüketiyor. Aynı zamanda, diğer fiziksel bölümlerdeki RU/s kullanılmıyor. Bir belirti olarak tüketilen toplam RU/sn, veritabanı veya kapsayıcıdaki genel olarak sağlanan RU/s değerinden daha az olacaktır. Hala etkin mantıksal bölüm anahtarına karşı isteklerde daraltma (429s) görürsünüz. İş yükünün sık erişimli bir bölümle karşılaşıyor olup olmadığını görmek için [NORMALLEŞTIRILMIŞ ru tüketim ölçümünü](monitor-normalized-request-units.md) kullanın. 

#### <a name="solution"></a>Çözüm:
İstek birimini ve depolamayı eşit bir şekilde dağıtan iyi bir bölüm anahtarı seçin. [Bölüm anahtarınızı değiştirme](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-dot-net-sdk.md) .
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md)için performans yönergeleri hakkında bilgi edinin.