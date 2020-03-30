---
title: Katmanlar ve skuslar için hizmet limitleri
titleSuffix: Azure Cognitive Search
description: Kapasite planlaması için kullanılan hizmet sınırları ve Azure Bilişsel Arama istekleri ve yanıtları için maksimum sınırlar.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 6ee339cb709a5d825b39b4accf294761c99ee41a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282984"
---
# <a name="service-limits-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki hizmet sınırları

Depolama, iş yükleri ve dizin lerin ve diğer nesnelerin miktarlarına ilişkin maksimum sınırlar, Azure Bilişsel Arama'yı **Ücretsiz,** **Temel,** **Standart**veya **Depolama Optimize edilmiş** fiyatlandırma katmanlarında [sağlayıp sağlamadığınıza](search-create-service-portal.md) bağlıdır.

+ **Ücretsiz,** Azure aboneliğinizle birlikte gelen çok kiracılı paylaşılan bir hizmettir. Dizin oluşturma ve sorgu istekleri, diğer kiracılar tarafından kullanılan yinelemeler ve bölümler üzerinde yürütülür.

+ **Basic,** daha küçük ölçekte üretim iş yükleri için özel bilgi işlem kaynakları sağlar, ancak bazı ağ altyapılarını diğer kiracılarla paylaşır.

+ **Standart,** her düzeyde daha fazla depolama ve işleme kapasitesine sahip özel makinelerde çalışır. Standart dört düzeyde gelir: S1, S2, S3, ve S3 HD.

+ **Depolama Optimize edilmiş,** **Standart'tan**daha fazla toplam depolama alanı, depolama bant genişliği ve belleğe sahip özel makinelerde çalışır. Depolama Optimize iki düzeyde gelir: L1 ve L2

> [!NOTE]
> 1 Temmuz itibariyle, Depolama Optimize Edilen Katman da dahil olmak üzere tüm katmanlar genel olarak kullanılabilir. Tüm [fiyatlandırmaları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfasında bulabilirsiniz.

  S3 Yüksek Yoğunluklu (S3 HD) belirli iş yükleri için tasarlanmıştır: [çoklu kira](search-modeling-multitenant-saas-applications.md) ve büyük miktarlarda küçük dizinler (dizin başına bir milyon belge, hizmet başına üç bin dizin). Bu katman [dizinleyici özelliğini](search-indexer-overview.md)sağlamaz. S3 HD'de, verileri kaynaktan dizine itmek için API çağrılarını kullanarak veri alım, itme yaklaşımından yararlanmalıdır. 

> [!NOTE]
> Bir hizmet belirli bir katmanda verilir. Kapasite kazanmak için atlama katmanları yeni bir hizmet sağlamaiçerir (yerinde yükseltme yoktur). Daha fazla bilgi için [bkz.](search-sku-tier.md) Önceden vermiş olduğunuz bir hizmetin kapasitesini ayarlama hakkında daha fazla bilgi edinmek [için sorgu ve dizin oluşturma iş yükleri için Ölçek kaynak düzeylerine](search-capacity-planning.md)bakın.
>

## <a name="subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Depolama limitleri
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Dizin sınırları

| Kaynak | Ücretsiz | Temel&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | ---- | ------------------- | --- | --- | --- | --- | --- | --- |
| En fazla dizin |3 |5 veya 15 |50 |200 |200 |Bölüm başına 1000 veya hizmet başına 3000 |10 |10 |
| Dizin başına maksimum basit alanlar |1000 |100 |1000 |1000 |1000 |1000 |1000 |1000 |
| Dizin başına maksimum karmaşık toplama alanları |40 |40 |40 |40 |40 |40 |40 |40 |
| Belge&nbsp;başına tüm karmaşık koleksiyonlarda maksimum öğeler<sup>2</sup> |3000 |3000 |3000 |3000 |3000 |3000 |3000 |3000 |
| Karmaşık alanların maksimum derinliği |10 |10 |10 |10 |10 |10 |10 |10 |
| Dizin başına maksimum [önerici](https://docs.microsoft.com/rest/api/searchservice/suggesters) |1 |1 |1 |1 |1 |1 |1 |1 |
| Dizin başına maksimum [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) |100 |100 |100 |100 |100 |100 |100 |100 |
| Profil başına maksimum fonksiyonlar |8 |8 |8 |8 |8 |8 |8 |8 |

<sup>1</sup> Aralık 2017'den önce oluşturulan temel hizmetlerin dizinlerde daha düşük sınırlar (15 yerine 5) vardır. Temel katman, dizin başına 100 alan dan daha düşük bir sınırı olan tek SKU'dur.

<sup>2</sup> Belge başına karmaşık koleksiyonlarda çok sayıda eleman olması şu anda yüksek depolama kullanımına neden olur. Bu bilinen bir sorundur. Bu arada, 3000 sınırı tüm hizmet katmanları için güvenli bir üst sınırdır. Bu sınır yalnızca karmaşık tür alanlarını ()`2019-05-06`ileriye doğru destekleyen en erken genel kullanılabilir (GA) API sürümünü kullanan dizin oluşturma işlemleri için uygulanır. Önceki önizleme API sürümlerini (karmaşık tür alanlarını destekleyen) kullanan istemcileri kırmamak için, bu önizleme API sürümlerini kullanan dizin oluşturma işlemleri için bu sınırı uygulamayacağız. Önizleme API sürümlerinin üretim senaryoları için kullanılmadığını ve müşterilerin en son GA API sürümüne geçmelerini şiddetle tavsiye ettiğimizi unutmayın.

<a name="document-limits"></a>

## <a name="document-limits"></a>Belge sınırları 

Ekim 2018 itibariyle, herhangi bir bölgede faturalandırılabilir katmanda (Basic, S1, S2, S3, S3 HD) oluşturulan yeni hizmetler için artık belge sınırı bulunmamaktadır. Çoğu bölge Kasım/Aralık 2017'den bu yana sınırsız belge sayımı na sahip ken, bu tarihten sonra belge limitleri uygulamaya devam eden birkaç bölge vardı. Bir arama hizmetini ne zaman ve nerede oluşturduğunuza bağlı olarak, belge sınırlarına hala tabi olan bir hizmeti çalıştırıyor olabilirsiniz.

Hizmetinizin belge sınırları olup olmadığını belirlemek için [GET Service Statistics REST API'yi](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics)kullanın. Belge sınırları yanıta yansıtılır `null` ve herhangi bir sınır belirtinilir.

> [!NOTE]
> SKU'ya özgü belge sınırları olmasa da, her dizin hizmetin kararlılığını sağlamak için hala maksimum güvenli sınıra tabidir. Bu sınır Lucene'den geliyor. Her Azure Bilişsel Arama belgesi dahili olarak bir veya daha fazla Lucene belgesi olarak dizine eklenir. Arama belgesi başına Lucene belgelerinin sayısı, karmaşık toplama alanlarındaki toplam öğe sayısına bağlıdır. Her öğe ayrı bir Lucene belge olarak dizine. Örneğin, karmaşık bir toplama alanında 3 öğeiçeren bir belge, 4 Lucene belgesi olarak dizine alınacaktır - belgenin kendisi için 1 ve öğeler için 3. Lucene belgelerinin maksimum sayısı indeks başına kabaca 25 milyardır.

### <a name="regions-previously-having-document-limits"></a>Daha önce belge sınırları olan bölgeler

Portal bir belge sınırı gösteriyorsa, hizmetiniz 2017 sonundan önce oluşturuldu veya Azure Bilişsel Arama hizmetlerini barındırmak için daha düşük kapasiteli kümeler kullanılarak bir veri merkezinde oluşturuldu:

+ Doğu Avustralya
+ Doğu Asya
+ Orta Hindistan
+ Batı Japonya
+ Orta Batı ABD

Belge sınırlarına tabi hizmetler için aşağıdaki maksimum sınırlar geçerlidir:

|  Ücretsiz | Temel | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10,000 |1&nbsp;milyon |Bölüm başına 15 milyon veya hizmet başına 180 milyon |Bölüm başına 60 milyon veya hizmet başına 720 milyon |Bölüm başına 120 milyon veya hizmet başına 1.4 milyar |Dizin başına 1 milyon veya bölüm başına 200 milyon |

Hizmetinizin sizi engelleyen sınırları varsa, yeni bir hizmet oluşturun ve ardından bu hizmete ilişkin tüm içeriği yeniden yayımlayın. Hizmetinizi arka planda yeni donanımlara sorunsuz bir şekilde yeniden sağlama mekanizması yoktur.

> [!Note] 
> 2017'nin sonundan sonra oluşturulan S3 Yüksek Yoğunluklu hizmetler için, bölüm başına 200 milyon belge kaldırıldı, ancak dizin başına 1 milyon belge sınırı kaldı.


### <a name="document-size-limits-per-api-call"></a>API çağrısı başına belge boyutu sınırları

Dizin API'sini ararken en yüksek belge boyutu yaklaşık 16 megabayttır.

Belge boyutu aslında Dizin API istek gövdesinin boyutunda bir sınırdır. Birden çok belgeyi aynı anda Index API'ye geçirebildiğinizden, boyut sınırı gerçekçi olarak toplu iş te kaç belge olduğuna bağlıdır. Tek bir belgeye sahip bir toplu iş için, en yüksek belge boyutu 16 MB JSON'dur.

Belge boyutunu düşük tutmak için, sorgulanabilir olmayan verileri istekten hariç tutmayı unutmayın. Görüntüler ve diğer ikili veriler doğrudan sorgulanabilir değildir ve dizinde depolanmamalıdır. Sorgulanamayan verileri arama sonuçlarına entegre etmek için, kaynağa URL başvurusu depolayan aranamayan bir alan tanımlayın.

## <a name="indexer-limits"></a>Dizinleyici sınırları

Hizmete bir bütün olarak denge ve kararlılık sağlamak için maksimum çalışma süreleri vardır, ancak daha büyük veri kümelerinin maksimum izin verdiğinden daha fazla dizin oluşturma süresi gerekebilir. Dizin oluşturma işi izin verilen maksimum süre içinde tamamlanamıyorsa, zamanlamada çalıştırmayı deneyin. Zamanlayıcı dizin oluşturma durumunu izler. Zamanlanmış bir dizin oluşturma işi herhangi bir nedenle kesilirse, dizinleyici bir sonraki zamanlanan çalıştırmada kaldığı yeri alabilir.


| Kaynak | Ücretsiz&nbsp;<sup>1</sup> | Temel&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;&nbsp;HD<sup>3</sup>|L1 |L2 |
| -------- | ----------------- | ----------------- | --- | --- | --- | --- | --- | --- |
| En fazla dizin oluşturucu |3 |5 veya 15|50 |200 |200 |Yok |10 |10 |
| En fazla veri kaynağı |3 |5 veya 15 |50 |200 |200 |Yok |10 |10 |
| Maksimum beceri setleri <sup>4</sup> |3 |5 veya 15 |50 |200 |200 |Yok |10 |10 |
| Çağırma başına maksimum dizin leme yükü |10.000 belge |Yalnızca maksimum belgelerle sınırlıdır |Yalnızca maksimum belgelerle sınırlıdır |Yalnızca maksimum belgelerle sınırlıdır |Yalnızca maksimum belgelerle sınırlıdır |Yok |Sınır yok |Sınır yok |
| Minimum zamanlama | 5 dakika |5 dakika |5 dakika |5 dakika |5 dakika |5 dakika |5 dakika | 5 dakika |
| Maksimum çalışma süresi <sup>5</sup> | 1-3 dakika |24 saat |24 saat |24 saat |24 saat |Yok  |24 saat |24 saat |
| Görüntü analizi ile bilişsel arama beceri setleri veya blob indeksleme için maksimum çalışma süresi <sup>5</sup> | 3-10 dakika |2 saat |2 saat |2 saat |2 saat |Yok  |2 saat |2 saat |
| Blob indeksleyici: maksimum blob boyutu, MB |16 |16 |128 |256 |256 |Yok  |256 |256 |
| Blob dizinleyici: bir blob çıkarılan içerik maksimum karakter |32,000 |64,000 |4&nbsp;milyon |8&nbsp;milyon |16&nbsp;milyon |Yok |4&nbsp;milyon |4&nbsp;milyon |

<sup>1</sup> Ücretsiz hizmetler blob kaynakları için 3 dakika ve diğer tüm veri kaynakları için 1 dakika dizinleyici maksimum yürütme süresine sahiptir. Bilişsel Hizmetlere çağrı yapan AI dizini için, ücretsiz hizmetler günde 20 ücretsiz işlemle sınırlıdır ve burada işlem zenginleştirme ardışık lığından başarıyla geçen bir belge olarak tanımlanır.

<sup>2</sup> Aralık 2017'den önce oluşturulan temel hizmetlerin dizinleyiciler, veri kaynakları ve beceri kümelerinde daha düşük sınırlar (15 yerine 5) vardır.

<sup>3</sup> S3 HD hizmetleri dizinleyici desteği içermez.

<sup>4</sup> Beceri başına en fazla 30 beceri.

<sup>5</sup> Azure blob dizinoluşturmada bilişsel arama iş yükleri ve görüntü çözümlemesi, normal metin dizini yapmaktan daha kısa çalışma sürelerine sahiptir. Görüntü analizi ve doğal dil işleme hesaplama açısından yoğundur ve kullanılabilir işlem gücü orantısız miktarda tüketir. Çalışma süresi, kuyruktaki diğer işlere çalışma fırsatı vermek için azaltıldı.  

> [!NOTE]
> [Dizin sınırlarında](#index-limits)belirtildiği gibi, dizinleyiciler, karmaşık türleri`2019-05-06`destekleyen en son GA API sürümünden başlayarak belge başına tüm karmaşık koleksiyonlarda 3000 öğenin üst sınırını da uygular. Bu, dizin oluşturucunuzu önceki bir API sürümüyle oluşturduysanız, bu sınıra tabi olmayacağınız anlamına gelir. Maksimum uyumluluğu korumak için, önceki bir API sürümüyle oluşturulan ve daha `2019-05-06` sonra bir API sürümü veya daha sonra güncelleştirilen bir dizin oluşturucu yine de sınırların **dışında tutulur.** Müşteriler çok büyük karmaşık koleksiyonlara sahip olmanın olumsuz etkisinin farkında olmalıdır (daha önce belirtildiği gibi) ve en son GA API sürümüne sahip yeni dizin oluşturup oluşturmanızı şiddetle tavsiye ediyoruz.

## <a name="synonym-limits"></a>Eş anlamlı lar

İzin verilen en fazla eşanlamlı eşanlamlı eşlem sayısı fiyatlandırma katmanına göre değişir. Her kuralın, genişletmenin eşdeğer bir terim olduğu en fazla 20 genişletmesi olabilir. Örneğin, verilen "kedi", "kedi", "kedi" ve "felis" (kediler için cins) ile ilişki 3 genişlemeleri olarak sayılır.

| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3-HD |L1 | L2 |
| -------- | -----|------ |----|----|----|-------|---|----|
| Maksimum eşanlamlı haritalar |3 |3|5 |10 |20 |20 | 10 | 10 |
| Harita başına maksimum kural sayısı |5000 |20000|20000 |20000 |20000 |20000 | 20000 | 20000  |

## <a name="queries-per-second-qps"></a>Saniyede sorgular (QPS)

QPS tahminleri her müşteri tarafından bağımsız olarak geliştirilmelidir. Dizin boyutu ve karmaşıklığı, sorgu boyutu ve karmaşıklığı ve trafik miktarı QPS'nin birincil belirleyicileridir. Bu tür etkenler bilinmiyorsa anlamlı tahminler sunmanın bir yolu yoktur.

Tahminler, özel kaynaklarla çalışan hizmetler (Temel ve Standart katmanlar) üzerinden hesaplandığında daha öngörülebilirdir. Parametrelerin daha fazla üzerinde denetime sahip olduğundan QPS'yi daha yakından tahmin edebilirsiniz. Tahmine nasıl yaklaşılabilenlere ilişkin kılavuz için Azure [Bilişsel Arama performansı ve optimizasyonu'na](search-performance-optimization.md)bakın.

Depolama Optimize edilen katmanlar için, Standart katmanlardan daha düşük bir sorgu iş tüvemi ve daha yüksek gecikme süreleri beklemeniz gerekir.  Karşılaşacağınız sorgu performansını tahmin etme metodolojisi Standart katmanlarla aynıdır.

## <a name="data-limits-ai-enrichment"></a>Veri limitleri (AI zenginleştirme)

[Varlık tanıma,](cognitive-search-skill-entity-recognition.md) [anahtar ifade çıkarma,](cognitive-search-skill-keyphrases.md) [duygu analizi,](cognitive-search-skill-sentiment.md) [dil algılama](cognitive-search-skill-language-detection.md)ve kişisel bilgi işlem algılama için Metin Analizi kaynağına çağrılar yapan bir [AI](cognitive-search-skill-pii-detection.md) [zenginleştirme ardışık hattı](cognitive-search-concept-intro.md) veri sınırlarına tabidir. Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Verilerinizi duygu çözümleyicisine göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanın.

## <a name="throttling-limits"></a>Azaltma limitleri

Sistem en yüksek kapasiteye yaklaştıkça arama sorgusu ve dizin oluşturma istekleri daraltılır. Azaltma, farklı API'ler için farklı şekilde davranılır. Sorgu API'leri (Arama/Öner/Otomatik Tamamlama) ve ekizin EKLEME API'leri hizmetteki yüke bağlı olarak dinamik olarak azaltma. Dizin API'lerinin statik istek oranı sınırları vardır. 

Bir dizinle ilgili işlemler için statik hız isteği sınırları:

+ Liste Dizinleri (GET /indexes): Arama birimi başına saniyede 5
+ Dizini al (GET /indexes/myindex): Arama birimi başına saniyede 10
+ Oluşturma Dizini (POST /indexes): Arama birimi başına dakikada 12
+ Create or Update Index (PUT /indexes/myindex): Arama birimi başına saniyede 6
+ Delete Index (DELETE /indexes/myindex): Arama birimi başına dakikada 12 

## <a name="api-request-limits"></a>API istek sınırları
* İstek başına maksimum 16 MB <sup>1</sup>
* Maksimum 8 KB URL uzunluğu
* Dizin yüklemeleri, birleştirmeleri veya silme toplu başına maksimum 1000 belge
* $orderby fıkrasında en fazla 32 alan
* Maksimum arama terimi boyutu UTF-8 kodlanmış metin 32.766 bayt (32 KB eksi 2 bayt)

<sup>1</sup> Azure Bilişsel Arama'da, bir isteğin gövdesi 16 MB'lık bir üst sınıra tabidir ve teorik sınırlamalarla kısıtlanmayan tek tek alanların veya koleksiyonların içeriğine pratik bir sınır getirmektedir (bkz. alan kompozisyonu ve kısıtlamalar hakkında daha fazla bilgi için [Desteklenen veri türleri).](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)

## <a name="api-response-limits"></a>API yanıt limitleri
* Arama sonuçlarının sayfası başına döndürülen maksimum 1000 belge
* ApI Öner isteği başına döndürülen maksimum 100 öneri

## <a name="api-key-limits"></a>API anahtar sınırları
API anahtarları hizmet kimlik doğrulaması için kullanılır. İki tür vardır. Yönetici anahtarları istek üstbilgisinde belirtilir ve hizmete tam okuma-yazma erişimi sağlar. Sorgu anahtarları salt okunur, URL'de belirtilir ve genellikle istemci uygulamalarına dağıtılır.

* Hizmet başına en fazla 2 yönetici anahtarı
* Hizmet başına en fazla 50 sorgu anahtarı