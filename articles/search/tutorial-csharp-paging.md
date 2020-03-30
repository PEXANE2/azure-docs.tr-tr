---
title: C# öğretici arama sonuçları pagination
titleSuffix: Azure Cognitive Search
description: Bu öğretici, arama sonuçlarının sayfalama gösterir. İlk, sonraki, önceki, son ve numaralı düğmelere göre sayfalama ile mevcut bir otel projesi üzerine inşa edilir. İkinci bir sayfalama sistemi, dikey kaydırma çubuğunu alt sınırına taşıyarak tetiklenen sonsuz kaydırma kullanır.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/10/2020
ms.openlocfilehash: 9abfeb54be6e22885b8e973034a6d89df8272146
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77121512"
---
# <a name="c-tutorial-search-results-pagination---azure-cognitive-search"></a>C# öğretici: Arama sonuçları pagination - Azure Bilişsel Arama

İlki sayfa numaralarına, ikincisi de sonsuz kaydırma yada iki farklı kaydırma sistemi olmak üzere nasıl uygulayacağınızı öğrenin. Her iki sayfalama sistemi de yaygın olarak kullanılır ve doğru sistemi seçmek, sonuçlarla istediğiniz kullanıcı deneyimine bağlıdır. Bu öğretici, [C# Tutorial: Create your first app - Azure Cognitive Search](tutorial-csharp-create-first-app.md) öğreticisinde oluşturulan projeye çağrılma sistemlerini oluşturur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Numaralanmış sayfalama ile uygulamanızı genişletin
> * Sonsuz kaydırma ile uygulamanızı genişletin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

[C# Tutorial: İlk uygulamanızı oluşturun - Azure Bilişsel Arama projeniz](tutorial-csharp-create-first-app.md) çalışır durumda. Bu proje ya kendi sürümünüz olabilir ya da GitHub'dan yükleyebilir: [İlk uygulamayı oluşturun.](https://github.com/Azure-Samples/azure-search-dotnet-samples)

## <a name="extend-your-app-with-numbered-paging"></a>Numaralanmış sayfalama ile uygulamanızı genişletin

Numaralı sayfalama ana internet arama motorları ve diğer birçok arama web sitelerinin seçim sayfalama sistemidir. Numaralı sayfalama genellikle gerçek sayfa numaraları aralığına ek olarak "sonraki" ve "önceki" seçeneği içerir. Ayrıca bir "ilk sayfa" ve "son sayfa" seçeneği de kullanılabilir. Bu seçenekler kesinlikle sayfa tabanlı sonuçlar arasında gezinme üzerinde bir kullanıcı denetimi sağlar.

İlk, önceki, sonraki ve son seçenekleri içeren bir sistem, 1'den başlamayan sayfa numaralarıyla birlikte, ancak bunun yerine kullanıcının bulunduğu geçerli sayfayı çevreleriz (örneğin, kullanıcı sayfa 10'a bakıyorsa, belki de sayfa numaraları 8 , 9, 10, 11 ve 12 görüntülenir).

Sistem, görünür sayfa numaralarının sayısının genel bir değişkende ayarlanabilmesiiçin yeterince esnek olacaktır.

Sistem, sol-en ve en sağdaki sayfa numarası düğmelerini özel olarak ele alacak, yani görüntülenen sayfa numaralarıaralığının değiştirilmesini tetikleyecek. Örneğin, 8, 9, 10, 11 ve 12 sayfa numaraları görüntülenirse ve kullanıcı 8'i tıklatırsa, görüntülenen sayfa numaraları aralığı 6, 7, 8, 9 ve 10'a dönüşür. Ve eğer 12'yi seçerlerse sağa benzer bir kayma olur.

### <a name="add-paging-fields-to-the-model"></a>Modele sayfalama alanları ekleme

Temel arama sayfası çözümlerini açık layın.

1. SearchData.cs model dosyasını açın.

2. Önce bazı genel değişkenler ekleyin. MVC'de, global değişkenler kendi statik sınıfında bildirilir. **ResultsPerPage** sayfa başına sonuç sayısını ayarlar. **MaxPageRange** görünümdeki görünür sayfa numaralarının sayısını belirler. **PageRangeDelta,** sayfa aralığının kaç sayfa nın sola veya sağa kaydırılması gerektiğini, en sol veya en çok sayfa numarası seçildiğinde belirler. Genellikle bu ikinci sayı **MaxPageRange**yarısı civarındadır. Ad alanına aşağıdaki kodu ekleyin.

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
        public static int MaxPageRange
        {
            get
            {
                return 5;
            }
        }

        public static int PageRangeDelta
        {
            get
            {
                return 2;
            }
        }
    }
    ```

    >[!Tip]
    >Bu projeyi dizüstü bilgisayar gibi daha küçük ekrana sahip bir aygıtta çalıştırıyorsanız, **ResultsPerPage'i** 2 olarak değiştirmeyi düşünün.

3. SearchText özelliğinden sonra **SearchData** sınıfına **searchText** sayfalama özellikleri ekleyin.

    ```cs
        // The current page being displayed.
        public int currentPage { get; set; }

        // The total number of pages of results.
        public int pageCount { get; set; }

        // The left-most page number to display.
        public int leftMostPage { get; set; }

        // The number of page numbers to display - which can be less than MaxPageRange towards the end of the results.
        public int pageRange { get; set; }

        // Used when page numbers, or next or prev buttons, have been selected.
        public string paging { get; set; }
    ```

### <a name="add-a-table-of-paging-options-to-the-view"></a>Görünüme sayfalama seçenekleri tablosu ekleme

1. Index.cshtml dosyasını açın ve kapanış &lt;/gövde&gt; etiketinden hemen önce aşağıdaki kodu ekleyin. Bu yeni kod sayfalama seçenekleri tablosu sunar: ilk, önceki, 1, 2, 3, 4, 5, sonraki, son.

    ```cs
    @if (Model != null && Model.pageCount > 1)
    {
    // If there is more than one page of results, show the paging buttons.
    <table>
        <tr>
            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("|<", "Page", "Home", new { paging = "0" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">|&lt;</p>
                }
            </td>

            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("<", "Page", "Home", new { paging = "prev" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&lt;</p>
                }
            </td>

            @for (var pn = Model.leftMostPage; pn < Model.leftMostPage + Model.pageRange; pn++)
            {
                <td>
                    @if (Model.currentPage == pn)
                    {
                        // Convert displayed page numbers to 1-based and not 0-based.
                        <p class="pageSelected">@(pn + 1)</p>
                    }
                    else
                    {
                        <p class="pageButton">
                            @Html.ActionLink((pn + 1).ToString(), "Page", "Home", new { paging = @pn }, null)
                        </p>
                    }
                </td>
            }

            <td>
                @if (Model.currentPage < Model.pageCount - 1)
                {
                    <p class="pageButton">
                        @Html.ActionLink(">", "Page", "Home", new { paging = "next" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&gt;</p>
                }
            </td>

            <td>
                @if (Model.currentPage < Model.pageCount - 1)
                {
                    <p class="pageButton">
                        @Html.ActionLink(">|", "Page", "Home", new { paging = Model.pageCount - 1 }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&gt;|</p>
                }
            </td>
        </tr>
    </table>
    }
    ```

    Olayları düzgün bir şekilde hizalamak için bir HTML tablosu kullanırız. Ancak tüm eylem @Html.ActionLink ifadeler geliyor, her daha önce eklediğimiz **sayfalama** özelliğine farklı girişleri ile oluşturulan **yeni** bir model ile denetleyici çağıran.

    İlk ve son sayfa seçenekleri "ilk" ve "son" gibi dizeleri göndermez, bunun yerine doğru sayfa numaralarını gönderir.

2. hotels.css dosyasındaki HTML stilleri listesine bazı sayfalama sınıfları ekleyin. **SayfaSeçili** sınıf, sayfa numaraları listesinde kullanıcının şu anda görüntülemakta olduğu sayfayı tanımlamak için (sayıyı kalın çevirerek) vardır.

    ```html
        .pageButton {
            border: none;
            color: darkblue;
            font-weight: normal;
            width: 50px;
        }

        .pageSelected {
            border: none;
            color: black;
            font-weight: bold;
            width: 50px;
        }

        .pageButtonDisabled {
            border: none;
            color: lightgray;
            font-weight: bold;
            width: 50px;
        }
    ```

### <a name="add-a-page-action-to-the-controller"></a>Denetleyiciye Sayfa eylemi ekleme

1. HomeController.cs dosyasını açın ve **Sayfa** eylemini ekleyin. Bu eylem, seçilen sayfa seçeneklerinden herhangi birini yanıtlar.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

                switch (model.paging)
                {
                    case "prev":
                        page = (int)TempData["page"] - 1;
                        break;

                    case "next":
                        page = (int)TempData["page"] + 1;
                        break;

                    default:
                        page = int.Parse(model.paging);
                        break;
                }

                // Recover the leftMostPage.
                int leftMostPage = (int)TempData["leftMostPage"];

                // Recover the search text and search for the data for the new page.
                model.searchText = TempData["searchfor"].ToString();

                await RunQueryAsync(model, page, leftMostPage);

                // Ensure Temp data is stored for next call, as TempData only stored for one call.
                TempData["page"] = (object)page;
                TempData["searchfor"] = model.searchText;
                TempData["leftMostPage"] = model.leftMostPage;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

    **RunQueryAsync** yöntemi şimdi bir sözdizimi hatası gösterecektir, üçüncü parametre nedeniyle, biz biraz gelecek.

    > [!Note]
    > **TempData,** yalnızca bir arama _için_ bu depolama devam etse de, geçici depolama alanında bir değer **(nesne)** depolamayı çağırır. Biz geçici veri bir şey saklarsanız, bir denetleyici eylem için bir sonraki çağrı için kullanılabilir olacak, ama kesinlikle bundan sonra arama tarafından gitmiş olacak! Bu kısa kullanım ömrü nedeniyle, arama metnini ve sayfalama özelliklerini **sayfaya**yapılan her çağrıyı geçici depolama alanında saklarız.

2. Geçici değişkenleri depolamak ve **RunQueryAsync** çağrısına en çok sayfa parametresini eklemek için **Dizin(model)** eyleminin güncellenmesi gerekir.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the search call for the first page.
                await RunQueryAsync(model, 0, 0);

                // Ensure temporary data is stored for the next call.
                TempData["page"] = 0;
                TempData["leftMostPage"] = 0;
                TempData["searchfor"] = model.searchText;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. **RunQueryAsync** yöntemi önemli ölçüde güncelleştirilmiş gerekir. **Arama Parametreleri** sınıfının **Skip**, **Top**ve **IncludeTotalResultCount** alanlarını, **Skip** ayarından başlayarak yalnızca bir sayfa değerinde sonuç istemek için kullanırız. Ayrıca bizim görünümü için sayfalama değişkenleri hesaplamak gerekir. Yöntemin tamamını aşağıdaki kodla değiştirin.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                   // Enter Hotel property names into this list so only these values will be returned.
                   // If Select is empty, all values will be returned, which can be inefficient.
                   Select = new[] { "HotelName", "Description" },
                   SearchMode = SearchMode.All,

                   // Skip past results that have already been returned.
                   Skip = page * GlobalVariables.ResultsPerPage,

                   // Take only the next page worth of results.
                   Top = GlobalVariables.ResultsPerPage,

                   // Include the total number of results.
                   IncludeTotalResultCount = true,
               };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // This variable communicates the total number of pages to the view.
            model.pageCount = ( (int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

            // This variable communicates the page number being displayed to the view.
            model.currentPage = page;

            // Calculate the range of page numbers to display.
            if (page == 0)
            {
                leftMostPage = 0;
            }
            else
               if (page <= leftMostPage)
            {
                // Trigger a switch to a lower page range.
                leftMostPage = Math.Max(page - GlobalVariables.PageRangeDelta, 0);
            }
            else
            if (page >= leftMostPage + GlobalVariables.MaxPageRange - 1)
            {
                // Trigger a switch to a higher page range.
                leftMostPage = Math.Min(page - GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
            }
            model.leftMostPage = leftMostPage;

            // Calculate the number of page numbers to display.
            model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

            return View("Index", model);
        }
    ```

4. Son olarak, görünümü küçük bir değişiklik yapmak gerekir. Değişken **resultsList.Results.Count** artık toplam sayıyı değil, bir sayfada (örneğimizde 3) döndürülen sonuç sayısını içerecektir. **IncludeTotalResultCount'u** doğru olarak ayarladığımız için, değişken **resultsList.Count** artık toplam sonuç sayısını içerir. Bu nedenle, görünümde sonuç sayısının nerede görüntülendiğini bulun ve aşağıdaki kodla değiştirin.

    ```cs
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>
    ```

    > [!Note]
    > Bu toplamın Azure Bilişsel Arama tarafından hesaplanması gerektiğinden, **IncludeTotalResultCount'u** doğru olarak ayarlayarak, genellikle bir performans isabeti vardır. Karmaşık veri kümeleri ile döndürülen değerin _yaklaşık_olduğu konusunda bir uyarı vardır. Otel verilerimiz için doğru olacaktır.

### <a name="compile-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Şimdi **Hata Ayıklama olmadan Başlat'ı** seçin (veya F5 tuşuna basın).

1. Bol sonuç verecek bazı metinlerde arama yapın (örneğin"wifi"). Sonuçlara düzgün bir şekilde sayfa atabilir misiniz?

    !["Havuz" sonuçları ile numaralanmış sayfalama](./media/tutorial-csharp-create-first-app/azure-search-numbered-paging.png)

2. Sağ-en ve daha sonra sol-en sayfa numaralarını tıklatma deneyin. Sayfa numaraları, içinde olduğunuz sayfayı merkeze almak için uygun şekilde ayarlıyor mu?

3. "İlk" ve "son" seçenekleri yararlı mı? Bazı popüler web aramaları bu seçenekleri kullanır, diğerleri kullanmaz.

4. Sonuçların son sayfasına gidin. Son sayfa, **ResultsPerPage** sonuçlarından daha az ını içerebilecek tek sayfadır.

    !["wifi" son sayfasının incelenmesi](./media/tutorial-csharp-create-first-app/azure-search-pool-last-page.png)

5. "Kasaba" yazın ve arama'yı tıklatın. Birden az sayfa değerinde sonuç varsa, sayfalama seçeneği görüntülenmez.

    !["Kasaba" aranıyor](./media/tutorial-csharp-create-first-app/azure-search-town.png)

Şimdi bu projeden tasarruf edin ve bu sayfalama biçimine bir alternatif deneyelim.

## <a name="extend-your-app-with-infinite-scrolling"></a>Sonsuz kaydırma ile uygulamanızı genişletin

Bir kullanıcı dikey kaydırma çubuğunu görüntülenen sonuçların sonuncusuna kaydırdığında sonsuz kaydırma tetiklenir. Bu durumda, sonuçların bir sonraki sayfası için sunucuya bir çağrı yapılır. Başka sonuç yoksa, hiçbir şey döndürülmez ve dikey kaydırma çubuğu değişmez. Daha fazla sonuç varsa, bunlar geçerli sayfaya eklenir ve kaydırma çubuğu daha fazla sonuç olduğunu göstermek için değişir.

Burada önemli olan nokta, görüntülenen sayfanın değiştirilmemesi, ancak yeni sonuçlarla eklenmiş olmasıdır. Bir kullanıcı her zaman aramanın ilk sonuçlarına geri kaydırabilir.

Sonsuz kaydırma uygulamak için, sayfa numarası kaydırma öğelerinden herhangi biri eklenmeden önce projeye başlayalım. Yani, gerekirse, GitHub temel arama sayfasının başka bir kopyasını yapmak: [İlk uygulama oluşturun.](https://github.com/Azure-Samples/azure-search-dotnet-samples)

### <a name="add-paging-fields-to-the-model"></a>Modele sayfalama alanları ekleme

1. İlk olarak, **SearchData** sınıfına (SearchData.cs modeli dosyasında) bir **sayfalama** özelliği ekleyin.

    ```cs
        // Record if the next page is requested.
        public string paging { get; set; }
    ```

    Bu değişken, sonuçların bir sonraki sayfası nın gönderilmesi veya bir aramanın ilk sayfasının null olması durumunda "sonraki" sözcük tutan bir dizedir.

2. Aynı dosyada ve ad alanı içinde, tek bir özelliği olan genel bir değişken sınıfı ekleyin. MVC'de, global değişkenler kendi statik sınıfında bildirilir. **ResultsPerPage** sayfa başına sonuç sayısını ayarlar. 

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
    }
    ```

### <a name="add-a-vertical-scroll-bar-to-the-view"></a>Görünüme dikey kaydırma çubuğu ekleme

1. Index.cshtml dosyasının sonuçları görüntüleyen bölümünü bulun ** @if ((Model != null)** ile başlar).

2. Bölümü aşağıdaki kodla değiştirin. Yeni ** &lt;div&gt; ** bölümü kaydırılabilir olması gereken alanın etrafındadır ve hem **taşması-y** özniteliği hem de "kaydırılmış()" adlı bir **kaydırma** işlevine çağrı ekler.

    ```cs
        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>

            <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    // Display the hotel name and description.
                    @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
                }
            </div>
        }
    ```

3. Döngünün hemen altında, &lt;/div&gt; etiketinden sonra **kaydırılan** işlevi ekleyin.

    ```javascript
        <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 2) {
                                div.innerHTML += '\n<textarea class="box1">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2">' + data[i + 1] + '</textarea>';
                            }
                        });
                    }
                }
        </script>
    ```

    Yukarıdaki komut dosyasındaki **if** deyimi, kullanıcının dikey kaydırma çubuğunun altına kaydırıp kaydırılmamasını test ediyor. Varsa, Sonraki **adlı**bir eylem için **Ev** denetleyicisi için bir çağrı yapılır. Denetleyici tarafından başka bir bilgiye gerek yoktur, bir sonraki veri sayfasını döndürecektir. Bu veriler daha sonra orijinal sayfayla aynı HTML stilleri kullanılarak biçimlendirilir. Hiçbir sonuç döndürülmezse, hiçbir şey eklenir ve her şey olduğu gibi kalır.

### <a name="handle-the-next-action"></a>Sonraki eylemi işleme

Denetleyiciye gönderilmesi gereken yalnızca üç eylem vardır: Uygulamanın ilk çalıştırılması, **Index()** olarak adlandırdığı, kullanıcı tarafından yapılan ilk arama, **Index(model)** çağıran, ve sonraki sonraki çağrılar **Next(model)** üzerinden daha fazla sonuç için.

1. Ev denetleyicisi dosyasını açın ve **RunQueryAsync** yöntemini orijinal öğreticiden silin.

2. **Index(model)** eylemini aşağıdaki kodla değiştirin. Artık **çağrılama** alanını null olduğunda veya "sonraki" olarak ayarlandığında işler ve Azure Bilişsel Arama çağrısını işler.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First call. Check for valid text input.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    page = 0;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Enter Hotel property names into this list so only these values will be returned.
                    // If Select is empty, all values will be returned, which can be inefficient.
                    Select = new[] { "HotelName", "Description" },
                    SearchMode = SearchMode.All,

                    // Skip past results that have already been returned.
                    Skip = page * GlobalVariables.ResultsPerPage,

                    // Take only the next page worth of results.
                    Top = GlobalVariables.ResultsPerPage,

                    // Include the total number of results.
                    IncludeTotalResultCount = true,
                };

                // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
                model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    Numaralanmış sayfalama yöntemine benzer şekilde, yalnızca ihtiyacımız olan verilerin döndürülülmasını istemek için **Atla** ve **Üst** arama ayarlarını kullanırız.

3. **Sonraki** eylemi ev denetleyicisine ekleyin. Bir listeyi nasıl döndürdettiğini, her otelin listeye iki öğe eklediğini unutmayın: bir otel adı ve otel açıklaması. Bu biçim, **kaydırılan** işlevin görünümdeki döndürülen verileri kullanmasıyla eşleşecek şekilde ayarlanır.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel name, then description, to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. **Liste&lt;dizesinde&gt;** sözdizimi hatası alıyorsanız, denetleyici dosyasının başına yönergeleri **kullanarak** aşağıdaki yönergeleri ekleyin.

    ```cs
    using System.Collections.Generic;
    ```

### <a name="compile-and-run-your-project"></a>Projenizi derle ve çalıştırın

Şimdi **Hata Ayıklama olmadan Başlat'ı** seçin (veya F5 tuşuna basın).

1. Bol sonuç verecek bir terim girin ("havuz" gibi) ve ardından dikey kaydırma çubuğunu test edin. Yeni bir sonuç sayfasını tetikler mi?

    !["Havuz" sonuçlarında sonsuz kaydırma](./media/tutorial-csharp-create-first-app/azure-search-infinite-scroll.png)

    > [!Tip]
    > İlk sayfada bir kaydırma çubuğunun göründüğünden emin olmak için, sonuçların ilk sayfasının görüntülenmekte oldukları alanın yüksekliğini biraz aşması gerekir. Örneğimizde **.box1'in** yüksekliği 30 piksel, **box2'nin** yüksekliği 100 piksel _ve_ alt kenar boşluğu 24 pikseldir. Yani her giriş 154 piksel kullanır. Üç giriş 3 x 154 = 462 piksel kadar sürer. Dikey kaydırma çubuğunun göründüğünden emin olmak için, ekran alanına bir yükseklik 462 pikselden daha küçük, hatta 461 çalışma ayarlanmalıdır. Bu sorun yalnızca ilk sayfada oluşur, bundan sonra bir kaydırma çubuğu nun görüntüleeceğinden emin olur. Güncelleme satırı: ** &lt;div id="myDiv" style="width: 800px; yükseklik: 450px; taşma-y: kaydırma;" onscroll="scrolled()"&gt;**.

2. Sonuçların en altına doğru ilerleyin. Tüm bilgilerin artık tek bir görünüm sayfasında nasıl olduğuna dikkat edin. Sunucu aramalarını tetiklemeden en tepeye kadar kaydırabilirsiniz.

Daha gelişmiş sonsuz kaydırma sistemleri, yeni bir sonuç sayfasının yüklenmesinin tetiklenmesi için fare tekerleğiveya benzer başka bir mekanizmayı kullanabilir. Biz bu öğreticiler daha sonsuz kaydırma alarak olmayacak, ama ekstra fare tıklamaları önler gibi belirli bir cazibesi vardır, ve daha fazla diğer seçenekleri araştırmak isteyebilirsiniz!

## <a name="takeaways"></a>Paketler

Bu projeden aşağıdaki paketleri göz önünde bulundurun:

* Numaralı sayfalama, sonuçların sırasının biraz rasgele olduğu aramalar için iyidir, yani sonraki sayfalarda kullanıcılarınız için ilgi çekici bir şey olabilir.
* Sonuçların sırası özellikle önemli olduğunda sonsuz kaydırma iyidir. Örneğin, sonuçlar bir hedef şehir merkezinden uzakta sıralanırsa.
* Numaralı sayfalama bazı daha iyi navigasyon sağlar. Örneğin, bir kullanıcı ilginç bir sonucun sayfa 6'da olduğunu hatırlayabilir, ancak sonsuz kaydırmada böyle kolay bir başvuru yoktur.
* Sonsuz kaydırma kolay bir itiraz vardır, yukarı ve aşağı tıklamak için telaşlı sayfa numaraları ile kaydırma.
* Sonsuz kaydırmanın önemli bir özelliği, sonuçların verimli olan bu sayfanın yerine değil, varolan bir sayfaya ekilmesidir.
* Geçici depolama yalnızca bir arama için devam eder ve ek aramalarda hayatta kalmak için sıfırlanması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Sayfalama internet aramaları için esastır. Iyi kapsanan sayfalama ile, bir sonraki adım daha fazla kullanıcı deneyimini geliştirmek için, tür-ileri aramaekleyerek.

> [!div class="nextstepaction"]
> [C# Tutorial: Otomatik tamamlama ve öneriler ekleme - Azure Bilişsel Arama](tutorial-csharp-type-ahead-and-suggestions.md)
