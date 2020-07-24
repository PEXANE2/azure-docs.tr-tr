---
title: Uygulamalarda arama gezinmesi için model filtreleri
titleSuffix: Azure Cognitive Search
description: Microsoft Azure üzerinde barındırılan bir bulut arama hizmeti olan Azure Bilişsel Arama 'deki sorgularda arama sonuçlarını azaltmak için ölçütleri Kullanıcı güvenliği kimliği, coğrafi konum veya sayısal değerlere göre filtreleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 852d8f8f85536dc62dd792e5727dd7ec0571ba29
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084220"
---
# <a name="how-to-build-a-facet-filter-in-azure-cognitive-search"></a>Azure Bilişsel Arama bir model filtresi oluşturma 

Çok yönlü gezinme, bir arama uygulamasındaki sorgu sonuçlarında kendi kendine yönlendirilmiş filtreleme için kullanılır; burada uygulamanız, belge grupları için arama için Kullanıcı arabirimi denetimleri sunar (örneğin, Kategoriler veya markalar) ve Azure Bilişsel Arama deneyimi geri yüklemek için veri yapısını sağlar. Bu makalede, sağlamak istediğiniz arama deneyimini yedekleyen çok yönlü bir gezinti yapısı oluşturmaya yönelik temel adımları hızlıca gözden geçirin. 

> [!div class="checklist"]
> * Filtreleme ve Faks oluşturma için alanları seçin
> * Alanda öznitelikleri ayarla
> * Dizin oluşturma ve verileri yükleme
> * Bir sorguya model filtreleri ekleme
> * Sonuçları işleme

Modeller dinamiktir ve sorgu üzerinde döndürülür. Arama yanıtları, sonuçlarda gezinmek için kullanılan model kategorileriyle birlikte getirir. Modellerle ilgili bilginiz yoksa, aşağıdaki örnek bir model Gezinti yapısının bir temsilidir.

  ![Filtrelenmiş arama sonuçlarıyla iş başlığına göre gruplanmış bir arama iletişim kutusu gösteren bir görüntü. Bir ok, sonuçların bir model gezinti yapısında görüntülenen modellerdir.](./media/search-filters-facets/facet-nav.png)

Çok yönlü gezinmede yeni ve daha fazla ayrıntı istiyor musunuz? Bkz. [Azure bilişsel arama 'da çok yönlü gezintiyi uygulama](search-faceted-navigation.md).

## <a name="choose-fields"></a>Alanları seçin

Modeller, tek değer alanları ve koleksiyonlar üzerinden hesaplanabilir. Çok yönlü gezinmede en iyi şekilde çalışan alanlar düşük kardinalite sahiptir: arama corpminizdeki belgeleri (örneğin, bir renk, ülke/bölge veya marka adı) tekrarlayan çok az sayıda benzersiz değer. 

Özniteliği öğesini olarak ayarlayarak, dizin oluşturduğunuzda alan temelinde, her zaman bir alan temelinde etkinleştirilir `facetable` `true` . Ayrıca, `filterable` `true` arama uygulamanızın, son kullanıcının seçtiği modellerle ilgili alanlara filtre yapabilmesi için, bu tür alanlar için özniteliğini genellikle olarak ayarlamanız gerekir. 

REST API kullanarak bir dizin oluştururken, çok yönlü gezintide kullanılabilecek olan herhangi bir [alan türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) varsayılan olarak olarak işaretlenir `facetable` :

+ `Edm.String`
+ `Edm.DateTimeOffset`
+ `Edm.Boolean`
+ Sayısal alan türleri: `Edm.Int32` , `Edm.Int64` ,`Edm.Double`
+ Yukarıdaki türlerin koleksiyonları (örneğin, `Collection(Edm.String)` veya `Collection(Edm.Double)` )

Çok `Edm.GeographyPoint` `Collection(Edm.GeographyPoint)` yönlü gezinmede veya alanlarını kullanamazsınız. Modeller düşük önem taşıyan alanlarda en iyi şekilde çalışır. Coğrafi koordinatların çözümlenme nedeniyle, her iki ortak ordinonun kümesi, belirli bir veri kümesinde eşit olacaktır. Bu nedenle, coğrafi koordinatlara yönelik modeller desteklenmez. Konuma göre model için bir şehir veya bölge alanı gerekir.

## <a name="set-attributes"></a>Öznitelikleri ayarla

Bir alanın nasıl kullanıldığını denetleyen dizin öznitelikleri, dizindeki ayrı alan tanımlarına eklenir. Aşağıdaki örnekte, düşük önem düzeyi olan alanlar, her zaman için yararlıdır,, aşağıdakilerden oluşur: `category` (otel, Motel, Hostel), `tags` ve `rating` . Bu alanlar, `filterable` `facetable` tanım amaçları için aşağıdaki örnekte açıkça ayarlanmış ve öznitelikleri vardır. 

> [!Tip]
> Performans ve depolama iyileştirmesi için en iyi uygulama olarak, bir model olarak asla kullanılmamalıdır. Özellikle, bir KIMLIK veya ürün adı gibi benzersiz değerler için dize alanları, çok `"facetable": false` yönlü olan gezinmede yanlışlıkla (ve verimsiz) kullanılmasını engellemek için olarak ayarlanmalıdır.


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
> Bu dizin tanımı [, REST API kullanılarak Azure bilişsel arama dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api)işleminden kopyalanır. Bu, alan tanımlarındaki yararlanmayan farklılıkları dışında aynıdır. `filterable`Ve öznitelikleri,,,, `facetable` `category` `tags` `parkingIncluded` `smokingAllowed` ve `rating` alanlarına açıkça eklenir. Uygulamada, `filterable` ve `facetable` REST API kullanılırken bu alanlarda varsayılan olarak etkinleştirilir. .NET SDK kullanırken, bu özniteliklerin açıkça etkinleştirilmesi gerekir.

## <a name="build-and-load-an-index"></a>Dizin oluşturma ve yükleme

Ara (ve belirgin) bir adım sorgu oluşturmadan önce [dizini derleyip doldurmanız](https://docs.microsoft.com/azure/search/search-get-started-dotnet#1---create-index) gerekir. Bu adımdan daha fazla bahsedin. Dizinin kullanılabilir olup olmadığını belirlemenin bir yolu, [portaldaki](https://portal.azure.com)dizinler listesini denetleyerek.

## <a name="add-facet-filters-to-a-query"></a>Bir sorguya model filtreleri ekleme

Uygulama kodu ' nda, arama ifadeleri, modeller, filtreler, Puanlama profilleri gibi geçerli bir sorgunun tüm parçalarını belirten bir sorgu oluşturun ve bir isteği formülleştirmek için kullanılan her şey vardır. Aşağıdaki örnek, konaklama, derecelendirme ve diğer değişiklik türlerini temel alan model gezintisi oluşturan bir istek oluşturur.

```csharp
var sp = new SearchParameters()
{
    ...
    // Add facets
    Facets = new[] { "category", "rating", "parkingIncluded", "smokingAllowed" }.ToList()
};
```

### <a name="return-filtered-results-on-click-events"></a>Tıklama olaylarına göre filtrelenmiş sonuçları döndür

Son Kullanıcı bir model değerine tıkladığında, Click olayının işleyicisi kullanıcının hedefini gerçekleştirmek için bir filtre ifadesi kullanmalıdır. Bir `category` model verildiğinde, "Motel" kategorisini tıklatmak, `$filter` Bu türün konaklamaları seçen bir ifadeyle uygulanır. Bir Kullanıcı yalnızca Motels gösterilmesi gerektiğini belirtmek için "Motel" öğesini tıkladığında, uygulamanın gönderdiği sonraki sorgu de kapsar `$filter=category eq 'motel'` .

Aşağıdaki kod parçacığı, bir Kullanıcı kategori modeli 'nden bir değer seçerse filtreye kategori ekler.

```csharp
if (!String.IsNullOrEmpty(categoryFacet))
    filter = $"category eq '{categoryFacet}'";
```

Kullanıcı gibi bir koleksiyon alanı için bir model değerine tıklarsa `tags` , örneğin, "Pool" değeri, uygulamanız aşağıdaki filtre sözdizimini kullanmalıdır:`$filter=tags/any(t: t eq 'pool')`

## <a name="tips-and-workarounds"></a>İpuçları ve geçici çözümler

### <a name="initialize-a-page-with-facets-in-place"></a>Özellikleri yerinde bir sayfa başlatın

Özellikleri yerinde bir sayfa başlatmak istiyorsanız, sayfayı bir başlangıç modeli yapısına göre temel almak için sayfa başlatmanın bir parçası olarak bir sorgu gönderebilirsiniz.

### <a name="preserve-a-facet-navigation-structure-asynchronously-of-filtered-results"></a>Filtrelenmiş sonuçlardan zaman uyumsuz bir model gezinti yapısını koruma

Azure Bilişsel Arama 'de model gezintisi ile ilgili güçlüklerden biri yalnızca geçerli sonuçlar için mevcut olan bir modeldir. Uygulamada, bir statik model kümesinin tutulması yaygın bir yöntemdir. böylece Kullanıcı, arama içeriği aracılığıyla alternatif yolları araştırmak için adımları yeniden izleyerek geri gidebilir. 

Bu, yaygın olarak kullanılan bir kullanım durumu olsa da, model Gezinti yapısının Şu anda kullanıma hazır olarak sağladığı bir şey değildir. Statik modeller isteyen geliştiriciler genellikle iki filtrelenmiş sorgu vererek sınırlamanın etrafında çalışır: sonuçlara kapsamlı bir kapsam, diğer bir deyişle, gezinme amacıyla bir statik model listesi oluşturmak için kullanılır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama filtreler](search-filters.md)
+ [Dizin REST API oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)
