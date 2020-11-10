---
title: Öneri oluşturucu oluşturma
titleSuffix: Azure Cognitive Search
description: Otomatik tamamlama veya otomatik önerilmiş sorgu koşulları çağıran istekleri öneri araçları ve formül oluşturarak Azure Bilişsel Arama 'de yazma öncesi sorgu eylemlerini etkinleştirin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: ed7b61e9e0379462e0dfbcdcc93acfccf470d95f
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427046"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>Sorgu için otomatik tamamlamayı ve önerilen sonuçları etkinleştirmek üzere bir öneri aracı oluşturun

Azure Bilişsel Arama, "yazarken arama" özelliği, bir [arama dizinine](search-what-is-an-index.md)eklenen bir **öneri aracı** yapısı aracılığıyla etkinleştirilir. Bir öneri aracı iki deneyimi destekler: *otomatik tamamlama* , bir dönem sorgusunun tamamına yönelik kısmi bir girişi tamamlar ve davet eden *öneriler* ile belirli bir eşleştirmeye kadar tıklamıştır. AutoComplete bir sorgu oluşturur. Öneriler, eşleşen bir belge oluşturur.

[C# ' de ilk uygulamanızı oluşturma bölümündeki](tutorial-csharp-type-ahead-and-suggestions.md) aşağıdaki ekran görüntüsünde her ikisi de gösterilmektedir. Otomatik tamamlama anticipates olası bir dönem, "tw" ile "ın" ile tamamlanıyor. Otel adı gibi bir alan, dizinden eşleşen bir otel arama belgesini temsil ettiğinde, öneriler mini arama sonuçlardır. Öneriler için, açıklayıcı bilgiler sağlayan herhangi bir alanı yüzeysel yapabilirsiniz.

![Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Otomatik tamamlama ve önerilen sorguların görsel karşılaştırması")

Bu özellikleri ayrı olarak veya birlikte kullanabilirsiniz. Bu davranışları Azure Bilişsel Arama uygulamak için bir dizin ve sorgu bileşeni vardır. 

+ Dizinde bir öneri aracı ekleyin. [Dizin oluşturma (REST) (/REST/api/SearchService/create-Index) veya bir [Öneri araçları özelliği](/dotnet/api/azure.search.documents.indexes.models.searchindex.suggesters)portalını kullanabilirsiniz. Bu makalenin geri kalanında bir öneri aracı oluşturmaya odaklanılmıştır.

+ Sorgu isteğinde, [aşağıda listelenen API](#how-to-use-a-suggester)'lerden birini çağırın.

İsteğe bağlı arama türü, dize alanları için alan temelinde desteklenir. Ekran görüntüsünde gösterilenle benzer bir deneyim istiyorsanız, aynı arama çözümü içinde her iki typeahead davranışını da uygulayabilirsiniz. Her iki istek de, bir Kullanıcı en az üç karakter girişi dizesi sağladıktan sonra, belirli dizin ve yanıtların *belge* koleksiyonunu hedefler.

## <a name="what-is-a-suggester"></a>Öneri aracı nedir?

Bir öneri aracı, kısmi sorgularda eşleşme için ön ekleri depolayarak, arama türü davranışlarını destekleyen bir iç veri yapısıdır. Belirteçlerin gösterildiği gibi, ön ekler, bir öneri aracı Fields koleksiyonunda belirtilen her bir alan için bir tane olmak üzere ters dizinler halinde depolanır.

## <a name="define-a-suggester"></a>Öneri aracı tanımlama

Bir öneri aracı oluşturmak için bir [dizin şemasına](/rest/api/searchservice/create-index) bir tane ekleyin ve [her bir özelliği ayarlayın](#property-reference). Bir öneri aracı oluşturmak için en iyi zaman, onu kullanacak alanı da tanımlamanız durumunda olur.

+ Yalnızca dize alanlarını kullan

+ Alanda varsayılan standart Lucene Analyzer ( `"analyzer": null` ) veya bir [dil Çözümleyicisi](index-add-language-analyzers.md) (örneğin,) kullanın `"analyzer": "en.Microsoft"`

Önceden var olan alanları kullanarak bir öneri aracı oluşturmaya çalışırsanız, API buna izin vermez. Ön ekler, iki veya daha fazla karakter kombinasyondaki kısmi koşullar tüm koşullara göre simgeleştirilir, dizin oluşturma sırasında oluşturulur. Mevcut alanlar zaten simgeleştirilmiş olduğunda, bir öneri aracı eklemek istiyorsanız dizini yeniden oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure bilişsel arama dizinini yeniden oluşturma](search-howto-reindex.md).

### <a name="choose-fields"></a>Alanları seçin

Bir öneri aracı birçok özelliğe sahip olsa da, öncelikle bir arama türü deneyiminizin etkinleştirilebileceği dize alanları koleksiyonudur. Her dizin için bir öneri aracı bulunur, bu nedenle öneri aracı listesi hem öneriler hem de otomatik tamamlama için içerik katkıda bulunan tüm alanları içermelidir.

Ek içeriğin daha fazla terim tamamlanma olasılığı olduğu için, öğesinden daha büyük bir alan için otomatik tamamlama avantajları.

Öneriler, diğer yandan, alan seçiminiz seçmeli olduğunda daha iyi sonuçlar üretir. Önerinizin bir arama belgesi için bir ara sunucu olduğunu unutmayın, böylece tek bir sonucu en iyi temsil eden alanları isteyeceksiniz. Adlar, başlıklar veya birden çok eşleşme arasında ayrım yapan diğer benzersiz alanlar en iyi şekilde çalışır. Alanlar Yinelenen değerlerden oluşur, öneriler aynı sonuçlardan oluşur ve bir Kullanıcı hangisinin tıklayabileceklerini bilmez.

Her iki arama türü deneyiminden de faydalanmak için, otomatik tamamlama için ihtiyacınız olan tüm alanları ekleyin, ancak **$Select** , **$top** , **$Filter** ve **searchfields** değerlerini kullanarak önerilerin sonuçlarını denetleyin.

### <a name="choose-analyzers"></a>Çözümleyiciler seçin

Bir çözümleyici seçiminiz, alanların nasıl simgeleştirilmiş ve daha sonra önekli olduğunu belirler. Örneğin, "bağlama duyarlı" gibi bir hecelenmiş dize için, dil Çözümleyicisi kullanmak bu belirteç kombinasyonlarına neden olur: "bağlam", "hassas", "bağlama duyarlı". Standart Lucene Çözümleyicisi 'ni kullandınız, hecelenmiş dize yok. 

Çözümleyiciler değerlendirirken, koşulların nasıl simgeleştirilmiş ve daha sonra önekli olduğunu öğrenmek için [metin ANALIZI API](/rest/api/searchservice/test-analyzer) 'sini kullanmayı göz önünde bulundurun. Bir dizin oluşturduktan sonra, belirteç çıkışını görüntülemek için bir dizedeki çeşitli Çözümleyicileri deneyebilirsiniz.

[Özel çözümleyiciler](index-add-custom-analyzers.md) veya [önceden tanımlanmış çözümleyiciler](index-add-custom-analyzers.md#predefined-analyzers-reference) kullanan alanlar (Standart Lucene hariç), zayıf sonuçları engellemek için açıkça izin verilmez.

> [!NOTE]
> Çözümleyici kısıtlamasından çalışmanız gerekiyorsa, örneğin belirli sorgu senaryolarında bir anahtar sözcük veya Ngram Çözümleyicisi gerekiyorsa, aynı içerik için iki ayrı alan kullanmanız gerekir. Bu, diğer alanlardan birinin bir öneri aracı sahip olmasını sağlar, ancak diğeri özel bir çözümleyici yapılandırmasıyla ayarlanacaktır.

## <a name="create-using-rest"></a>REST kullanarak oluşturma

REST API [Dizin oluşturma](/rest/api/searchservice/create-index) veya [güncelleştirme dizini](/rest/api/searchservice/update-index)aracılığıyla öneri araçları ekleyin. 

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

C# ' de, bir [SearchSuggester nesnesi](/dotnet/api/azure.search.documents.indexes.models.searchsuggester)tanımlayın. `Suggesters` , bir Searchındex nesnesi üzerinde bir koleksiyon, ancak yalnızca bir öğe alabilir. 

```csharp
private static void CreateIndex(string indexName, SearchIndexClient indexClient)
{
    FieldBuilder fieldBuilder = new FieldBuilder();
    var searchFields = fieldBuilder.Build(typeof(Hotel));

    //var suggester = new SearchSuggester("sg", sourceFields = "HotelName", "Category");

    var definition = new SearchIndex(indexName, searchFields);

    var suggester = new SearchSuggester("sg", new[] { "HotelName", "Category"});

    definition.Suggesters.Add(suggester);

    indexClient.CreateOrUpdateIndex(definition);
}
```

## <a name="property-reference"></a>Özellik başvurusu

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        |Öneri aracı adı.|
|`searchMode`  |Aday tümcecikleri aramak için kullanılan strateji. Şu anda desteklenen tek mod, `analyzingInfixMatching` bir terimin başlangıcında Şu anda eşleşen.|
|`sourceFields`|Önerilerin içerik kaynağı olan bir veya daha fazla alanın listesi. Alanların ve türünde olması gerekir `Edm.String` `Collection(Edm.String)` . Alanda bir çözümleyici belirtilmişse, [Bu listeden](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzername) bir adlandırılmış çözümleyici olmalıdır (özel çözümleyici değil).<p/> En iyi uygulama olarak, bir arama çubuğunda veya açılan listede bir tamamlanmış dize olup olmadığı için, yalnızca beklenen ve uygun bir yanıta ödünç veren alanları belirtin.<p/>Bir otel adı duyarlık içerdiğinden iyi bir adaydır. Açıklamalar ve açıklamalar gibi ayrıntılı alanlar çok yoğun. Benzer şekilde, Kategoriler ve Etiketler gibi yinelenen alanlar daha az etkilidir. Örneklerde, birden fazla alanı dahil etbileceğinizi göstermek için, "Category" de yer alır. |

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>Öneri aracı kullanma

Bir sorguda bir öneri aracı kullanılır. Bir öneri aracı oluşturulduktan sonra, bir arama türü deneyimi için aşağıdaki API 'lerden birini çağırın:

+ [Öneriler REST API](/rest/api/searchservice/suggestions)
+ [Otomatik tamamlama REST API](/rest/api/searchservice/autocomplete)
+ [Mümütasync yöntemi](/dotnet/api/azure.search.documents.searchclient.suggestasync)
+ [Oto Tamteasync yöntemi](/dotnet/api/azure.search.documents.searchclient.autocompleteasync)

Bir arama uygulamasında, istemci kodu, kısmi sorguyu toplamak ve eşleşmeyi sağlamak için [jQuery UI AutoComplete](https://jqueryui.com/autocomplete/) gibi bir kitaplıktan faydalanmalıdır. Bu görev hakkında daha fazla bilgi için bkz. [otomatik tamamlama veya önerilen sonuçları istemci koduna ekleme](search-autocomplete-tutorial.md).

API kullanımı, AutoComplete REST API aşağıdaki çağrısında gösterilmiştir. Bu örnekte iki örnek vardır. İlk olarak, tüm sorgularda olduğu gibi, işlem bir dizinin belgeler koleksiyonuna karşı yapılır ve sorgu, bu örnekte kısmi sorgu sağlayan bir **arama** parametresi içerir. İkinci olarak, isteğe **suggesterName** eklemeniz gerekir. Dizinde tanımlı bir öneri aracı yoksa otomatik tamamlama veya önerilere çağrı başarısız olur.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2020-06-30
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>Örnek kod

+ [C# ' de ilk uygulamanızı oluşturma (3. ders-arama](tutorial-csharp-type-ahead-and-suggestions.md) -adım-yazma) örneği bir öneri aracı yapımı, önerilen sorgular, otomatik tamamlama ve çok yönlü gezinme gösterir. Bu kod örneği, korumalı alan Azure Bilişsel Arama hizmetinde çalışır ve tüm yapmanız gerekir. böylece, uygulamayı çalıştırmak için F5 'e basın. Abonelik ya da oturum açma gerekli değildir.

## <a name="next-steps"></a>Sonraki adımlar

İsteklerin formül oluşturma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye önerilir.

> [!div class="nextstepaction"]
> [İstemci koduna otomatik tamamlama ve öneriler ekleme](search-autocomplete-tutorial.md)