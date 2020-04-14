---
title: Arama kutusuna otomatik tamamlama ve öneriler ekleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da, bir arama kutusunu tamamlanmış terimler veya tümceciklerle otomatik olarak tamamlayan istekler oluşturarak ve istekler oluşturarak Azure Bilişsel Arama'da istediğiniz gibi arama sorgu eylemlerini etkinleştirin. Önerilen eşleşmeleri de döndürebilirsiniz.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/10/2020
ms.openlocfilehash: 8b64a583c11e794c30e1de12eb66941874a25462
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262238"
---
# <a name="add-suggestions-or-autocomplete-to-your-azure-cognitive-search-application"></a>Azure Bilişsel Arama uygulamanıza öneri veya otomatik tamamlama ekleme

Bu örnek, ara-as-you türü davranışları destekleyen bir arama kutusu gösterir. Birlikte veya ayrı ayrı kullanabileceğiniz iki özellik vardır:

+ *Öneriler,* siz yazarken, her önerinin şimdiye kadar yazdıklarınla eşleşen dizinden tek bir sonuç veya arama belgesi olduğu arama sonuçları oluşturur. 

+ *Otomatik tamamlama* sözcüğü veya tümceciği "bitirerek" sorgular oluşturur. Sonuçları döndürmek yerine, sonuçları döndürmek için yürütebileceğiniz bir sorgu tamamlar. Önerilerde olduğu gibi, sorgudaki tamamlanmış bir sözcük veya tümcecik dizindeki bir eşleşmeye göre yapılır. Hizmet, dizinde sıfır sonuç döndüren sorgular sunmuyor.

Örnek kod hem C# hem de JavaScript dil sürümlerinde hem önerileri hem de otomatik tamamlamayı gösterir. 

C# [geliştiricileri, Azure Bilişsel Arama .NET SDK'yı](https://aka.ms/search-sdk)kullanan ASP.NET MVC tabanlı bir uygulamadan geçebilir. Otomatik tamamlama ve önerilen sorgu çağrıları yapma mantığı HomeController.cs dosyasında bulunabilir. 

JavaScript geliştiricileri, [Azure Bilişsel Arama REST API'sine](https://docs.microsoft.com/rest/api/searchservice/)doğrudan çağrılar içeren IndexJavaScript.cshtml'de eşdeğer sorgu mantığı bulacaklar. 

Her iki dil sürümü için de ön uç kullanıcı deneyimi [jQuery UI](https://jqueryui.com/autocomplete/) ve [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) kitaplıklarını temel alır. Bu kitaplıkları hem önerileri hem de otomatik tamamlamayı destekleyen arama kutusunu oluşturmak için kullanırız. Arama kutusunda toplanan girişler, HomeController.cs veya IndexJavaScript.cshtml'de tanımlandığı gibi öneriler ve otomatik tamamlama eylemleriyle eşlenir.

## <a name="prerequisites"></a>Ön koşullar

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)

Çözüm barındırılan bir hizmet ve NYCJobs demo dizinini kullandığından, azure bilişsel arama hizmeti bu alıştırma için isteğe bağlıdır. Bu dizini kendi arama hizmetinizde oluşturmak istiyorsanız, yönergeler için [NYC İşdizini Oluştur'a](#configure-app) bakın. Aksi takdirde, bir JavaScript istemci uygulamasını yedeklemek için varolan hizmeti ve dizini kullanabilirsiniz.

<!-- The sample is comprehensive, covering suggestions, autocomplete, faceted navigation, and client-side caching. Review the readme and comments for a full description of what the sample offers. -->

## <a name="download-files"></a>Dosyaları indirme

Hem C# hem de JavaScript geliştiricileri için örnek kod, **Azure Örnekleri/search-dotnet-getting-started** GitHub deposunun [DotNetHowToAutoComplete klasöründe](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) bulunabilir.

Örnek, mevcut bir demo arama hizmetini ve [NYCJobs demo verileriyle](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)dolu önceden oluşturulmuş bir dizini hedefler. NYCJobs dizini, öneriler veya otomatik tamamlama kullanmak için bir gereklilik olan Bir [Önerici yapısı](index-add-suggesters.md)içerir.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Visual **Studio'da AutocompleteTutorial.sln'yi** açın. Çözüm, varolan bir arama hizmeti ne ve dizin bağlantısı olan ASP.NET bir MVC projesi içerir.

1. NuGet Paketlerini Güncelleyin:

   1. Solution Explorer'da **DotNetHowToAutoComplete'e** sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin.  
   1. **Güncellemeler** sekmesini seçin, tüm paketleri seçin ve **Güncelleştir'i**tıklatın. Lisans sözleşmelerini kabul edin. Tüm paketleri güncelleştirmek için birden fazla geçiş gerekebilir.

1. Projeyi çalıştırmak ve sayfayı tarayıcıya yüklemek için F5 tuşuna basın.

En üstte C# ve JavaScript seçeneklerini göreceksiniz. C# seçeneği tarayıcıdan HomeController'ı arar ve sonuçları almak için Azure Bilişsel Arama .NET SDK'yı kullanır. 

JavaScript seçeneği, Azure Bilişsel Arama REST API'sini doğrudan tarayıcıdan çağırır. Denetleyiciyi akıştan çıkardığından, bu seçenek genellikle belirgin bir şekilde daha iyi bir performansa sahip olur. İhtiyaçlarınıza ve dil tercihlerinize uygun seçeneği tercih edebilirsiniz. Sayfada her biri için bazı kılavuzlar içeren birkaç otomatik tamamlama örneği vardır. Her örnekte deneyebileceğiniz önerilen örnek metin vardır.  

![Örnek başlangıç sayfası](media/search-autocomplete-tutorial/startup-page.png "Localhost'ta örnek başlangıç sayfası")

Gerçekleştirilen işlemleri görmek için her arama kutusuna birkaç harf yazmayı deneyin.

## <a name="query-inputs"></a>Sorgu girişleri

Hem C# hem de JavaScript sürümleriiçin arama kutusu uygulaması tamamen aynıdır. 

Kodu görüntülemek için \Views\Home klasörü altında **Index.cshtml** dosyasını açın:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Bu örnek, stil için bir sınıf, JavaScript tarafından başvurulacak bir kimlik ve yer tutucu metin içeren basit bir giriş metin kutusudur.  Sihirli gömülü JavaScript bulunmaktadır.

C# dil [örneği, jQuery UI Autocomplete kitaplığını](https://jqueryui.com/autocomplete/)sağlamak için Index.cshtml'de JavaScript kullanır. Bu kitaplık, önerileri almak için MVC denetleyicisine eşzamanlı çağrılar yaparak arama kutusuna otomatik tamamlama deneyimi ekler. JavaScript dil sürümü IndexJavaScript.cshtml bulunmaktadır. Arama çubuğu nun altındaki komut dosyasının yanı sıra Azure Bilişsel Arama'ya REST API çağrıları içerir.

JQuery UI Autocomplete işlevini çağıran ilk örnek için JavaScript koduna bakalım, öneriler için bir istek geçen:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Yukarıdaki kod "example1a" giriş kutusu için jQuery UI otomatik tamamlama yapılandırmak için sayfa yükü tarayıcıda çalışır.  `minLength: 3`, önerilerin yalnızca arama kutusuna en az üç karakter girildiğinde gösterilmesini sağlar.  Kaynak değeri önemlidir:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Yukarıdaki satır, arama kutusunun altında gösterilen öğelerin listesini nereden alacağınız jQuery UI Otomatik Tamamlama işlevini bildirir. Bu proje bir MVC projesi olduğundan, sorgu önerilerini döndürme mantığını içeren HomeController.cs'daki Öner işlevini çağırır (sonraki bölümde Öner hakkında daha fazla bilgi). Bu işlev, vurguları, bulanık eşleştirmeyi ve terimi denetlemek için birkaç parametreyi de geçer. Otomatik tamamlama JavaScript API terimi parametresi ekler.

### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Örneği benzer öğe eşleştirmeyi destekleyecek şekilde genişletme

Benzer öğe araması, kullanıcı bir kelimeyi arama kutusuna yanlış yazsa bile yakın eşleşmelere göre sonuçlara ulaşmanızı sağlar. Gerekli olmamakla birlikte, bir typeahead deneyiminin sağlamlığını önemli ölçüde artırır. Şimdi kaynak satırını benzer öğe eşleştirmeyi etkinleştirecek şekilde değiştirerek bir deneme yapalım.

Aşağıdaki satırı değiştirin:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

şu şekilde:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

F5 tuşuna basarak uygulamayı başlatın.

"execative" gibi bir terim yazmayı deneyin. Yazdığınız harflerle tam olarak eşleşmediği halde "executive" terimi sonuçlarının döndürüldüğünü göreceksiniz.

### <a name="jquery-autocomplete--backed-by-azure-cognitive-search-autocomplete"></a>jQuery Otomatik Tamamlama Azure Bilişsel Arama otomatik tamamlama tarafından desteklenen

Şimdiye kadar, arama UX kodu öneriler üzerinde ortalanmıştır. Sonraki kod bloğu, Azure Bilişsel Arama otomatik tamamlama isteğinde geçen jQuery UI Otomatik Tamamlama işlevini (index.cshtml'deki satır 91) gösterir:

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 https://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#example2").autocompleteInline({
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
    $("#example2").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#example2").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#example2").val("");
        }
    });
});
```

## <a name="c-example"></a>C# örneği

Artık web sayfasının JavaScript kodunu gözden geçirdiğimize göre, Önerilen eşleşmeleri Azure Bilişsel Arama .NET SDK'yı kullanarak gerçekten alan C# sunucu tarafı denetleyici koduna bakalım.

Denetleyiciler dizininin altındaki **HomeController.cs** dosyasını açın. 

Fark edebilirsiniz ilk şey sınıfın üst kısmında bir `InitSearch`yöntemdir . Bu yöntem, Azure Bilişsel Arama hizmetinde kimlik doğrulaması yapılan bir HTTP dizin istemcisi oluşturur. Daha fazla bilgi için [,.NET Uygulamasından Azure Bilişsel Arama'yı nasıl kullanacağınız](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)abakın.

Satır 41'de Öner işlevine dikkat edin. [Bu DocumentsOperationsExtensions.Suggest yöntemidayanmaktadır.](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet)

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

Satır 69'da Otomatik Tamamlama işlevine dikkat edin. [Bu DocumentsOperationsExtensions.Autocomplete yöntemidayanmaktadır.](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet)

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

    // Conver the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

Otomatik Tamamlama işlevi arama terimi girdisini alır. Yöntem bir [Otomatik Tamamlama Parametreleri nesnesi](https://docs.microsoft.com/rest/api/searchservice/autocomplete)oluşturur. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

(İsteğe bağlı) Suggest işlevinin başlangıcına bir kesme noktası ekleyin ve kodu adım adım inceleyin. SDK tarafından döndürülen yanıta ve yöntemden döndürülen sonuca nasıl dönüştürüldüğünü fark edin.

Sayfadaki diğer örnekler, otomatik tamamlama sonuçlarının istemci tarafı önbelleğe alınmış olmasını desteklemek için isabet vurgulama ve yüzeklemek için aynı deseni izler. Her birini gözden geçirerek nasıl çalıştıklarını ve arama deneyiminizde nasıl kullanabileceğinizi görebilirsiniz.

## <a name="javascript-example"></a>JavaScript örneği

Otomatik tamamlama ve önerilerin JavaScript uygulaması, dizini ve işlemi belirtmek için kaynak olarak URI kullanarak REST API'yi çağırır. 

JavaScript uygulamasını incelemek için **IndexJavaScript.cshtml'i**açın. JQuery UI Otomatik Tamamlama işlevinin arama kutusu için de kullanıldığına dikkat edin, arama terimi girişlerini toplayın ve önerilen eşleşmeleri veya tamamlanmış terimleri almak için Azure Bilişsel Arama'ya eşzamanlı çağrılar yapın. 

İlk örneğin JavaScript koduna bakalım:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Bu örneği, Ana Sayfa denetleyicisini çağıran yukarıdaki örnekle karşılaştırırsanız, birkaç benzerlik fark esiniz.  Için otomatik tamamlama `minLength` `position` yapılandırması ve tam olarak aynıdır. 

Buradaki önemli değişiklik kaynaktır. Ev denetleyicisinde Öner yöntemini aramak yerine, Bir JavaScript işlevinde BIR REST isteği oluşturulur ve Ajax kullanılarak yürütülür. Ardından gelen yanıt "success" parametresinde işlenir ve kaynak olarak kullanılır.

REST aramaları, [Otomatik Tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) veya [Öneriler](https://docs.microsoft.com/rest/api/searchservice/suggestions) API çağrısının yapılıp yapılmadığını belirtmek için URI'leri kullanır. Aşağıdaki URI'ler sırasıyla 9 ve 10 satırlarında dır.

```javascript
var suggestUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/suggest?api-version=" + apiVersion;
var autocompleteUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/autocomplete?api-version=" + apiVersion;
```

Satır 148'de, ". `autocompleteUri` İlk arama `suggestUri` 39 numaralı hatta.

> [!Note]
> JavaScript'teki hizmete REST çağrıları yapmak, REST API'sinin uygun bir gösterimi olarak burada sunulur, ancak en iyi uygulama veya öneri olarak yorumlanmamalıdır. Bir KOMUT dosyasına bir API anahtarı ve bitiş noktasının eklenmesi, hizmetinizi, komut dosyasındaki bu değerleri okuyabilen herkese hizmet reddi saldırılarına açar. JavaScript'i öğrenme amacıyla, belki de ücretsiz hizmette barındırılan dizinlerde kullanmak güvenli olsa da, üretim kodundaki dizin oluşturma ve sorgulama işlemleri için Java veya C# kullanmanızı öneririz. 

<a name="configure-app"></a>

## <a name="create-an-nycjobs-index"></a>NYCJobs dizini oluşturma

Şimdiye kadar, barındırılan NYCJobs demo dizini kullanıyorsunuz. Dizin de dahil olmak üzere kodun tümünün tam görünürlüğünü istiyorsanız, dizini kendi arama hizmetinizde oluşturmak ve yüklemek için bu yönergeleri izleyin.

1. [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu örnek için ücretsiz bir hizmet kullanabilirsiniz. 

   > [!Note]
   > Ücretsiz Azure Bilişsel Arama hizmetini kullanıyorsanız, üç dizinle sınırlıdırsınız. NYCJobs veri yükleyici iki dizin oluşturur. Hizmetinizde yeni dizinleri kabul edecek kadar yer olduğundan emin olun.

1. [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek kodunu indirin.

1. NYCJobs örnek kodunun DataLoader klasöründe Visual Studio'da **DataLoader.sln'yi** açın.

1. Azure Bilişsel Arama hizmetiniz için bağlantı bilgilerini ekleyin. DataLoader projesindeki App.config'i açın ve TargetSearchServiceName ve TargetSearchServiceApiKey uygulama Ayarlarını Azure Bilişsel Arama hizmetinizi ve Azure Bilişsel Arama hizmeti API Anahtarınızı yansıtacak şekilde değiştirin. Bu bilgiler Azure portalında bulunabilir.

1. Uygulamayı başlatmak için F5 tuşuna basın, iki dizin oluşturup NYCJob örnek verilerini içe aktarın.

1. **Otomatik TamamlamaTutorial.sln'yi** açın ve **AutocompleteTutorial** projesinde Web.config'i edin. SearchServiceName ve SearchServiceApiKey değerlerini arama hizmetiniz için geçerli değerlerle değiştirin.

1. Uygulamayı çalıştırmak için F5'e basın. Örnek web uygulaması varsayılan tarayıcıda açılır. Deneyim, sanal sürümle aynıdır, yalnızca dizin ve veriler hizmetinizde barındırılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, otomatik tamamlama ve önerileri destekleyen bir arama kutusu oluşturmak için temel adımları gösterir. Önerileri almak için ASP.NET bir MVC uygulamasını nasıl oluşturabileceğinizi ve Azure Bilişsel Arama .NET SDK veya REST API'yi nasıl kullanabileceğinizi gördünüz.

Bir sonraki adım olarak, önerileri entegre etmeye ve arama deneyiminize otomatik tamamlamayı deneyerek. Aşağıdaki başvuru makaleleri yardımcı olmalıdır.

> [!div class="nextstepaction"]
> [Otomatik tamamlama REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Önerileri REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Facets dizin özniteliği bir Create Index REST API](https://docs.microsoft.com/rest/api/searchservice/create-index)