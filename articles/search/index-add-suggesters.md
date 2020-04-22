---
title: Öneri oluşturucu oluşturma
titleSuffix: Azure Cognitive Search
description: Otomatik tamamlama veya otomatik önerilen sorgu terimlerini çağıran önericiler oluşturarak ve istekler formüle ederek Azure Bilişsel Arama'da önceden yazma sorgu eylemlerini etkinleştirin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 7eb2988628d60fa72c7d83b81a58a1e0fae5de33
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770094"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>Sorguda otomatik tamamlama ve önerilen sonuçları etkinleştirmek için bir önerici oluşturma

Azure Bilişsel Arama'da, "ara-you-type" bir [arama dizinine](search-what-is-an-index.md)eklenen **bir düşündürücü** yapı aracılığıyla etkinleştirilir. Bir önerici iki deneyimi destekler: *otomatik tamamlama,* tüm dönem sorgusu için kısmi bir giriş tamamlar ve belirli bir eşleşmeye tıklamaya davet eden *öneriler.* Otomatik tamamlama bir sorgu üretir. Öneriler eşleşen bir belge üretir.

[C#'daki ilk uygulamanızı](tutorial-csharp-type-ahead-and-suggestions.md) oluştur'undan sonraki ekran görüntüsü her ikisini de gösterir. Otomatik tamamlama potansiyel bir terim bekliyor, "tw" ile "in" ile bitirme. Öneriler, otel adı gibi bir alanın dizinden eşleşen bir otel arama belgesini temsil ettiği mini arama sonuçlarıdır. Öneriler için, açıklayıcı bilgiler sağlayan herhangi bir alanı yüzebilirsiniz.

![Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması")

Bu özellikleri ayrı ayrı veya birlikte kullanabilirsiniz. Bu davranışları Azure Bilişsel Arama'da uygulamak için bir dizin ve sorgu bileşeni vardır. 

+ Dizine bir önerici ekleyin. Portalı, [REST API'yi](https://docs.microsoft.com/rest/api/searchservice/create-index)veya [.NET SDK'yı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)kullanabilirsiniz. Bu makalenin geri kalanı bir önerici oluşturmaya odaklanmıştır.

+ Sorgu isteğinde, [aşağıda listelenen API'lerden](#how-to-use-a-suggester)birini arayın.

Dize alanları için alan başına olarak arama türü desteği etkinleştirilir. Ekran görüntüsünde belirtilene benzer bir deneyim istiyorsanız, aynı arama çözümü içinde her iki tür ilerlemesi davranışı uygulayabilirsiniz. Her iki istek de belirli dizin *belgelerinin* toplanmasını hedefalır ve kullanıcı en az üç karakterli giriş dizesini sağladıktan sonra yanıtlar döndürülür.

## <a name="what-is-a-suggester"></a>Öneren nedir?

Önerici, kısmi sorgularda eşleştirme için önekleri depolayarak ara-son sınıf davranışlarını destekleyen bir iç veri yapısıdır. Belirteçterimleri olduğu gibi, önekler, önerici alanlar koleksiyonunda belirtilen her alan için bir tane olmak üzere ters dizinlerde depolanır.

## <a name="define-a-suggester"></a>Bir önerici tanımlayın

Bir önerici oluşturmak için, bir [dizin şema](https://docs.microsoft.com/rest/api/searchservice/create-index) bir ekleyin ve [her özellik ayarlayın.](#property-reference) Bir önerici oluşturmak için en iyi zaman da onu kullanacak alanı tanımlayan zaman.

+ Yalnızca dize alanlarını kullanma

+ Varsayılan standart Lucene çözümleyicisini ( )`"analyzer": null`veya bir dil [çözümleyicisini](index-add-language-analyzers.md) (örneğin,) `"analyzer": "en.Microsoft"`sahada kullanma

### <a name="choose-fields"></a>Alanları seçin

Bir önericinin birkaç özelliği olsa da, öncelikle sizin gibi bir arama deneyimi ne kadar etkinleştirdiğiniz dize alanları topluluğudur. Her dizin için bir önerici vardır, bu nedenle önerici listesi hem öneriler hem de otomatik tamamlama için içerik katkıda bulunan tüm alanları içermelidir.

Ek içeriğin daha fazla dönem tamamlama potansiyeline sahip olması nedeniyle, daha büyük bir alan havuzundan otomatik tamamlama avantajları ndan yararlanın.

Diğer taraftan, öneriler, alan seçiminiz seçici olduğunda daha iyi sonuçlar verir. Tek bir sonucu en iyi temsil eden alanları isteyeceğiniz şekilde, önerinin bir arama belgesi için bir proxy olduğunu unutmayın. Birden çok eşleşme arasında ayrım yapan adlar, başlıklar veya diğer benzersiz alanlar en iyi şekilde çalışır. Alanlar yinelenen değerlerden oluşuyorsa, öneriler aynı sonuçlardan oluşur ve kullanıcı hangisini tıklatacağını bilmez.

Her iki arama-as-you-type deneyimleri karşılamak için, otomatik tamamlama için gereken tüm alanları ekleyin, ancak sonra öneriler için sonuçları kontrol etmek için **$select**, **$top**, **$filter**ve **arama Alanları** kullanın.

### <a name="choose-analyzers"></a>Çözümleyicileri seçin

Çözümleyici seçiminiz, alanların nasıl belirteçleştirilme ve daha sonra önceden önceden belirlenmiş olduğunu belirler. Örneğin, "içeriğe duyarlı" gibi tireli bir dize için, bir dil çözümleyicisi kullanmak şu belirteç kombinasyonlarına neden olur: "bağlam", "duyarlı", "bağlam duyarlı". Standart Lucene çözümleyicisini kullansaydınız, tireli dize mevcut olmazdı. 

Çözümleyicileri değerlendirirken, terimlerin nasıl belirteçleştirilip daha sonra önceden belirlenmiş olduğunu anlamak için [Çözümleme Metni API'sini](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) kullanmayı düşünün. Bir dizin oluşturduğunuzda, belirteç çıktısını görüntülemek için dize üzerinde çeşitli çözümleyiciler deneyebilirsiniz.

[Özel çözümleyiciler](index-add-custom-analyzers.md) veya [önceden tanımlanmış çözümleyiciler](index-add-custom-analyzers.md#predefined-analyzers-reference) (standart Lucene hariç) kullanan alanlar, kötü sonuçları önlemek için açıkça izin verilmez.

> [!NOTE]
> Çözümleyici kısıtlaması üzerinde çalışmanız gerekiyorsa, örneğin belirli sorgu senaryoları için bir anahtar kelime veya ngram çözümleyicisine ihtiyacınız varsa, aynı içerik için iki ayrı alan kullanmanız gerekir. Bu, alanlardan birinin bir önericiye sahip olmasına olanak sağlarken, diğeri özel bir çözümleyici yapılandırması ile ayarlanabilir.

### <a name="when-to-create-a-suggester"></a>Ne zaman bir önerici oluşturmak için

Bir önerici oluşturmak için en iyi zaman da alan tanımı kendisi oluştururken.

Önceden varolan alanları kullanarak bir önerici oluşturmaya çalışırsanız, API buna izin vermez. Önekler, iki veya daha fazla karakter birleşimindeki kısmi terimlerin tüm terimlerle birlikte belirtildiğinde, dizin oluşturma sırasında oluşturulur. Varolan alanların zaten belirteçolarak verildiği göz önüne alındığında, bunları bir önericiye eklemek istiyorsanız dizini yeniden oluşturmanız gerekir. Daha fazla bilgi için Azure [Bilişsel Arama dizinini nasıl yeniden oluşturabilirsiniz.](search-howto-reindex.md)

## <a name="create-using-rest"></a>REST kullanarak oluşturma

REST API'sinde, [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [Güncelleştirme Dizini](https://docs.microsoft.com/rest/api/searchservice/update-index)aracılığıyla öneriyi ekleyin. 

  ```json
  {
    "name": "hotels-sample-index",
    "fields": [
      . . .
          {
              "name": "HotelName",
              "type": "Edm.String",
              "facetable": false,
              "filterable": false,
              "key": false,
              "retrievable": true,
              "searchable": true,
              "sortable": false,
              "analyzer": "en.microsoft",
              "indexAnalyzer": null,
              "searchAnalyzer": null,
              "synonymMaps": [],
              "fields": []
          },
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["HotelName"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

## <a name="create-using-net"></a>.NET kullanarak oluşturma

C#'da, [Bir Önerici nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)tanımlayın. `Suggesters`bir koleksiyondur, ancak yalnızca bir öğe alabilir. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels-sample-index",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelName", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

## <a name="property-reference"></a>Özellik başvurusu

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        |Önerenin adı.|
|`searchMode`  |Strateji aday ifadeleri aramak için kullanılır. Şu anda desteklenen `analyzingInfixMatching`tek mod, şu anda bir dönemin başında eşleşen.|
|`sourceFields`|Öneriler için içeriğin kaynağı olan bir veya daha fazla alanın listesi. Alanlar türünde `Edm.String` olmalı `Collection(Edm.String)`ve . Alanda bir çözümleyici belirtilmişse, [bu listeden](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) adlandırılmış bir çözümleyici olmalıdır (özel bir çözümleyici değil).<p/> En iyi uygulama olarak, yalnızca bir arama çubuğunda tamamlanmış bir dize veya açılır bırakma listesinde tamamlanmış bir dize olsun, beklenen ve uygun yanıtiçin kendilerini ödünç alanları belirtin.<p/>Bir otel adı iyi bir adaydır çünkü hassastır. Açıklamalar ve açıklamalar gibi ayrıntılı alanlar çok yoğun. Benzer şekilde, kategoriler ve etiketler gibi yinelenen alanlar daha az etkilidir. Örneklerde, birden çok alan ekleyebileceğinizi göstermek için yine de "kategori"yi ekleriz. |

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>Önerici kullanma

Bir sorguda bir önerici kullanılır. Bir önerici oluşturulduktan sonra, istediğiniz gibi arama deneyimi için aşağıdaki API'lerden birini arayın:

+ [Öneriler REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [Otomatik tamamlama REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [Otomatik TamamlamaWithHttpMessagesAsync yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

Bir arama uygulamasında, istemci kodu kısmi sorguyu toplamak ve eşleşmeyi sağlamak için [jQuery UI Autocomplete](https://jqueryui.com/autocomplete/) gibi bir kitaplılıklıkkullanmalıdır. Bu görev hakkında daha fazla bilgi için [bkz.](search-autocomplete-tutorial.md)

API kullanımı, Otomatik Tamamlama REST API'sine yapılan aşağıdaki çağrıda gösterilmiştir. Bu örnekten iki paket vardır. İlk olarak, tüm sorgularda olduğu gibi, işlem bir dizin belgeleri koleksiyonuna karşıdır ve sorgu, bu durumda kısmi sorgu sağlayan bir **arama** parametresi içerir. İkinci olarak, isteğe **suggesterName** eklemeniz gerekir. Diziniçinde bir önerici tanımlanmamışsa, otomatik tamamlama çağrısı veya öneriler başarısız olur.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2019-05-06
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>Örnek kod

+ [C# (ders 3 - You-type ara) örneğinde ilk uygulamanızı oluşturun,](tutorial-csharp-type-ahead-and-suggestions.md) önerici bir yapı, önerilen sorgular, otomatik tamamlama ve yönlü gezinmeyi gösterir. Bu kod örneği bir sandbox Azure Bilişsel Arama hizmetinde çalışır ve önceden yüklenmiş oteller dizini kullanır, bu nedenle tek yapmanız gereken uygulamayı çalıştırmak için F5 tuşuna basmaktır. Abonelik veya oturum açma gerekmez.

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) hem C# hem de Java kodu içeren eski bir örnektir. Ayrıca, düşündürücü bir yapı, önerilen sorgular, otomatik tamamlama ve yönlü gezinme yi de gösterir. Bu kod örneği barındırılan [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek verilerini kullanır. 

## <a name="next-steps"></a>Sonraki adımlar

İsteklerin nasıl formüle ediş olduğu hakkında daha fazla bilgi edinmek için aşağıdaki makaleyi öneririz.

> [!div class="nextstepaction"]
> [İstemci koduna otomatik tamamlama ve öneriler ekleme](search-autocomplete-tutorial.md) 
