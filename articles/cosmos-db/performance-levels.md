---
title: Azure Cosmos DB performans düzeyleri kullanımdan kaldırıldı
description: Daha önce Azure Cosmos DB ' de bulunan S1, S2 ve S3 performans düzeyleri hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.openlocfilehash: 2d9e9ed1a65265a119f59edee7da1b8f244e2f7f
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85112744"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>S1, S2 ve S3 performans düzeyleri kullanımdan kaldırılıyor

> [!IMPORTANT] 
> Bu makalede ele alınan S1, S2 ve S3 performans düzeyleri kullanımdan kaldırılıyor ve yeni Azure Cosmos DB hesapları için artık kullanılamıyor.
>

Bu makalede S1, S2 ve S3 performans düzeylerine genel bir bakış sağlanır ve bu performans düzeylerini kullanan koleksiyonların tek bölümlenmiş koleksiyonlara nasıl geçirilebileceği açıklanır. Bu makaleyi okuduktan sonra aşağıdaki soruları cevaplayabilirsiniz:

- [S1, S2 ve S3 performans düzeyleri neden kullanımdan kaldırılıyor?](#why-retired)
- [Tek bölüm koleksiyonları ve bölümlenmiş koleksiyonlar S1, S2, S3 performans düzeyleri ile nasıl karşılaştırılır?](#compare)
- [Verilerinize kesintisiz erişim sağlamak için ne yapmam gerekir?](#uninterrupted-access)
- [Koleksiyonum geçişten sonra nasıl değiştirilir?](#collection-change)
- [Tek bölüm koleksiyonlarına geçirildikten sonra faturalandırma değiştirmem nasıl olacak?](#billing-change)
- [20 GB 'den fazla depolamaya ihtiyacım varsa ne yapmalıyım?](#more-storage-needed)
- [Planlı geçişten önce S1, S2 ve S3 performans düzeyleri arasında değişiklik yapabilir miyim?](#change-before)
- [S1, S2, S3 performans düzeyinden kendi kendine tek bölümlü koleksiyonlara geçiş Nasıl yaparım?.](#migrate-diy)
- [EA müşteriysem ne etkilenir?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>S1, S2 ve S3 performans düzeyleri neden kullanımdan kaldırıldı?

S1, S2 ve S3 performans düzeyleri, standart Azure Cosmos DB teklifinin sağladığı esnekliği sunmaz. S1, S2, S3 performans düzeylerinde, hem aktarım hızı hem de depolama kapasitesi önceden ayarlanmıştır ve esneklik sunmaz. Azure Cosmos DB, aktarım hızını ve depolamayı özelleştirme olanağı sunarak gereksinimleriniz değiştikçe ölçeklendirmeniz için çok daha fazla esneklik sunar.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Tek bölüm koleksiyonları ve bölümlenmiş koleksiyonlar S1, S2, S3 performans düzeyleri ile nasıl karşılaştırılır?

Aşağıdaki tabloda, tek bölüm koleksiyonlarında, bölümlenmiş koleksiyonlarda ve S1, S2, S3 performans düzeylerinde bulunan aktarım hızı ve depolama seçenekleri karşılaştırılmaktadır. ABD Doğu 2 bölge için bir örnek aşağıda verilmiştir:

|   |Bölümlenmiş koleksiyon|Tek bölümlü toplama|S1|S2|S3|
|---|---|---|---|---|---|
|Aktarım hızı üst sınırı|Sınırsız|10.000 RU/sn|250 RU/sn|1 K RU/sn|2,5 K RU/sn|
|En düşük aktarım hızı|2,5 K RU/sn|400 RU/sn|250 RU/sn|1 K RU/sn|2,5 K RU/sn|
|Maksimum depolama alanı|Sınırsız|20 GB|20 GB|20 GB|20 GB|
|Fiyat (aylık)|Aktarım hızı: $6/100 RU/s<br><br>Depolama alanı: $0,25/GB|Aktarım hızı: $6/100 RU/s<br><br>Depolama alanı: $0,25/GB|$25 ABD DOLARı|$50 ABD DOLARı|$100 ABD DOLARı|

EA müşterisi misiniz? Öyleyse, [BIR EA Müşteriysem nasıl etkilenirim?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Verilerinize kesintisiz erişim sağlamak için ne yapmam gerekir?

S1, S2 veya S3 koleksiyonunuz varsa, [.NET SDK 'yı kullanarak](#migrate-diy)koleksiyonu tek bir bölüm koleksiyonuna geçirmeniz gerekir. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Koleksiyonum geçişten sonra nasıl değiştirilir?

S1 koleksiyonunuz varsa, bunları 400 RU/sn aktarım hızı ile tek bir bölüm koleksiyonuna geçirebilirsiniz. 400 RU/s, tek bölümlü koleksiyonlarla kullanılabilir en düşük aktarım hızı. Ancak, tek bir bölüm koleksiyonundaki 400 RU/sn maliyeti, S1 koleksiyonunuz ve 250 RU/s ile ödeirken yaklaşık olarak aynıdır. bu nedenle, size sunulan ekstra 150 RU/sn için ödeme yapmak zorunda değilsiniz.

Bir S2 koleksiyonunuz varsa, bunları 1 K RU/sn ile tek bir bölüm koleksiyonuna geçirebilirsiniz. Aktarım hızı düzeyinize hiçbir değişiklik görmezsiniz.

S3 koleksiyonunuz varsa, bunları 2,5 K RU/s ile tek bir bölüm koleksiyonuna geçirebilirsiniz. Aktarım hızı düzeyinize hiçbir değişiklik görmezsiniz.

Bu durumların her birinde, koleksiyonu geçirdikten sonra, verimlilik düzeyinizi özelleştirebilir veya kullanıcılarınıza düşük gecikme süreli erişim sağlamak için gerektiğinde ölçeği yukarı veya aşağı ölçeklendirebilirsiniz. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Tek bölüm koleksiyonlarına geçirildikten sonra faturalandırma değiştirmem nasıl olacak?

10 S1 koleksiyonunuz, her biri için 1 GB depolama alanı, ABD Doğu bölgesinde ve bu 10 S1 koleksiyonlarını, 400 RU/sn (en düşük düzeyde) konumundaki 10 tek bölüm koleksiyonuna geçiriyorsanız kabul edersiniz. 10 tek bölümlü koleksiyonları tam bir ay boyunca tutarsanız faturanız aşağıdaki gibi görünür:

:::image type="content" source="./media/performance-levels/s1-vs-standard-pricing.png" alt-text="10 koleksiyonların S1 fiyatlandırması, tek bir bölüm koleksiyonu için fiyatlandırma kullanarak 10 koleksiyonla karşılaştırılır" border="false":::

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-20-gb-of-storage"></a>20 GB 'den fazla depolamaya ihtiyacım varsa ne yapmalıyım?

S1, S2 veya S3 performans düzeyine sahip bir koleksiyonunuz olsun ya da hepsi 20 GB depolama alanı bulunan tek bir bölüm koleksiyonuna sahipseniz, verilerinizi neredeyse sınırsız depolama alanı olan bölümlenmiş bir koleksiyona geçirmek için Azure Cosmos DB veri geçiş aracı ' nı kullanabilirsiniz. Bölümlenmiş bir koleksiyonun avantajları hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB bölümleme ve ölçeklendirme](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Planlı geçişten önce S1, S2 ve S3 performans düzeyleri arasında değişiklik yapabilir miyim?

Yalnızca S1, S2 ve S3 performansına sahip mevcut hesaplar, [.NET SDK kullanılarak](#migrate-diy)programlı bir şekilde değiştirilebilir ve performans düzeyi katmanlarını değiştirebilir. S1, S3 veya S3 sürümünden tek bir bölüm koleksiyonuna geçiş yaparsanız S1, S2 veya S3 performans düzeylerine geri dönemezsiniz.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>S1, S2, S3 performans düzeyinden kendi kendine tek bölümlü koleksiyonlara geçiş Nasıl yaparım?.

S1, S2 ve S3 performans düzeylerinden, [.NET SDK kullanarak](#migrate-diy)programlı bir şekilde tek bölümlü koleksiyonlara geçiş yapabilirsiniz. Bu seçeneği, planlı geçişten önce, tek bölümlü koleksiyonlarla kullanılabilen esnek aktarım hızı seçeneklerinden faydalanmak için kullanabilirsiniz.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>.NET SDK kullanarak tek bölümlü koleksiyonlara geçirme

Bu bölüm yalnızca [SQL .NET API 'si](sql-api-sdk-dotnet.md)kullanılarak bir koleksiyonun performans düzeyinin değiştirilmesini içerir, ancak Işlem diğer SDK 'larımız için benzerdir.

Saniye başına 5.000 istek birimi olarak koleksiyon aktarım hızını değiştirmek için bir kod parçacığı aşağıda verilmiştir:
    
```csharp
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Ek örnekleri görüntülemek ve teklif yöntemlerimiz hakkında daha fazla bilgi edinmek için [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) 'yi ziyaret edin:

* [**Readoffsilinebilir eşitleme**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**Readoffersfeedadsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**Replaceoffsilinebilir eşitleme**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>EA müşteriysem ne etkilenir?

EA müşterileri, geçerli sözleşmesinin sonuna kadar fiyat korumalı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB, fiyatlandırma ve verileri yönetme hakkında daha fazla bilgi edinmek için şu kaynakları bulun:

1.  [Cosmos DB verileri bölümleme](sql-api-partition-data.md). Tek bölümlü kapsayıcı ve bölümlenmiş kapsayıcılar arasındaki farkı anlayın ve sorunsuz ölçeklendirmek için bölümleme stratejisi uygulama ipuçları.
2.  [Cosmos DB fiyatlandırması](https://azure.microsoft.com/pricing/details/cosmos-db/). Sağlama verimi ve depolama alanı kullanma maliyeti hakkında bilgi edinin.
3.  [İstek birimleri](request-units.md). Örneğin okuma, yazma, sorgulama gibi farklı işlem türleri için üretilen iş kullanımını anlayın.
