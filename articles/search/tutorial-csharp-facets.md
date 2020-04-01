---
title: Gezinmeye yardımcı olmak için yönlerini kullanma konusunda C# öğreticisi
titleSuffix: Azure Cognitive Search
description: Bu öğretici, farklı gezinme eklemek için "Arama sonuçları pagination - Azure Bilişsel Arama" projesi üzerine inşa edilmiştir. Bir aramayı kolayca daraltmak için nasıl kullanılan yönleri öğrenin.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/10/2020
ms.openlocfilehash: d88a9d7efdabd493fd31b961748bb6ad3bd8d738
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77121574"
---
# <a name="c-tutorial-use-facets-to-aid-navigation---azure-cognitive-search"></a>C# öğreticisi: Gezinmeye yardımcı olmak için yönlerini kullanın - Azure Bilişsel Arama

Fasetler, kullanıcıya aramalarını odaklamak için kullanılacak bir dizi bağlantı sağlayarak gezinmeye yardımcı olmak için kullanılır. Fasonlar, örnek verilerimizdeki bir otelin kategorisi veya belirli bir özelliği gibi verilerin öznitelikleridir.

Bu öğretici, [C# Tutorial: Search sonuçları pagination - Azure Bilişsel Arama](tutorial-csharp-paging.md) öğretici oluşturulan sayfalama projesi üzerine inşa edilmiştir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Model özelliklerini _IsFacetable_ olarak ayarlama
> * Uygulamanıza yönlü gezinme ekleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

[C# Tutorial: Arama sonuçları pagination var - Azure Bilişsel Arama](tutorial-csharp-paging.md) projesi up ve çalışıyor. Bu proje ya kendi sürümünüz olabilir ya da GitHub'dan yükleyebilir: [İlk uygulamayı oluşturun.](https://github.com/Azure-Samples/azure-search-dotnet-samples)

## <a name="set-model-properties-as-isfacetable"></a>Model özelliklerini IsFacetable olarak ayarlama

Bir model özelliğinin fason bir aramada bulunabilmesi için **IsFacetable**ile etiketlenmiş olması gerekir.

1. **Otel** dersini inceleyin. **Kategori** ve **Etiketler**, örneğin, **IsFacetable**olarak etiketlenir , ancak **HotelName** ve **Açıklama** değildir. 

    ```cs
    public partial class Hotel
    {
        [System.ComponentModel.DataAnnotations.Key]
        [IsFilterable]
        public string HotelId { get; set; }

        [IsSearchable, IsSortable]
        public string HotelName { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnLucene)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrLucene)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Category { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public bool? ParkingIncluded { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public DateTimeOffset? LastRenovationDate { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public double? Rating { get; set; }

        public Address Address { get; set; }

        [IsFilterable, IsSortable]
        public GeographyPoint Location { get; set; }

        public Room[] Rooms { get; set; }

    }
    ```

2. Bu öğreticinin bir parçası olarak etiket değiştirilmeyiz, bu nedenle hotel.cs dosyasını değiştirilmeden kapatın.

    > [!Note]
    > Aramada istenen bir alan uygun şekilde etiketlenmezse, fason bir arama hata sağlar.


## <a name="add-facet-navigation-to-your-app"></a>Uygulamanıza yönlü gezinme ekleme

Bu örnekte, kullanıcının sonuçların solunda gösterilen bağlantı listelerinden bir otel kategorisi veya bir olanak seçmesine olanak sağlayacağız. Kullanıcı bazı arama metni girerek başlar, daha sonra bir kategori seçerek arama sonuçlarını daraltabilir ve bir uygunluk seçerek sonuçları daha da daraltabilir, ya da önce amenity 'yi seçebilir (sipariş önemli değildir).

Görüşe fason listelerini geçirmek için denetleyiciye ihtiyacımız var. Arama ilerledikçe kullanıcı seçimlerini korumamız gerekir ve yine verileri korumak için geçici depolama mekanizması olarak kullanırız.

!["Havuz" aramasını daraltmak için fason gezinmeyi kullanma](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="add-filter-strings-to-the-searchdata-model"></a>SearchData modeline filtre dizeleri ekleme

1. SearchData.cs dosyasını açın ve fason filtre dizelerini tutmak için **SearchData** sınıfına dize özellikleri ekleyin.

    ```cs
        public string categoryFilter { get; set; }
        public string amenityFilter { get; set; }
    ```

### <a name="add-the-facet-action-method"></a>Facet eylem yöntemini ekleme

Ev denetleyicisi yeni bir eylem, **Facet**ve mevcut **Dizin** ve **Sayfa** eylemleri güncellemeleri yanı sıra **RunQueryAsync** yöntemi güncellemeleri gerekir.

1. **Liste&lt;dizesi&gt; ** yapısını etkinleştirmek için ev denetleyicidosyasını açın ve **kullanılan** ifadeyi ekleyin.

    ```cs
    using System.Collections.Generic;
    ```

2. **Dizin (SearchData modeli)** eylem yöntemini değiştirin.

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
                await RunQueryAsync(model, 0, 0, "", "");
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. **Sayfa(SearchData modeli)** eylem yöntemini değiştirin.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

                // Calculate the page that should be displayed.
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

                // Recover the filters.
                string catFilter = TempData["categoryFilter"].ToString();
                string ameFilter = TempData["amenityFilter"].ToString();

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();

                // Search for the new page.
                await RunQueryAsync(model, page, leftMostPage, catFilter, ameFilter);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

4. Kullanıcı bir fason bağlantısını tıklattığında etkinleştirilecek bir **Faf (SearchData modeli)** eylem yöntemi ekleyin. Model, bir kategori arama filtresi veya bir olanaklar arama filtresi içerir. Belki **de Sayfa** eyleminden sonra ekleyin.

    ```cs
        public async Task<ActionResult> Facet(SearchData model)
        {
            try
            {
                // Filters set by the model override those stored in temporary data.
                string catFilter;
                string ameFilter;
                if (model.categoryFilter != null)
                {
                    catFilter = model.categoryFilter;
                } else
                {
                    catFilter = TempData["categoryFilter"].ToString();
                }

                if (model.amenityFilter != null)
                {
                    ameFilter = model.amenityFilter;
                } else
                {
                    ameFilter = TempData["amenityFilter"].ToString();
                }

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();

                // Initiate a new search.
                await RunQueryAsync(model, 0, 0, catFilter, ameFilter);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

### <a name="set-up-the-search-filter"></a>Arama filtresini ayarlama

Örneğin, bir kullanıcı belirli bir yönü seçtiğinde, **Resort and Spa** kategorisini tıklatır, ardından sonuçlarda yalnızca bu kategori olarak belirtilen oteller döndürülmelidir. Bu şekilde bir aramayı daraltmak için bir _filtre_ayarlamamız gerekir.

1. **RunQueryAsync** yöntemini aşağıdaki kodla değiştirin. Öncelikle, bir kategori filtre dizesi ve bir amenity filtre dizealır ve **SearchParameters** **Filtre** parametresini ayarlar.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage, string catFilter, string ameFilter)
        {
            InitSearch();

            string facetFilter = "";

            if (catFilter.Length > 0 && ameFilter.Length > 0)
            {
                // Both facets apply.
                facetFilter = $"{catFilter} and {ameFilter}"; 
            } else
            {
                // One, or zero, facets apply.
                facetFilter = $"{catFilter}{ameFilter}";
            }

            var parameters = new SearchParameters
            {
                Filter = facetFilter,

                // Return information on the text, and number, of facets in the data.
                Facets = new List<string> { "Category,count:20", "Tags,count:20" },

                // Enter Hotel property names into this list, so only these values will be returned.
                Select = new[] { "HotelName", "Description", "Category", "Tags" },

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
            model.pageCount = ((int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

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

            // Ensure Temp data is stored for the next call.
            TempData["page"] = page;
            TempData["leftMostPage"] = model.leftMostPage;
            TempData["searchfor"] = model.searchText;
            TempData["categoryFilter"] = catFilter;
            TempData["amenityFilter"] = ameFilter;

            // Return the new view.
            return View("Index", model);
        }
    ```

    Döndürülecek **Seç** öğeleri listesine **Kategori** ve **Etiketler** özelliklerini ekledik. Bu ek, fatür gezinmenin çalışması için bir gereklilik değildir, ancak bu bilgileri doğru filtreleme yaptığımızı doğrulamak için kullanırız.

### <a name="add-lists-of-facet-links-to-the-view"></a>Görünüme yönlü bağlantıların listesini ekleme

Görünüm bazı önemli değişiklikler gerektirecektir. 

1. hotels.css dosyasını açarak başlayın (wwwroot/css klasöründe) ve aşağıdaki sınıfları ekleyin.

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 250px;
        display: normal;
        color: #666;
        margin: 10px;
        padding: 5px;
    }

    .facetheader {
        font-size: 10pt;
        font-weight: bold;
        color: darkgreen;    
    }
    ```

2. Görünüm için, soldaki yönlü listeleri ve sağdaki sonuçları düzgün bir şekilde hizalamak için çıktıyı bir tablo halinde düzenleriz. index.cshtml dosyasını açın. HTML &lt;gövde&gt; etiketlerinin tüm içeriğini aşağıdaki kodla değiştirin.

    ```cs
    <body>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        <table>
            <tr>
                <td></td>
                <td>
                    <h1 class="sampleTitle">
                        <img src="~/images/azure-logo.png" width="80" />
                        Hotels Search - Facet Navigation
                    </h1>
                </td>
            </tr>

            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it.-->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input value="" class="searchBoxSubmit" type="submit">
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">

                        @if (Model != null && Model.resultList != null)
                        {
                            List<string> categories = Model.resultList.Facets["Category"].Select(x => x.Value.ToString()).ToList();

                            if (categories.Count > 0)
                            {
                                <h5 class="facetheader">Category:</h5>
                                <ul class="facetlist">
                                    @for (var c = 0; c < categories.Count; c++)
                                    {
                                        var facetLink = $"{categories[c]} ({Model.resultList.Facets["Category"][c].Count})";
                                        <li>
                                            @Html.ActionLink(facetLink, "Facet", "Home", new { categoryFilter = $"Category eq '{categories[c]}'" }, null)
                                        </li>
                                    }
                                </ul>
                            }

                            List<string> tags = Model.resultList.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

                            if (tags.Count > 0)
                            {
                                <h5 class="facetheader">Amenities:</h5>
                                <ul class="facetlist">
                                    @for (var c = 0; c < tags.Count; c++)
                                    {
                                        var facetLink = $"{tags[c]} ({Model.resultList.Facets["Tags"][c].Count})";
                                        <li>
                                            @Html.ActionLink(facetLink, "Facet", "Home", new { amenityFilter = $"Tags/any(t: t eq '{tags[c]}')" }, null)
                                        </li>
                                    }
                                </ul>
                            }
                        }
                    </div>
                </td>
                <td valign="top">
                    <div id="resultsplace">
                        @if (Model != null && Model.resultList != null)
                        {
                            // Show the result count.
                            <p class="sampleText">
                                @Html.DisplayFor(m => m.resultList.Count) Results
                            </p>

                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);

                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nCategory: {Model.resultList.Results[i].Document.Category}";
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel name and description.
                                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box2" })
                            }
                        }
                    </div>
                </td>
            </tr>

            <tr>
                <td></td>
                <td valign="top">
                    @if (Model != null && Model.pageCount > 1)
                    {
                        // If there is more than one page of results, show the paging buttons.
                        <table>
                            <tr>
                                <td class="tdPage">
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

                                <td class="tdPage">
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
                                    <td class="tdPage">
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

                                <td class="tdPage">
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

                                <td class="tdPage">
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
                </td>
            </tr>
        </table>
    }
    </body>
    ```

    **Html.ActionLink** çağrısının kullanımına dikkat edin. Bu çağrı, kullanıcı bir fason bağlantısını tıklattığında, geçerli filtre dizelerini denetleyiciye iletır. 

### <a name="run-and-test-the-app"></a>Uygulamayı çalıştırın ve test edin

Kullanıcıya fason gezinmenin avantajı, aramaları tek bir tıklamayla daraltabilmektir ve bunu aşağıdaki sırayla gösterebilirsiniz.

1. Uygulamayı çalıştırın, arama metni olarak "havaalanı" yazın. Faset listesinin düzgün bir şekilde solda göründüğünü doğrulayın. Bu yönleriyle metin verilerinde "havaalanı" bulunan oteller için geçerli olan tüm bu bilgiler, ne sıklıkta oluştuklarını saymaktadır.

    !["Havaalanı" aramasını daraltmak için fason navigasyon kullanma](./media/tutorial-csharp-create-first-app/azure-search-facet-airport.png)

2. Resort **ve Spa** kategorisini tıklatın. Tüm sonuçların bu kategoride olduğunu doğrulayın.

    ![Aramayı "Resort and Spa" olarak daraltma](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras.png)

3. **Kontinental kahvaltı** servisini tıklayın. Tüm sonuçların hala "Resort and Spa" kategorisinde olduğunu ve seçilen olanaklarla birlikte doğrulayın.

    ![Aramayı "kontinental kahvaltı" olarak daraltma](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras-cb.png)

4. Başka bir kategori seçmeyi deneyin, sonra bir olanaklar ve daraltma sonuçlarını görüntüleyin. Sonra başka bir yol deneyin, bir olanaklar, sonra bir kategori.

    >[!Note]
    > Bir facet listesinde (kategori gibi) bir seçim yapıldığında, kategori listesindeki önceki seçimleri geçersiz kılar.

## <a name="takeaways"></a>Paketler

Bu projeden aşağıdaki paketleri göz önünde bulundurun:

* Her özelliği **IsFacetable**olarak işaretlemek zorunludur , eğer fason gezinmeye dahil edileceklerse.
* Fason gezinme, bir aramayı daraltmanın kolay ve sezgisel bir yolunu sağlar.
* Faset navigasyon en iyi bölümlere ayrılır (otel kategorileri, bir otel olanakları, fiyat aralıkları, derecelendirme aralıkları, vb), uygun bir başlık ile her bölüm.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki öğreticide, sıralama sonuçlarına bakacağız. Bu noktaya kadar, sonuçlar yalnızca veritabanında bulunan sırayla sıralanır.

> [!div class="nextstepaction"]
> [C# öğretici: Sonuçları sipariş edin- Azure Bilişsel Arama](tutorial-csharp-orders.md)
