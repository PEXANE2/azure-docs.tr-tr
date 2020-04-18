---
title: Otomatik tamamlama ve öneriler
titleSuffix: Azure Cognitive Search
description: Bu öğretici, açılır listeyi kullanan kullanıcılardan arama terimi girdisi toplamanın bir yolu olarak otomatik tamamlama ve önerileri gösterir. Mevcut bir otel projesi üzerine inşa edilmiştir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: 6b74c3bbb811c122950fd969a8797e87f8f77f86
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641065"
---
# <a name="c-tutorial-add-autocomplete-and-suggestions---azure-cognitive-search"></a>C# öğretici: Otomatik tamamlama ve öneriler ekleme - Azure Bilişsel Arama

Bir kullanıcı arama kutusuna yazmaya başladığında otomatik tamamlamayı (önceden yazma sorguları ve önerilen belgeler) nasıl uygulayacağınızı öğrenin. Bu öğreticide, otomatik olarak tamamlanan sorguları ve öneri sonuçlarını ayrı ayrı ve sonra birlikte göstereceğiz. Bir kullanıcı, kullanılabilir tüm sonuçları bulmak için yalnızca iki veya üç karakter yazmak zorunda kalabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Öneri ekle
> * Önerilere vurgulama ekleme
> * Otomatik tamamlama ekleme
> * Otomatik tamamlama ve önerileri birleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici bir serinin parçasıdır ve C# Tutorial oluşturulan sayfalama projesi üzerine inşa: [Arama sonuçları pagination - Azure Bilişsel Arama](tutorial-csharp-paging.md).

Alternatif olarak, bu özel öğretici için çözümü indirebilir ve çalıştırabilirsiniz: [3-add-typeahead](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/3-add-typeahead).

## <a name="add-suggestions"></a>Öneri ekle

Kullanıcıya alternatifler sunmanın en basit örneğiyle başlayalım: önerilerin açılır listesi.

1. index.cshtml dosyasında, `@id` **TextBoxFor** deyimini **azureautosuggest**olarak değiştirin.

    ```cs
     @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautosuggest" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

2. Bu ifadeden sonra, kapanış ** &lt;/div&gt;** sonra, bu komut dosyası girin. Bu komut dosyası, önerilen sonuçların açılır listesini sunmak için açık kaynak jQuery UI kitaplığından [Otomatik Tamamlama widget'ından](https://api.jqueryui.com/autocomplete/) yararlanır. 

    ```javascript
    <script>
        $("#azureautosuggest").autocomplete({
            source: "/Home/Suggest?highlights=false&fuzzy=false",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

    "Azureautosuggest" kimliği yukarıdaki komut dosyasını arama kutusuna bağlar. Widget'ın kaynak seçeneği, API Öner'i iki sorgu parametresiyle çağıran bir Öner yöntemine ayarlanmıştır: **vurgular** ve **bulanık**, her ikisi de bu örnekte yanlış olarak ayarlanmıştır. Ayrıca, aramayı tetiklemek için en az iki karakter gerekir.

### <a name="add-references-to-jquery-scripts-to-the-view"></a>Görünüme jQuery komut dosyasına referansekleme

1. jQuery kitaplığına erişmek için &lt;&gt; görünüm dosyasının baş bölümünü aşağıdaki kodla değiştirin:

    ```cs
    <head>
        <meta charset="utf-8">
        <title>Typeahead</title>
        <link href="https://code.jquery.com/ui/1.12.1/themes/start/jquery-ui.css"
              rel="stylesheet">
        <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
        <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>

        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

2. Yeni bir jQuery başvurusu sunduğumuz için, _Layout.cshtml dosyasındaki **(Görünümler/Paylaşılan** klasörde) varsayılan jQuery başvurusunu kaldırmamız veya açıklama yapmamız gerekir. Aşağıdaki satırları bulun ve gösterildiği gibi ilk komut dosyası satırı nın dışına yorum yapın. Bu değişiklik, jQuery için çakışan başvuruları önler.

    ```html
    <environment include="Development">
        <!-- <script src="~/lib/jquery/dist/jquery.js"></script> -->
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>
    </environment>
    ```

    Şimdi önceden tanımlanmış Otomatik tamamlama jQuery işlevlerini kullanabiliriz.

### <a name="add-the-suggest-action-to-the-controller"></a>Denetleyiciye Öner eylemini ekleme

1. Ev denetleyicisinde, **Öner** eylemini ekleyin (sayfa **eyleminden** sonra.

    ```cs
        public async Task<ActionResult> Suggest(bool highlights, bool fuzzy, string term)
        {
            InitSearch();

            // Setup the suggest parameters.
            var parameters = new SuggestParameters()
            {
                UseFuzzyMatching = fuzzy,
                Top = 8,
            };

            if (highlights)
            {
                parameters.HighlightPreTag = "<b>";
                parameters.HighlightPostTag = "</b>";
            }

            // Only one suggester can be specified per index. It is defined in the index schema.
            // The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and simply searches the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", parameters);

            // Convert the suggest query results to a list that can be displayed in the client.
            List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();

            // Return the list of suggestions.
            return new JsonResult(suggestions);
        }
    ```

    **Üst** parametre, kaç sonuç döndürüleceklerini belirtir (belirtilmemişse varsayılan değer 5'tir). Veri ayarlandığında yapılan olan Azure dizininde bir _önerici_ belirtilir ve bu öğretici gibi bir istemci uygulaması tarafından değil. Bu durumda, önerici "sg" olarak adlandırılır ve **HotelName** alanını arar - başka bir şey değil. 

    Bulanık eşleştirme, "neredeyse ıskalar"ın çıkışa, bir edit mesafesine kadar dahil olmasını sağlar. **Vurgular** parametresi doğru olarak ayarlanırsa, çıktıya kalın HTML etiketleri eklenir. Bu iki parametreyi bir sonraki bölümde doğru olarak ayarlayacaktır.

2. Bazı sözdizimi hataları alabilirsiniz. Bu nedenle, aşağıdaki iki **sini dosyanın** en üstüne kullanarak ifadeleri ekleyin.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

3. Uygulamayı çalıştırın. Örneğin "po"yı girdiğinizde çeşitli seçenekler edinirken mi? Şimdi "pa"yı dene.

    !["po" yazmak iki öneriyi ortaya çıkarır](./media/tutorial-csharp-create-first-app/azure-search-suggest-po.png)

    Girdiğiniz harflerin bir sözcük başlatması ve yalnızca sözcük içinde yer almayan bir sözcük başlatması _gerektiğine_ dikkat edin.

4. Görünüm komut dosyasında, **&** bulanık olarak doğru ayarlayın ve uygulamayı yeniden çalıştırın. Şimdi "po" girin. Arama bir harfi yanlış var varsayar dikkat edin!
 
    ![Doğru bulanık ayarlanmış "pa" yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-fuzzy.png)

    İlgileniyorsanız, [Azure Bilişsel Arama'daki Lucene sorgu sözdizimi](https://docs.microsoft.com/azure/search/query-lucene-syntax) bulanık aramalarda kullanılan mantığı ayrıntılı olarak açıklar.

## <a name="add-highlighting-to-the-suggestions"></a>Önerilere vurgulama ekleme

**Vurgular** parametresini doğru olarak ayarlayarak önerilerin kullanıcıya görünümünü iyileştirebiliriz. Ancak, önce kalın metin görüntülemek için görünüme bazı kod eklemeniz gerekir.

1. Görünümde (index.cshtml), yukarıda girdiğiniz **azureautosuggest** komut dosyasından sonra aşağıdaki komut dosyasını ekleyin.

    ```javascript
    <script>
        var updateTextbox = function (event, ui) {
            var result = ui.item.value.replace(/<\/?[^>]+(>|$)/g, "");
            $("#azuresuggesthighlights").val(result);
            return false;
        };

        $("#azuresuggesthighlights").autocomplete({
            html: true,
            source: "/home/suggest?highlights=true&fuzzy=false&",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },
            select: updateTextbox,
            focus: updateTextbox
        }).data("ui-autocomplete")._renderItem = function (ul, item) {
            return $("<li></li>")
                .data("item.autocomplete", item)
                .append("<a>" + item.label + "</a>")
                .appendTo(ul);
        };
    </script>
    ```

2. Şimdi metin kutusunun kimliğini aşağıdaki gibi olacak şekilde değiştirin.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azuresuggesthighlights" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. Uygulamayı yeniden çalıştırın ve girilen metninizin önerilerde kalın olduğunu görmeniz gerekir. "Pa" yazmayı dene.
 
    ![Vurgulama ile "pa" yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-highlight.png)

4. Yukarıdaki vurgulama komut dosyasında kullanılan mantık kusursuz değildir. Aynı adla iki kez görünen bir terim girerseniz, kalın sonuçlar istediğiniz gibi değildir. "Mo" yazmayı dene.

    Bir geliştiricinin yanıtlaması gereken sorulardan biri, "yeterince iyi" çalışan bir komut dosyasının ne zaman olduğu ve tuhaflıklarının ne zaman ele alınması gerektiğidir. Bu öğreticide daha fazla vurgulama almayacağız, ancak kesin bir algoritma bulmak, verileriniz için etkili değilse vurgulamanın göz önünde bulundurulması gereken bir şeydir. Daha fazla bilgi için [Bkz. Hit vurgulama.](search-pagination-page-layout.md#hit-highlighting)

## <a name="add-autocomplete"></a>Otomatik tamamlama ekleme

Önerilerden biraz farklı olan başka bir varyasyon, sorgu terimini tamamlayan otomatik tamamlamadır (bazen "ileri de yazı" olarak adlandırılır). Yine, kullanıcı deneyimini geliştirmeden önce, en basit uygulama ile başlayacaktır.

1. Önceki komut dosyalarınızı izleyerek görünüme aşağıdaki komut dosyasını girin.

    ```javascript
    <script>
        $("#azureautocompletebasic").autocomplete({
            source: "/Home/Autocomplete",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

2. Şimdi metin kutusunun kimliğini değiştirin, böylece aşağıdaki gibi okur.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocompletebasic" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. Ev denetleyicisinde, **örneğin Öner** eyleminin altında **Otomatik Tamamlama** eylemini girmemiz gerekir.

    ```cs
        public async Task<ActionResult> AutoComplete(string term)
        {
            InitSearch();

            // Setup the autocomplete parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 6
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Convert the results to a list that can be displayed in the client.
            List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();

            // Return the list.
            return new JsonResult(autocomplete);
        }
    ```

    Öneriler için yaptığımız gibi otomatik tamamlama aramasında "sg" adı verilen aynı *önerici* işlevini kullandığımıza dikkat edin (bu yüzden sadece otel adlarını otomatik tamamlamaya çalışıyoruz).

    Bir dizi **AutocompleteMode** ayarı vardır ve **OneTermWithContext**kullanıyoruz. Ek seçeneklerin açıklaması için [Otomatik Tamamlama API'sine](https://docs.microsoft.com/rest/api/searchservice/autocomplete) bakın.

4. Uygulamayı çalıştırın. Açılan listede görüntülenen seçenekler aralığının tek sözcük olduğuna dikkat edin. "re" ile başlayan sözcükleri yazmayı deneyin. Daha fazla harf yazdıkça seçenek sayısının nasıl azaldığına dikkat edin.

    ![Temel otomatik tamamlama ile yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocompletebasic.png)

    Haliyle, daha önce çalıştırdığınız öneriler komut dosyası muhtemelen bu otomatik tamamlama komut dosyasından daha yararlıdır. Otomatik tamamlamayı daha kullanıcı dostu hale getirmek için öneri aramasına en iyi şekilde eklenir.

## <a name="combine-autocompletion-and-suggestions"></a>Otomatik tamamlama ve önerileri birleştirme

Otomatik tamamlama ve önerileri birleştirmek seçeneklerimizin en karmaşıkıdır ve muhtemelen en iyi kullanıcı deneyimini sağlar. İstediğimiz şey, yazılmakta olan metinle birlikte, metni otomatik olarak tamamlamak için Azure Bilişsel Arama'nın ilk tercihidir. Ayrıca, açılan liste olarak bir dizi öneri istiyoruz.

Genellikle "satır içi otomatik tamamlama" veya benzer bir ad olarak adlandırılan bu işlevselliği sunan kitaplıklar vardır. Ancak, bu özelliği yerel olarak uygulayacağız, böylece neler olup bittiğini görebilirsiniz. Bu örnekte önce denetleyici üzerinde çalışmaya başlayacağız.

1. Denetleyiciye, belirli sayıda öneriyle birlikte tek bir otomatik tamamlama sonucu döndüren bir eylem eklememiz gerekir. Biz bu eylem **AutoCompleteAndSuggest**çağıracaktır. Ev denetleyicisinde, diğer yeni eylemlerinizi izleyerek aşağıdaki eylemi ekleyin.

    ```cs
        public async Task<ActionResult> AutocompleteAndSuggest(string term)
        {
            InitSearch();

            // Setup the type-ahead search parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 1,
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Setup the suggest search parameters.
            var sp = new SuggestParameters()
            {
                Top = 8,
            };

            // Only one suggester can be specified per index. The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and it searches only the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", sp);

            // Create an empty list.
            var results = new List<string>();

            if (autocompleteResult.Results.Count > 0)
            {
                // Add the top result for type-ahead.
                results.Add(autocompleteResult.Results[0].Text);
            }
            else
            {
                // There were no type-ahead suggestions, so add an empty string.
                results.Add("");
            }
            for (int n = 0; n < suggestResult.Results.Count; n++)
            {
                // Now add the suggestions.
                results.Add(suggestResult.Results[n].Text);
            }

            // Return the list.
            return new JsonResult(results);
        }
    ```

    Bir otomatik tamamlama **seçeneği, tüm** önerilerin ardından sonuç listesinin en üstünde döndürülür.

2. Görünümde, önce bir hile uygularız, böylece açık gri bir otomatik tamamlama sözcüğü kullanıcı tarafından girilen cesur metnin hemen altında işlenir. HTML bu amaç için göreli konumlandırma içerir. **TextBoxFor** deyimini (ve &lt;çevresindeki&gt; div ifadelerini) aşağıdaki gibi değiştirin, **altında** ikinci bir arama kutusu nun normal arama kutumuzun hemen altında olduğunu belirterek, bu arama kutusunu varsayılan konumundan 39 piksel çekerek!

    ```cs
    <div id="underneath" class="searchBox" style="position: relative; left: 0; top: 0">
    </div>

    <div id="searchinput" class="searchBoxForm" style="position: relative; left: 0; top: -39px">
        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocomplete" }) <input value="" class="searchBoxSubmit" type="submit">
    </div>
    ```

    Bu durumda kimliği yeniden **azureautocomplete** olarak değiştirdiğimizi unutmayın.

3. Ayrıca görünümde, şimdiye kadar girdiğiniz tüm komut dosyalarından sonra aşağıdaki komut dosyasını girin. Çok fazla şey var.

    ```javascript
    <script>
        $('#azureautocomplete').autocomplete({
            delay: 500,
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },

            // Use Ajax to set up a "success" function.
            source: function (request, response) {
                var controllerUrl = "/Home/AutoCompleteAndSuggest?term=" + $("#azureautocomplete").val();
                $.ajax({
                    url: controllerUrl,
                    dataType: "json",
                    success: function (data) {
                        if (data && data.length > 0) {

                            // Show the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = data[0];

                            // Remove the top suggestion as it is used for inline autocomplete.
                            var array = new Array();
                            for (var n = 1; n < data.length; n++) {
                                array[n - 1] = data[n];
                            }

                            // Show the drop-down list of suggestions.
                            response(array);
                        } else {

                            // Nothing is returned, so clear the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }
                });
            }
        });

        // Complete on TAB.
        // Clear on ESC.
        // Clear if backspace to less than 2 characters.
        // Clear if any arrow key hit as user is navigating the suggestions.
        $("#azureautocomplete").keydown(function (evt) {

            var suggestedText = document.getElementById("underneath").innerHTML;
            if (evt.keyCode === 9 /* TAB */ && suggestedText.length > 0) {
                $("#azureautocomplete").val(suggestedText);
                return false;
            } else if (evt.keyCode === 27 /* ESC */) {
                document.getElementById("underneath").innerHTML = "";
                $("#azureautocomplete").val("");
            } else if (evt.keyCode === 8 /* Backspace */) {
                if ($("#azureautocomplete").val().length < 2) {
                    document.getElementById("underneath").innerHTML = "";
                }
            } else if (evt.keyCode >= 37 && evt.keyCode <= 40 /* Any arrow key */) {
                document.getElementById("underneath").innerHTML = "";
            }
        });

        // Character replace function.
        function setCharAt(str, index, chr) {
            if (index > str.length - 1) return str;
            return str.substr(0, index) + chr + str.substr(index + 1);
        }

        // This function is needed to clear the "underneath" text when the user clicks on a suggestion, and to
        // correct the case of the autocomplete option when it does not match the case of the user input.
        // The interval function is activated with the input, blur, change, or focus events.
        $("#azureautocomplete").on("input blur change focus", function (e) {

            // Set a 2 second interval duration.
            var intervalDuration = 2000, 
                interval = setInterval(function () {

                    // Compare the autocorrect suggestion with the actual typed string.
                    var inputText = document.getElementById("azureautocomplete").value;
                    var autoText = document.getElementById("underneath").innerHTML;

                    // If the typed string is longer than the suggestion, then clear the suggestion.
                    if (inputText.length > autoText.length) {
                        document.getElementById("underneath").innerHTML = "";
                    } else {

                        // If the strings match, change the case of the suggestion to match the case of the typed input.
                        if (autoText.toLowerCase().startsWith(inputText.toLowerCase())) {
                            for (var n = 0; n < inputText.length; n++) {
                                autoText = setCharAt(autoText, n, inputText[n]);
                            }
                            document.getElementById("underneath").innerHTML = autoText;

                        } else {
                            // The strings do not match, so clear the suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }

                    // If the element loses focus, stop the interval checking.
                    if (!$input.is(':focus')) clearInterval(interval);

                }, intervalDuration);
        });
    </script>
    ```

    Kullanıcının yazdıklarla eşleşip aynı durumda (üst veya alt) kullanıcı nın yazdığı gibi ("pa" "PA", "pA", "Pa" ararken eşleşirken) temel metni temizlemek için hem temel metni temizlemek **için, hem** de üst üste bindirilen metnin düzgün olması için aralık işlevinin akıllıca kullanıldığına dikkat edin.

    Daha iyi anlamak için senaryodaki yorumları okuyun.

4. Son olarak, saydam hale getirmek için iki HTML sınıfına küçük bir ayarlama yapmamız gerekir. Hotels.css dosyasındaki **searchBoxForm** ve **searchBox** sınıflarına aşağıdaki satırı ekleyin.

    ```html
        background: rgba(0,0,0,0);
    ```

5. Şimdi uygulamayı çalıştırın. Arama kutusuna "pa" girin. Otomatik tamamlama önerisi olarak "saray"ı ve içinde "pa" içeren iki otel alıyor musunuz?

    ![Satır İçi otomatik tamamlama ve önerilerle yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocomplete.png)

6. Otomatik tamamlama önerisini kabul etmek için sekme yapmayı deneyin ve ok tuşlarını ve sekme tuşunu kullanarak öneriler seçmeyi deneyin ve fareyi ve tek bir tıklamayla yeniden deneyin. Komut dosyasının tüm bu durumları düzgün bir şekilde işlettiğini doğrulayın.

    Sizin için bu özelliği sunan bir kütüphanede yüklemek için daha kolay olduğuna karar verebilirsiniz, ama şimdi çalışmak için satır lı otomatik tamamlama almak için en az bir yol biliyorum!

## <a name="takeaways"></a>Paketler

Bu projeden aşağıdaki paketleri göz önünde bulundurun:

* Otomatik tamamlama ("ileri yazma" olarak da bilinir) ve öneriler, kullanıcının tam olarak ne istediğini bulmak için yalnızca birkaç anahtar yazmasını sağlayabilir.
* Otomatik tamamlama ve öneriler birlikte çalışan zengin bir kullanıcı deneyimi sağlayabilir.
* Otomatik tamamlama işlevlerini her zaman tüm giriş biçimleriyle test edin.
* **setInterval** işlevinin kullanılması, UI öğelerini doğrulamada ve düzeltmede yararlı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki öğreticide, tek bir tıklamayla aramaları daraltmak için yönlerini kullanarak kullanıcı deneyimini geliştirmenin başka bir yolunu görüyoruz.

> [!div class="nextstepaction"]
> [C# Tutorial: Gezinmeye yardımcı olmak için yönlerini kullanın - Azure Bilişsel Arama](tutorial-csharp-facets.md)


