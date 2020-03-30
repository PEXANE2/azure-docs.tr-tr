---
title: Azure Cosmos DB'deki sorguları çalıştırmak için maliyeti ve RU/ları optimize etme
description: Bir sorgu için istek birimi ücretlerini nasıl değerlendireceklerini ve sorguyu performans ve maliyet açısından nasıl optimize edebilirsiniz öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: dd75ad4ed1024292868f113e474fe8b8b73679b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445124"
---
# <a name="optimize-query-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de sorgu maliyetini iyileştirme

Azure Cosmos DB, kapsayıcının içindeki öğeler üzerinde çalışan ilişkisel ve hiyerarşik sorgular da dahil olmak üzere zengin bir veritabanı işlemleri kümesi sunar. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, işlemi tamamlamak için gereken CPU, GÇ ve belleğe göre değişiklik gösterir. Donanım kaynakları hakkında düşünmek ve bunları yönetmek yerine, bir isteğe hizmet sağlamak üzere çeşitli veritabanı işlemlerini gerçekleştirmek için gereken kaynaklar için istek birini (RU) tek ölçü olarak düşünebilirsiniz. Bu makalede sorgu için istek birimi ücretlerinin değerlendirilmesi, ayrıca performans ve maliyet açısından sorgunun iyileştirilmesi açıklanır. 

Azure Cosmos DB'deki sorgular genellikle en hızlı/en verimliden daha yavaş/daha az verimli olana aşağıdaki gibi sıralanır:  

* Tek bir bölüm tuşu ve öğe anahtarı üzerinde get işlemi.

* Tek bir bölüm tuşu içinde filtre yan tümcesi ile sorgula.

* Herhangi bir özellik üzerinde eşitlik veya aralık filtresi yan tümcesi olmadan sorgula.

* Filtresiz sorgula.

Bir veya daha fazla bölümdeki verileri okuyan sorgular daha yüksek gecikme ye neden eder ve daha fazla sayıda istek birimi tüketir. Her bölüm tüm özellikler için otomatik dizin oluşturma olduğundan, sorgu dizin verimli bir şekilde sunulabilir. Paralellik seçeneklerini kullanarak birden çok bölümü daha hızlı kullanan sorgular yapabilirsiniz. Bölümleme ve bölümleme tuşları hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'de Bölümleme bölümüne](partitioning-overview.md)bakın.

## <a name="evaluate-request-unit-charge-for-a-query"></a>Sorgu için istek birimi ücretini değerlendirme

Azure Cosmos kapsayıcılarınızda bazı verileri depoladıktan sonra, sorgularınızı oluşturmak ve çalıştırmak için Azure portalındaki Veri Gezgini'ni kullanabilirsiniz. Ayrıca veri gezgini kullanarak sorguların maliyetini alabilirsiniz. Bu yöntem, sisteminizin desteklediği tipik sorgular ve işlemlerle ilgili fiili ücretler hakkında bir fikir verir.

Ayrıca, SDK'ları kullanarak sorgu ların maliyetini programlı olarak da alabilirsiniz. REST API'yi kullanırken `x-ms-request-charge` üstbilgiyi oluşturma, güncelleştirme veya silme gibi herhangi bir işlemin genel merkezin ek yüküölçmek için. .NET veya Java SDK kullanıyorsanız, `RequestCharge` özellik istek ücretini almak için eşdeğer özelliktir ve bu özellik ResourceResponse veya FeedResponse içinde bulunur.

```csharp
// Measure the performance (request units) of writes 
ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument); 

Console.WriteLine("Insert of an item consumed {0} request units", response.RequestCharge); 

// Measure the performance (request units) of queries 
IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery(); 

while (queryable.HasMoreResults) 
     { 
          FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
          Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge); 
     }
```

## <a name="factors-influencing-request-unit-charge-for-a-query"></a>Bir sorgu için istek birimi ücretini etkileyen etkenler

Sorgular için istek birimleri bir dizi etkene bağlıdır. Örneğin, yüklenen/döndürülen Azure Cosmos öğelerinin sayısı, dizin karşısındaki arama sayısı, sorgu derleme süresi vb. ayrıntılar. Azure Cosmos DB, aynı verilerüzerinde yürütüldüğünde aynı sorgunun, yinelenen yürütmelerle bile her zaman aynı sayıda istek birimini tüketeceğini garanti eder. Sorgu yürütme ölçümlerini kullanan sorgu profili, istek birimlerinin nasıl harcandığı hakkında iyi bir fikir verir.  

Bazı durumlarda, sorguların sayfalı yürütülmesinde 200 ve 429 yanıttan oluşan bir dizi ve değişken istek birimleri görebilirsiniz, çünkü sorgular kullanılabilir RUs'lara göre olabildiğince hızlı çalışır. Sunucu ve istemci arasında birden çok sayfa/gidiş-dönüş gezileri içine bir sorgu yürütme sonu görebilirsiniz. Örneğin, her biri o sayfa için gerçekleştirilen hesaplamaya göre ücretlendirilen 10.000 öğe birden çok sayfa olarak döndürülebilir. Bu sayfalarda toplam aldığınızda, tüm sorgu için alacağınız rus sayısını elde etmelisiniz.  

## <a name="metrics-for-troubleshooting"></a>Sorun giderme ölçümleri

Performans ve sorgular tarafından tüketilen iş, kullanıcı tanımlı işlevler (UDFs) çoğunlukla işlev gövdesine bağlıdır. Sorgu yürütmesinin UDF'de ne kadar zaman harcandıdığını ve tüketilen RUs sayısını öğrenmenin en kolay yolu, Sorgu Ölçümleri'ni etkinleştirmektir. .NET SDK kullanıyorsanız, SDK tarafından döndürülen örnek sorgu ölçümleri aşağıda verilmiştir:

```bash
Retrieved Document Count                 :               1              
Retrieved Document Size                  :           9,963 bytes        
Output Document Count                    :               1              
Output Document Size                     :          10,012 bytes        
Index Utilization                        :          100.00 %            
Total Query Execution Time               :            0.48 milliseconds 
  Query Preparation Times 
    Query Compilation Time               :            0.07 milliseconds 
    Logical Plan Build Time              :            0.03 milliseconds 
    Physical Plan Build Time             :            0.05 milliseconds 
    Query Optimization Time              :            0.00 milliseconds 
  Index Lookup Time                      :            0.06 milliseconds 
  Document Load Time                     :            0.03 milliseconds 
  Runtime Execution Times 
    Query Engine Execution Time          :            0.03 milliseconds 
    System Function Execution Time       :            0.00 milliseconds 
    User-defined Function Execution Time :            0.00 milliseconds 
  Document Write Time                    :            0.00 milliseconds 
  Client Side Metrics 
    Retry Count                          :               1              
    Request Charge                       :            3.19 RUs  
```

## <a name="best-practices-to-cost-optimize-queries"></a>Sorguları en iyi duruma getirmek için en iyi uygulamalar 

Maliyet sorgularını optimize ederken aşağıdaki en iyi uygulamaları göz önünde bulundurun:

* **Birden çok varlık türünü bir ertele**

   Tek veya daha az sayıda kapsayıcı içinde birden çok varlık türünü birleştirmeye çalışın. Bu yöntem, yalnızca fiyatlandırma açısından değil, sorgu yürütme ve hareketler için de avantajlar sağlar. Sorgular tek bir kapsayıcıya kadar kapsamlıdır; ve depolanan yordamlar/tetikleyiciler aracılığıyla birden çok kayıt üzerindeki atomik işlemler tek bir kapsayıcı içinde bir bölüm anahtarına kadar kapsamlıdır. Aynı kapsayıcı içindeki varlıkları niçin colocating kayıtları arasındaki ilişkileri çözmek için ağ gidiş-dönüş lerinin sayısını azaltabilir. Bu nedenle uçlardan uca performansı artırır, daha büyük bir veri kümesi için birden çok kayıt üzerinden atomik işlemlere olanak tanır ve sonuç olarak maliyetleri düşürür. Tek veya daha az sayıda kapsayıcı içinde birden çok varlık türünü colocating senaryonuz için zor ise, genellikle varolan bir uygulama geçiş çünkü ve herhangi bir kod değişikliği yapmak istemiyorum - o zaman sağlama düşünmelisiniz veritabanı düzeyinde iş tir.  

* **Daha düşük istek birimleri/ikinci kullanım için ölçün ve ayarlayın**

   Bir sorgunun karmaşıklığı, bir işlem için kaç istek birimi (RUs) tüketilir etkiler. Yüklemlerin sayısı, yüklemlerin yapısı, UDF sayısı ve kaynak veri kümesinin boyutu. Tüm bu etkenler sorgu işlemlerinin maliyetini etkiler. 

   İstek üstbilgisinde döndürülen istek ücreti, belirli bir sorgunun maliyetini gösterir. Örneğin, bir sorgu 1000 1-KB öğesini döndürürse, işlemin maliyeti 1000'dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlamadan önce yalnızca iki tür isteği onurlandırır. Daha fazla bilgi için [istek birimleri](request-units.md) makalesine ve istek birimi hesap makinesine bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makalelerle Azure Cosmos DB'de maliyet optimizasyonu hakkında daha fazla bilgi edinebilirsiniz:

* [Azure Cosmos fiyatlandırması nasıl çalışır](how-pricing-works.md) hakkında daha fazla bilgi edinin
* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB rezerve etme kapasitesi](cosmos-db-reserved-capacity.md) hakkında daha fazla bilgi edinin

