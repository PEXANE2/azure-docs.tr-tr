---
title: Arama kutusuna otomatik tamamlama ve öneriler ekleme
titleSuffix: Azure Cognitive Search
description: Öneri araçları oluşturarak ve bir arama kutusunu tamamlanmış koşullara veya tümceciklere göre otomatik hale getirerek oluşan istekleri formülleyerek Azure Bilişsel Arama 'de arama yaparken arama eylemlerini etkinleştirin. Ayrıca, önerilen eşleşmeleri de döndürebilirsiniz.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/15/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: be873ed122bb521ce00e2d18d55a9be8197a0048
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936767"
---
# <a name="add-autocomplete-and-suggestions-to-client-apps"></a>İstemci uygulamalarına otomatik tamamlama ve öneriler ekleme

Yazarken ara, Kullanıcı tarafından başlatılan sorguların üretkenliğini iyileştirmeye yönelik ortak bir tekniktir. Azure Bilişsel Arama, bu deneyim kısmi girişe dayalı bir terimi veya tümceciği tamamlayan *AutoComplete*aracılığıyla desteklenir ("Micro" ile "Microsoft" arasında tamamlanır). Başka bir form *öneriler*: eşleşen belgelerin kısa bir listesi (ayrıntı sayfasına bağlantı yapabilmeniz IÇIN bir kimliği olan kitap başlıkları döndürülüyor). Hem otomatik tamamlama hem de öneriler dizindeki bir eşleşmeden tahmin edilir. Hizmet, sıfır sonuç döndüren sorgular sunmaz.

Bu deneyimleri Azure Bilişsel Arama uygulamak için şunlar gerekir:

+ Arka uçta bir *öneri aracı* .
+ İstekte [otomatik tamamlama](/rest/api/searchservice/autocomplete) veya [öneriler](/rest/api/searchservice/suggestions) API 'si belirten bir *sorgu* .
+ İstemci uygulamanızda arama-yazma etkileşimini işlemek için bir *Kullanıcı arabirimi denetimi* . Bu amaçla mevcut bir JavaScript kitaplığı kullanmanızı öneririz.

Azure Bilişsel Arama 'de, bir öneri aracı kaydettiğiniz seçili alanlardan, oto tamamlanmış sorgular ve önerilen sonuçlar arama dizininden alınır. Bir öneri aracı, dizinin bir parçasıdır ve bir sorguyu tamamlayan, bir sonuç öneren veya her ikisini de yapan içeriği belirtir. Dizin oluşturulup yüklendiğinde, kısmi sorgularda eşleşme için kullanılan önekleri depolamak üzere dahili olarak bir öneri aracı veri yapısı oluşturulur. Öneriler için, deneyim için benzersiz olan veya en az tekrarlanmayan uygun alanları seçme için gereklidir. Daha fazla bilgi için bkz. [Create a öneri aracı](index-add-suggesters.md).

Bu makalenin geri kalanı sorgulara ve istemci koduna odaklanılmıştır. Anahtar noktalarını göstermek Için JavaScript ve C# kullanır. REST API örnekleri, her işlemi öz için kullanılır. Uçtan uca kod örneklerine bağlantılar için bkz. [sonraki adımlar](#next-steps).

## <a name="set-up-a-request"></a>İstek ayarlama

Bir isteğin öğeleri, arama türü API 'Lerinden, kısmi bir sorgudan ve bir öneri aracı oluşur. Aşağıdaki betik, bir isteğin bileşenlerini bir örnek olarak otomatik tamamlama REST API kullanarak gösterir.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2020-06-30
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

**SuggesterName** , hüküm veya önerileri tamamlamaya yönelik olarak kullanılan öneri aracı uyumlu alanları sağlar. Özellikle ilgili öneriler için, alan listesi, eşleşen sonuçlar arasında açık seçimler sunan olanlardan oluşmalıdır. Bilgisayar oyunları satan bir sitede, alan oyun başlığı olabilir.

**Arama** parametresi, bir sorgu Isteğine jQuery AutoComplete denetimi aracılığıyla bir karakter beslendiği kısmi sorguyu sağlar. Yukarıdaki örnekte, "minecraf" denetimin ne şekilde geçtiğini gösteren statik bir örnektir.

API 'Ler kısmi sorguda minimum uzunluk gereksinimlerini karşılamıyor; bir karakter çok az olabilir. Ancak jQuery AutoComplete, en az bir uzunluk sağlar. En az iki veya üç karakter normaldir.

Eşleşmeler, giriş dizesinin herhangi bir yerindeki bir terim başlangıcıdır. "Hızlı kahverengi Fox" verildiğinde, hem otomatik tamamlama hem de öneriler "rown" veya "Ox" gibi kısmi bir not, "yeni", "hızlı", "kahverengi" veya "Fox" gibi kısmi sürümlerde eşleşir. Ayrıca, her eşleşme aşağı akış genişleimleri için kapsamı ayarlar. "Hızlı" veya "hızlı içerik" için kısmi bir sorgu "hızlı kahverengi" veya "hızlı içerik" ile eşleşir, ancak "hızlı" işaretinden önce "kahverengi" veya "içerik" eşleşmesi gerekir.

### <a name="apis-for-search-as-you-type"></a>Yazarken arama için API 'Ler

REST ve .NET SDK başvuru sayfaları için bu bağlantıları izleyin:

+ [Öneriler REST API](/rest/api/searchservice/suggestions) 
+ [Otomatik tamamlama REST API](/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync yöntemi](/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [AutocompleteWithHttpMessagesAsync yöntemi](/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

## <a name="structure-a-response"></a>Yanıt yapısı

Otomatik tamamlama ve önerilerle ilgili yanıtlar, bu model için beklediğiniz şeydir: [otomatik tamamlama](/rest/api/searchservice/autocomplete#response) bir terim listesi döndürüyor, [ÖNERILER](/rest/api/searchservice/suggestions#response) bir belge kimliği ve belgeyi alabilmeniz için (belirli bir belgeyi bir ayrıntı sayfasına getirmek üzere [Arama belgesi](/rest/api/searchservice/lookup-document) API 'sini kullanın) sağlar.

Yanıtlar istekteki parametrelere göre şekillendirilir. Otomatik tamamlama için, bir veya iki terimde metin tamamlanmasının yapılıp yapılmayacağını öğrenmek için [**autocompleteMode**](/rest/api/searchservice/autocomplete#autocomplete-modes) ayarlayın. Öneriler için, seçtiğiniz alan yanıtın içeriğini belirler.

Öneriler için, yinelemeleri önlemek için yanıtı daha da belirginleştirin veya ilişkisiz sonuçlar olarak görünür. Sonuçları denetlemek için, isteğe daha fazla parametre ekleyin. Aşağıdaki parametreler hem otomatik tamamlama hem de öneriler için geçerlidir, ancak özellikle de bir öneri aracı birden çok alan içerdiğinde öneriler için daha fazla gerekli olabilir.

| Parametre | Kullanım |
|-----------|-------|
| **$select** | Bir öneri aracı içinde birden çok **Sourcefields** varsa, hangi alanın () değer katkıda bulunduğunu seçmek için **$Select** kullanın `$select=GameTitle` . |
| **searchFields** | Sorguyu belirli alanlarla sınırlayın. |
| **$filter** | Sonuç kümesinde () eşleşme ölçütlerini uygulayın `$filter=Category eq 'ActionAdventure'` . |
| **$top** | Sonuçları belirli bir sayıyla ( `$top=5` ) sınırlayın.|

## <a name="add-user-interaction-code"></a>Kullanıcı etkileşimi kodu ekle

Bir sorgu terimini otomatik olarak doldurma veya eşleşen bağlantıların bir listesini bırakma, Kullanıcı etkileşim kodu, tipik olarak JavaScript, bir Azure Search bilişsel dizine karşı otomatik tamamlama veya öneri sorguları gibi dış kaynaklardan gelen istekleri kullanabilen Kullanıcı etkileşimi kodu gerektirir.

Bu kodu yerel olarak yazabilseniz de, mevcut JavaScript kitaplığındaki işlevleri kullanmak çok daha kolay. Bu makalede, bir, öneriler için bir diğeri ve otomatik tamamlama için bir diğeri gösterilmektedir. 

+ [Otomatik tamamlama pencere öğesi (jQuery kullanıcı arabirimi)](https://jqueryui.com/autocomplete/) , öneri örneğinde kullanılır. Bir arama kutusu oluşturabilir ve sonra otomatik tamamlama pencere öğesini kullanan bir JavaScript işlevinde buna başvurabilirsiniz. Pencere öğesi üzerindeki özellikler, kaynak (otomatik tamamlama veya öneriler işlevi), eylem yapılmadan önce en az giriş karakteri uzunluğu ve konumlandırmayı ayarlar.

+ [Xdsoft AutoComplete eklentisi](https://xdsoft.net/jqplugins/autocomplete/) , AutoComplete örneğini kullanır.

Bu kitaplıkları, hem önerileri hem de otomatik tamamlamayı destekleyen arama kutusunu oluşturmak için kullanırız. Arama kutusunda toplanan girişler, öneriler ve otomatik tamamlama eylemleriyle eşleştirilir.

## <a name="suggestions"></a>Öneriler

Bu bölüm, arama kutusu tanımıyla başlayarak önerilen sonuçların bir uygulamasında size kılavuzluk eder. Ayrıca, bu makalede başvurulan ilk JavaScript otomatik tamamlama kitaplığını çağıran nasıl ve betiğin olduğunu gösterir.

### <a name="create-a-search-box"></a>Arama kutusu oluşturma

[JQuery kullanıcı arabirimi otomatik tamamlama kitaplığı](https://jqueryui.com/autocomplete/) ve C# ' de MVC projesi varsayıldığında, **Dizin. cshtml** dosyasında JavaScript kullanarak arama kutusunu tanımlayabilirsiniz. Bu kitaplık, önerileri almak için MVC denetleyicisine zaman uyumsuz çağrılar yaparak arama kutusuna arama yapmak için arama yapma etkileşimini ekler.

\Views\Home klasörünün altındaki **Index. cshtml** dosyasında, arama kutusu oluşturmak için bir satır aşağıdaki gibi olabilir:

```html
<input class="searchBox" type="text" id="searchbox1" placeholder="search">
```

Bu örnek, stil, JavaScript ve yer tutucu metni için bir sınıf olan basit bir giriş metin kutusudur.  

Aynı dosya içinde, arama kutusuna başvuran JavaScript 'ı ekleyin. Aşağıdaki işlev, kısmi dönem girdilerine göre önerilen eşleşen belgeleri isteyen önerme API 'sini çağırır:

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

`source`JQuery kullanıcı arabirimi otomatik tamamlama işlevine, arama kutusu altında gösterilecek öğelerin listesinin nereden alınacağını söyler. Bu proje bir MVC projesi olduğundan, sorgu önerilerini döndürme mantığını içeren **HomeController.cs** içinde **önerme** işlevini çağırır. Bu işlev, vurguları, belirsiz eşleştirmeyi ve terimi denetlemek için birkaç parametreyi de geçirir. AutoComplete JavaScript API 'SI, term parametresini ekler.

, `minLength: 3` Arama kutusunda yalnızca en az üç karakter olduğunda önerilerin gösterilmesi sağlanır.

### <a name="enable-fuzzy-matching"></a>Benzer eşleştirmeyi etkinleştir

Benzer öğe araması, kullanıcı bir kelimeyi arama kutusuna yanlış yazsa bile yakın eşleşmelere göre sonuçlara ulaşmanızı sağlar. Düzenleme uzaklığı 1 ' dir. Bu, Kullanıcı girişi ile eşleşme arasında bir karakter üzerinde en fazla tutarsızlık olabileceği anlamına gelir. 

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

### <a name="enable-highlighting"></a>Vurgulamayı etkinleştir

Vurgulama, girilen yazı tipi stilini, girişe karşılık gelen sonuçdaki karakterlere uygular. Örneğin, kısmi giriş "mikro" ise, sonuç **mikro**Soft, **mikro**kapsam vs. olarak görünür. Vurgulama, öneri işleviyle satır içi olarak tanımlanmış HighlightPreTag ve HighlightPostTag parametrelerini temel alır.

```javascript
source: "/home/suggest?highlights=true&fuzzy=true&",
```

### <a name="suggest-function"></a>Öner işlevi

C# ve MVC uygulaması kullanıyorsanız, **HomeController.cs** dosyası denetleyiciler dizini altında, önerilen sonuçlar için bir sınıf oluşturabileceğiniz yerdir. .NET ' te, bir önerme işlevi [Documentsoperationsextensions. önerme metodunu](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet)temel alır.

`InitSearch`Yöntemi, Azure bilişsel arama hizmetine kimliği doğrulanmış BIR http Dizin istemcisi oluşturur. .NET SDK hakkında daha fazla bilgi için bkz. [.NET uygulamasından Azure bilişsel arama kullanma](./search-howto-dotnet-sdk.md).

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        Select = HotelName,
        SearchFields = HotelName,
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

Suggest işlevi, arama terimi girişine ek olarak isabet vurgularının veya benzer öğe eşleştirme özelliğinin kullanılıp kullanılmadığını belirleyen iki parametre alır. Yöntemi, daha sonra öneri API 'sine geçirilen bir [SuggestParameters nesnesi](/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet)oluşturur. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

## <a name="autocomplete"></a>Otomatik Tamamlama

Şimdiye kadar, arama UX kodu önerilerle ortalandı. Sonraki kod bloğunda, Azure Bilişsel Arama AutoComplete için bir istek geçirerek XDSoft jQuery UI AutoComplete işlevini kullanarak AutoComplete gösterilmektedir. Önerilerle birlikte, bir C# uygulamasında, kullanıcı etkileşimini destekleyen kod **index. cshtml**'ye gider.

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

### <a name="autocomplete-function"></a>AutoComplete işlevi

AutoComplete, [Documentsoperationsextensions. AutoComplete yöntemine](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet)göre belirlenir. Önerilerle birlikte bu kod bloğu **HomeController.cs** dosyasına gidebilirler.

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

AutoComplete işlevi, arama terimi girişini alır. Yöntemi bir [AutoCompleteParameters nesnesi](/rest/api/searchservice/autocomplete)oluşturur. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca yönergeler veya hem arama hem de arama-yazma deneyimlerini gösteren kod için bu bağlantıları izleyin. Her iki kod örneği de önerilerin karma uygulamalarını ve birlikte otomatik tamamlamayı içerir.

+ [Öğretici: C# ' de ilk uygulamanızı oluşturma (Ders 3)](tutorial-csharp-type-ahead-and-suggestions.md)
+ [C# kod örneği: Azure-Search-DotNet-Samples/Create-First-App/3-Add-typeahead/](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/3-add-typeahead)
+ [REST yan yana kod örneği ile C# ve JavaScript](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete)