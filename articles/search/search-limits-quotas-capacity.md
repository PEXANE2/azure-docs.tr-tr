---
title: Katmanlar ve SKU 'lar için hizmet limitleri-Azure Search
description: Kapasite planlaması ve Azure Search istek ve yanıtlara yönelik maksimum limitlerde kullanılan hizmet limitleri.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: heidist
ms.openlocfilehash: 308eb90e7ae244442a603491044e90dc3b8d052a
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141145"
---
# <a name="service-limits-in-azure-search"></a>Azure Search hizmet limitleri
Depolama, iş yükleri ve dizin, belge ve diğer nesneler için maksimum sınırlar, **ücretsiz**, **temel**, **Standart**veya **depolama için iyileştirilmiş** fiyatlandırma katmanlarında [Azure Search](search-create-service-portal.md) sağlayıp sağlamadığınıza bağlıdır.

+ **Ücretsiz** , Azure aboneliğinizle birlikte gelen çok kiracılı bir paylaşılan hizmettir.

+ **Temel** , üretim iş yükleri için daha küçük bir ölçekte adanmış bilgi işlem kaynakları sağlar.

+ **Standart** , her düzeyde daha fazla depolama ve işleme kapasitesine sahip adanmış makinelerde çalışır. Standart dört düzeyde gelir: S1, S2, S3 ve S3 HD.

+ **Depolama Için iyileştirilmiş** , ayrılmış makinelerde daha fazla toplam depolama, depolama bant genişliği ve **Standart**bellek içeren çalışır. Depolama için Iyileştirilmiş iki düzeyde gelir: L1 ve L2

> [!NOTE]
> 1 Temmuz itibariyle, depolama için Iyileştirilmiş katmanı da dahil olmak üzere tüm katmanlar genel kullanıma sunulmuştur. Fiyatlandırma [ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfasında tüm fiyatlandırmalar bulunabilir.

  S3 yüksek yoğunluk (S3 HD), belirli iş yükleri için tasarlanmıştır: [çok kiracılı](search-modeling-multitenant-saas-applications.md) ve büyük miktarlarda küçük dizinler (dizin başına 1.000.000 belge, hizmet başına 3000 dizin). Bu katman, [Dizin Oluşturucu özelliğini](search-indexer-overview.md)sağlamıyor. S3 HD 'de veri alımı, verileri kaynaktan dizine göndermek için API çağrılarını kullanarak anında iletme yaklaşımına uymalıdır. 

> [!NOTE]
> Belirli bir katmanda bir hizmet sağlanır. Kapasite kazanmak için katman atlama, yeni bir hizmet sağlamayı içerir (yerinde yükseltme yoktur). Daha fazla bilgi için bkz. [BIR SKU veya katman seçme](search-sku-tier.md). Zaten sağladığınız bir hizmette kapasiteyi ayarlama hakkında daha fazla bilgi edinmek için bkz. [sorgu ve dizin oluşturma iş yükleri Için ölçek kaynak düzeyleri](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Depolama sınırları
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Dizin sınırları

| Resource | Boş | Temel&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| En fazla dizin |3 |5 veya 15 |50 |200 |200 |Bölüm başına 1000 veya hizmet başına 3000 |10 |10 |
| Dizin başına en fazla basit alan |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Dizin başına en fazla karmaşık koleksiyon alanı |40 |40 |40 |40 |40 |40 |40 |40 |
| Belge başına tüm karmaşık koleksiyonlardaki en fazla öğe |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Karmaşık alanların en büyük derinliği |10 |10 |10 |10 |10 |10 |10 |10 |
| Dizin başına maksimum [Öneri araçları](https://docs.microsoft.com/rest/api/searchservice/suggesters) |1\. |1\. |1\. |1\. |1\. |1\. |1\. |1\. |
| Dizin başına maksimum [Puanlama profili](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) |100 |100 |100 |100 |100 |100 |100 |100 |
| Profil başına en fazla işlev |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Aralık 2017 ' den önce oluşturulan temel hizmetler, dizinlerde alt limitlere (15 yerine 5) sahip olmalıdır. Temel katman, dizin başına 100 alan için alt sınıra sahip tek SKU.

<a name="document-limits"></a>

## <a name="document-limits"></a>Belge limitleri 

2018 Ekim itibariyle, herhangi bir bölgede faturalandırılabilir katmanda (temel, S1, S2, S3, S3 HD) oluşturulan hiçbir yeni hizmet için artık hiçbir belge sınırı yoktur. Çoğu bölgede, Kasım/Aralık 2017 ' den beri sınırsız sayıda belge sayısı vardı, belge sınırlarını sağlamaya devam eden beş bölge vardır. Bir arama hizmeti oluşturduğunuz zaman ve yere bağlı olarak, hala belge sınırlarına tabi olan bir hizmet çalıştırıyor olabilirsiniz.

Hizmetinizin belge sınırlarına sahip olup olmadığını anlamak için hizmetinizin Genel Bakış sayfasında kullanım kutucuğunu kontrol edin. Belge sayıları sınırsız veya katmana dayalı bir sınıra tabidir.

  ![Kullanım kutucuğu](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-previously-having-document-limits"></a>Daha önce belge sınırlarına sahip bölgeler

Portal bir belge sınırını gösteriyorsa, hizmetiniz en geç 2017 ' dan önce oluşturulmuştur veya Azure Search Hizmetleri barındırmak için düşük kapasite kümeleri kullanılarak bir veri merkezinde oluşturulmuştur:

+ Avustralya Doğu
+ Doğu Asya
+ Orta Hindistan
+ Japonya Batı
+ Batı Orta ABD

Belge sınırlarına tabi olan hizmetler için aşağıdaki en fazla sınır geçerlidir:

|  Boş | Temel | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10,000 |1&nbsp;milyon |Bölüm başına 15 milyon veya hizmet başına 180 milyon |Bölüm başına 60 milyon veya hizmet başına 720 milyon |Bölüm başına 120 milyon veya hizmet başına 1.4 milyar |Dizin başına 1 milyon veya bölüm başına 200 milyon |

Hizmetiniz sizi engelleyen sınırlar içeriyorsa, yeni bir hizmet oluşturun ve tüm içeriği bu hizmete yeniden yayımlayın. Hizmetinizi arka planda yeni donanıma sorunsuz bir şekilde yeniden sağlamaya yönelik bir mekanizma yoktur.

> [!Note] 
> En geç 2017 sonrasında oluşturulan S3 yüksek yoğunluklu hizmetler için, bölüm başına 200.000.000 belgesi kaldırılmıştır, ancak her dizin sınırı için 1.000.000 belgesi kalır.


### <a name="document-size-limits-per-api-call"></a>API çağrısı başına belge boyutu sınırları

Bir dizin API 'SI çağrılırken en büyük belge boyutu yaklaşık 16 megabayttır.

Belge boyutu Aslında Dizin API 'SI istek gövdesinin boyutu için bir sınır olur. Tek seferde Dizin API 'sine birden çok belge toplu işi geçirebileceğinizden, boyut sınırı, toplu işte kaç belge olduğuna göre değişir. Tek bir belge içeren bir toplu iş için en büyük belge boyutu 16 MB 'dir.

Belge boyutunu aşağı tutmak için, sorgulanabilir olmayan verileri istekten çıkarmayı unutmayın. Görüntüler ve diğer ikili veriler doğrudan sorgulanabilir değildir ve dizinde depolanmamalıdır. Sorgulanabilir olmayan verileri arama sonuçlarıyla bütünleştirmek için, kaynağa bir URL başvurusu depolayan aranabilir olmayan bir alan tanımlayın.

## <a name="indexer-limits"></a>Dizin Oluşturucu sınırları

Hizmete bir bütün olarak denge ve kararlılık sağlamak için en fazla çalışma süresi mevcuttur, ancak daha büyük veri kümelerinde izin verilen en fazla dizin oluşturma süresi daha fazla olabilir. Bir dizin oluşturma işi izin verilen en uzun süre içinde tamamlanamaz, bir zamanlamaya göre çalıştırmayı deneyin. Zamanlayıcı, dizin oluşturma durumunu izler. Zamanlanan bir dizin oluşturma işi herhangi bir nedenle kesintiye uğrarsa, Dizin Oluşturucu bir sonraki zamanlanmış çalıştırmada en son kaldığınız yeri alabilir.


| Resource | Ücretsiz&nbsp;<sup>1</sup> | Temel&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| En fazla dizin oluşturucu |3 |5 veya 15|50 |200 |200 |Yok |10 |10 |
| En fazla veri kaynağı |3 |5 veya 15 |50 |200 |200 |Yok |10 |10 |
| En fazla becerileri <sup>4</sup> |3 |5 veya 15 |50 |200 |200 |Yok |10 |10 |
| Çağrı başına en fazla dizin oluşturma yükü |10.000 belge |Yalnızca en fazla belgelerle sınırlı |Yalnızca en fazla belgelerle sınırlı |Yalnızca en fazla belgelerle sınırlı |Yalnızca en fazla belgelerle sınırlı |Yok |Sınır yok |Sınır yok |
| En düşük zamanlama | 5 dakika |5 dakika |5 dakika |5 dakika |5 dakika |5 dakika |5 dakika | 5 dakika |
| Maksimum çalışma süresi <sup>5</sup> | 1-3 dakika |24 saat |24 saat |24 saat |24 saat |Yok  |24 saat |24 saat |
| Bilişsel arama becerileri için maksimum çalışma süresi veya görüntü analizi ile blob dizin oluşturma <sup>5</sup> | 3-10 dakika |2 saat |2 saat |2 saat |2 saat |Yok  |2 saat |2 saat |
| Blob Indexer: maksimum BLOB boyutu, MB |16 |16 |128 |256 |256 |Yok  |256 |256 |
| Blob Indexer: bir bloba ayıklanan maksimum içerik karakterleri |32,000 |64,000 |4&nbsp;milyon |4&nbsp;milyon |4&nbsp;milyon |Yok |4&nbsp;milyon |4&nbsp;milyon |

<sup>1</sup> ücretsiz hizmet dizin oluşturucunun en yüksek yürütme süresi olan blob kaynakları için 3 dakika ve diğer tüm veri kaynakları için 1 dakikadır. Bilişsel hizmetler 'e çağıran AI dizin oluşturma için ücretsiz hizmetler, bir işlemin, enzenginleştirme ardışık düzeninde başarıyla geçen bir belge olarak tanımlandığı gün başına 20 ücretsiz işlem ile sınırlıdır.

<sup>2</sup> Aralık 2017 ' den önce oluşturulan temel hizmetler, Dizin oluşturucular, veri kaynakları ve becerileri üzerinde daha düşük sınırlara sahip (15 yerine 5).

<sup>3</sup> S3 HD Services Dizin Oluşturucu desteğini içermez.

<sup></sup> beceri başına en fazla 30 yetenek.

<sup>5</sup> bilişsel arama iş yükleri ve Azure Blob dizinlemesi içindeki görüntü analizi, normal metin dizinlemesi dışında daha kısa çalışma zamanına sahiptir Görüntü analizi ve doğal dil işleme, yoğun şekilde yoğundur ve orantısız miktarları kullanılabilir işlem gücü kullanıyor. Kuyruktaki diğer işlere bir fırsat sağlamak için çalışma süresi düşürüldü.  

## <a name="synonym-limits"></a>Eş anlamlı sınırları

İzin verilen en fazla eş anlamlı eşleme sayısı fiyatlandırma katmanına göre değişir. Her kural, bir genişlemenin eş bir terim olduğu en fazla 20 genişlemeye sahip olabilir. Örneğin, "Cat", "Kitty", "Feline" ve "Felis" (kediler için Genus) ilişkisi, 3 genişletmeleri olarak sayılır.

| Resource | Boş | Temel | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| En fazla eş anlamlı eşlemeler |3 |3|5 |10 |20 |20 | 10 | 10 |
| Eşleme başına en fazla kural sayısı |5000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>Saniyedeki sorgu sayısı (QPS)

QPS tahminleri her müşteri tarafından bağımsız olarak geliştirilmelidir. Dizin boyutu ve karmaşıklık, sorgu boyutu ve karmaşıklık ve trafik miktarı, QPS 'nin birinciline sahiptir. Bu faktörler bilinmiyorsa anlamlı tahminler sunmanın bir yolu yoktur.

Tahminler, adanmış kaynaklar üzerinde (temel ve standart Katmanlar) çalışan hizmetler üzerinde hesaplandığında daha öngörülebilir hale ayarlanır. Daha fazla parametre üzerinde denetiminiz olduğundan QPS 'yi daha yakından tahmin edebilirsiniz. Tahmine yaklaşıma ilişkin yönergeler için bkz. [Azure Search performans ve iyileştirme](search-performance-optimization.md).

Depolama için Iyileştirilmiş katmanlar için, standart katmanlardan daha düşük bir sorgu üretilen işi ve daha yüksek gecikme süresi beklemelisiniz.  Deneyimimiz sorgu performansını tahmin etme yöntemi, standart katmanlarla aynıdır.

## <a name="data-limits-cognitive-search"></a>Veri limitleri (bilişsel arama)

[Varlık tanıma](cognitive-search-skill-entity-recognition.md), [anahtar ifade ayıklama](cognitive-search-skill-keyphrases.md), yaklaşım [analizi](cognitive-search-skill-sentiment.md)ve [dil algılama](cognitive-search-skill-language-detection.md) için metin analizi kaynağa çağrı yapan bilişsel [Arama işlem hattı](cognitive-search-concept-intro.md) veri sınırlarına tabidir. Bir kaydın en büyük boyutu, tarafından [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçülen 50.000 karakter olmalıdır. Bu verileri yaklaşım Çözümleyicisi 'ne göndermeden önce bölmeniz gerekirse, [metin bölünmüş yeteneği](cognitive-search-skill-textsplit.md)kullanın.

## <a name="api-request-limits"></a>API isteği sınırları
* <sup>1</sup> . istek başına en fazla 16 MB
* En fazla 8 KB URL uzunluğu
* Dizin yükleme, birleştirme veya silme toplu iş başına en fazla 1000 belge
* $orderby yan tümcesindeki maksimum 32 alan
* En fazla arama terimi boyutu, UTF-8 ile kodlanmış metnin 32.766 bayttır (32 KB eksi 2 bayt)

<sup>1</sup> Azure Search, bir isteğin GÖVDESI 16 MB üst sınırına tabidir ve başka bir şekilde (örneğin, daha fazla için [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) ), tek tek alanların veya koleksiyonların içerikleri üzerinde pratik bir sınır sağlar (daha fazla bilgi için bkz. alan kompozisyonu ve kısıtlamaları hakkında bilgi).

## <a name="api-response-limits"></a>API yanıt limitleri
* Arama sonuçları sayfası başına döndürülen en fazla 1000 belge
* Önerilen API isteği başına en fazla 100 öneri döndürüldü

## <a name="api-key-limits"></a>API anahtarı sınırları
API anahtarları hizmet kimlik doğrulaması için kullanılır. İki tür vardır. Yönetici anahtarları istek üstbilgisinde belirtilir ve hizmete tam okuma/yazma erişimi verir. Sorgu anahtarları salt okunurdur, URL 'de belirtilir ve genellikle istemci uygulamalarına dağıtılır.

* Hizmet başına en fazla 2 yönetici anahtarı
* Hizmet başına en fazla 50 sorgu anahtarı
