---
title: Dizin için typeahead sorguları ekleme
titleSuffix: Azure Cognitive Search
description: Otomatik tamamlama veya otomatik önerilen sorgu terimlerini çağıran önericiler oluşturarak ve istekler formüle ederek Azure Bilişsel Arama'da önceden yazma sorgu eylemlerini etkinleştirin.
manager: nitinme
author: Brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
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
ms.openlocfilehash: a312068d5c8c574e7b069263cf37e3b855810e4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72790111"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da typeahead için bir dizin için öneri toplayıcıları ekleme

Azure Bilişsel Arama'da , "ara-iş olarak arama" veya typeahead işlevi, arama [dizinine](search-what-is-an-index.md)eklediğiniz bir **önerici** yapıyı temel almaktadır. Bu, typeahead etkin olmasını istediğiniz bir veya daha fazla alanın listesidir.

Önerici iki türilerlemesi destekler: *otomatik tamamlama,* yazdığınız terimi veya tümcecik tamamlar ve eşleşen belgelerin kısa bir listesini döndüren *öneriler.*  

[C# örneğindeki ilk uygulamanızı oluştur'dan](tutorial-csharp-type-ahead-and-suggestions.md) aşağıdaki ekran görüntüsü, typeahead'ı gösterir. Otomatik tamamlama, kullanıcının arama kutusuna ne yazabileceğini tahmin eder. Gerçek giriş "tw", otomatik tamamlama "in" ile bitirir, "ikiz" olarak potansiyel arama terimi olarak çözülme. Öneriler açılır listede görselleştirilir. Öneriler için, belgenin sonucu en iyi açıklayan herhangi bir bölümünü yüzebilirsiniz. Bu örnekte, öneriler otel adlarıdır. 

![Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması")

Bu davranışları Azure Bilişsel Arama'da uygulamak için bir dizin ve sorgu bileşeni vardır. 

+ Dizine bir önerici ekleyin. Portalı, [REST API'yi](https://docs.microsoft.com/rest/api/searchservice/create-index)veya [.NET SDK'yı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)kullanabilirsiniz. Bu makalenin geri kalanı bir önerici oluşturmaya odaklanmıştır. 

+ Sorgu isteğinde, [aşağıda listelenen API'lerden](#how-to-use-a-suggester)birini arayın.

Sizin gibi ara desteği, her alan için ayrı ayrı etkinleştirilir. Ekran görüntüsünde belirtilene benzer bir deneyim istiyorsanız, aynı arama çözümü içinde her iki tür ilerlemesi davranışı uygulayabilirsiniz. Her iki istek de belirli dizin *belgelerinin* toplanmasını hedefalır ve kullanıcı en az üç karakterli giriş dizesini sağladıktan sonra yanıtlar döndürülür.

## <a name="create-a-suggester"></a>Öneri oluşturucu oluşturma

Bir önericinin birkaç özelliği olsa da, öncelikle bir tür ileri deneyimi etkinleştirdiğiniz alanlar topluluğudur. Örneğin, bir seyahat uygulaması varış noktalarında, şehirlerde ve konumlarda önceden arama yapılmasını etkinleştirmek isteyebilir. Bu nedenle, her üç alan alanları toplama gider.

Bir önerici oluşturmak için, bir dizin şemasına ekleyin. Bir dizin (özellikle, önerici koleksiyonunda bir önerici) bir önerici olabilir. 

### <a name="when-to-create-a-suggester"></a>Ne zaman bir önerici oluşturmak için

Bir önerici oluşturmak için en iyi zaman da alan tanımı kendisi oluştururken.

Önceden varolan alanları kullanarak bir önerici oluşturmaya çalışırsanız, API buna izin vermez. Dizin oluşturma sırasında, iki veya daha fazla karakter birleşimindeki kısmi terimler tüm terimlerle birlikte belirtildiğinde oluşturulur. Varolan alanların zaten belirteçolarak verildiği göz önüne alındığında, bunları bir önericiye eklemek istiyorsanız dizini yeniden oluşturmanız gerekir. Yeniden dizin oluşturma hakkında daha fazla bilgi için Azure [Bilişsel Arama dizinini nasıl yeniden oluşturabildiğini](search-howto-reindex.md)öğrenin.

### <a name="create-using-the-rest-api"></a>REST API'sini kullanarak oluşturma

REST API'sinde, [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [Güncelleştirme Dizini](https://docs.microsoft.com/rest/api/searchservice/update-index)aracılığıyla öneriyi ekleyin. 

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


### <a name="create-using-the-net-sdk"></a>.NET SDK'yı kullanarak oluşturma

C#'da, [Bir Önerici nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)tanımlayın. `Suggesters`bir koleksiyondur, ancak yalnızca bir öğe alabilir. 

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

### <a name="property-reference"></a>Özellik başvurusu

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        |Önerenin adı.|
|`searchMode`  |Strateji aday ifadeleri aramak için kullanılır. Şu anda desteklenen `analyzingInfixMatching`tek mod, tümceciklerin başında veya ortasında esnek eşleştirme gerçekleştirir.|
|`sourceFields`|Öneriler için içeriğin kaynağı olan bir veya daha fazla alanın listesi. Alanlar türünde `Edm.String` olmalı `Collection(Edm.String)`ve . Alanda bir çözümleyici belirtilmişse, [bu listeden](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) adlandırılmış bir çözümleyici olmalıdır (özel bir çözümleyici değil).<p/>En iyi uygulama olarak, yalnızca bir arama çubuğunda tamamlanmış bir dize veya açılır bırakma listesinde tamamlanmış bir dize olsun, beklenen ve uygun yanıtiçin kendilerini ödünç alanları belirtin.<p/>Bir otel adı iyi bir adaydır çünkü hassastır. Açıklamalar ve açıklamalar gibi ayrıntılı alanlar çok yoğun. Benzer şekilde, kategoriler ve etiketler gibi yinelenen alanlar daha az etkilidir. Örneklerde, birden çok alan ekleyebileceğinizi göstermek için yine de "kategori"yi ekleriz. |

### <a name="analyzer-restrictions-for-sourcefields-in-a-suggester"></a>Bir önericide kaynak Alanlar için çözümleyici kısıtlamaları

Azure Bilişsel Arama, tek tek terimlerle sorgulamayı etkinleştirmek için alan içeriğini analiz eder. Önericiler, kaynak alanlar üzerinde ek çözümleme gerektiren tam terimlere ek olarak öneeklerin dizine eklenmesini gerektirir. Özel çözümleyici yapılandırmaları, çeşitli belirteçleri ve filtrelerden herhangi birini, genellikle öneriler için gerekli önekleri oluşturmayı imkansız hale getirecek şekilde birleştirebilir. Bu nedenle, Azure Bilişsel Arama özel çözümleyicileri içeren alanların bir önericiye eklenmesini önler.

> [!NOTE] 
>  Yukarıdaki sınırlamayı aşmanız gerekiyorsa, aynı içerik için iki ayrı alan kullanın. Bu, alanlardan birinin bir önericiye sahip olmasına olanak sağlarken, diğeri özel bir çözümleyici yapılandırması ile ayarlanabilir.

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester-in-a-query"></a>Sorguda önerici kullanma

Bir önerici oluşturulduktan sonra, özelliği çağırmak için sorgu mantığınızdaki uygun API'yi arayın. 

+ [Öneriler REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [Otomatik tamamlama REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [Otomatik TamamlamaWithHttpMessagesAsync yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

API kullanımı, Otomatik Tamamlama REST API'sine yapılan aşağıdaki çağrıda gösterilmiştir. Bu örnekten iki paket vardır. İlk olarak, tüm sorgularda olduğu gibi, işlem bir dizin belgeleri toplama karşıdır. İkinci olarak, sorgu parametreleri ekleyebilirsiniz. Birçok sorgu parametresi her iki API için de ortak olsa da, liste her biri için farklıdır.

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs/autocomplete?[query parameters]  
api-key: [admin or query key]
```

Diziniçinde bir önerici tanımlanmamışsa, otomatik tamamlama çağrısı veya öneriler başarısız olur.

## <a name="sample-code"></a>Örnek kod

+ [C# örneğinde ilk uygulamanızı oluşturun,](tutorial-csharp-type-ahead-and-suggestions.md) önerilen sorgular, otomatik tamamlama ve yönlü gezinme yi önerir. Bu kod örneği bir sandbox Azure Bilişsel Arama hizmetinde çalışır ve önceden yüklenmiş oteller dizini kullanır, bu nedenle tek yapmanız gereken uygulamayı çalıştırmak için F5 tuşuna basmaktır. Abonelik veya oturum açma gerekmez.

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) hem C# hem de Java kodu içeren eski bir örnektir. Ayrıca, düşündürücü bir yapı, önerilen sorgular, otomatik tamamlama ve yönlü gezinme yi de gösterir. Bu kod örneği barındırılan [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek verilerini kullanır. 


## <a name="next-steps"></a>Sonraki adımlar

İsteklerin nasıl formüle edilebildiğini görmek için aşağıdaki örneği öneririz.

> [!div class="nextstepaction"]
> [Öneriler ve otomatik tamamlama örnekleri](search-autocomplete-tutorial.md) 
