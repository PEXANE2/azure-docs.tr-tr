---
title: REST API sürümlerini yükseltin
titleSuffix: Azure Cognitive Search
description: API sürümlerindeki farklılıkları gözden geçirin ve varolan kodu en yeni Azure Bilişsel Arama hizmeti REST API sürümüne geçirmek için hangi eylemlerin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: edb45eebc2c4eacc2f30d13988943f097a7190fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112158"
---
# <a name="upgrade-to-the-latest-azure-cognitive-search-service-rest-api-version"></a>En son Azure Bilişsel Arama hizmeti REST API sürümüne yükseltin

[Search REST API'nin](https://docs.microsoft.com/rest/api/searchservice/)önceki bir sürümünü kullanıyorsanız, bu makale, uygulamanızı en yeni genel olarak kullanılabilir API sürümü olan 2019-05-06'yı kullanmak üzere yükseltmenize yardımcı olur.

SÜRÜM 2019-05-06 REST API önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla geriye dönük uyumludur, bu nedenle kodunuzu değiştirmek, daha önce kullandığınız sürüme bağlı olarak yalnızca en az çaba yı gerektirmelidir. [Yükseltme adımları,](#UpgradeSteps) yeni özellikleri kullanmak için gereken kod değişikliklerini özetler.

> [!NOTE]
> Azure Bilişsel Arama hizmeti örneği, öncekiler de dahil olmak üzere bir dizi REST API sürümlerini destekler. Bu API sürümlerini kullanmaya devam edebilirsiniz, ancak yeni özelliklere erişebilmeniz için kodunuzu en yeni sürüme geçirmenizi öneririz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2019-05-06"></a>Sürümdeki yenilikler 2019-05-06
Sürüm 2019-05-06 REST API en yeni genel sürümüdür. Bu API sürümünde genel olarak kullanılabilir duruma geçiş yapmış özellikler şunlardır:

* [Otomatik tamamlama,](index-add-suggesters.md) kısmen belirtilen terim girdisini tamamlayan bir tür yazma özelliğidir.

* [Karmaşık türler,](search-howto-complex-data-types.md) arama dizinindeki yapılandırılmış nesne verileri için yerel destek sağlar.

* Azure Blob dizini ninizin bir parçası olan [JsonLines ayrıştırma modları,](search-howto-index-json-blobs.md)JSON varlığı başına yeni bir satırla ayrılmış bir arama belgesi oluşturur.

* [AI zenginleştirme,](cognitive-search-concept-intro.md) Bilişsel Hizmetler'in AI zenginleştirme motorlarından yararlanan indeksleme sağlar.

Çeşitli önizleme özelliği sürümleri, genel olarak kullanılabilen bu güncelleştirmeyle çakışıyor. Yeni önizleme özellikleri listesini incelemek için bkz: [Search REST api-version 2019-05-06-Preview](search-api-preview.md).

## <a name="breaking-changes"></a>Yeni değişiklikler

Aşağıdaki işlevselliği içeren varolan kod api-version=2019-05-06'da kırılır.

### <a name="indexer-for-azure-cosmos-db---datasource-is-now-type-cosmosdb"></a>Azure Cosmos DB için Dizinleyici - datasource artık "yazın": "cosmosdb"

[Bir Cosmos DB dizinleyici](search-howto-index-cosmosdb.md )kullanıyorsanız, `"type": "documentdb"` `"type": "cosmosdb"`'ye değiştirmeniz gerekir.

### <a name="indexer-execution-result-errors-no-longer-have-status"></a>Dizinleyici yürütme sonucu hataları artık durum var

Dizinleyici yürütme için hata yapısı `status` daha önce bir öğe vardı. Bu öğe, yararlı bilgiler sağlamadığı için kaldırıldı.

### <a name="indexer-data-source-api-no-longer-returns-connection-strings"></a>Dizinleyici veri kaynağı API artık bağlantı dizeleri döndürür

API sürümleri 2019-05-06 ve 2019-05-06-Önizleme itibaren, veri kaynağı API artık herhangi bir REST işlemi yanıt olarak bağlantı dizeleri döndürür. Önceki API sürümlerinde, POST kullanılarak oluşturulan veri kaynakları için, Azure Bilişsel Arama **201'i** döndürmüş ve ardından düz metindeki bağlantı dizesini içeren OData yanıtı göndermiştir.

### <a name="named-entity-recognition-cognitive-skill-is-now-discontinued"></a>Adlandırılmış Varlık Tanıma bilişsel beceri artık kesilir

Kodunuzda [Ad Varlık Tanıma](cognitive-search-skill-named-entity-recognition.md) becerisini çağırırsanız, arama başarısız olur. Değiştirme işlevi [Varlık Tanıma'dır.](cognitive-search-skill-entity-recognition.md) Beceri referansını başka bir değişiklik olmadan değiştirebilmelisin. API imzası her iki sürüm için de aynıdır. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
Önceki GA sürümüolan 2017-11-11 veya 2016-09-01'den yükseltme yapacaksanız, büyük olasılıkla sürüm numarasını değiştirmek dışında kodunuzda herhangi bir değişiklik yapmak zorunda kalmazsınız. Kodu değiştirmeniz gereken tek durumlar şunlardır:

* Tanımlanmamış özellikler API yanıtında döndürüldüğünde kodunuz başarısız olur. Varsayılan olarak uygulamanız anlamadığı özellikleri yok saymalıdır.

* Kodunuz API isteklerini devam ettir ve bunları yeni API sürümüne yeniden göndermeye çalışır. Örneğin, uygulamanız Arama API'sinden döndürülen devam belirteçlerini devam ettiremezse `@search.nextPageParameters` bu durum olabilir (daha fazla bilgi için [Arama API Başvurusu'nda](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)arayın).

Bu durumlardan biri sizin için geçerliyse, kodunuzu buna göre değiştirmeniz gerekebilir. Aksi takdirde, sürüm 2019-05-06'nın [yeni özelliklerini](#WhatsNew) kullanmaya başlamak istemediğiniz sürece hiçbir değişiklik gerekmez.

Önizleme API sürümünden yükseltme yapıyorsanız, yukarıdakiler de geçerlidir, ancak bazı önizleme özelliklerinin 2019-05-06 sürümünde kullanılmadığını da unutmayın:

* ["Daha böyle" sorguları](search-more-like-this.md)
* [CSV blob dizini oluşturma](search-howto-index-csv-blobs.md)
* [Cosmos DB dizinleyiciler için MongoDB API desteği](search-howto-index-cosmosdb.md)

Kodunuz bu özellikleri kullanıyorsa, bunları kullanımınızı kaldırmadan 2019-05-06 API sürümüne yükseltilemezsiniz.

> [!IMPORTANT]
> Önizleme API'leri test ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır.
> 

### <a name="upgrading-complex-types"></a>Karmaşık türleri yükseltme

Kodunuz eski önizleme API sürümleri 2017-11-11-Preview veya 2016-09-01-Preview ile karmaşık türleri kullanıyorsa, 2019-05-06 sürümünde bilmeniz gereken bazı yeni ve değiştirilmiş sınırlar vardır:

+ Alt alanların derinliği ve dizin başına karmaşık koleksiyon sayısı üzerindeki sınırlar düşürüldü. Önizleme api sürümlerini kullanarak bu sınırları aşan dizinler oluşturduysanız, 2019-05-06 API sürümünü kullanarak güncelleştirme veya yeniden oluşturma girişimleri başarısız olur. Bu sizin için geçerliyse, şemanızı yeni sınırlara sığacak şekilde yeniden tasarlamanız ve ardından diziniyeniden oluşturmanız gerekir.

+ Api-sürüm 2019-05-06'da belge başına karmaşık koleksiyonların öğelerinin sayısına ilişkin yeni bir sınır vardır. Önizleme api sürümlerini kullanarak bu sınırları aşan belgelerle dizinler oluşturduysanız, api-sürüm 2019-05-06 kullanarak bu verileri yeniden dizine alma girişimi başarısız olur. Bu sizin için geçerliyse, verilerinizi yeniden dizine almadan önce belge başına karmaşık toplama öğesi sayısını azaltmanız gerekir.

Daha fazla bilgi için [Azure Bilişsel Arama için Hizmet sınırlarına](search-limits-quotas-capacity.md)bakın.

### <a name="how-to-upgrade-an-old-complex-type-structure"></a>Eski karmaşık bir tür yapısı nasıl yükseltilir?

Kodunuz eski önizleme API sürümlerinden biriyle karmaşık türler kullanıyorsa, aşağıdaki gibi görünen bir dizin tanımı biçimi kullanıyor olabilirsiniz:

```json
{
  "name": "hotels",  
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.microsoft" },
    { "name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.microsoft" },
    { "name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true, "analyzer": "tagsAnalyzer" },
    { "name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true },
    { "name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address", "type": "Edm.ComplexType" },
    { "name": "Address/StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
    { "name": "Address/City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)" }, 
    { "name": "Rooms/Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene" },
    { "name": "Rooms/Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "Rooms/Type", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true },
    { "name": "Rooms/BedOptions", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/SleepsCount", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "Rooms/SmokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true },
    { "name": "Rooms/Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "facetable": true, "analyzer": "tagsAnalyzer" }
  ]
}  
```

Dizin alanlarını tanımlamak için daha yeni bir ağaç benzeri biçim API sürümü 2017-11-11-Preview'da tanıtıldı. Yeni biçimde, her karmaşık alanın alt alanlarının tanımlandığı bir alan koleksiyonu vardır. API sürümü2019-05-06'da, bu yeni biçim yalnızca kullanılır ve eski biçimi kullanarak bir dizin oluşturmaya veya güncelleştirmeye çalışmak başarısız olur. Eski biçimi kullanarak oluşturulan dizinler varsa, API sürümü 2019-05-06 kullanılarak yönetilebilmeleri için bunları yeni biçimde güncelleştirmek için API sürümünü 2017-11-11-Preview'u kullanmanız gerekir.

API sürüm 2017-11-11-Preview'u kullanarak aşağıdaki adımları kullanarak "düz" dizinleri yeni biçime güncelleyebilirsiniz:

1. Dizininizi almak için GET isteği gerçekleştirin. Zaten yeni biçimde ise, bitti.

2. Dizin"düz" biçiminden yeni biçime çevirin. Bu yazının yazıldığı sırada örnek kod bulunmadığından bunun için kod yazmanız gerekir.

3. Dizini yeni biçimde güncelleştirmek için PUT isteği gerçekleştirin. Güncelleştirme Dizini API'si tarafından izin verilmediğinden, alanların aranabilirliği/filtrelenebilirliği gibi dizinle ilgili diğer ayrıntıları değiştirmemediğinden emin olun.

> [!NOTE]
> Azure portalından eski "düz" biçimle oluşturulan dizinleri yönetmek mümkün değildir. Lütfen dizinlerinizi "düz" gösterimden en kısa zamanda "ağaç" temsiline yükseltin.

## <a name="next-steps"></a>Sonraki adımlar

Arama REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız, [StackOverflow](https://stackoverflow.com/) hakkında bizden yardım isteyin veya [desteğe başvurun.](https://azure.microsoft.com/support/community/?product=search)

> [!div class="nextstepaction"]
> [Arama hizmeti REST API Başvuru](https://docs.microsoft.com/rest/api/searchservice/)

