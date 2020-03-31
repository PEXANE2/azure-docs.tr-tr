---
title: Azure Cosmos DB'de depolama maliyetini optimize edin
description: Bu makalede, Azure Cosmos DB'de depolanan verilerin depolama maliyetlerinin nasıl yönetilen
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: 1508adda761fcba7ba70df3bb212d3eb4e32f242
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72754954"
---
# <a name="optimize-storage-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de depolama maliyetini optimize edin

Azure Cosmos DB sınırsız depolama ve iş artışı sunar. Azure Cosmos kapsayıcılarınızda veya veritabanlarınızda sağlamanız/yapılandırmanız gereken iş biriminin aksine, depolama alanı tüketim bazında faturalandırılır. Yalnızca tükettiğiniz mantıksal depolama için faturalandırılırsınız ve önceden herhangi bir depolama alanı ayırmanız gerekmez. Depolama, bir Azure Cosmos kapsayıcısına eklediğiniz veya kaldırdığınız verilere göre otomatik olarak yukarı ve aşağı ölçeklendirilir.

## <a name="storage-cost"></a>Depolama maliyeti

Depolama, GB'lerin birimiyle birlikte faturalandırılır. Yerel SSD destekli depolama, verileriniz ve dizin oluşturmanız tarafından kullanılır. Kullanılan toplam depolama alanı, Azure Cosmos DB kullandığınız tüm bölgelerde kullanılan veriler ve dizinler tarafından gereken depolama alanına eşittir. Bir Azure Cosmos hesabını üç bölgede genel olarak çoğaltırsanız, bu üç bölgenin her birinde toplam depolama maliyetini ödenirsiniz. Depolama gereksiniminizi tahmin etmek için [kapasite planlayıcısı](https://www.documentdb.com/capacityplanner) aracına bakın. Azure Cosmos DB'deki depolama maliyeti ayda $0,25 GB'dır, en son güncelleştirmeler için [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. Azure Cosmos kapsayıcınız tarafından kullanılan depolama alanını belirlemek, depolama alanınızı izlemek için uyarılar ayarlayabilir, [Azure Cosmos DB'yi izle](monitor-accounts.md)makalesine bakınız.

## <a name="optimize-cost-with-item-size"></a>Madde boyutuyla maliyeti optimize edin

Azure Cosmos DB, en iyi performans ve maliyet avantajları için madde boyutunun 2 MB veya daha az olmasını bekler. 2 MB'dan büyük verileri depolamak için herhangi bir öğeye ihtiyacınız varsa, öğe şemasını yeniden tasarlamayı düşünün. Şema'yı yeniden tasarlayamayacağınız nadir bir durumda, öğeyi alt öğelere bölebilir ve bunları ortak bir tanımlayıcı (ID) ile mantıksal olarak bağlayabilirsiniz. Tüm Azure Cosmos DB özellikleri, bu mantıksal tanımlayıcıya sabitleyerek tutarlı bir şekilde çalışır.

## <a name="optimize-cost-with-indexing"></a>Dizin oluşturma ile maliyeti optimize etme

Varsayılan olarak, veriler otomatik olarak dizine eklenir ve bu da tüketilen toplam depolama alanını artırabilir. Ancak, bu ek yükü azaltmak için özel dizin ilkeleri uygulayabilirsiniz. İlke aracılığıyla ayarlanmamış otomatik dizin oluşturma, öğe boyutunun yaklaşık %10-20'si kadardır. Dizin ilkelerini kaldırarak veya özelleştirerek, yazmalar için ek maliyet ödemezsiniz ve ek iş yapma kapasitesi gerektirmezsiniz. Özel dizin oluşturma ilkelerini yapılandırmak için [Azure Cosmos DB'de Dizin Oluşturma'ya](indexing-policies.md) bakın. Daha önce ilişkisel veritabanları ile çalıştıysanız, "her şeyi dizine" depolama veya daha yüksek iki katına anlamına geldiğini düşünebilirsiniz. Ancak, Azure Cosmos DB'de, ortanca durumda, çok daha düşüktür. Azure Cosmos DB'de, dizin depolama yükü genellikle düşüktür (%10-20) düşük depolama ayak izi için tasarlandığından otomatik indeksleme ile bile. Dizin oluşturma ilkesini yöneterek, dizin ayak izinin ve sorgu performansının tradeoff'unu daha ince taneli bir şekilde denetleyebilirsiniz.

## <a name="optimize-cost-with-time-to-live-and-change-feed"></a>Yaşamak ve akışı değiştirmek için zaman ile maliyeti optimize edin

Verilere artık ihtiyacınız [olmadığında, yaşamak için zaman](time-to-live.md)kullanarak Azure Cosmos hesabınızdan düzgün bir şekilde silebilirsiniz, [özet akışını değiştirebilir](change-feed.md) veya eski verileri Azure blob depolama veya Azure veri ambarı gibi başka bir veri deposuna geçirebilirsiniz. Azure Cosmos DB, yaşamak veya TTL'ye zamanla belirli bir süre sonra öğeleri bir kapsayıcıdan otomatik olarak silme olanağı sağlar. Varsayılan olarak, zamanı kapsayıcı düzeyinde yaşayacak şekilde ayarlayabilir ve madde başına değeri geçersiz kılabilirsiniz. TTL'yi bir kapsayıcıda veya öğe düzeyinde ayarladıktan sonra, Azure Cosmos DB bu öğeleri en son değiştirilme tarihinden sonraki süre sonunda otomatik olarak kaldırır. Değişiklik akışı'nı kullanarak, verileri Azure Cosmos DB'deki başka bir kapsayıcıya veya harici bir veri deposuna geçirebilirsiniz. Geçiş sıfır zaman alır ve geçiş bittiğinde, kaynak Azure Cosmos kapsayıcısını silmek için zamanı silebilir veya canlı olarak yapılandırabilirsiniz.

## <a name="optimize-cost-with-rich-media-data-types"></a>Zengin medya veri türleri ile maliyeti optimize edin 

Videolar, resimler vb. gibi zengin medya türlerini depolamak istiyorsanız, Azure Cosmos DB'de birkaç seçeneğiniz vardır. Seçeneklerden biri, bu zengin ortam türlerini Azure Cosmos öğeleri olarak depolamaktır. Öğe başına 2 MB sınırı vardır ve veri öğesini birden çok alt öğeye zincirleyerek bu sınırdan kaçınabilirsiniz. Veya bunları Azure Blob depolama alanında saklayabilir ve Azure Cosmos öğelerinizden referans almak için meta verileri kullanabilirsiniz. Bu yaklaşım ile artıları ve eksileri bir dizi vardır. İlk yaklaşım, normal Azure Cosmos öğelerinize ek olarak zengin medya veri türleri için gecikme, üretim SLA'ları ve anahtar teslimi küresel dağıtım özellikleri açısından en iyi performansı sağlar. Ancak destek daha yüksek bir fiyata kullanılabilir. Ortamı Azure Blob depolama alanında depolayarak, toplam maliyetlerinizi düşürebilirsiniz. Gecikme sonu kritikse, Azure Cosmos öğelerinden başvurulan zengin medya dosyaları için premium depolama alanı kullanabilirsiniz. Bu, coğrafi kısıtlamayı aşmak için kenar sunucusundan görüntüleri daha düşük maliyetle sunmak için CDN ile yerel olarak tümleşir. Bu senaryonun alt tarafı, operasyonel maliyetleri artırabilecek iki hizmetle -Azure Cosmos DB ve Azure Blob depolama alanı- uğraşmanız gerektiğidir. 

## <a name="check-storage-consumed"></a>Tüketilen depolamayı kontrol edin

Azure Cosmos kapsayıcısının depolama tüketimini denetlemek için, kapsayıcıda bir HEAD veya GET `x-ms-request-quota` isteği `x-ms-request-usage` çalıştırabilir ve üstbilgileri ve üstbilgileri inceleyebilirsiniz. Alternatif olarak, .NET SDK ile çalışırken, depolama nın tüketilmesi için [DocumentSizeQuota](https://docs.microsoft.com/previous-versions/azure/dn850325(v%3Dazure.100))ve [DocumentSizeKullanım](https://msdn.microsoft.com/library/azure/dn850324.aspx) özelliklerini kullanabilirsiniz.

## <a name="using-sdk"></a>SDK’yı kullanma

```csharp
// Measure the item size usage (which includes the index size)
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));   

Console.WriteLine("Item size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makalelerle Azure Cosmos DB'de maliyet optimizasyonu hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin

