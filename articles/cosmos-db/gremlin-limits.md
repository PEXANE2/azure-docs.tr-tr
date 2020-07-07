---
title: Azure Cosmos DB Gremlin sınırları
description: Graph Engine çalışma zamanı sınırlamaları için başvuru belgeleri
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 10/04/2019
ms.author: lbosq
ms.openlocfilehash: 76ad787990c355d29613c05ca9fce31885a2eccc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "72029858"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB Gremlin sınırları
Bu makale, Azure Cosmos DB Gremlin motorunun sınırlarını ve müşteri traversals nasıl etkileyebileceğini açıklar.

Cosmos DB Gremlin, Cosmos DB altyapısının üzerine kurulmuştur. Bu nedenle, [Azure Cosmos DB hizmet limitlerinde](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) açıklanan tüm sınırlar hala geçerlidir. 

## <a name="limits"></a>Sınırlar

Gremlin sınırına ulaşıldığında, bir azaltma hatasını gösteren bir **x-MS-Status-code** 429 ile çapraz geçiş iptal edilir. Daha fazla bilgi için bkz. [Gremlin sunucu yanıt üst bilgileri](gremlin-limits.md) .

**Kaynak**    | **Varsayılan sınır** | **Açıklama**
--- | --- | ---
*Betik uzunluğu* | **64 KB** | İstek başına Gremlin dolaşma betiği uzunluğu üst sınırı.
*İşleç derinliği* | **400** |  Dolaşmadaki benzersiz adımların toplam sayısı. Örneğin, işleç ```g.V().out()``` sayısı 2: v () ve out (), bir işleç parametresi olan ```g.V('label').repeat(out()).times(100)``` 3: v (), Yinele () ve out () işleç derinliğine sahiptir ```.times(100)``` ```.repeat()``` .
*Paralellik derecesi* | **32** | Depolama katmanına yönelik tek bir istekte sorgulanan depolama bölümü sayısı üst sınırı. Yüzlerce bölümden oluşan grafikler bu sınıra göre etkilenecektir.
*Yineleme sınırı* | **32** | Bir ```.repeat()``` işleci yürütmesi için yineleme sayısı üst sınırı. ```.repeat()```Çoğu durumda her adımın yineleme, birinci düzey çapraz geçişi çalıştırır, bu da herhangi bir geçişinin köşeler arasında en çok 32 sıçrama ile sınırlı olduğu anlamına gelir.
*Çapraz geçiş zaman aşımı* | **30 saniye** | Bu süre geçtiğinde çapraz geçiş iptal edilecek. Cosmos DB Graph, dolaşmaların büyük çoğunluğunun milisaniyeler içinde tamamlandığı bir OLTP veritabanıdır. Cosmos DB Graph üzerinde OLAP sorguları çalıştırmak için [grafik veri çerçeveleri](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) ve [Cosmos DB Spark Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark)ile [Apache Spark](https://azure.microsoft.com/services/cosmos-db/) kullanın.
*Boştaki bağlantı zaman aşımı* | **1 saat** | Gremlin hizmetinin boş WebSocket bağlantılarını açık tutamayacağı zaman miktarı. TCP etkin tut paketleri veya HTTP etkin tut istekleri bu sınırın ötesinde bağlantı ömrü genişletmez. Cosmos DB Graph Engine, üzerinde çalışan etkin bir Gremlin isteği yoksa, WebSocket bağlantılarını boşta olarak değerlendirir.
*Kaynak belirteci/saat* | **100** | Gremlin istemcileri tarafından bölgedeki Gremlin hesabına bağlanmak için kullanılan benzersiz kaynak belirteçlerinin sayısı. Uygulama saatlik benzersiz belirteç sınırını aştığında, `"Exceeded allowed resource token limit of 100 that can be used concurrently"` sonraki kimlik doğrulama isteğinde döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB Gremlin yanıt üst bilgileri](gremlin-headers.md) 
* [Gremlin ile kaynak belirteçleri Azure Cosmos DB](how-to-use-resource-tokens-gremlin.md)
