---
title: Azure Cosmos DB Gremlin sınırları
description: Graph Engine çalışma zamanı sınırlamaları için başvuru belgeleri
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: olignat
ms.openlocfilehash: f1fe4cfac22a651f44338986d0a767fe3e1a360b
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910871"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB Gremlin limitleri
Bu makale, Azure Cosmos DB Gremlin motorunun sınırlarını ve müşteri traversals nasıl etkileyebileceğini açıklar.

Cosmos DB Gremlin, [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) tüm limitlerin halen uygulandığına ilişkin Cosmos DB altyapısının üzerine kurulmuştur. 

## <a name="limits"></a>Sınırlar

Gremlin sınırına ulaşıldığında, azaltma hatasını gösteren **x-MS-Status-Code** = 429 ile çapraz geçiş işlemi iptal edilir.

**Kaynak**    | **Varsayılan sınır** | **Açıklama**
--- | --- | ---
*İstek başına bellek* | **2 GB** | İsteğin işleme sırasında tüketebileceği bellek üst sınırı. Büyük veri kümelerini hesaplamak için gereken istekler ek bellek tüketir. Bu sınırı geçmemek veya OLAP çözümlerini kullanmak için istekleri daha küçük veri kümelerine kapsamları değerlendirin.
*Betik uzunluğu* | **64 KB** | İstek başına Gremlin dolaşma betiği uzunluğu üst sınırı.
*İşleç derinliği* | **400** |  Dolaşmadaki benzersiz adımların toplam sayısı. Örneğin, işleç ```g.V().out()``` sayısı 2 ' dir: V () ve out (), ```g.V('label').repeat(out()).times(100)``` işleç derinliğini 3 ' e sahiptir: V (), Repeat () ve out () ```.times(100)``` , bir ```.repeat()``` işleç parametresidir.
*Paralellik derecesi* | **32** | Depolama katmanına yönelik tek bir istekte sorgulanan depolama bölümü sayısı üst sınırı. Yüzlerce bölümden oluşan grafikler bu sınıra göre etkilenecektir.
*Yineleme sınırı* | **32** | Bir ```.repeat()``` işleci yürütmesi için yineleme sayısı üst sınırı. Çoğu durumda her ```.repeat()``` adımın yineleme, birinci düzey çapraz geçişi çalıştırır, bu da herhangi bir geçişinin köşeler arasında en çok 32 sıçrama ile sınırlı olduğu anlamına gelir.
*Çapraz geçiş zaman aşımı* | **30 saniye** | Bu süre geçtiğinde çapraz geçiş iptal edilecek. Cosmos DB Graph, dolaşmaların büyük çoğunluğunun milisaniyeler içinde tamamlandığı bir OLTP veritabanıdır. Cosmos DB Graph üzerinde OLAP sorguları çalıştırmak için [grafik veri çerçeveleri](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) ve [Cosmos DB Spark Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark)ile [Apache Spark](https://azure.microsoft.com/services/cosmos-db/) kullanın.
*Koşul sınırı* | **20** | Tek bir köşede veya kenarda uygulanan ```.has()``` veya ```.hasNot()``` adımlarının sayısı. Bu sınıra ulaşıldığında uygulamaya ```The SQL query exceeded the maximum number of joins. The allowed limit is 20``` hatası gösterilir. Ekip bu sınırı kaldırmak için çalıştığı için geçici bir sorun. 
*Boştaki bağlantı zaman aşımı* | **5 saat** | Grafik sunucusu üzerinde trafik olmadan açık olan WebSocket bağlantısını tutacaktır. TCP etkin tut paketleri veya HTTP etkin tut istekleri bu sınırın ötesinde bağlantı ömrü genişlemez, ancak gönderilmemişse, temel alınan Azure altyapısı bağlantıyı daha da kapatabilir. Cosmos DB Graph Engine, üzerinde çalışan bir Gremlin traversals yoksa boş olarak kabul edilir.
*Kaynak belirteci/saat* | **100** | Bir bölgedeki Gremlin hesabına bağlanmak için Gremlin istemcileri tarafından kullanılan benzersiz kaynak belirteçleri sayısı. Uygulama saatlik benzersiz belirteç sınırını aştığında, `"Exceeded allowed resource token limit of 100 that can be used concurrently"` sonraki kimlik doğrulama isteğinde döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB Gremlin yanıt üst bilgileri](gremlin-headers.md) 
* [Gremlin ile kaynak belirteçleri Azure Cosmos DB](how-to-use-resource-tokens-gremlin.md)
