---
title: Katmanlar ve SKU 'lar için hizmet limitleri
titleSuffix: Azure Cognitive Search
description: Kapasite planlaması ve Azure Bilişsel Arama istek ve yanıtlarının en fazla sınırı için kullanılan hizmet limitleri.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 35cac2c05a8603313bb2bbe1bde3817dc88c6ed2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83682640"
---
# <a name="service-limits-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki hizmet sınırları

Depolama, iş yükleri ve dizin ve diğer nesneler için maksimum sınırlar, [Azure bilişsel arama](search-create-service-portal.md) **ücretsiz**, **temel**, **Standart**veya **depolama için iyileştirilmiş** fiyatlandırma katmanlarında sağlayıp sağlamadığınıza bağlıdır.

+ **Ücretsiz** , Azure aboneliğinizle birlikte gelen çok kiracılı bir paylaşılan hizmettir. 

+ **Temel** , üretim iş yükleri için daha küçük bir ölçekte özel bilgi işlem kaynakları sağlar, ancak bazı ağ altyapısını diğer kiracılar ile paylaşır.

+ **Standart** , her düzeyde daha fazla depolama ve işleme kapasitesine sahip adanmış makinelerde çalışır. Standart dört düzeyde gelir: S1, S2, S3 ve S3 HD. S3 yüksek yoğunluk (S3 HD), [çok kiracılı](search-modeling-multitenant-saas-applications.md) ve büyük miktarlarda küçük dizinler (hizmet başına 3000 dizin) için tasarlanmıştır. S3 HD, [Dizin Oluşturucu özelliğini](search-indexer-overview.md) sağlamaz ve veri alımı, verileri kaynaktan dizine ileten API 'lerden faydalamamalıdır. 

+ **Depolama Için iyileştirilmiş** , ayrılmış makinelerde daha fazla toplam depolama, depolama bant genişliği ve **Standart**bellek içeren çalışır. Bu katman, büyük, yavaş değişen dizinleri hedefler. Depolama için Iyileştirilmiş iki düzeyde gelir: L1 ve L2.

## <a name="subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Depolama sınırları
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Dizin sınırları

| Kaynak | Ücretsiz | Temel &nbsp; <sup>1</sup>  | S1 | S2 | S3 | S3 &nbsp; HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| En fazla dizin |3 |5 veya 15 |50 |200 |200 |Bölüm başına 1000 veya hizmet başına 3000 |10 |10 |
| Dizin başına en fazla basit alan |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Dizin başına en fazla karmaşık koleksiyon alanı |40 |40 |40 |40 |40 |40 |40 |40 |
| Belge başına tüm karmaşık koleksiyonlardaki en fazla öğe &nbsp; <sup>2</sup> |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Karmaşık alanların en büyük derinliği |10 |10 |10 |10 |10 |10 |10 |10 |
| Dizin başına maksimum [Öneri araçları](https://docs.microsoft.com/rest/api/searchservice/suggesters) |1 |1 |1 |1 |1 |1 |1 |1 |
| Dizin başına maksimum [Puanlama profili](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) |100 |100 |100 |100 |100 |100 |100 |100 |
| Profil başına en fazla işlev |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Aralık 2017 ' den önce oluşturulan temel hizmetler, dizinlerde alt limitlere (15 yerine 5) sahip olmalıdır. Temel katman, dizin başına 100 alan için alt sınıra sahip tek SKU.

<sup>2</sup> her belge için karmaşık koleksiyonlarda çok fazla sayıda öğe olması, yüksek depolama kullanımına neden oluyor. Bu bilinen bir sorundur. Bu sırada, 3000 sınırı tüm hizmet katmanları için güvenli bir üst sınırdır. Bu sınır yalnızca karmaşık tür alanlarını () destekleyen en erken kullanılabilir (GA) API sürümünü kullanan dizin oluşturma işlemleri için zorlanır `2019-05-06` . Önceki önizleme API sürümleri (karmaşık tür alanlarını destekleyen) kullanan istemcileri bozmak için, bu önizleme API sürümlerini kullanan dizin oluşturma işlemleri için bu sınırı zorlayamıyoruz. Önizleme API 'SI sürümlerinin üretim senaryolarında kullanılmadığını ve müşterilerin en son GA API sürümüne taşınmasını önerdiğimiz unutulmamalıdır.

<a name="document-limits"></a>

## <a name="document-limits"></a>Belge limitleri 

2018 Ekim itibariyle, hiçbir bölgede faturalandırılabilir katmanda (temel, S1, S2, S3, S3 HD) oluşturulan tüm yeni hizmetler için hiçbir belge sayısı sınırı yoktur. Ekim 2018 ' den önce oluşturulan eski hizmetler belge sayısı sınırlarına tabi olmaya devam edebilir.

Hizmetinizin belge sınırlarına sahip olup olmadığını anlamak için [REST API hizmeti Istatistiklerini al](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics)' ı kullanın. Belge limitleri, yanıt olarak, `null` hiçbir sınır belirtilmediğini gösterecek şekilde yansıtılır.

> [!NOTE]
> Hizmet tarafından uygulanan bir belge sınırı olmasa da, temel, S1, S2 ve S3 Arama hizmetlerinde Dizin başına yaklaşık 24.000.000.000 belge sınırı vardır. S3 HD için parça sınırı, dizin başına 2.000.000.000 belgelerdir. Karmaşık bir koleksiyondaki her öğe, parça sınırları bakımından ayrı bir belge olarak sayılır.

### <a name="document-size-limits-per-api-call"></a>API çağrısı başına belge boyutu sınırları

Bir dizin API 'SI çağrılırken en büyük belge boyutu yaklaşık 16 megabayttır.

Belge boyutu Aslında Dizin API 'SI istek gövdesinin boyutu için bir sınır olur. Tek seferde Dizin API 'sine birden çok belge toplu işi geçirebileceğinizden, boyut sınırı, toplu işte kaç belge olduğuna göre değişir. Tek bir belge içeren bir toplu iş için en büyük belge boyutu 16 MB 'dir.

Belge boyutunu tahmin etmek için, yalnızca bir arama hizmeti tarafından tüketilen alanları göz önünde bulundurun. Kaynak belgelerdeki tüm ikili veriler veya resim verileri hesaplamalarınızda atlanmalıdır.  

## <a name="indexer-limits"></a>Dizin Oluşturucu sınırları

Hizmete bir bütün olarak denge ve kararlılık sağlamak için en fazla çalışma süresi mevcuttur, ancak daha büyük veri kümelerinde izin verilen en fazla dizin oluşturma süresi daha fazla olabilir. Bir dizin oluşturma işi izin verilen en uzun süre içinde tamamlanamaz, bir zamanlamaya göre çalıştırmayı deneyin. Zamanlayıcı, dizin oluşturma durumunu izler. Zamanlanan bir dizin oluşturma işi herhangi bir nedenle kesintiye uğrarsa, Dizin Oluşturucu bir sonraki zamanlanmış çalıştırmada en son kaldığınız yeri alabilir.


| Kaynak | Ücretsiz &nbsp; <sup>1</sup> | Temel &nbsp; <sup>2</sup>| S1 | S2 | S3 | S3 &nbsp; HD &nbsp; <sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| En fazla dizin oluşturucu |3 |5 veya 15|50 |200 |200 |YOK |10 |10 |
| En fazla veri kaynağı |3 |5 veya 15 |50 |200 |200 |YOK |10 |10 |
| En fazla becerileri <sup>4</sup> |3 |5 veya 15 |50 |200 |200 |YOK |10 |10 |
| Çağrı başına en fazla dizin oluşturma yükü |10.000 belge |Yalnızca en fazla belgelerle sınırlı |Yalnızca en fazla belgelerle sınırlı |Yalnızca en fazla belgelerle sınırlı |Yalnızca en fazla belgelerle sınırlı |YOK |Sınır yok |Sınır yok |
| En düşük zamanlama | 5 dakika |5 dakika |5 dakika |5 dakika |5 dakika |5 dakika |5 dakika | 5 dakika |
| Maksimum çalışma süresi <sup>5</sup> | 1-3 dakika |24 saat |24 saat |24 saat |24 saat |YOK  |24 saat |24 saat |
| Bilişsel arama becerileri için maksimum çalışma süresi veya görüntü analizi ile blob dizin oluşturma <sup>5</sup> | 3-10 dakika |2 saat |2 saat |2 saat |2 saat |YOK  |2 saat |2 saat |
| Blob Indexer: maksimum BLOB boyutu, MB |16 |16 |128 |256 |256 |YOK  |256 |256 |
| Blob Indexer: bir bloba ayıklanan maksimum içerik karakterleri |32.000 |64.000 |4 &nbsp; milyon |8 &nbsp; milyon |16 &nbsp; milyon |YOK |4 &nbsp; milyon |4 &nbsp; milyon |

<sup>1</sup> ücretsiz hizmet dizin oluşturucunun en yüksek yürütme süresi olan blob kaynakları için 3 dakika ve diğer tüm veri kaynakları için 1 dakikadır. Bilişsel hizmetler 'e çağıran AI dizin oluşturma için ücretsiz hizmetler, bir işlemin, enzenginleştirme ardışık düzeninde başarıyla geçen bir belge olarak tanımlandığı gün başına 20 ücretsiz işlem ile sınırlıdır.

<sup>2</sup> Aralık 2017 ' den önce oluşturulan temel hizmetler, Dizin oluşturucular, veri kaynakları ve becerileri üzerinde daha düşük sınırlara sahip (15 yerine 5).

<sup>3</sup> S3 HD Services Dizin Oluşturucu desteğini içermez.

Beceri <sup>başına en fazla</sup> 30 yetenek.

<sup>5</sup> AI zenginleştirme ve görüntü analizi yoğun bir şekilde yoğun ve orantısız miktarları kullanılabilir işlem gücü kullanıyor. Bu iş yükleri için çalışma süresi, kuyruktaki diğer işlerin daha fazla fırsatta çalışmasına izin verecek şekilde kısaltıldı.  

> [!NOTE]
> [Dizin sınırları](#index-limits)bölümünde belirtildiği gibi, Dizin oluşturucular, karmaşık türler () ile birlikte en son GA API sürümü ile başlayarak her belge için tüm karmaşık koleksiyonlarda 3000 öğelerin üst sınırını da uygular `2019-05-06` . Yani, Dizin oluşturucuyu önceki bir API sürümüyle oluşturduysanız bu sınıra tabi olmayacaktır. En yüksek uyumluluğu korumak için, önceki bir API sürümüyle oluşturulmuş ve daha sonra bir API sürümüyle veya daha sonraki bir sürümle güncelleştirilmiş bir Dizin Oluşturucu, `2019-05-06` sınırlara **dahil** edilmez. Müşteriler çok büyük karmaşık koleksiyonlara sahip olmanın olumsuz etkisinin farkında olmalıdır (daha önce belirtildiği gibi) ve en son GA API sürümüyle yeni Dizin oluşturucular oluşturmanız önerilir.

## <a name="synonym-limits"></a>Eş anlamlı sınırları

En fazla eş anlamlı eşleme sayısı katmana göre değişir. Her kural, bir genişletmenin eşdeğer bir terim olduğu en fazla 20 genişlemeye sahip olabilir. Örneğin, "Cat", "Kitty", "Feline" ve "Felis" (kediler için Genus) ilişkisi, 3 genişletmeleri olarak sayılır.

| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| En fazla eş anlamlı eşlemeler |3 |3|5 |10 |20 |20 | 10 | 10 |
| Eşleme başına en fazla kural sayısı |5000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>Saniyedeki sorgu sayısı (QPS)

QPS tahminleri her müşteri tarafından bağımsız olarak geliştirilmelidir. Dizin boyutu ve karmaşıklık, sorgu boyutu ve karmaşıklık ve trafik miktarı, QPS 'nin birinciline sahiptir. Bu faktörler bilinmiyorsa anlamlı tahminler sunmanın bir yolu yoktur.

Tahminler, adanmış kaynaklar üzerinde (temel ve standart Katmanlar) çalışan hizmetler üzerinde hesaplandığında daha öngörülebilir hale ayarlanır. Daha fazla parametre üzerinde denetiminiz olduğundan QPS 'yi daha yakından tahmin edebilirsiniz. Tahmine yaklaşıma ilişkin yönergeler için bkz. [Azure bilişsel arama performansı ve iyileştirmesi](search-performance-optimization.md).

Depolama için Iyileştirilmiş Katmanlar (L1 ve L2) için, standart katmanlardan daha düşük bir sorgu üretilen işi ve daha yüksek gecikme süresi beklemelisiniz. 

## <a name="data-limits-ai-enrichment"></a>Veri limitleri (AI zenginleştirme)

[Varlık tanıma](cognitive-search-skill-entity-recognition.md), [anahtar ifade ayıklama](cognitive-search-skill-keyphrases.md), yaklaşım [Analizi](cognitive-search-skill-sentiment.md), [dil algılama](cognitive-search-skill-language-detection.md)ve [kişisel bilgi algılama](cognitive-search-skill-pii-detection.md) için bir metin analizi kaynağına çağrılar yapan bir [AI zenginleştirme işlem hattı](cognitive-search-concept-intro.md) veri sınırlarına tabidir. Bir kaydın en büyük boyutu, tarafından ölçülen 50.000 karakter olmalıdır [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length) . Bu verileri yaklaşım Çözümleyicisi 'ne göndermeden önce bölmeniz gerekirse, [metin bölünmüş yeteneği](cognitive-search-skill-textsplit.md)kullanın.

## <a name="throttling-limits"></a>Azaltma sınırları

Sistem yoğun kapasiteye yaklaşırsa, arama sorgusu ve dizin oluşturma istekleri azaltılır. Kısıtlama, farklı API 'Ler için farklı davranır. Sorgu API 'Leri (arama/önerme/otomatik tamamlama) ve dizin oluşturma API 'Leri, hizmet üzerindeki yüke göre dinamik olarak azaldı. Dizin API 'Leri statik istek hızı sınırlarına sahiptir. 

Bir dizinle ilgili işlemler için statik hız isteği sınırları:

+ Dizinleri listeleme (/Indexes al): her arama birimi için saniyede 5
+ Dizini Al (GET/Indexes/MyIndex): her arama birimi için saniyede 10
+ Dizin Oluştur (POST/Indexes): arama birimi başına dakika başına 12
+ Dizin Oluştur veya güncelleştir (PUT/Indexes/MyIndex): arama birimi başına saniyede 6
+ Dizini Sil (DELETE/Indexes/MyIndex): arama birimi başına dakika başına 12 

## <a name="api-request-limits"></a>API isteği sınırları
* <sup>1</sup> . istek başına en fazla 16 MB
* En fazla 8 KB URL uzunluğu
* Dizin yükleme, birleştirme veya silme toplu iş başına en fazla 1000 belge
* $orderby yan tümcesindeki maksimum 32 alan
* En fazla arama terimi boyutu, UTF-8 ile kodlanmış metnin 32.766 bayttır (32 KB eksi 2 bayt)

<sup>1</sup> Azure bilişsel arama 'de, bir isteğin GÖVDESI 16 MB üst sınırına tabidir. Bu, başka bir şekilde (alan oluşturma ve kısıtlamalar hakkında daha fazla bilgi için bkz. [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) )

## <a name="api-response-limits"></a>API yanıt limitleri
* Arama sonuçları sayfası başına döndürülen en fazla 1000 belge
* Önerilen API isteği başına en fazla 100 öneri döndürüldü

## <a name="api-key-limits"></a>API anahtarı sınırları
API anahtarları hizmet kimlik doğrulaması için kullanılır. İki tür vardır. Yönetici anahtarları istek üstbilgisinde belirtilir ve hizmete tam okuma/yazma erişimi verir. Sorgu anahtarları salt okunurdur, URL 'de belirtilir ve genellikle istemci uygulamalarına dağıtılır.

* Hizmet başına en fazla 2 yönetici anahtarı
* Hizmet başına en fazla 50 sorgu anahtarı