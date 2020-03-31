---
title: Arama sonuçlarıyla çalışma
titleSuffix: Azure Cognitive Search
description: Arama sonuçlarını yapılayın ve sıralayın, belge sayısı alın ve Azure Bilişsel Arama'daki arama sonuçlarına içerik gezintisi ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: 124f1ce3d30ce87d5e9d8fa027e5a7d6c0b3cb17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481611"
---
# <a name="how-to-work-with-search-results-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da arama sonuçlarıyla çalışma
Bu makalede, toplam sayımlar, belge alma, sıralama siparişleri ve gezinme gibi bir arama sonuçları sayfasının standart öğelerinin nasıl uygulanacağı konusunda kılavuz lar verilmektedir. Arama sonuçlarınıza veri veya bilgi sağlayan sayfayla ilgili seçenekler, Azure Bilişsel Arama hizmetinize gönderilen [Arama Belgesi](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) istekleri aracılığıyla belirtilir. 

REST API'sinde, istekler, hizmete neyin istendiğini ve yanıtı nasıl formüle edeceğimi bildiren bir GET komutu, yol ve sorgu parametrelerini içerir. .NET SDK'da, eşdeğer API [DocumentSearchResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1)Class'tır.

İstemciniz için hızlı bir arama sayfası oluşturmak için şu seçenekleri keşfedin:

+ Bir arama çubuğu, yönlü navigasyon ve sonuç alanı içeren bir HTML sayfası oluşturmak için portaldaki [uygulama jeneratörünü](search-create-app-portal.md) kullanın.
+ İşlevsel bir istemci oluşturmak için [C# öğreticisinde ilk uygulamanızı oluşturun'u](tutorial-csharp-create-first-app.md) izleyin.

Çeşitli kod örnekleri burada bulabilirsiniz bir web ön uç arayüzü, şunlardır: [New York City Jobs demo uygulaması](https://aka.ms/azjobsdemo), Canlı bir demo sitesi ile [JavaScript örnek kodu](https://github.com/liamca/azure-search-javascript-samples), ve [CognitiveSearchFrontEnd](https://github.com/LuisCabrer/CognitiveSearchFrontEnd).

> [!NOTE]
> Geçerli bir istek, hizmet URL'si ve yolu, HTTP `api-version`fiili ve benzeri öğeler içerir. Kısalık için, biz pagination ile ilgili sadece sözdizimi vurgulamak için örnekleri kesilmiş. İstek sözdizimi hakkında daha fazla bilgi için [Azure Bilişsel Arama REST API'leri'ne](https://docs.microsoft.com/rest/api/searchservice)bakın.
>

## <a name="total-hits-and-page-counts"></a>Toplam isabet sayısı ve Sayfa Sayısı

Bir sorgudan döndürülen toplam sonuç sayısını göstermek ve bu sonuçları daha küçük parçalar halinde döndürmek, hemen hemen tüm arama sayfaları için çok önemlidir.

![][1]

Azure Bilişsel Arama'da, `$count` `$top`bu `$skip` değerleri döndürmek için , ve parametreleri kullanırsınız. Aşağıdaki örnek, "çevrimiçi katalog" adı verilen bir dizindeki toplam `@odata.count`isabetler için örnek bir istek gösterir ve şu şekilde döndürülür:

    GET /indexes/online-catalog/docs?$count=true

Belgeleri 15'erli gruplar halinde alın ve ilk sayfadan başlayarak toplam isabetleri de gösterin:

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=0&$count=true

Paginating sonuçları `$top` hem `$skip`gerektirir `$top` ve , bir toplu iş dönmek `$skip` için kaç öğe belirtir ve kaç öğe atlamak için belirtir. Aşağıdaki örnekte, her sayfa parametredeki artımlı atlayışlarla `$skip` gösterilen sonraki 15 öğeyi gösterir.

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=0&$count=true

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=15&$count=true

    GET /indexes/online-catalog/docs?search=*&$top=15&$skip=30&$count=true

## <a name="layout"></a>Düzen

Arama sonuçları sayfasında, küçük resim görüntüsünü, alanların bir alt kümesini ve tam ürün sayfasına bağlantı göstermek isteyebilirsiniz.

 ![][2]

Azure Bilişsel Arama'da, `$select` bu deneyimi uygulamak için bir [Arama API isteği](https://docs.microsoft.com/rest/api/searchservice/search-documents) kullanırsınız.

Karo düzen için alanların bir alt kümesini döndürmek için:

    GET /indexes/online-catalog/docs?search=*&$select=productName,imageFile,description,price,rating

Görüntüler ve medya dosyaları doğrudan aranmaz ve maliyetleri azaltmak için Azure Blob depolama alanı gibi başka bir depolama platformunda depolanmalıdır. Dizin ve belgelerde, dış içeriğin URL adresini depolayan bir alan tanımlayın. Daha sonra alanı görüntü başvurusu olarak kullanabilirsiniz. Resmin URL'si belgede olmalıdır.

**Bir onClick** olayı için ürün açıklaması sayfası almak için, almak için belgenin anahtarını geçirmek için [Arama Belgesi'ni](https://docs.microsoft.com/rest/api/searchservice/Lookup-Document) kullanın. Anahtarın veri türü `Edm.String`. Bu örnekte, *246810*olduğunu.

    GET /indexes/online-catalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Alaka düzeyine, derecelendirmeye veya fiyata göre sıralama

Siparişleri genellikle alaka düzeyine göre sıralayın, ancak müşterilerin varolan sonuçları hızla farklı bir sıralama siparişinde değiştirebilmesi için alternatif sıralama siparişlerini hazır hale getirmek yaygındır.

 ![][3]

Azure Bilişsel Arama'da sıralama, `$orderby` `"Sortable": true.` `$orderby` yan tümce olarak dizine eklenmiş tüm alanlar için bir OData ifadesi olan ifadeyi temel alınr. Sözdizimi hakkında bilgi için, [filtreler ve sipariş eki yan tümceleri için OData ifade sözdizimine](query-odata-filter-orderby-syntax.md)bakın.

Alaka düzeyi, puanlama profilleriyle güçlü bir şekilde ilişkilidir. Metin çözümlemesi ve istatistiklere dayanan varsayılan puanlamayı kullanarak tüm sonuçları sıralayabilir ve daha yüksek puanlar bir arama teriminde daha fazla veya daha güçlü eşleşmelere sahip belgelere giderek sıralanır.

Alternatif sıralama siparişleri genellikle sıralama sırasını oluşturan bir yönteme geri çağrı yapan **onClick** olayları ile ilişkilidir. Örneğin, bu sayfa öğesi göz önüne alındığında:

 ![][4]

Seçili sıralama seçeneğini giriş olarak kabul eden ve bu seçenekle ilişkili ölçütler için sıralı bir liste döndüren bir yöntem oluşturursunuz.

 ![][5]

> [!NOTE]
> Varsayılan puanlama birçok senaryo için yeterli olsa da, alaka düzeyini bunun yerine özel bir puanlama profiline dayandırmanızı öneririz. Özel puanlama profili, işletmeniz için daha yararlı olan öğeleri artırmanın bir yolunu sunar. Bkz. Daha fazla bilgi için [puanlama profilleri ekle.](index-add-scoring-profiles.md)
>

## <a name="hit-highlighting"></a>İsabet vurgulama

Biçimlendirmeyi arama sonuçlarında eşleşen terimlere uygulayarak eşleşmeyi kolayca fark edebilirsiniz. Hit vurgulama yönergeleri sorgu [isteğinde](https://docs.microsoft.com/rest/api/searchservice/search-documents)sağlanır. 

Biçimlendirme tüm terim sorgularına uygulanır. Bulanık arama veya joker karakter araması gibi, motorda sorgu genişletmesi ile sonuçlanan kısmi terimlerdeki sorgular, isabet vurgulamayı kullanamaz.

```http
POST /indexes/hotels/docs/search?api-version=2019-05-06 
    {  
      "search": "something",  
      "highlight": "Description"  
    }
```

> [!IMPORTANT]
> 15 Temmuz 2020'den sonra oluşturulan hizmetler farklı bir vurgulama deneyimi sağlayacaktır. Bu tarihten önce oluşturulan hizmetler, vurgulama davranışlarında değişmez. Bu değişiklikle, yalnızca tam tümcecik sorgusuyla eşleşen tümcecikler döndürülür. Ayrıca, vurgu için döndürülen parça boyutunu belirtmek de mümkün olacaktır.
>
> Isabet vurgulamayı uygulayan istemci kodu yazarken, bu değişikliğin farkında olun. Tamamen yeni bir arama hizmeti oluşturmadığınız sürece bunun sizi etkilemeyeceğini unutmayın.

## <a name="faceted-navigation"></a>Çok yönlü gezinme

Arama gezintisi, genellikle bir sayfanın yanında veya üstünde bulunan bir sonuç sayfasında yaygındır. Azure Bilişsel Arama'da, yönlü gezinme önceden tanımlanmış filtreleri temel alan kendi kendini yönlendirmiş arama sağlar. Ayrıntılar için [Azure Bilişsel Arama'da Faceted navigasyonuna](search-faceted-navigation.md) bakın.

## <a name="filters-at-the-page-level"></a>Sayfa düzeyindefiltreler

Çözüm tasarımınız belirli içerik türleri için özel arama sayfaları (örneğin, sayfanın üst kısmında bölümler listelenen bir çevrimiçi perakende uygulaması) yer alıyorsa, önceden filtrelenmiş bir durumda bir sayfa açmak için **bir onClick** etkinliğinin yanına bir [filtre ifadesi](search-filters.md) ekleyebilirsiniz.

Arama ifadesi olan veya olmayan bir filtre gönderebilirsiniz. Örneğin, aşağıdaki istek yalnızca onunla eşleşen belgeleri döndürerek marka adına filtre uygulayacaktır.

    GET /indexes/online-catalog/docs?$filter=brandname eq 'Microsoft' and category eq 'Games'

İfadeler hakkında `$filter` daha fazla bilgi için [Arama Belgeleri 'ne (Azure Bilişsel Arama API' si)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) bakın.

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure Bilişsel Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice)
- [Dizin İşlemleri](https://docs.microsoft.com/rest/api/searchservice/Index-operations)
- [Belge İşlemleri](https://docs.microsoft.com/rest/api/searchservice/Document-operations)
- [Azure Bilişsel Aramada Yönlü Gezinme](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png
