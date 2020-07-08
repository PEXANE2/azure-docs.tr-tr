---
title: Azure Cosmos DB ile performans ve ölçek testi
description: Azure Cosmos DB ile ölçek ve performans testi yapmayı öğrenin. Daha sonra, yüksek performanslı uygulama senaryoları için Azure Cosmos DB işlevlerini değerlendirebilirsiniz.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: ffe368d763ee93d1864f0f807cbe18b8ebfe41c9
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851661"
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Azure Cosmos DB ile performans ve ölçek testi

Performans ve ölçek testi, uygulama geliştirmede önemli bir adımdır. Birçok uygulama için veritabanı katmanının genel performans ve ölçeklenebilirlik üzerinde önemli bir etkisi vardır. Bu nedenle, performans testi 'nin kritik bir bileşenidir. [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) , elastik ölçek ve öngörülebilir performans için oluşturulmuştur. Bu yetenekler, yüksek performanslı bir veritabanı katmanına ihtiyacı olan uygulamalar için harika bir uyum yapar. 

Bu makale, Azure Cosmos DB iş yükleri için performans testi paketleri uygulayan geliştiricilere yönelik bir başvurudur. Ayrıca, yüksek performanslı uygulama senaryolarında Azure Cosmos DB değerlendirmek için de kullanılabilir. Birincil olarak veritabanının yalıtılmış performans testinde odaklanır, ancak üretim uygulamalarına yönelik en iyi yöntemleri de içerir.

Bu makaleyi okuduktan sonra aşağıdaki soruları cevaplayabilirsiniz: 

* Azure Cosmos DB performans testi için örnek bir .NET istemci uygulamasını nereden bulabilirim? 
* Nasıl yaparım? istemci uygulamamın Azure Cosmos DB yüksek verimlilik düzeylerine mi ulaşsın?

Kodu kullanmaya başlamak için [Azure Cosmos DB performans testi örneğinden](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)projeyi indirin. 

> [!NOTE]
> Bu uygulamanın amacı, az sayıda istemci makinesi ile Azure Cosmos DB en iyi performansı nasıl alınacağını göstermektir. Örneğin amacı, Azure Cosmos DB en yüksek aktarım hızı kapasitesini elde etmez (hiçbir sınır olmadan ölçeklendirilebilen).

Azure Cosmos DB performansını geliştirmek için istemci tarafı yapılandırma seçeneklerini arıyorsanız bkz. [Azure Cosmos DB performans ipuçları](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Performans testi uygulamasını çalıştırma
Kullanmaya başlamanın en hızlı yolu, aşağıdaki adımlarda açıklandığı gibi .NET örneğini derleyip çalıştırmamaktadır. Ayrıca, kaynak kodu gözden geçirebilir ve kendi istemci uygulamalarınıza benzer yapılandırmalara de uygulayabilirsiniz.

**1. Adım:** Projeyi [Azure Cosmos DB performans testi örneğinden](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)Indirin veya GitHub deposunu çatalla.

**2. Adım:** App.config 'de EndpointUrl, AuthorizationKey, Collectionüretilen Iş ve DocumentTemplate (isteğe bağlı) ayarlarını değiştirin.

> [!NOTE]
> Yüksek aktarım hızı ile koleksiyonlar sağlamadan önce, koleksiyon başına maliyetleri tahmin etmek için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. , Faturalandırılır ve üretilen iş üretimini her saat temelinde bağımsız olarak Azure Cosmos DB. Test ettikten sonra Azure Cosmos kapsayıcılarınızın verimini silerek veya azaltarak maliyetleri tasarruf edebilirsiniz.
> 
> 

**Adım 3:** Konsol uygulamasını komut satırından derleyin ve çalıştırın. Aşağıdakine benzer bir çıktı görmeniz gerekir:

```bash
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
```

**4. adım (gerekliyse):** Araçtan bildirilen aktarım hızı (RU/s), koleksiyonun veya bir koleksiyon kümesinin sağlanan aktarım hızına göre aynı veya daha yüksek olmalıdır. Aksi takdirde, Degreeofparalelliği küçük artışlarla artırmak sınıra ulaşmanıza yardımcı olabilir. İstemci uygulama platetinizden üretilen iş hacmi, ek istemci makinelerde uygulamanın birden çok örneğini başlatın. Bu adımla ilgili yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com)bir destek bileti.

Uygulamayı çalıştırdıktan sonra, aktarım hızı ve gecikme süresi üzerinde etkilerini anlamak için farklı [Dizin oluşturma ilkeleri](index-policy.md) ve [tutarlılık düzeyleri](consistency-levels.md) deneyebilirsiniz. Ayrıca, kaynak kodu gözden geçirebilir ve kendi test paketleriniz veya üretim uygulamalarınıza benzer konfigürasyonlar uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, .NET konsol uygulaması kullanarak Azure Cosmos DB nasıl performans ve ölçek testi gerçekleştirekullanabileceğinizi inceledik. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

* [Azure Cosmos DB performans testi örneği](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Azure Cosmos DB performansını geliştirmek için istemci yapılandırma seçenekleri](performance-tips.md)
* [Azure Cosmos DB sunucu tarafında bölümlendirme](partition-data.md)


