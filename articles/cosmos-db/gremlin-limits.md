---
title: Azure Cosmos DB Gremlin Sınırları
description: Grafik altyapısının çalışma zamanı sınırlamaları için başvuru belgeleri
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 10/04/2019
ms.author: lbosq
ms.openlocfilehash: 76ad787990c355d29613c05ca9fce31885a2eccc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72029858"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB Gremlin sınırları
Bu makalede Azure Cosmos DB Gremlin motorunun sınırları anlatılıyor ve bunların müşteri geçişlerini nasıl etkileyebilecekleri açıklanmaktadır.

Cosmos DB Gremlin, Cosmos DB altyapısının üzerine inşa edilmiştir. Bu nedenle, [Azure Cosmos DB hizmet sınırları](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) ile açıklanan tüm sınırlar hala geçerlidir. 

## <a name="limits"></a>Sınırlar

Gremlin sınırına ulaşıldığında, geçiş süresi 429 **x-ms durum kodu** ile iptal edilir ve bu da azaltma hatası gösterir. Daha fazla bilgi için [Gremlin sunucu yanıt üstbilgilerine](gremlin-limits.md) bakın.

**Kaynak**    | **Varsayılan limit** | **Açıklama**
--- | --- | ---
*Komut dosyası uzunluğu* | **64 KB** | İstek başına Gremlin dolaşma betiği uzunluğu üst sınırı.
*Operatör derinliği* | **400** |  Dolaşmadaki benzersiz adımların toplam sayısı. Örneğin, ```g.V().out()``` 2: V() ve out(), ```g.V('label').repeat(out()).times(100)``` operatör derinliği 3: V(), repeat() ve out() operatöre bir parametre olduğu ```.times(100)``` için ```.repeat()``` bir operatör sayısı vardır.
*Paralellik derecesi* | **32** | Depolama katmanına yönelik tek bir istekte sorgulanan depolama bölümü sayısı üst sınırı. Yüzlerce bölüme sahip grafikler bu sınırdan etkilenir.
*Yineleme sınırı* | **32** | Bir ```.repeat()``` işleci yürütmesi için yineleme sayısı üst sınırı. Çoğu durumda adım ```.repeat()``` Her yineleme genişlik-ilk geçiş çalışır, hangi herhangi bir geçiş vertices arasında en fazla 32 atlamaları ile sınırlı olduğu anlamına gelir.
*Geçiş zaman* | **30 saniye** | Bu sefer ilerlendiğinde geçiş iptal edilecektir. Cosmos DB Graph, dolaşmaların büyük çoğunluğunun milisaniyeler içinde tamamlandığı bir OLTP veritabanıdır. Cosmos DB Grafiği'nde OLAP sorgularını çalıştırmak için [Grafik Veri Çerçeveleri](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) ve [Cosmos DB Spark Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark)ile [Apache Spark'ı](https://azure.microsoft.com/services/cosmos-db/) kullanın.
*Boştaki bağlantı zaman aşımı* | **1 saat** | Gremlin hizmetinin boşta websocket bağlantılarını açık tutma süresi. TCP paketleri veya HTTP canlı tutma istekleri bağlantı ömrünü bu sınırın ötesine uzatmaz. Cosmos DB Graph motoru, üzerinde etkin Gremlin istekleri yoksa websocket bağlantılarını boşta olarak kabul eder.
*Kaynak belirteci/saat* | **100** | Gremlin istemcileri tarafından bölgedeki Gremlin hesabına bağlanmak için kullanılan benzersiz kaynak belirteçlerinin sayısı. Uygulama saatlik benzersiz belirteç sınırını `"Exceeded allowed resource token limit of 100 that can be used concurrently"` aştığında, bir sonraki kimlik doğrulama isteğinde döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB Gremlin yanıt başlıkları](gremlin-headers.md) 
* [Gremlin ile Azure Cosmos DB Kaynak Belirteçleri](how-to-use-resource-tokens-gremlin.md)
