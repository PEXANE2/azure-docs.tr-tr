---
title: C# öğretici ilk uygulama nızı oluşturmak için
titleSuffix: Azure Cognitive Search
description: İlk C# arama uygulamanızı adım adım nasıl oluşturarak geliştireceklerinizi öğrenin. Öğretici, hem GitHub'da çalışan bir uygulamaya bağlantı sağlar hem de uygulamayı sıfırdan oluşturmak için tam bir işlem sağlar. Azure Bilişsel Arama'nın temel bileşenleri hakkında bilgi edinin.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/10/2020
ms.openlocfilehash: 2b4f67fc448d98239947fd764d4926f1d590c5e2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77121578"
---
# <a name="c-tutorial-create-your-first-app---azure-cognitive-search"></a>C# öğreticisi: İlk uygulamanızı oluşturun - Azure Bilişsel Arama

Azure Bilişsel Arama'yı kullanarak bir dizinden arama sonuçlarını sorgulamak ve sunmak için nasıl bir web arabirimi oluşturup sunmayı öğrenin. Bu öğretici, bir arama sayfası oluşturmaya odaklanabilmeniz için varolan, barındırılan bir diziniyle başlar. Dizin hayali otel verilerini içerir. Temel bir sayfanız olduğunda, sonraki derslerde sayfalama, fason ve ileri adımlık bir deneyim eklemek için sayfayı geliştirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Geliştirme ortamı nı kurulum
> * Model veri yapıları
> * Web sayfası oluşturma
> * Yöntemleri tanımlama
> * Uygulamayı test edin

Ayrıca arama aramanın ne kadar basit olduğunu da öğreneceksiniz. Geliştireceğiniz koddaki anahtar ifadeler aşağıdaki birkaç satırda özetlenir.

```cs
var parameters = new SearchParameters
{
    // Enter Hotel property names into this list, so only these values will be returned.
    Select = new[] { "HotelName", "Description" }
};

DocumentSearchResult<Hotel> results  = await _indexClient.Documents.SearchAsync<Hotel>("search text", parameters);
```

Bu tek çağrı, Azure verilerinde bir arama başlatır ve sonuçları döndürür.

!["Havuz" aranıyor](./media/tutorial-csharp-create-first-app/azure-search-pool.png)


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

IDE olarak kullanmak için [Visual Studio'yu yükleyin.](https://visualstudio.microsoft.com/)

### <a name="install-and-run-the-project-from-github"></a>Projeyi GitHub'dan yükleyin ve çalıştırın

1. Örneği GitHub'da bulun: [İlk uygulamayı oluşturun.](https://github.com/Azure-Samples/azure-search-dotnet-samples)
1. **Clone'u seçin veya projenin** özel yerel kopyasını indirin ve yapın.
1. Visual Studio'yu kullanarak, temel arama sayfasına gidin ve çözümü açın ve **hata ayıklamadan Başlat'ı** (veya F5 tuşuna basın) seçin.
1. Bazı sözcükler yazın (örneğin "wifi", "görünüm", "bar", "park etme") ve sonuçları inceleyin!

    !["wifi" aranıyor](./media/tutorial-csharp-create-first-app/azure-search-wifi.png)

Umarım bu proje sorunsuz çalışır ve Azure uygulaması çalışır. Daha karmaşık aramalar için gerekli bileşenlerin çoğu bu uygulamada yer alıyor, bu nedenle uygulamadan geçmek ve adım adım yeniden oluşturmak iyi bir fikirdir.

Bu projeyi sıfırdan oluşturmak ve dolayısıyla zihninizdeki Azure Bilişsel Arama bileşenlerini güçlendirmeye yardımcı olmak için aşağıdaki adımları izleyin.

## <a name="set-up-a-development-environment"></a>Geliştirme ortamı ayarlama

1. Visual Studio 2017 veya daha sonra, **Yeni/Proje'yi** seçin ve **ardından Core Web Application'ı ASP.NET.** Projeye "FirstAzureSearchApp" gibi bir ad verin.

    ![Bulut projesi oluşturma](./media/tutorial-csharp-create-first-app/azure-search-project1.png)

2. Bu proje türü için **Tamam'ı** tıklattıktan sonra, bu proje için geçerli olan ikinci bir seçenek kümesi verilir. **Web Uygulaması (Model-View-Controller) seçin.**

    ![MVC projesi oluşturma](./media/tutorial-csharp-create-first-app/azure-search-project2.png)

3. Ardından, **Araçlar** menüsünde **NuGet Paket Yöneticisi'ni** seçin ve **ardından Çözüm için NuGet Paketlerini Yönet... seçeneğini belirleyin.** Yüklememiz gereken bir paket var. **Gözat** sekmesini seçin ve arama kutusuna "Azure Bilişsel Arama" yazın. **Microsoft.Azure.Search'ü** listede göründüğünde yükleyin (sürüm 9.0.1 veya daha sonra). Yüklemeyi tamamlamak için birkaç ek iletişim kutusunu tıklatmanız gerekir.

    ![Azure kitaplıkları eklemek için NuGet'i kullanma](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

### <a name="initialize-azure-cognitive-search"></a>Azure Bilişsel Arama'yı Başlatma

Bu örnek için, halka açık otel verilerini kullanıyoruz. Bu veriler, yalnızca demo verileri sağlamak amacıyla oluşturulan 50 kurgusal otel adı ve açıklamasından oluşan keyfi bir koleksiyondur. Bu verilere erişmek için bir ad ve anahtar belirtmeniz gerekir.

1. Yeni projenizde appsettings.json dosyasını açın ve varsayılan satırları aşağıdaki ad ve anahtarla değiştirin. Burada gösterilen API anahtarı bir anahtar örneği değildir, _tam olarak_ otel verilerine erişmek için ihtiyacınız olan anahtardır. Appsettings.json dosyanız artık bu şekilde görünmelidir.

    ```cs
    {
        "SearchServiceName": "azs-playground",
        "SearchServiceQueryApiKey": "EA4510A6219E14888741FCFC19BFBB82"
    }
    ```

2. Bu dosyayla henüz işimiz bitmedi, bu dosyanın özelliklerini seçin ve daha yeniyse **Çıktı Dizini** ayarını **Kopyala**olarak değiştirin.

    ![Uygulama ayarlarını çıktıya kopyalama](./media/tutorial-csharp-create-first-app/azure-search-copy-if-newer.png)

## <a name="model-data-structures"></a>Model veri yapıları

Modeller (C# sınıfları), MVC (model, görünüm, denetleyici) mimarisini kullanarak istemci (görünüm), sunucu (denetleyici) ve azure bulutu arasındaki verileri iletmek için kullanılır. Genellikle, bu modeller erişilen verilerin yapısını yansıtır. Ayrıca, görünüm/denetleyici iletişimini işlemek için bir modele ihtiyacımız vardır.

1. Solution Explorer'ı kullanarak projenizin **Modeller** klasörünü açın ve orada bir varsayılan model görürsünüz: **ErrorViewModel.cs.**

2. **Modeller** klasörüne sağ tıklayın ve **Sonra Yeni Öğe** **Ekle'yi** seçin. Daha sonra, görünen iletişim kutusunda, **ASP.NET Core'u** ve ilk seçenek **Olan Sınıf'ı**seçin. .cs dosyasını Hotel.cs yeniden adlandırın ve **Ekle'yi**tıklatın. Hotel.cs tüm içeriğini aşağıdaki kodla değiştirin. Sınıfın **Adres** ve **Oda** üyelerine dikkat edin, bu alanlar sınıfların kendileridir, bu yüzden onlar için de modellere ihtiyacımız olacak.

    ```cs
    using System;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Microsoft.Spatial;
    using Newtonsoft.Json;

    namespace FirstAzureSearchApp.Models
    {
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
    }
    ```

3. Dosyanın Address.cs adı dışında **Adres** sınıfı için bir model oluşturma işlemini izleyin. İçeriği aşağıdakilerle değiştirin.

    ```cs
    using Microsoft.Azure.Search;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Address
        {
            [IsSearchable]
            public string StreetAddress { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string City { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string StateProvince { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string PostalCode { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string Country { get; set; }
        }
    }
    ```

4. Ve yine, **dosyayı** Room.cs adlandırarak Oda sınıfını oluşturmak için aynı işlemi izleyin. Yine, aşağıdaki ile içeriğini değiştirin.

    ```cs
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Newtonsoft.Json;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Room
        {
            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnMicrosoft)]

            public string Description { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
            [JsonProperty("Description_fr")]
            public string DescriptionFr { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string Type { get; set; }

            [IsFilterable, IsFacetable]
            public double? BaseRate { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string BedOptions { get; set; }

            [IsFilterable, IsFacetable]

            public int SleepsCount { get; set; }

            [IsFilterable, IsFacetable]
            public bool? SmokingAllowed { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string[] Tags { get; set; }
        }
    }
    ```

5. Azure'da [_karmaşık türler_](search-howto-complex-data-types.md)olarak bilinen **Otel,** **Adres**ve **Oda** sınıfları kümesi, Azure Bilişsel Arama'nın önemli bir özelliğidir. Karmaşık türler sınıfların ve alt sınıfların çok daha derin düzeyleri olabilir ve _basit türleri_ (yalnızca ilkel üyeleri içeren bir sınıf) kullanarak daha çok daha karmaşık veri yapıları temsil edilmesini sağlar. Bir modele daha ihtiyacımız var, bu yüzden yeni bir model sınıfı oluşturma işleminden tekrar geç, ancak bu kez sınıfı SearchData.cs çağırın ve varsayılan kodu aşağıdakilerle değiştirin.

    ```cs
    using Microsoft.Azure.Search.Models;

    namespace FirstAzureSearchApp.Models
    {
        public class SearchData
        {
            // The text to search for.
            public string searchText { get; set; }

            // The list of results.
            public DocumentSearchResult<Hotel> resultList;
        }
    }
    ```

    Bu sınıf, kullanıcının girişini **(searchText)** ve aramaçıktısını **(resultList)** içerir. Çıktı türü önemlidir, **DocumentSearchResult&lt;Hotel&gt;**, Bu tür tam olarak arama sonuçları eşleşir gibi, ve biz görünümüne bu referans geçmek gerekir.



## <a name="create-a-web-page"></a>Web sayfası oluşturma

Oluşturduğunuz proje varsayılan olarak bir dizi istemci görünümü oluşturur. Tam görünümler kullandığınız Core .NET sürümüne bağlıdır (bu örnekte 2.1 kullanıyoruz). Hepsi projenin **Görünümler** klasöründe. Yalnızca Index.cshtml dosyasını **(Görünümler/Giriş** klasöründe) değiştirmeniz gerekir.

Index.cshtml'in içeriğini tamamen silin ve aşağıdaki adımlarla dosyayı yeniden oluşturun.

1. Görünümde iki küçük resim kullanıyoruz. GitHub projesindeki resimlerde kendi kullanın veya kopyalayabilirsiniz: azure-logo.png ve search.png. Bu iki resim **wwwroot/images** klasörüne yerleştirilmelidir.

2. Index.cshtml'in ilk satırı, oluşturduğumuz **SearchData** modeli olan istemci (görünüm) ve sunucu (denetleyici) arasında veri iletmek için kullandığımız modele başvurulmalıdır. Bu satırı Index.cshtml dosyasına ekleyin.

    ```cs
    @model FirstAzureSearchApp.Models.SearchData
    ```

3. Görünüm için bir başlık girmek standart bir uygulamadır, bu nedenle bir sonraki satırlar olmalıdır:

    ```cs
    @{
        ViewData["Title"] = "Home Page";
    }
    ```

4. Başlığın ardından, kısa süre içinde oluşturacağınız html stil sayfasına bir başvuru girin.

    ```cs
    <head>
        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

5. Şimdi manzaranın ete. Unutulmaması gereken önemli bir şey, görünümün iki durumla başa çıkabilmek zorunda olduğudur. İlk olarak, uygulama ilk başlatıldığında ve kullanıcı henüz herhangi bir arama metni girmediğinde ekranı işlemesi gerekir. İkinci olarak, kullanıcı tarafından tekrarlanan kullanım için arama metin kutusuna ek olarak sonuçların görüntülenmesini işlemelidir. Bu iki durumu işlemek için, görünüme sağlanan modelin null olup olmadığını kontrol etmek gerekir. Null modeli, iki durumdan ilkinde olduğumuzu gösterir (uygulamanın ilk çalıştırış). Index.cshtml dosyasına aşağıdakileri ekleyin ve yorumları okuyun.

    ```cs
    <body>
    <h1 class="sampleTitle">
        <img src="~/images/azure-logo.png" width="80" />
        Hotels Search
    </h1>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        // Display the search text box, with the search icon to the right of it.
        <div class="searchBoxForm">
            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
        </div>

        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Results.Count) Results
            </p>

            @for (var i = 0; i < Model.resultList.Results.Count; i++)
            {
                // Display the hotel name and description.
                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
            }
        }
    }
    </body>
    ```

6. Son olarak, stil sayfası ekleyin. Visual Studio'da, **Dosya** menüsünde **Yeni/Dosya** sonra **Stil Sayfası 'nı** **(Genel** vurgulanmış) seçin. Varsayılan kodu aşağıdakilerle değiştirin. Biz daha ayrıntılı olarak bu dosyaya gidiyor olmayacak, stilleri standart HTML vardır.

    ```html
    textarea.box1 {
        width: 648px;
        height: 30px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
    }

    textarea.box2 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }

    .sampleTitle {
        font: 32px/normal 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 32px;
        text-align: left;
    }

    .sampleText {
        font: 16px/bold 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 14px;
        text-align: left;
        height: 30px;
    }

    .searchBoxForm {
        width: 648px;
        box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 2px 4px 0 rgba(0,0,0,.16);
        background-color: #fff;
        display: inline-block;
        border-collapse: collapse;
        border-spacing: 0;
        list-style: none;
        color: #666;
    }

    .searchBox {
        width: 568px;
        font-size: 16px;
        margin: 5px 0 1px 20px;
        padding: 0 10px 0 0;
        border: 0;
        max-height: 30px;
        outline: none;
        box-sizing: content-box;
        height: 35px;
        vertical-align: top;
    }

    .searchBoxSubmit {
        background-color: #fff;
        border-color: #fff;
        background-image: url(/images/search.png);
        background-repeat: no-repeat;
        height: 20px;
        width: 20px;
        text-indent: -99em;
        border-width: 0;
        border-style: solid;
        margin: 10px;
        outline: 0;
    }
    ```

7. Stylesheet dosyasını hotels.css olarak, varsayılan site.css dosyasının yanında wwwroot/css klasörüne kaydedin.

Bu görüşümüzü tamamlar. İyi bir ilerleme kaydediyoruz. Modeller ve görünümler tamamlanır, sadece denetleyici her şeyi birbirine bağlamak için bırakılır.

## <a name="define-methods"></a>Yöntemleri tanımlama

Varsayılan olarak oluşturulan tek denetleyicinin **(Home Controller)** içeriğini değiştirmemiz gerekir.

1. HomeController.cs dosyasını açın ve **aşağıdakilerle birlikte ifadeleri değiştirin.**

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using FirstAzureSearchApp.Models;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    ```

### <a name="add-index-methods"></a>Dizin Ekle yöntemleri

İki **Dizin** yöntemine ihtiyacımız var, biri parametre alma (uygulamailk açıldığında olduğu gibi) ve bir modelin parametre olarak alınması (kullanıcı arama metnine girdiğinde için). Bu yöntemlerden ilki varsayılan olarak oluşturulur. 

1. Varsayılan **Index()** yönteminden sonra aşağıdaki yöntemi ekleyin.

    ```cs
        [HttpPost]
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the Azure Cognitive Search call.
                await RunQueryAsync(model);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

    Yöntemin **async** bildirimine dikkat edin ve **RunQueryAsync**için **bekleyen** arama . Bu anahtar kelimeler çağrılarımızı eşzamanlı hale getirmekle ilgilenir ve böylece sunucudaki iş parçacıklarını engellemekten kaçınır.

    **Catch** bloğu varsayılan olarak bizim için oluşturulan hata modelini kullanır.

### <a name="note-the-error-handling-and-other-default-views-and-methods"></a>Hata işleme ve diğer varsayılan görünümler ve yöntemlere dikkat edin

.NET Core'un hangi sürümünü kullandığınıza bağlı olarak, varsayılan olarak biraz farklı bir varsayılan görünüm kümesi oluşturulur. .NET Core 2.1 için varsayılan görünümler Dizin, Hakkında, İletişim, Gizlilik ve Hata'dır. .NET Core 2.2 için, örneğin varsayılan görünümler Dizin, Gizlilik ve Hata'dır. Her iki durumda da, uygulamayı çalıştırırken bu varsayılan sayfaları görüntüleyebilir ve denetleyicide nasıl işleneceğini inceleyebilirsiniz.

Bu öğreticide hata görünümünü daha sonra sınayacağız.

GitHub örneğinde, kullanılmayan görünümleri ve bunların ilişkili eylemlerini sildik.

### <a name="add-the-runqueryasync-method"></a>RunQueryAsync yöntemini ekle

Azure Bilişsel Arama **araması, RunQueryAsync** yöntemimizde özetlenir.

1. Önce Azure hizmetini ayarlamak için bazı statik değişkenler ve bunları başlatmak için bir çağrı ekleyin.

    ```cs
        private static SearchServiceClient _serviceClient;
        private static ISearchIndexClient _indexClient;
        private static IConfigurationBuilder _builder;
        private static IConfigurationRoot _configuration;

        private void InitSearch()
        {
            // Create a configuration using the appsettings file.
            _builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            _configuration = _builder.Build();

            // Pull the values from the appsettings.json file.
            string searchServiceName = _configuration["SearchServiceName"];
            string queryApiKey = _configuration["SearchServiceQueryApiKey"];

            // Create a service and index client.
            _serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(queryApiKey));
            _indexClient = _serviceClient.Indexes.GetClient("hotels");
        }
    ```

2. Şimdi, **RunQueryAsync** yönteminin kendisini ekleyin.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                // Enter Hotel property names into this list so only these values will be returned.
                // If Select is empty, all values will be returned, which can be inefficient.
                Select = new[] { "HotelName", "Description" }
            };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // Display the results.
            return View("Index", model);
        }
    ```

    Bu yöntemde, önce Azure yapılandırmamızın başlatıldığından, sonra da bazı arama parametrelerini ayarladığımızdan emin oluyoruz. **Seç** parametresi'ndeki alanların adları **otel** sınıfındaki özellik adlarıyla tam olarak eşleşir. **Select** parametresini dışarıda bırakmak mümkündür, bu durumda tüm özellikler döndürülür. Ancak, yalnızca verilerin bir alt kümesiyle ilgileniyorsak, **Select** parametrelerini belirlemeniz verimsizdir. İlgilendiğimiz özellikleri belirterek, yalnızca bu özellikler döndürülür.

    Asynchronous arama çağrısı (**model.resultList = _indexClient.Documents.SearchAsync&lt;Hotel&gt;(model.searchText, parametreler);**) bu öğretici ve uygulama hakkında ne bekliyor. **DocumentSearchResult** sınıfı ilginç bir ve iyi bir fikir (uygulama çalışırken) burada bir kesme noktası ayarlamak ve **model.resultList**içeriğini incelemek için bir hata ayıklama kullanmaktır. İstediğiniz verileri sağlayan sezgisel olduğunu bulmalısınız, ve çok fazla değil.

Şimdi doğruluk anı.

### <a name="test-the-app"></a>Uygulamayı test edin

Şimdi, uygulamanın doğru çalıştığını kontrol edelim.

1. **Hata Ayıklama yapmadan Hata Ayıklama/Başlat'ı** seçin veya F5 tuşuna basın. İşleri doğru şekilde kodladınsa, ilk Dizin görünümünü alırsınız.

     ![Uygulamayı açma](./media/tutorial-csharp-create-first-app/azure-search-index.png)

2. "Plaj" (veya akla gelen herhangi bir metin) gibi metinleri girin ve arama simgesini tıklatın. Bazı sonuçlar almalısın.

     !["Plaj" aranıyor](./media/tutorial-csharp-create-first-app/azure-search-beach.png)

3. "Beş yıldız"a girmeyi dene. Nasıl sonuç alabildiğinize dikkat edin. Daha sofistike bir arama "lüks" için eşanlamlı olarak "beş yıldız" tedavi ve bu sonuçları döndürecek. Eş anlamlıların kullanımı Azure Bilişsel Arama'da kullanılabilir, ancak ilk eğitimlerde bunu ele alamayacağız.
 
4. Arama metni olarak "sıcak" girmeyi deneyin. "Otel" kelimesi bulunan girişleri iade _etmez._ Birkaç sonuç döndürülse de, aramamız yalnızca tüm sözcükleri bulmaktır.

5. Diğer kelimeleri deneyin: "havuz", "güneş ışığı", "görünüm", ve her neyse. Azure Bilişsel Arama'nın en basit, ancak yine de ikna edici düzeyde çalıştığını göreceksiniz.

## <a name="test-edge-conditions-and-errors"></a>Test kenarı koşulları ve hataları

Hata işleme özelliklerimizin, işler mükemmel çalışıyor olsa bile olması gerektiği gibi çalıştığını doğrulamak önemlidir. 

1. **Dizin** yönteminde, **{call'u denedikten** sonra yeni **Özel Durum At()** satırıgirin. Bu özel durum, metin üzerinde arama yaptığımızda bir hata zorlar.

2. Uygulamayı çalıştırın, arama metni olarak "çubuk" girin ve arama simgesine tıklayın. Özel durum hata görünümüne neden olmalıdır.

     ![Hata zorlama](./media/tutorial-csharp-create-first-app/azure-search-error.png)

    > [!Important]
    > Hata sayfalarında iç hata numaralarını döndürmek bir güvenlik riski olarak kabul edilir. Uygulamanız genel kullanım için tasarlandıysa, bir hata oluştuğunda ne iade edilene dair güvenli ve en iyi uygulamalar hakkında bazı incelemeler yapın.

3. Hata işleme nin olması gerektiği gibi çalıştığından memnun olduğunuzda **yeni Özel Durum At()** kaldır.

## <a name="takeaways"></a>Paketler

Bu projeden aşağıdaki paketleri göz önünde bulundurun:

* Azure Bilişsel Arama araması kısa ve kısa dır ve sonuçları yorumlamak kolaydır.
* Eşkron çağrılar denetleyiciye az miktarda karmaşıklık ekler, ancak kaliteli uygulamalar geliştirmek istiyorsanız en iyi uygulamadır.
* Bu uygulama, **searchParametreler'de**ayarlananlarla tanımlanan basit bir metin araması gerçekleştirdi. Ancak, bu bir sınıf bir arama sofistike eklemek birçok üye ile doldurulabilir. Bu uygulamayı çok daha güçlü hale getirmek için çok fazla ek iş gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

Azure Bilişsel Arama'yı kullanarak en iyi kullanıcı deneyimini sağlamak için, özellikle sayfalama (sayfa numaralarını veya sonsuz kaydırma yı kullanarak) ve otomatik tamamlama/öneriler olmak üzere daha fazla özellik eklememiz gerekir. Ayrıca daha karmaşık arama parametrelerini de göz önünde bulundurmalıyız (örneğin, belirli bir noktanın belirli bir yarıçapı içindeki otellerde coğrafi aramalar ve arama sonuçları siparişi).

Bu sonraki adımlar öğreticiler bir dizi ele alınır. Sayfalama yla başlayalım.

> [!div class="nextstepaction"]
> [C# Tutorial: Arama sonuçları pagination - Azure Bilişsel Arama](tutorial-csharp-paging.md)


