---
title: Azure Cosmos DB'deki ölçümlerle izleme ve hata ayıklama
description: Sık karşılaşılan sorunları ayıklamak ve veritabanını izlemek için Azure Cosmos DB'deki ölçümleri kullanın.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: b65bc6097d4841c79a68d4313ac7a3f89f6d1dbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065928"
---
# <a name="monitor-and-debug-with-metrics-in-azure-cosmos-db"></a>Azure Cosmos DB'deki ölçümlerle izleme ve hata ayıklama

Azure Cosmos DB işleme hızı, depolama, tutarlılık, kullanılabilirlik ve gecikme süresi ölçümleri sağlar. Azure portalı bu ölçümlerin bir toplu görünümünü sağlar. Azure Cosmos DB ölçümlerini Azure İzleyici API'sinden de görüntüleyebilirsiniz. Azure monitöründen ölçümleri nasıl görüntüleyin hakkında bilgi edinmek için [Azure Monitor makalesinden metrik al](cosmos-db-azure-monitor-metrics.md) makalesine bakın. 

Bu makalede, ortak kullanım örnekleri ve Azure Cosmos DB ölçümlerinin bu sorunları çözümlemek ve hata ayıklamak için nasıl kullanılabileceği ü Ölçümler her beş dakikada bir toplanır ve yedi gün saklanır.

## <a name="view-metrics-from-azure-portal"></a>Azure portalından ölçümleri görüntüleme

1. Azure [portalında](https://portal.azure.com/) oturum açma

1. **Ölçümler** bölmesini açın. Varsayılan olarak, ölçümler bölmesi Azure Cosmos hesabınızdaki tüm veritabanları için depolama, dizin, istek birimleri ölçümlerini gösterir. Bu ölçümleri veritabanı, kapsayıcı veya bölge başına filtreleyebilirsiniz. Ölçümleri belirli bir zaman parçalı nda da filtreleyebilirsiniz. İş çıktısı, depolama, kullanılabilirlik, gecikme süresi ve tutarlılık ölçümleri hakkında daha fazla ayrıntı ayrı sekmelerde sağlanır. 

   ![Azure portalında Cosmos DB performans ölçümleri](./media/use-metrics/performance-metrics.png)

Aşağıdaki **ölçümler, Ölçümler** bölmesinden edinilebilir: 

* **İş memseri ölçümleri** - Bu metrik, kapsayıcı için sağlanan iş ortası veya depolama kapasitesi aşıldığından tüketilen veya başarısız olan istek sayısını (429 yanıt kodu) gösterir.

* **Depolama ölçümleri** - Bu metrik, verilerin ve dizin kullanımının boyutunu gösterir.

* **Kullanılabilirlik ölçümleri** - Bu ölçüm, saat başına toplam istekler üzerinden başarılı isteklerin yüzdesini gösterir. Başarı oranı Azure Cosmos DB SLa'lar tarafından tanımlanır.

* **Gecikme ölçümleri** - Bu metrik, Azure Cosmos DB tarafından hesabınızın çalıştığı bölgede gözlenen okuma ve yazma gecikmesini gösterir. Coğrafi olarak çoğaltılmış bir hesap için bölgeler arasında gecikme yi görselleştirebilirsiniz. Bu metrik, uçuça istek gecikmesini temsil etmez.

* **Tutarlılık ölçümleri** - Bu metrik, seçtiğiniz tutarlılık modelinin tutarlılığının ne kadar nihai olduğunu gösterir. Çok bölgeli hesaplar için bu ölçüm, seçtiğiniz bölgeler arasındaki çoğaltma gecikmesini de gösterir.

* **Sistem ölçümleri** - Bu metrik, ana bölüm tarafından kaç meta veri isteği sunulduğunu gösterir. Ayrıca, daraltılmış istekleri belirlemeye de yardımcı olur.

Aşağıdaki bölümlerde Azure Cosmos DB ölçümlerini kullanabileceğiniz yaygın senaryolar açıklanmaktadır. 

## <a name="understand-how-many-requests-are-succeeding-or-causing-errors"></a>Kaç istekte başarılı olduğunu veya hatalara neden olduğunu anlama

Başlamak için [Azure portalına](https://portal.azure.com) gidin ve **Metrics** bıçağına gidin. Bıçakta, **1 dakikalık grafik başına kapasiteyi aşan istek sayısını bulun. Bu grafik, durum kodu tarafından bölümlenen dakika dakika toplam isteklerini gösterir. HTTP durum kodları hakkında daha fazla bilgi için [Azure Cosmos DB için HTTP durum kodlarına](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb)bakın.

En yaygın hata durum kodu 429 'dur (oran sınırlama/azaltma). Bu hata, Azure Cosmos DB'ye yapılan isteklerin sağlanan iş bilgisinden daha fazla olduğu anlamına gelir. Bu sorunun en yaygın çözümü, verilen koleksiyon için [RUs'ları ölçeklendirmektir.](./set-throughput.md)

![Dakika başına istek sayısı](media/use-metrics/metrics-12.png)

## <a name="determine-the-throughput-distribution-across-partitions"></a>Bölümler arasında iş bölümü dağılımını belirleme

Bölüm tuşları iyi bir kardinallik olması herhangi bir ölçeklenebilir uygulama için esastır. Bölümlere göre ayrılmış herhangi bir kapsayıcının üretim dağılımını belirlemek için Azure [portalındaki](https://portal.azure.com) **Ölçümler bıçağına** gidin. İş **Meçla** sekmesinde, depolama dökümü her fiziksel bölüm grafiği **tarafından tüketilen Max RU/saniye'de** gösterilir. Aşağıdaki grafik, en soldaki çarpık bölüm tarafından gösterildiği gibi verilerin kötü dağılımıbir örnek göstermektedir.

![15:05'te ağır kullanım gören tek bölüm](media/use-metrics/metrics-17.png)

Düzensiz bir iş bölümü dağılımı *sıcak* bölümlere neden olabilir, bu da daraltılmış isteklere neden olabilir ve yeniden bölümleme gerektirebilir. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için [Azure Cosmos DB'de Bölümleme ve ölçeklendirme bölümüne](./partition-data.md)bakın.

## <a name="determine-the-storage-distribution-across-partitions"></a>Bölümler arasında depolama dağılımını belirleme

Bölüm iyi bir kardinallik olması herhangi bir ölçeklenebilir uygulama için esastır. Bölümlere göre ayrılmış herhangi bir bölmeli kapsayıcının depolama dağıtımını belirlemek için [Azure portalındaki](https://portal.azure.com)Ölçümler bıçağına gidin. Depolama sekmesinde, depolama dökümü üst bölüm tuşları grafiği tarafından tüketilen Veri + Dizin depolamasında gösterilir. Aşağıdaki grafik, en soldaki çarpık bölümtarafından gösterildiği gibi veri depolama kötü dağılımını göstermektedir.

![Zayıf veri dağıtımı örneği](media/use-metrics/metrics-07.png)

Grafikteki bölüme tıklayarak hangi bölüm anahtarının dağıtımı çarpıtıyor olması nın nedenini kökleyebilirsiniz.

![Bölüm anahtarı dağıtımı çarpıtıyor](media/use-metrics/metrics-05.png)

Dağıtımda eğrilmeye neden olan bölüm anahtarını belirledikten sonra, kapsayıcınızı daha dağıtılmış bir bölüm anahtarıyla yeniden bölmeniz gerekebilir. Azure Cosmos DB'de bölümleme hakkında daha fazla bilgi için [Azure Cosmos DB'de Bölümleme ve ölçeklendirme bölümüne](./partition-data.md)bakın.

## <a name="compare-data-size-against-index-size"></a>Veri boyutunu dizin boyutuyla karşılaştırma

Azure Cosmos DB'de, toplam tüketilen depolama, hem Veri boyutu hem de Dizin boyutunun birleşimidir. Genellikle, dizin boyutu veri boyutunun bir kısmını. [Azure portalındaki](https://portal.azure.com)Metrics bıçaklarında Depolama sekmesi, veri ve dizin temel alınabilen depolama tüketiminin dökümünü sergiler.

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

Dizin alanını korumak istiyorsanız, [dizin oluşturma ilkesini](index-policy.md)ayarlayabilirsiniz.

## <a name="debug-why-queries-are-running-slow"></a>Sorguların neden yavaş çalıştığını hata ayıklama

SQL API SDK'larında Azure Cosmos DB sorgu yürütme istatistikleri sağlar.

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName),
 "SELECT * FROM c WHERE c.city = 'Seattle'",
 new FeedOptions
 {
 PopulateQueryMetrics = true,
 MaxItemCount = -1,
 MaxDegreeOfParallelism = -1,
 EnableCrossPartitionQuery = true
 }).AsDocumentQuery();
FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;
```

*QueryMetrics,* sorgunun her bir bileşeninin yürütmeye ne kadar süreceğine ilişkin ayrıntıları sağlar. Uzun süren sorgular için en yaygın kök nedeni taramalar, sorgu dizinleri kaldıraç mümkün değildi anlamına gelir. Bu sorun daha iyi bir filtre koşuluyla çözülebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure portalında sağlanan ölçümleri kullanarak sorunları izlemeyi ve hata ayıklamayı artık öğrendiniz. Aşağıdaki makaleleri okuyarak veritabanı performansını artırma hakkında daha fazla bilgi edinmek isteyebilirsiniz:

* Azure monitöründen ölçümleri nasıl görüntüleyin hakkında bilgi edinmek için [Azure Monitor makalesinden metrik al](cosmos-db-azure-monitor-metrics.md) makalesine bakın. 
* [Azure Cosmos DB ile performans ve ölçek testi](performance-testing.md)
* [Azure Cosmos DB için performans ipuçları](performance-tips.md)
