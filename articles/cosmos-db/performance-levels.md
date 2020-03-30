---
title: Emekli Azure Cosmos DB performans düzeyleri
description: Azure Cosmos DB'de daha önce kullanılabilen S1, S2 ve S3 performans düzeyleri hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.openlocfilehash: 40735f91e2ca58cc42f723c7993686d92f0e5ff0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623337"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>S1, S2 ve S3 performans düzeyleri kullanımdan kaldırılıyor

> [!IMPORTANT] 
> Bu makalede tartışılan S1, S2 ve S3 performans düzeyleri kullanımdan kaldırılmakta ve artık yeni Azure Cosmos DB hesapları için kullanılamaz.
>

Bu makalede, S1, S2 ve S3 performans düzeylerine genel bir bakış sağlar ve bu performans düzeylerini kullanan koleksiyonların tek bölümlü koleksiyonlara nasıl geçirilebileceği açıklanır. Bu makaleyi okuduktan sonra, aşağıdaki soruları yanıtlamak mümkün olacak:

- [S1, S2 ve S3 performans düzeyleri neden emekli yekpare?](#why-retired)
- [Tek bölümlü koleksiyonlar ve bölümlenmiş koleksiyonlar S1, S2, S3 performans düzeyleriyle nasıl karşılaştırılır?](#compare)
- [Verilerime kesintisiz erişim sağlamak için ne yapmam gerekir?](#uninterrupted-access)
- [Geçişten sonra koleksiyonum nasıl değişecek?](#collection-change)
- [Tek bölüm koleksiyonlarına geçtikten sonra faturam nasıl değişecek?](#billing-change)
- [20 GB'dan fazla depolama alanına ihtiyacım olursa ne olur?](#more-storage-needed)
- [Planlanan geçişten önce S1, S2 ve S3 performans düzeyleri arasında geçiş yapabilir miyim?](#change-before)
- [S1, S2, S3 performans düzeylerinden tek bölüm koleksiyonlarına tek başıma nasıl geçiş yapabilirim?](#migrate-diy)
- [EA müşterisi ysem nasıl etkilenirim?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>S1, S2 ve S3 performans seviyeleri neden emekli oluyor?

S1, S2 ve S3 performans düzeyleri, standart Azure Cosmos DB teklifinin sağladığı esnekliği sağlamaz. S1, S2, S3 performans seviyeleri ile hem iş hem de depolama kapasitesi önceden ayarlanmış ve elastikiyet sunmuyordu. Azure Cosmos DB artık iş ve depolama alanınızı özelleştirme olanağı sunarak, gereksinimleriniz değiştikçe ölçeklendirme yeteneğinizde çok daha fazla esneklik sunar.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Tek bölümlü koleksiyonlar ve bölümlenmiş koleksiyonlar S1, S2, S3 performans düzeyleriyle nasıl karşılaştırılır?

Aşağıdaki tablo, tek bölümlü koleksiyonlarda, bölümlenmiş koleksiyonlarda ve S1, S2, S3 performans düzeylerinde bulunan iş ve depolama seçeneklerini karşılaştırır. Burada ABD Doğu 2 bölgesi için bir örnektir:

|   |Bölümlenmiş koleksiyon|Tek bölüm koleksiyonu|S1|S2|S3|
|---|---|---|---|---|---|
|Aktarım hızı üst sınırı|Sınırsız|10K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Minimum iş elde|2.5 K RU/s|400 RU/sn|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maksimum depolama alanı|Sınırsız|20 GB|20 GB|20 GB|20 GB|
|Fiyat (aylık)|Satış Tarihi: $6 / 100 RU/s<br><br>Depolama: $0.25/GB|Satış Tarihi: $6 / 100 RU/s<br><br>Depolama: $0.25/GB|25 $ ABD Doları|50 TL|100 TL|

EA müşterisi misiniz? Eğer öyleyse, [ea müşterisi ysem nasıl etkileneceğimi görün?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Verilerime kesintisiz erişim sağlamak için ne yapmam gerekir?

S1, S2 veya S3 koleksiyonunuz varsa, [.NET SDK'yı kullanarak](#migrate-diy)koleksiyonu programlı olarak tek bir bölüm koleksiyonuna geçirmelisiniz. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Geçişten sonra koleksiyonum nasıl değişecek?

Bir S1 koleksiyonunuz varsa, bunları 400 RU/s iş bölümü içeren tek bir bölüm koleksiyonuna geçirebilirsiniz. 400 RU/s, tek bölümlü koleksiyonlarla kullanılabilen en düşük iş bölümüdür. Ancak, tek bir bölüm koleksiyonundaki 400 RU/s'nin maliyeti, S1 koleksiyonunuz ve 250 RU/s'inizle yaklaşık olarak aynıdır – bu nedenle size sunulan ekstra 150 RU/s için ödeme yapmazsınız.

Bir S2 koleksiyonunuz varsa, bunları 1 K RU/s içeren tek bir bölüm koleksiyonuna geçirebilirsiniz. İş verme seviyenizde bir değişiklik göremezsiniz.

Bir S3 koleksiyonunuz varsa, bunları 2,5 K RU/s ile tek bir bölüm koleksiyonuna geçirebilirsiniz. İş verme seviyenizde bir değişiklik göremezsiniz.

Bu durumların her birinde, koleksiyonu niçin geçirebildikten sonra, iş düzeyi düzeyinizi özelleştirebilir veya kullanıcılarınıza düşük gecikmeli erişim sağlamak için gerektiğinde yukarı ve aşağı ölçeklendirebilirsiniz. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Tek bölüm koleksiyonlarına geçtikten sonra faturam nasıl değişecek?

ABD Doğu bölgesinde 10 S1 koleksiyonunuz, her biri için 1 GB depolama alanınız olduğunu varsayarsak ve bu 10 S1 koleksiyonunu 400 RU/sn (minimum düzeyde) 10 tek bölüm koleksiyonuna geçirin. 10 tek bölüm koleksiyonunu tam bir ay boyunca tutarsanız faturanız aşağıdaki gibi görünecektir:

![10 koleksiyon için S1 fiyatlandırması, tek bir bölüm koleksiyonu için fiyatlandırmayı kullanarak 10 koleksiyonla karşılaştırır](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-20-gb-of-storage"></a>20 GB'dan fazla depolama alanına ihtiyacım olursa ne olur?

İster S1, S2 veya S3 performans düzeyine sahip bir koleksiyonunuz olsun, ister 20 GB depolama alanı bulunan tek bir bölüm koleksiyonunuz olsun, verilerinizi neredeyse bölümlere ayrılmıştır bir koleksiyona geçirmek için Azure Cosmos DB Veri Geçişi aracını kullanabilirsiniz sınırsız depolama. Bölümlenmiş koleksiyonun yararları hakkında daha fazla bilgi için [Azure Cosmos DB'de Bölümleme ve ölçekleme](sql-api-partition-data.md)bölümüne bakın. 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Planlanan geçişten önce S1, S2 ve S3 performans düzeyleri arasında geçiş yapabilir miyim?

Sadece S1, S2 ve S3 performansına sahip varolan hesaplar [.NET SDK kullanılarak](#migrate-diy)değiştirilebilir ve performans düzeyi katmanlarını programlı olarak değiştirebilir. S1, S3 veya S3'ten tek bir bölüm koleksiyonuna geçiş ederseniz, S1, S2 veya S3 performans düzeylerine geri dönemezsiniz.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>S1, S2, S3 performans düzeylerinden tek bölüm koleksiyonlarına tek başıma nasıl geçiş yapabilirim?

S1, S2 ve S3 performans düzeylerinden [.NET SDK'yı kullanarak](#migrate-diy)programlı olarak tek bölüm koleksiyonlarına geçiş yapabilirsiniz. Tek bölümlü koleksiyonlarla kullanılabilen esnek iş verme seçeneklerinden yararlanmak için, planlanan geçişten önce bunu kendi başına yapabilirsiniz.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>.NET SDK'yı kullanarak tek bölüm koleksiyonlarına geçiş

Bu bölüm yalnızca [SQL .NET API'yi](sql-api-sdk-dotnet.md)kullanarak bir koleksiyonun performans düzeyini değiştirmeyi kapsar, ancak işlem diğer SDK'larımız için benzerdir.

Burada, koleksiyon iş parçacığının saniyede 5.000 istek birimiyle değiştirilmesi için bir kod parçacığı verme vardır:
    
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

Ek örnekleri görüntülemek ve teklif yöntemlerimiz hakkında daha fazla bilgi edinmek için [MSDN'yi](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) ziyaret edin:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**YerineOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>EA müşterisi ysem nasıl etkilenirim?

EA müşterileri mevcut sözleşmelerinin sonuna kadar fiyat korumalı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB ile fiyatlandırma ve verileri yönetme hakkında daha fazla bilgi edinmek için şu kaynakları keşfedin:

1.  [Cosmos DB'de veri bölümleme.](sql-api-partition-data.md) Tek bölümlü kapsayıcı ve bölümlenmiş kapsayıcılar arasındaki farkı ve sorunsuz ölçeklendirmek için bir bölümleme stratejisi uygulama ipuçlarını anlayın.
2.  [Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/). İş maliyeti sağlama nın ve depolamanın tüketme maliyeti hakkında bilgi edinin.
3.  [İstek birimleri](request-units.md). Okuma, Yazma, Sorgula gibi farklı işlem türleri için iş elde edilen iş için iş elde tüketimini anlayın.
