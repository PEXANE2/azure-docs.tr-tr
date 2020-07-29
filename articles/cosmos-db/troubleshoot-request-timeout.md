---
title: Azure Cosmos DB hizmet isteği zaman aşımı özel durumuyla ilgili sorun giderme
description: Cosmos DB hizmet isteği zaman aşımı özel durumunu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: bd8102ee73721e2a56185709d1076e489ecc7607
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294527"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-timeout"></a>Tanılama ve sorun giderme Azure Cosmos DB istek zaman aşımı
Azure Cosmos DB bir HTTP 408 istek zaman aşımı döndürdü

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, istek zaman aşımı özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="1-check-the-sla"></a>1. SLA 'yı denetleyin
Müşteri, 408 sayısının Cosmos DB SLA 'yı ihlal ettiğini denetlemek için [Azure Cosmos DB izlemeyi](monitor-cosmos-db.md) denetlemelidir.

#### <a name="solution-1-it-did-not-violate-the-cosmos-db-sla"></a>Çözüm 1: Cosmos DB SLA 'sını ihlal etmedi
Uygulamanın bu senaryoyu işlemesi ve bu geçici hatalarda yeniden denemesi gerekir.

#### <a name="solution-2-it-did-violate-the-cosmos-db-sla"></a>Çözüm 2: Cosmos DB SLA 'sını ihlal ediyor
Azure desteği 'ne başvurun:https://aka.ms/azure-support
 
### <a name="2-hot-partition-key"></a>2. sık erişimli bölüm anahtarı
Azure Cosmos DB, genel olarak sağlanan üretilen işi fiziksel bölümler arasında eşit olarak dağıtır. Bir sıcak bölüm olduğunda, fiziksel bölümdeki bir veya daha fazla mantıksal bölüm, tüm fiziksel bölümlerin RU/sn 'sini tüketirken diğer fiziksel bölümlerdeki RU/s kullanılmıyor olur. Bir belirti olarak, tüketilen toplam RU/sn, veritabanı veya kapsayıcıdaki genel olarak sağlanan RU/s değerinden daha az olacaktır, ancak etkin mantıksal bölüm anahtarına karşı isteklerde daraltma (429s) görmeye devam edersiniz. İş yükünün sık erişimli bir bölümle karşılaşıyor olup olmadığını görmek için [NORMALLEŞTIRILMIŞ ru tüketim ölçümünü](monitor-normalized-request-units.md) kullanın. 

#### <a name="solution"></a>Çözüm:
İstek birimini ve depolamayı eşit bir şekilde dağıtan iyi bir bölüm anahtarı seçin. [Bölüm anahtarınızı değiştirme](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin