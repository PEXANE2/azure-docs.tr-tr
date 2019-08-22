---
title: Dizine typeahead sorguları ekleme-Azure Search
description: Otomatik tamamlama veya otomatik önerilen sorgu koşullarını çağıran istekleri öneri araçları ve formülleyerek tür ön sorgu Azure Search eylemlerini etkinleştirin.
ms.date: 05/02/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
manager: nitinme
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 73cfdb6a4185689a6485f55a4f6bdd1e7e3b14be
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648854"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-search"></a>Azure Search typeahead için bir dizine öneri araçları ekleyin

Bir **öneri aracı** , bir "yazarken arama" deneyimi destekleyen [Azure Search dizindeki](search-what-is-an-index.md) bir yapıdır. Typeahead sorgu girişlerini etkinleştirmek istediğiniz alanların listesini içerir. Bir dizin içinde, aynı öneri aracı bu iki typeahead varyantının birini ya da her ikisini de destekler: *otomatik tamamlama* , yazmakta olduğunuz terimi veya tümceciği tamamlar, *öneriler* , sonuçların kısa bir listesini sağlar. 

Aşağıdaki ekran görüntüsünde, typeahead özelliklerinin her ikisi de gösterilmektedir. Bu Xbox arama sayfasında, otomatik tamamlama öğeleri sizi bu sorgu için yeni bir arama sonuçları sayfasına götürür, ancak öneriler sizi söz konusu oyunun sayfasına götürür. Otomatik tamamlamayı bir arama çubuğundaki bir öğeyle sınırlayabilir veya burada gösterilenle benzer bir liste sağlayabilirsiniz. Öneriler için, sonucu en iyi açıklayan bir belgenin herhangi bir bölümünü yüzeyinize ekleyebilirsiniz.

![Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması](./media/index-add-suggesters/visual-comparison-suggest-complete.png "Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması")

Bu davranışları Azure Search uygulamak için bir dizin ve sorgu bileşeni vardır. 

+ Dizin bileşeni bir öneri aracı. Öneri aracı oluşturmak için Portal, REST API veya .NET SDK kullanabilirsiniz. 

+ Sorgu bileşeni sorgu isteğinde belirtilen bir eylemdir (bir öneri veya otomatik tamamlama eylemi). 

Arama ölçütü türü, alan temelinde desteklenir. Ekran görüntüsünde gösterilenle benzer bir deneyim istiyorsanız, aynı arama çözümü içinde her iki typeahead davranışını da uygulayabilirsiniz. Her iki istek de, bir Kullanıcı en az üç karakter girişi dizesi sağladıktan sonra, belirli dizin ve yanıtların *belge* koleksiyonunu hedefler.

## <a name="create-a-suggester"></a>Öneri oluşturucu oluşturma

Bir öneri aracı birçok özelliğe sahip olsa da, birincil olarak bir typeahead deneyimini etkinleştirilebilmeniz için bir alanlar koleksiyonudur. Örneğin, bir seyahat uygulaması, hedefler, şehirler ve atlamalar üzerinde typeahead aramasını etkinleştirmek isteyebilir. Bu nedenle, üç alan de alanlar koleksiyonunda gider.

Bir öneri aracı oluşturmak için bir dizin şemasına bir tane ekleyin. Bir dizinde (özellikle, öneri araçları koleksiyonunda bir öneri aracı) bir öneri aracı olabilir. 

### <a name="use-the-rest-api"></a>REST API kullanma

REST API [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [güncelleştirme dizini](https://docs.microsoft.com/rest/api/searchservice/update-index)aracılığıyla öneri araçları ekleyebilirsiniz. 

  ```json
  {
    "name": "hotels",
    "fields": [
      . . .
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["hotelName", "category"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```
Bir öneri aracı oluşturulduktan sonra, özelliği çağırmak için sorgu mantığınızdaki [ÖNERILER API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 'Sini veya [AutoComplete API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 'sini ekleyin.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

İçinde C#, bir [öneri aracı nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)tanımlayın. `Suggesters`bir koleksiyon ancak yalnızca bir öğe alabilir. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelId", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

## <a name="property-reference"></a>Özellik Başvurusu

Öneri araçları hakkında dikkat edilecek önemli noktalara, bir ad (öneri araçları bir istek üzerinde ada göre başvuruluyor), bir searchMode (Şu anda tek bir "çözümleyiciler," çözümleyiciler ") ve typeahead 'in etkinleştirildiği alanların listesi olduğu anlamına gelir. 

Bir öneri aracı tanımlayan özellikler şunları içerir:

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        |Öneri aracı adı. [Öneriler REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete)çağrılırken öneri aracı adını kullanırsınız.|
|`searchMode`  |Aday tümcecikleri aramak için kullanılan strateji. Şu anda desteklenen tek mod, `analyzingInfixMatching`Tümcelerin başındaki veya ortasındaki tümceciklerden esnek eşleme gerçekleştiren ' dir.|
|`sourceFields`|Önerilerin içerik kaynağı olan bir veya daha fazla alanın listesi. Yalnızca türündeki `Edm.String` alanlar ve `Collection(Edm.String)` öneriler için kaynaklar olabilir. Yalnızca özel dil Çözümleyicisi kümesi olmayan alanlar kullanılabilir.<p/>Bir arama çubuğunda veya açılan listede bir tamamlanmış dize olup olmadığı gibi, yalnızca beklenen ve uygun bir yanıta ödünç veren alanları belirtin.<p/>Bir otel adı duyarlık içerdiğinden iyi bir adaydır. Açıklamalar ve açıklamalar gibi ayrıntılı alanlar çok yoğun. Benzer şekilde, Kategoriler ve Etiketler gibi yinelenen alanlar daha az etkilidir. Örneklerde, birden fazla alanı dahil etbileceğinizi göstermek için, "Category" de yer alır. |

#### <a name="analysis-of-sourcefields-in-a-suggester"></a>Bir öneri aracı içinde SourceFields Analizi

Azure Search, alan içeriğini çözümleyerek tek tek şartlar üzerinde sorgu yapmayı etkinleştirir. Öneri araçları, kaynak alanları üzerinde ek analizler gerektiren tüm koşullara ek olarak ön eklerin dizinlenmesini gerektirir. Özel çözümleyici yapılandırması, çeşitli belirteçlerin ve filtrelerin herhangi birini, genellikle öneriler için gerekli olan ön ekleri üreten yöntemlerle birleştirebilir. Bu nedenle Azure Search, **özel Çözümleyicileri olan alanların bir öneri aracı dahil edilmesini engeller**.

> [!NOTE] 
>  Yukarıdaki sınırlamanın etrafında çalışmak için önerilen yaklaşım, aynı içerik için 2 ayrı alan kullanmaktır. Bu, alanlardan birinin öneri araçları sahip olmasını sağlar ve diğeri özel bir çözümleyici yapılandırmasıyla ayarlanabilir.

## <a name="when-to-create-a-suggester"></a>Ne zaman bir öneri aracı oluşturulur?

Dizin yeniden oluşturmayı önlemek için, içinde `sourceFields` belirtilen bir öneri aracı ve alanlar aynı anda oluşturulmalıdır.

Var olan alanların içine `sourceFields`dahil edildiği mevcut dizine bir öneri aracı eklerseniz, temel olarak değişiklik alan tanımı ve yeniden oluşturma gereklidir. Daha fazla bilgi için bkz. [Azure Search dizin oluşturma](search-howto-reindex.md).

## <a name="how-to-use-a-suggester"></a>Öneri aracı kullanma

Daha önce belirtildiği gibi, önerilen sorgular, otomatik tamamlama veya her ikisi için bir öneri aracı kullanabilirsiniz. 

İşlemle birlikte, istek üzerinde bir öneri aracı öğesine başvurulur. Örneğin, bir rest çağrısında, belgeler koleksiyonunda ya da `suggest` `autocomplete` ' i belirtin. REST için, bir öneri aracı oluşturulduktan sonra, sorgu mantığınızdaki [ÖNERILER API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 'Sini veya [AutoComplete API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 'sini kullanın.

.NET için [SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet) veya [AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)kullanın.

Her iki isteği de gösteren bir örnek için, [Azure Search otomatik tamamlama ve öneri ekleme örneğine](search-autocomplete-tutorial.md)bakın.

## <a name="sample-code"></a>Örnek kod

[Dotnethowtoautocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) örneği hem hem de C# Java kodunu içerir ve bir öneri aracı yapımı, önerilen sorgular, otomatik tamamlama ve model gezintisini gösterir. 

Bir korumalı alan Azure Search hizmeti ve önceden yüklenmiş bir dizin kullanır, bu nedenle tüm yapmanız için F5 tuşuna basın. Abonelik veya oturum açma gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

İsteklerin nasıl formül altında olduğunu görmek için aşağıdaki örneği öneririz.

> [!div class="nextstepaction"]
> [Öneriler ve AutoComplete örnekleri](search-autocomplete-tutorial.md) 
