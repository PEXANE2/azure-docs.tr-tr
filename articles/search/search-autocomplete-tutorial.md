---
title: Arama kutusuna otomatik tamamlama ve öneriler ekleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da, bir arama kutusunu tamamlanmış terimler veya tümceciklerle otomatik olarak tamamlayan istekler oluşturarak ve istekler oluşturarak Azure Bilişsel Arama'da istediğiniz gibi arama sorgu eylemlerini etkinleştirin. Önerilen eşleşmeleri de döndürebilirsiniz.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 1d8085c6056cb0d2541999c3e9c249cde3da8834
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641262"
---
# <a name="add-autocomplete-and-suggestions-to-client-apps"></a>İstemci uygulamalarına otomatik tamamlama ve öneriler ekleme

You-as-type ara, kullanıcı tarafından başlatılan sorguların verimliliğini artırmak için yaygın bir tekniktir. Azure Bilişsel Arama'da bu deneyim, kısmi girdiye dayalı bir terimi veya tümceciği tamamlayan *otomatik tamamlama*("mikro"yı "microsoft" ile tamamlama) ile desteklenir. Başka bir form *öneriler:* eşleşen belgelerin kısa bir listesi (bir ayrıntı sayfasına bağlantı böylece bir kimlik ile kitap başlıkları dönen). Hem otomatik tamamlama hem de öneriler dizindeki bir eşleşmeye bağlıdır. Hizmet, sıfır sonuç döndüren sorgular sunmuyor.

Azure Bilişsel Arama'da bu deneyimleri uygulamak için şunları yapmanız gerekir:

+ Arka tarafta bir *önerici.*
+ İstekte Otomatik Tamamlama veya Öneriler API'sini belirten bir *sorgu.*
+ İstemci uygulamanızda ara-as-you türü etkileşimleri işlemek için bir *Web-u II denetimi.* Bu amaçla varolan bir JavaScript kitaplığını kullanmanızı öneririz.

Azure Bilişsel Arama'da, otomatik olarak tamamlanan sorgular ve önerilen sonuçlar, bir önericiye kaydolduğunuz seçili alanlardan arama dizininden alınır. Bir önerici dizinin bir parçasıdır ve hangi alanların sorguyu tamamlayan, bir sonucu öneren veya her ikisini birden yapan içeriği sağlayacağını belirtir. Dizin oluşturulduğunda ve yüklendiğinde, kısmi sorgularda eşleştirme kığımızda kullanılan önekleri depolamak için dahili olarak bir önerileyici veri yapısı oluşturulur. Öneriler için, benzersiz veya en azından tekrarlayıcı olmayan uygun alanları seçmek deneyim için gereklidir. Daha fazla bilgi için [bkz.](index-add-suggesters.md)

Bu makalenin geri kalanı sorgular ve istemci kodu üzerinde odaklanmıştır. Önemli noktaları göstermek için JavaScript ve C# kullanır. REST API örnekleri, her işlemi kısa bir şekilde sunmak için kullanılır. Uçuça kod örneklerine bağlantılar için [Sonraki adımlara](#next-steps)bakın.

## <a name="set-up-a-request"></a>İstek ayarlama

İsteğin öğeleri api[(Otomatik TAMAMLAMA REST](https://docs.microsoft.com/rest/api/searchservice/autocomplete) veya [Öneri REST),](https://docs.microsoft.com/rest/api/searchservice/suggestions)kısmi bir sorgu ve bir önerici içerir.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2019-05-06
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

**SuggesterName,** terimleri veya önerileri tamamlamak için kullanılan öneriye duyarlı alanları sağlar. Özellikle öneriler için, alan listesi eşleşen sonuçlar arasında net seçenekler sunanlardan oluşmalıdır. Bilgisayar oyunları satan bir sitede, alan oyun başlığı olabilir.

**Arama** parametresi, karakterlerin jQuery Otomatik Tamamlama denetimi aracılığıyla sorgu isteğine aktarıldığı kısmi sorguyu sağlar. Yukarıdaki örnekte, "minecraf" denetim geçti ne olabilir statik bir örnektir.

API'ler kısmi sorguya minimum uzunluk gereksinimleri empoze etmez; bir karakter kadar küçük olabilir. Ancak, jQuery Autocomplete en az uzunlukta sağlar. En az iki veya üç karakter tipiktir.

Eşleşmeler, giriş dizesinde herhangi bir yerde bir terimin başlangıcındadır. "Hızlı kahverengi tilki" göz önüne alındığında, hem otomatik tamamlama hem de öneriler "the", "quick", "brown" veya "fox"un kısmi versiyonlarında eşleşir, ancak "rown" veya "öküz" gibi kısmi düzeltme terimleriyle eşleşmez. Ayrıca, her eşleşme aşağı genişletme için kapsamı ayarlar. "Hızlı br" kısmi bir sorgu "hızlı kahverengi" veya "hızlı ekmek" maç, ancak ne "kahverengi" ne de "ekmek" kendileri tarafından "hızlı" onlardan önce sürece maç olacaktır.

### <a name="apis"></a>API'ler

REST ve .NET SDK başvuru sayfaları için aşağıdaki bağlantıları izleyin:

+ [Öneriler REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [Otomatik tamamlama REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [Otomatik TamamlamaWithHttpMessagesAsync yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

## <a name="structure-a-response"></a>Yanıtı yapılandırma

Otomatik tamamlama ve önerileriçin yanıtlar desen için bekleyebileceğiniz şeydir: [Otomatik tamamlama,](https://docs.microsoft.com/rest/api/searchservice/autocomplete#response) belgeyi getirebilmeniz için terimlerin listesini ve belge kimliğini [döndürür](https://docs.microsoft.com/rest/api/searchservice/suggestions#response) (ayrıntı sayfası için belirli bir belgeyi almak için Arama [Belgesi](https://docs.microsoft.com/rest/api/searchservice/lookup-document) API'sini kullanın).

Yanıtlar istekteki parametrelere göre şekillenir. Otomatik tamamlama için, metin tamamlamanın bir veya iki terimde gerçekleşip gerçekleşmediğini belirlemek için [**otomatik tamamlamaMode'u**](https://docs.microsoft.com/rest/api/searchservice/autocomplete#autocomplete-modes) ayarlayın. Öneriler için seçtiğiniz alan yanıtın içeriğini belirler.

Yanıtı daha da hassaslaştırmak için, istek üzerinde daha fazla parametre ekleyin. Aşağıdaki parametreler hem Otomatik Tamamlama hem de Öneriler için geçerlidir.

| Parametre | Kullanım |
|-----------|-------|
| **$select** | Birden çok **kaynak Alanınız**varsa, hangi alanın değer`$select=GameTitle`katettiğini seçmek için **$select** kullanın ( ). |
| **$filter** | Sonuç kümesine maç ölçütleri uygulayın (`$filter=ActionAdventure`). |
| **$top** | Sonuçları belirli bir sayıyla`$top=5`sınırlandırın ( ).|

## <a name="add-user-interaction-code"></a>Kullanıcı etkileşim kodu ekleme

Sorgu terimini otomatik doldurma veya eşleşen bağlantılar listesini düşürme, azure arama bilişsel dizinine karşı otomatik tamamlama veya öneri sorguları gibi dış kaynaklardan gelen istekleri tüketebilen kullanıcı etkileşim kodu (genellikle JavaScript) gerektirir.

Bu kodu yerel olarak yazabilmekle birlikte, varolan JavaScript kitaplığından işlevleri kullanmak çok daha kolaydır. Bu makalede, biri öneriler için, diğeri otomatik tamamlama için olmak üzere iki tane gösterilmiştir. 

+ Öneri örneğinde [otomatik tamamlama widget'ı (jQuery UI)](https://jqueryui.com/autocomplete/) kullanılır. Bir arama kutusu oluşturabilir ve ardından Otomatik Tamamlama widget'ını kullanan bir JavaScript işlevinde başvuruyapabilirsiniz. Widget'taki özellikler kaynağı (otomatik tamamlama veya öneriler işlevi), işlem başlamadan önce giriş karakterlerinin minimum uzunluğunu ve konumlandırmayı ayarlar.

+ [XDSoft Autocomplete eklentisi](https://xdsoft.net/jqplugins/autocomplete/) Otomatik Tamamlama örneği kullanılır.

Bu kitaplıkları hem önerileri hem de otomatik tamamlamayı destekleyen arama kutusunu oluşturmak için kullanırız. Arama kutusunda toplanan girişler öneriler ve otomatik tamamlama eylemleri ile eşlenir.

## <a name="suggestions"></a>Öneriler

Bu bölüm, arama kutusu tanımından başlayarak önerilen sonuçların bir uygulaması boyunca size yol gösterin. Ayrıca, bu makalede başvurulan ilk JavaScript otomatik tamamlama kitaplığını çağıran komut dosyasının nasıl ve nasıl olduğunu da gösterir.

### <a name="create-a-search-box"></a>Arama kutusu oluşturma

[jQuery UI Autocomplete kitaplığını](https://jqueryui.com/autocomplete/) ve C#'daki bir MVC projesini varsayarsak, **Index.cshtml** dosyasındaki JavaScript'i kullanarak arama kutusunu tanımlayabilirsiniz. Kitaplık, önerileri almak için MVC denetleyicisine eşzamanlı çağrılar yaparak arama kutusuna istediğiniz gibi arama etkileşimi ekler.

\Views\Home klasörü altında **Index.cshtml'de,** arama kutusu oluşturmak için bir satır aşağıdaki gibi olabilir:

```html
<input class="searchBox" type="text" id="searchbox1" placeholder="search">
```

Bu örnek, stil için bir sınıf, JavaScript tarafından başvurulacak bir kimlik ve yer tutucu metin içeren basit bir giriş metin kutusudur.  

Aynı dosya içinde, arama kutusuna başvuran JavaScript'i gömün. Aşağıdaki işlev, kısmi terim girişlerine dayalı eşleşen belgeleri öner API'yi çağırır:

```javascript
$(function () {
    $("#searchbox1").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

JQuery `source` UI Otomatik Tamamlama işlevini, arama kutusunun altında gösterilen öğelerin listesini nereden alacağını söyler. Bu proje bir MVC projesi olduğundan, sorgu önerilerini döndürme mantığını içeren **HomeController.cs'daki** **Öner** işlevini çağırır. Bu işlev, vurguları, bulanık eşleştirmeyi ve terimi denetlemek için birkaç parametreyi de geçer. Otomatik tamamlama JavaScript API terimi parametresi ekler.

Önerilerin `minLength: 3` yalnızca arama kutusunda en az üç karakter olduğunda gösterilmesini sağlar.

### <a name="enable-fuzzy-matching"></a>Bulanık eşleştirmeyi etkinleştirme

Benzer öğe araması, kullanıcı bir kelimeyi arama kutusuna yanlış yazsa bile yakın eşleşmelere göre sonuçlara ulaşmanızı sağlar. Edinme mesafesi 1'dir, bu da kullanıcı girişi ile eşleşme arasında bir karakterde maksimum tutarsızlık olabileceği anlamına gelir. 

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

### <a name="enable-highlighting"></a>Vurgulamayı etkinleştirme

Vurgulama, girişe karşılık gelen sonuçtaki karakterlere yazı tipi stili uygular. Örneğin, kısmi giriş "mikro" ise, sonuç **mikro**yumuşak, **mikro**kapsam ve benzeri olarak görünür. Vurgulama, Öneri işleviyle birlikte sırayla tanımlanan HighlightPreTag ve HighlightPostTag parametrelerini temel alınarak belirlenir.

```javascript
source: "/home/suggest?highlights=true&fuzzy=true&",
```

### <a name="suggest-function"></a>Önerme fonksiyonu

C# ve MVC uygulaması kullanıyorsanız, Denetleyiciler dizininin altındaki **HomeController.cs** dosya, önerilen sonuçlar için bir sınıf oluşturabileceğiniz yerdir. .NET'te Bir Önerme işlevi [DocumentsOperationsExtensions.Suggest yöntemini](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet)temel alıntır.

Yöntem, `InitSearch` Azure Bilişsel Arama hizmetinde kimlik doğrulaması yapılan bir HTTP dizin istemcisi oluşturur. .NET SDK hakkında daha fazla bilgi için [,.NET Uygulamasından Azure Bilişsel Arama'yı nasıl kullanacağınız](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)bilgisini agörün.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

Suggest işlevi, arama terimi girişine ek olarak isabet vurgularının veya benzer öğe eşleştirme özelliğinin kullanılıp kullanılmadığını belirleyen iki parametre alır. Yöntem, daha sonra API Öner'e geçirilen bir [SuggestParametreler nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet)oluşturur. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

## <a name="autocomplete"></a>Otomatik Tamamlama

Şimdiye kadar, arama UX kodu öneriler üzerinde ortalanmıştır. Bir sonraki kod bloğu, XDSoft jQuery UI Autocomplete işlevini kullanarak Azure Bilişsel Arama otomatik tamamlama isteğini ileterek otomatik tamamlamayı gösterir. Önerilerde olduğu gibi, C# uygulamasında kullanıcı etkileşimini destekleyen kod **index.cshtml'de**gider.

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 https://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#searchbox1").autocompleteInline({
        appendMethod: "replace",
        source: [
            function (text, add) {
                if (!text) {
                    return;
                }

                $.getJSON("/home/autocomplete?term=" + text, function (data) {
                    if (data && data.length > 0) {
                        currentSuggestion2 = data[0];
                        add(data);
                    }
                });
            }
        ]
    });

    // complete on TAB and clear on ESC
    $("#searchbox1").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#searchbox1").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#searchbox1").val("");
        }
    });
});
```

### <a name="autocomplete-function"></a>Otomatik tamamlama fonksiyonu

Otomatik tamamlama [DocumentsOperationsExtensions.Autocomplete yöntemini temel alınr.](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet) Önerilerde olduğu gibi, bu kod bloğu **HomeController.cs** dosyasına gider.

```csharp
public ActionResult AutoComplete(string term)
{
    InitSearch();
    //Call autocomplete API and return results
    AutocompleteParameters ap = new AutocompleteParameters()
    {
        AutocompleteMode = AutocompleteMode.OneTermWithContext,
        UseFuzzyMatching = false,
        Top = 5
    };
    AutocompleteResult autocompleteResult = _indexClient.Documents.Autocomplete(term, "sg", ap);

    // Convert the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

Otomatik Tamamlama işlevi arama terimi girdisini alır. Yöntem bir [Otomatik Tamamlama Parametreleri nesnesi](https://docs.microsoft.com/rest/api/searchservice/autocomplete)oluşturur. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

## <a name="next-steps"></a>Sonraki adımlar

Uçuça yönergeleri veya her iki arama türüyle ilgili deneyimleri gösteren kod için bu bağlantıları izleyin. Her iki kod örneği de önerilerin karma uygulamalarını içerir ve birlikte otomatik tamamlamayı içerir.

+ [Öğretici: C# (ders 3) ilk uygulamanızı oluşturun](tutorial-csharp-type-ahead-and-suggestions.md)
+ [C# kod örneği: azure-search-dotnet-samples/create-first-app/3-add-typeahead/](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/3-add-typeahead)
+ [REST yan yana kod örneği ile C# ve JavaScript](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete)