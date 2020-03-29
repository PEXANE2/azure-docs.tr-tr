---
title: Azure Cosmos DB ile performans ve ölçek testi
description: Azure Cosmos DB ile ölçek ve performans testi yapmayı öğrenin. Daha sonra yüksek performanslı uygulama senaryoları için Azure Cosmos DB işlevselliğini değerlendirebilirsiniz.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: fb510c5628913fb3fa37b572c4409aee5d1028ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76313765"
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Azure Cosmos DB ile performans ve ölçek testi

Performans ve ölçek testi, uygulama geliştirmede önemli bir adımdır. Birçok uygulama için veritabanı katmanının genel performans ve ölçeklenebilirlik üzerinde önemli bir etkisi vardır. Bu nedenle, performans testinin önemli bir bileşenidir. [Azure Cosmos DB,](https://azure.microsoft.com/services/cosmos-db/) elastik ölçek ve öngörülebilir performans için amaca yönelik olarak üretilmiştir. Bu özellikler, yüksek performanslı bir veritabanı katmanı gerektiren uygulamalar için mükemmel bir uyum sağlar. 

Bu makale, Azure Cosmos DB iş yükleri için performans testi paketleri uygulayan geliştiriciler için bir başvurudur. Ayrıca, yüksek performanslı uygulama senaryoları için Azure Cosmos DB'yi değerlendirmek için de kullanılabilir. Öncelikle veritabanının yalıtılmış performans sınaması üzerinde duruluyor, ama aynı zamanda üretim uygulamaları için en iyi uygulamaları içerir.

Bu makaleyi okuduktan sonra, aşağıdaki soruları yanıtlamak mümkün olacak: 

* Azure Cosmos DB performans testi için örnek bir .NET istemci uygulamasını nerede bulabilirim? 
* Müşteri uygulamamdan Azure Cosmos DB ile yüksek iş düzeyine nasıl ulaşabilirim?

Kodla başlamak için projeyi [Azure Cosmos DB performans testi örneğinden](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)indirin. 

> [!NOTE]
> Bu uygulamanın amacı, az sayıda istemci makinesiyle Azure Cosmos DB'den en iyi performansı nasıl elde edilebildiğini göstermektir. Örneğin amacı, Azure Cosmos DB'nin (herhangi bir sınır olmaksızın ölçeklendirilebilen) en yüksek iş yapma kapasitesine ulaşmak değildir.
> 
> 

Azure Cosmos DB performansını artırmak için istemci tarafı yapılandırma seçenekleri arıyorsanız, [Azure Cosmos DB performans ipuçlarına](performance-tips.md)bakın.

## <a name="run-the-performance-testing-application"></a>Performans testi uygulamasını çalıştırma
Başlamak için en hızlı yol, aşağıdaki adımlarda açıklandığı gibi .NET örneğini derlemek ve çalıştırmaktır. Ayrıca kaynak kodu gözden geçirebilir ve kendi istemci uygulamalarınızda benzer yapılandırmalar uygulayabilirsiniz.

**Adım 1:** Projeyi [Azure Cosmos DB performans testi örneğinden](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)indirin veya GitHub deposunu çatallayın.

**Adım 2:** App.config'de EndpointUrl, AuthorizationKey, CollectionThroughput ve DocumentTemplate (isteğe bağlı) ayarlarını değiştirin.

> [!NOTE]
> Yüksek iş maliyetine sahip tahsilatları sağlamadan önce, tahsilat başına maliyetleri tahmin etmek için [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. Azure Cosmos DB depolama ve iş ortalarını saatlik olarak bağımsız olarak faturalar. Test ten sonra Azure Cosmos kaplarınızın iş kısmını silerek veya azaltarak maliyetlerden tasarruf edebilirsiniz.
> 
> 

**Adım 3:** Konsol uygulamasını komut satırından derle ve çalıştırın. Aşağıdaki gibi çıktı görmelisiniz:

    C:\Users\cosmosdb\Desktop\Benchmark>DocumentDBBenchmark.exe
    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://arramacquerymetrics.documents.azure.com:443/
    Collection : db.data at 100000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: -1
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Found collection data with 100000 RU/s
    Starting Inserts with 100 tasks
    Inserted 4503 docs @ 4491 writes/s, 47070 RU/s (122B max monthly 1KB reads)
    Inserted 17910 docs @ 8862 writes/s, 92878 RU/s (241B max monthly 1KB reads)
    Inserted 32339 docs @ 10531 writes/s, 110366 RU/s (286B max monthly 1KB reads)
    Inserted 47848 docs @ 11675 writes/s, 122357 RU/s (317B max monthly 1KB reads)
    Inserted 58857 docs @ 11545 writes/s, 120992 RU/s (314B max monthly 1KB reads)
    Inserted 69547 docs @ 11378 writes/s, 119237 RU/s (309B max monthly 1KB reads)
    Inserted 80687 docs @ 11345 writes/s, 118896 RU/s (308B max monthly 1KB reads)
    Inserted 91455 docs @ 11272 writes/s, 118131 RU/s (306B max monthly 1KB reads)
    Inserted 102129 docs @ 11208 writes/s, 117461 RU/s (304B max monthly 1KB reads)
    Inserted 112444 docs @ 11120 writes/s, 116538 RU/s (302B max monthly 1KB reads)
    Inserted 122927 docs @ 11063 writes/s, 115936 RU/s (301B max monthly 1KB reads)
    Inserted 133157 docs @ 10993 writes/s, 115208 RU/s (299B max monthly 1KB reads)
    Inserted 144078 docs @ 10988 writes/s, 115159 RU/s (298B max monthly 1KB reads)
    Inserted 155415 docs @ 11013 writes/s, 115415 RU/s (299B max monthly 1KB reads)
    Inserted 166126 docs @ 10992 writes/s, 115198 RU/s (299B max monthly 1KB reads)
    Inserted 173051 docs @ 10739 writes/s, 112544 RU/s (292B max monthly 1KB reads)
    Inserted 180169 docs @ 10527 writes/s, 110324 RU/s (286B max monthly 1KB reads)
    Inserted 192469 docs @ 10616 writes/s, 111256 RU/s (288B max monthly 1KB reads)
    Inserted 199107 docs @ 10406 writes/s, 109054 RU/s (283B max monthly 1KB reads)
    Inserted 200000 docs @ 9930 writes/s, 104065 RU/s (270B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 200000 docs @ 9928 writes/s, 104063 RU/s (270B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.
    Press any key to exit...


**Adım 4 (gerekirse):** Araçtan bildirilen iş (RU/s) elde edilen iş, koleksiyonun veya bir dizi koleksiyonun sağlanan işletilmesinden aynı veya daha yüksek olmalıdır. Değilse, küçük artışlarla DereceParalellik'i artırmak sınıra ulaşmanıza yardımcı olabilir. Müşteri uygulama platonuzdan gelen iş bvarsa, ek istemci makinelerinde uygulamanın birden çok örneğini başlatın. Bu adımla ilgili yardıma ihtiyacınız varsa [Azure portalından](https://portal.azure.com)bir destek bileti dosyala.

Uygulamayı çalıştırdıktan sonra, bunların iş yaratma ve gecikme süresi üzerindeki etkilerini anlamak için farklı [dizin oluşturma ilkeleri](index-policy.md) ve [tutarlılık düzeylerini](consistency-levels.md) deneyebilirsiniz. Ayrıca kaynak kodu gözden geçirebilir ve kendi test paketleri veya üretim uygulamaları benzer yapılandırmaları uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir .NET konsol uygulaması kullanarak Azure Cosmos DB ile performans ve ölçek testlerini nasıl gerçekleştirebileceğinizi inceledik. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Cosmos DB performans testi örneği](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Azure Cosmos DB performansını artırmak için istemci yapılandırma seçenekleri](performance-tips.md)
* [Azure Cosmos DB'de sunucu tarafı bölümleme](partition-data.md)


