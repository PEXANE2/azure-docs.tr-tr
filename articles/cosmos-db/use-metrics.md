---
title: Azure Cosmos DB ölçümlerle izleme ve hata ayıklama
description: Ortak sorunları ayıklamak ve veritabanını izlemek için Azure Cosmos DB ölçümleri kullanın.
ms.service: cosmos-db
author: kanshiG
ms.author: sngun
ms.topic: conceptual
ms.date: 06/18/2019
ms.reviewer: sngun
ms.openlocfilehash: ef457fe8c21bc7e62f910a78913069df32bea1a3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246467"
---
# <a name="monitor-and-debug-with-metrics-in-azure-cosmos-db"></a>Azure Cosmos DB ölçümlerle izleme ve hata ayıklama

Azure Cosmos DB işleme hızı, depolama, tutarlılık, kullanılabilirlik ve gecikme süresi ölçümleri sağlar. Azure portalı bu ölçümlerin bir toplu görünümünü sağlar. Azure Cosmos DB ölçümlerini Azure İzleyici API'sinden de görüntüleyebilirsiniz. Azure izleyici 'de ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [Azure izleyici 'den ölçümleri alma](cosmos-db-azure-monitor-metrics.md) makalesine bakın. 

Bu makalede, yaygın kullanım durumları ve bu sorunları çözümlemek ve hatalarını ayıklamak için Azure Cosmos DB ölçümlerinin nasıl kullanılabileceği anlatılmaktadır. Ölçümler her beş dakikada bir toplanır ve yedi gün boyunca tutulur.

## <a name="view-metrics-from-azure-portal"></a>Azure portal ölçümleri görüntüleme

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. **Ölçümler** bölmesini açın. Varsayılan olarak, ölçümler bölmesi Azure Cosmos hesabınızdaki tüm veritabanları için depolama, dizin, istek birimleri ölçümlerini gösterir. Her veritabanı, kapsayıcı veya bir bölge için bu ölçümleri filtreleyebilirsiniz. Ayrıca ölçümleri belirli bir zaman düzeyinde filtreleyebilirsiniz. Aktarım hızı, depolama, kullanılabilirlik, gecikme süresi ve tutarlılık ölçümleri hakkında daha fazla ayrıntı, ayrı sekmelerde sunulmaktadır. 

   ![Azure portal Cosmos DB performans ölçümleri](./media/use-metrics/performance-metrics.png)

**Ölçümler** bölmesinde aşağıdaki ölçümler mevcuttur: 

* **İşleme ölçümleri** -Bu ölçüm, kapsayıcı için sağlanan aktarım hızı veya depolama kapasitesi aşıldığı için tüketilen veya başarısız olan isteklerin (429 yanıt kodu) sayısını gösterir.

* **Depolama ölçümleri** -Bu ölçüm, veri ve Dizin kullanımının boyutunu gösterir.

* **Kullanılabilirlik ölçümleri** -Bu ölçüm, toplam istek üzerindeki başarılı isteklerin yüzdesini saat başına gösterir. Başarı oranı Azure Cosmos DB SLA 'Lar tarafından tanımlanır.

* **Gecikme ölçümleri** -Bu ölçüm, hesabınızın çalıştığı bölgedeki Azure Cosmos DB tarafından gözlenen okuma ve yazma gecikmesini gösterir. Coğrafi olarak çoğaltılan bir hesap için bölgeler genelinde gecikme süresini görselleştirebilirsiniz. Bu ölçüm, uçtan uca istek gecikmesini temsil etmez.

* **Tutarlılık ölçümleri** -Bu ölçüm, seçtiğiniz tutarlılık modeli için ne kadar tutarlılık olduğunu gösterir. Bu ölçüm, çok bölgeli hesaplar için seçtiğiniz bölgeler arasındaki çoğaltma gecikmesini de gösterir.

* **Sistem ölçümleri** -Bu ölçüm, ana bölüm tarafından sunulan veri isteklerinin sayısını gösterir. Ayrıca, kısıtlanan istekleri belirlemesine de yardımcı olur.

Aşağıdaki bölümlerde Azure Cosmos DB ölçümlerini kullanabileceğiniz yaygın senaryolar açıklanmaktadır. 

## <a name="understand-how-many-requests-are-succeeding-or-causing-errors"></a>Kaç isteğin başarılı olduğunu veya hatalara neden olduğunu anlayın

Başlamak için [Azure Portal](https://portal.azure.com) gidin ve **ölçümler** dikey penceresine gidin. Dikey pencerede, * * istek sayısı ' nı 1 dakikalık bir grafik olarak aşmış olan istek sayısını bulun. Bu grafik, durum koduna göre bölünmüş toplam dakikalık isteği gösterir. HTTP durum kodları hakkında daha fazla bilgi için bkz. [Azure Cosmos DB Için http durum kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

En yaygın hata durum kodu 429 ' dir (hız sınırlandırma/azaltma). Bu hata, Azure Cosmos DB isteklerinin sağlanan aktarım hızına göre daha fazla olduğu anlamına gelir. Bu sorunun en yaygın çözümü, belirtilen koleksiyon için [Rus ölçeğini ölçeklendirmektir](./set-throughput.md) .

![Dakika başına istek sayısı](media/use-metrics/metrics-12.png)

## <a name="determine-the-throughput-distribution-across-partitions"></a>Bölümler arasında üretilen iş dağıtımını belirleme

Bölüm anahtarlarınızın iyi bir önemliliğine sahip olmak, ölçeklenebilir bir uygulama için gereklidir. Bölümlere göre ayrılmış herhangi bir bölümlenmiş kapsayıcının üretilen iş dağıtımını öğrenmek için [Azure Portal](https://portal.azure.com) **ölçümler dikey penceresine** gidin. **Aktarım hızı** sekmesinde, depolama dökümü **her bir fiziksel bölüm GRAFIĞININ en fazla ru/saniye** cinsinden gösterilir. Aşağıdaki grafik, en soldaki çarpıtılmış bölümde gösterildiği gibi verilerin yanlış dağıtımına ilişkin bir örnek gösterir.

![Tek bölüm 3:05 PM 'de ağır kullanımı görüntüleme](media/use-metrics/metrics-17.png)

Düzensiz bir üretilen iş dağıtımı, kısıtlanmış isteklere yol açabilecek ve yeniden bölümleme gerektirebilecek *etkin* bölümlere neden olabilir. Azure Cosmos DB bölümlendirme hakkında daha fazla bilgi için bkz. [Azure Cosmos DB bölüm ve ölçek](./partition-data.md).

## <a name="determine-the-storage-distribution-across-partitions"></a>Bölümler arasında depolama dağılımını belirleme

Tüm ölçeklenebilir uygulamalar için bölümünüzün iyi bir önemliliğine sahip olmak önemlidir. Bölümlere göre ayrılmış herhangi bir bölümlenmiş kapsayıcının depolama dağılımını öğrenmek için [Azure Portal](https://portal.azure.com)ölçümler dikey penceresine gidin. Depolama alanı sekmesinde, depolama dökümü üst bölüm anahtarları tarafından tüketilen veri + dizin depolaması bölümünde gösterilir. Aşağıdaki grafik, en soldaki asimetrik bölümde gösterildiği gibi veri depolamanın kötü bir dağıtımını gösterir.

![Kötü veri dağıtımı örneği](media/use-metrics/metrics-07.png)

Grafikteki bölüme tıklayarak, hangi bölüm anahtarının dağıtımı eğriltir.

![Bölüm anahtarı dağılımı eğriltme](media/use-metrics/metrics-05.png)

Hangi bölüm anahtarının dağıtım içinde eğilmesine neden olduğunu tanımladıktan sonra, kapsayıcınızı daha fazla Dağıtılmış bölüm anahtarıyla yeniden bölümlemeniz gerekebilir. Azure Cosmos DB bölümlendirme hakkında daha fazla bilgi için bkz. [Azure Cosmos DB bölüm ve ölçek](./partition-data.md).

## <a name="compare-data-size-against-index-size"></a>Veri boyutunu Dizin boyutuyla karşılaştırın

Azure Cosmos DB, tüketilen toplam depolama alanı, hem veri boyutu hem de dizin boyutunun birleşimidir. Genellikle, dizin boyutu veri boyutunun bir kesiri olur. [Azure Portal](https://portal.azure.com)ölçümler dikey penceresinde, depolama sekmesi verileri ve dizini temel alarak depolama tüketiminin dökümünü alır.

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

Dizin alanını korumak istiyorsanız, [Dizin oluşturma ilkesini](index-policy.md)ayarlayabilirsiniz.

## <a name="debug-why-queries-are-running-slow"></a>Sorguların neden yavaş çalıştığını hata ayıkla

SQL API SDK 'lerinde, Azure Cosmos DB sorgu yürütme istatistikleri sağlar.

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

*Queryölçümler* , sorgunun her bileşeninin yürütme için ne kadar süreceğine ilişkin ayrıntılar sağlar. Uzun süre çalışan sorgulara ilişkin en yaygın temel neden, sorgunun dizinlerden yararlanamaz anlamına gelir. Bu sorun, daha iyi bir filtre koşuluyla çözülebilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure portal belirtilen ölçümleri kullanarak sorunları nasıl izleyip ayıklayacağınız hakkında daha fazla öğrendiniz. Aşağıdaki makaleleri okuyarak veritabanı performansını artırma hakkında daha fazla bilgi edinmek isteyebilirsiniz:

* Azure izleyici 'de ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [Azure izleyici 'den ölçümleri alma](cosmos-db-azure-monitor-metrics.md) makalesine bakın. 
* [Azure Cosmos DB ile performans ve ölçek testi](performance-testing.md)
* [Azure Cosmos DB için performans ipuçları](performance-tips.md)
