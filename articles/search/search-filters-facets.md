---
title: Uygulamalarda arama navigasyonu için facet filtreleri
titleSuffix: Azure Cognitive Search
description: Microsoft Azure'da barındırılan bir bulut arama hizmeti olan Azure Bilişsel Arama'daki sorgularda arama sonuçlarını azaltmak için kullanıcı güvenlik kimliğine, coğrafi konuma veya sayısal değerlere göre filtreleme ölçütlerine filtre uygulayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 082575a67ea43d62f322e177cff087e5bd572c27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792888"
---
# <a name="how-to-build-a-facet-filter-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da bir fason filtresi oluşturma 

Faceted navigasyon, uygulamanızın belge gruplarına (örneğin kategoriler veya markalar) arama yı kapsamlandırmak için kullanıcı arabirimi denetimleri sunduğu bir arama uygulamasında sorgu sonuçlarında kendi kendini yönlendirmiş filtreleme için kullanılır ve Azure Bilişsel Arama veri yapısını sağlar deneyimi geri getirmek için. Bu makalede, sağlamak istediğiniz arama deneyimini destekleyen yönlü bir gezinti yapısı oluşturmak için temel adımları hızla gözden geçirin. 

> [!div class="checklist"]
> * Filtreleme ve yüzleme için alanları seçin
> * Alandaki öznitelikleri ayarlama
> * Dizin oluşturma ve verileri yükleme
> * Sorguya fason filtreler ekleme
> * Sonuçları işleme

Fasonlar dinamiktir ve sorguda döndürülür. Arama yanıtları, sonuçlarda gezinmek için kullanılan fason kategorileri de beraberinde getirir. Eğer yönlerini aşina değilseniz, aşağıdaki örnek bir fason navigasyon yapısının bir örneğidir.

  ![](./media/search-filters-facets/facet-nav.png)

Yeni yönlü navigasyon ve daha fazla ayrıntı istiyorum? Azure [Bilişsel Arama'da yönlü gezinmeyi nasıl uygulayacağınızda](search-faceted-navigation.md)görün.

## <a name="choose-fields"></a>Alanları seçin

Fasonlar, tek değer alanları ve koleksiyonlar üzerinden hesaplanabilir. Yönlü gezinmede en iyi çalışan alanlar düşük ciddiyetliliğe sahiptir: arama şirketinizdeki belgelerde yinelenen az sayıda farklı değer (örneğin, renklerin, ülkelerin/bölgelerin veya marka adlarının listesi). 

Özniteliği 'ne ayarlayarak `facetable` dizin oluştururken faceting alan bazında `true`etkinleştirilir. Arama uygulamanızın son `filterable` kullanıcının `true` seçtiği yönlerini temel alan bu alanlara filtre uygulayabilmesi için genellikle bu tür alanlara özniteliği de ayarlamanız gerekir. 

REST API'yi kullanarak bir dizin oluştururken, yönlü gezinmede kullanılabilecek `facetable` herhangi bir alan [türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) varsayılan olarak işaretlenir:

+ `Edm.String`
+ `Edm.DateTimeOffset`
+ `Edm.Boolean`
+ Sayısal alan tipleri: `Edm.Int32` `Edm.Int64`, ,`Edm.Double`
+ Yukarıdaki türlerin koleksiyonları (örneğin, `Collection(Edm.String)` veya `Collection(Edm.Double)`)

Yüzyüze `Edm.GeographyPoint` gezinmede kullanamazsınız veya `Collection(Edm.GeographyPoint)` alanlar. Fatlar en iyi düşük kardinallik alanlarında çalışır. Coğrafi koordinatların çözünürlüğü nedeniyle, belirli bir veri kümesinde herhangi iki koordinat kümesinin eşit olması nadirdir. Bu nedenle, fasonlar coğrafi koordinatlar için desteklenmez. Konuma göre bir şehir veya bölge alanı gerekir.

## <a name="set-attributes"></a>Öznitelikleri ayarlama

Bir alanın nasıl kullanıldığını denetleyen dizin öznitelikleri, dizindeki tek tek alan tanımlarına eklenir. Aşağıdaki örnekte, düşük kardinallik ile alanlar, yüz yüze `category` için yararlı, oluşur: (otel, motel, hostel), `tags`ve `rating`. Bu alanlar `filterable` açıklayıcı `facetable` amaçlar için aşağıdaki örnekte açıkça ayarlanmış ve öznitelikleri vardır. 

> [!Tip]
> Performans ve depolama optimizasyonu için en iyi uygulama olarak, hiçbir zaman bir fason olarak kullanılmaması gereken alanlar için yüzleme kapatın. Özellikle, kimlik veya ürün adı gibi benzersiz değerlere sahip dize alanları, bunların yüzlü gezinmede yanlışlıkla (ve etkisiz) kullanımını önlemek için `"facetable": false` ayarlanmalıdır.


```json
{
  "name": "hotels",  
  "fields": [
    { "name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false },
    { "name": "baseRate", "type": "Edm.Double" },
    { "name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
    { "name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "hotelName", "type": "Edm.String", "facetable": false },
    { "name": "category", "type": "Edm.String", "filterable": true, "facetable": true },
    { "name": "tags", "type": "Collection(Edm.String)", "filterable": true, "facetable": true },
    { "name": "parkingIncluded", "type": "Edm.Boolean",  "filterable": true, "facetable": true, "sortable": false },
    { "name": "smokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true, "sortable": false },
    { "name": "lastRenovationDate", "type": "Edm.DateTimeOffset" },
    { "name": "rating", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "location", "type": "Edm.GeographyPoint" }
  ]
}
```

> [!Note]
> Bu dizin [tanımı, REST API kullanılarak Bir Azure Bilişsel Arama dizini oluşturma'dan](https://docs.microsoft.com/azure/search/search-create-index-rest-api)kopyalanır. Alan tanımlarında yüzeysel farklılıklar dışında aynıdır. Ve `filterable` `facetable` öznitelikleri açıkça , `category` `tags`, `parkingIncluded` `smokingAllowed`, `rating` , ve alanlara eklenir. Uygulamada `filterable` ve `facetable` REST API kullanırken bu alanlarda varsayılan olarak etkin olacaktır. .NET SDK kullanılırken, bu özniteliklerin açıkça etkinleştirilmesi gerekir.

## <a name="build-and-load-an-index"></a>Bir dizin oluşturma ve yükleme

Bir ara (ve belki de bariz) adım oluşturmak ve bir sorgu formüle etmeden önce [dizin doldurmak](https://docs.microsoft.com/azure/search/search-get-started-dotnet#1---create-index) olmasıdır. Biz bütünlük için burada bu adım söz. Dizin kullanılabilir olup olmadığını belirlemek için bir yolu [portalda](https://portal.azure.com)dizinler listesini kontrol etmektir.

## <a name="add-facet-filters-to-a-query"></a>Sorguya fason filtreler ekleme

Uygulama kodunda, bir isteği formüle etmek için kullanılan arama ifadeleri, fatektler, filtreler, puanlama profilleri dahil olmak üzere geçerli bir sorgunun tüm bölümlerini belirten bir sorgu oluşturun. Aşağıdaki örnek, konaklama türüne, derecelendirmeye ve diğer olanaklara göre farklı bir gezinme oluşturan bir istek oluşturur.

```csharp
var sp = new SearchParameters()
{
    ...
    // Add facets
    Facets = new[] { "category", "rating", "parkingIncluded", "smokingAllowed" }.ToList()
};
```

### <a name="return-filtered-results-on-click-events"></a>Tıklama olaylarında filtreuygulanmış sonuçları döndürme

Son kullanıcı bir fason değeri tıklattığında, tıklama olayının işleyicisi kullanıcının amacını gerçekleştirmek için bir filtre ifadesi kullanmalıdır. Bir `category` fason göz önüne alındığında, kategori "motel" `$filter` tıklayarak bu tür konaklama seçer bir ifade ile uygulanır. Bir kullanıcı yalnızca motellerin gösterilmesi gerektiğini belirtmek için "motel"i tıklattığında, `$filter=category eq 'motel'`uygulamanın gönderdiği bir sonraki sorguyu içerir.

Kullanıcı kategori fatondan bir değer seçerse, aşağıdaki kod snippet'i filtreye kategori ekler.

```csharp
if (!String.IsNullOrEmpty(categoryFacet))
    filter = $"category eq '{categoryFacet}'";
```

Kullanıcı `tags`bir koleksiyon alanı için bir fason değerine tıklarsa , örneğin "havuz" değeri, uygulamanız aşağıdaki filtre sözdizimini kullanmalıdır:`$filter=tags/any(t: t eq 'pool')`

## <a name="tips-and-workarounds"></a>İpuçları ve geçici çözüm

### <a name="initialize-a-page-with-facets-in-place"></a>Yerinde yönleri olan bir sayfayı başlatma

Yerinde yönleri olan bir sayfayı başlatmak istiyorsanız, sayfanın başlangıç bölümünün bir parçası olarak sayfanın ilk yönlü yapısıyla tohumlama için bir sorgu gönderebilirsiniz.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Filtre uygulanmış sonuçlardan oluşan bir yönlü gezinme yapısını eşit bir şekilde koruyun

Azure Bilişsel Arama'da farklı gezinmenin zorluklarından biri, yalnızca geçerli sonuçlar için farklı yönlerin mevcut olmasıdır. Uygulamada, kullanıcının arama içeriği aracılığıyla alternatif yolları keşfetmek için adımları izleyerek ters yönde gezinmesi için statik bir yön kümesini korumak yaygındır. 

Bu yaygın bir kullanım örneği olmasına rağmen, şu anda facet navigasyon yapısı out-of-the-box sağlayan bir şey değil. Statik yönler isteyen geliştiriciler genellikle iki filtreuygulanmış sorgu ları vererek sınırlama etrafında çalışır: biri sonuçlara yönelik, diğeri navigasyon amacıyla statik bir faturlar listesi oluşturmak için kullanılır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
+ [Dizin REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)
