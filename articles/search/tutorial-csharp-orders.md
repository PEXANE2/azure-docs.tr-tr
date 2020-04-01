---
title: Sonuçları sıralama konusunda C# eğitimi
titleSuffix: Azure Cognitive Search
description: Bu öğretici, arama sonuçlarının nasıl sıralanın nasıl sıralanın gösterildiğini gösterir. Bir önceki otel projesi, birincil mülkiyet, ikincil mülkiyet tarafından sipariş ve artırma kriterleri eklemek için bir puanlama profili içerir inşa eder.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/10/2020
ms.openlocfilehash: 812085a5a4b3e8d1233f19c947d2fd5e433f6ab7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77121551"
---
# <a name="c-tutorial-order-the-results---azure-cognitive-search"></a>C# öğreticisi: Sonuçları sipariş edin - Azure Bilişsel Arama

Öğreticiler serimizde bu noktaya kadar, sonuçlar döndürülür ve varsayılan sırada görüntülenir. Bu, verilerin bulunduğu sıra olabilir veya sipariş parametreleri belirtilmediğinde kullanılacak varsayılan _puanlama profili_ tanımlanabilir. Bu öğreticide, birincil bir özelliğe dayalı sonuçları nasıl sıralayabildiğimize ve daha sonra aynı birincil özelliğe sahip sonuçlar alacaktır, ikincil bir özellik üzerinde bu seçimin nasıl sıralandığına. Sayısal değerlere dayalı sıralamaya alternatif olarak, son örnek, özel bir puanlama profiline dayalı siparişin nasıl yapılacağını gösterir. Biz de _karmaşık türlerin_ekran içine biraz daha derin gidecek.

İade edilen sonuçları kolayca karşılaştırmak için, bu proje [C# Tutorial: Search sonuçları pagination - Azure Bilişsel Arama](tutorial-csharp-paging.md) öğretici oluşturulan sonsuz kaydırma projesi üzerine inşa edilmiştir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Tek bir özelliğe dayalı sipariş sonuçları
> * Birden çok özelliği temel alan sipariş sonuçları
> * Sonuçları coğrafi bir noktadan uzaklık durumuna göre filtreleme
> * Puanlama profiline dayalı sipariş sonuçları

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

[C# Tutorial: Search sonuçları nın](tutorial-csharp-paging.md) sonsuz kaydırma sürümüne sahip olun - Azure Bilişsel Arama projesi çalışır durumda. Bu proje ya kendi sürümünüz olabilir ya da GitHub'dan yükleyebilir: [İlk uygulamayı oluşturun.](https://github.com/Azure-Samples/azure-search-dotnet-samples)

## <a name="order-results-based-on-one-property"></a>Tek bir özelliğe dayalı sipariş sonuçları

Sonuçları tek bir mülke göre sipariş ettiğimizde, örneğin otel derecelendirmesi, sadece sipariş edilen sonuçları değil, aynı zamanda siparişin doğru olduğunun da teyidini isteriz. Başka bir deyişle, derecelendirme ye sıralanırsak, derecelendirmeyi görünümde görüntülememiz gerekir.

Bu eğitimde, biz de her otel için, sonuçların, en ucuz oda oranı ve en pahalı oda oranı ekranbiraz daha ekleyeceğiz. Sipariş vermeye daldığımızda, neyi sipariş ettiğimizden emin olmak için değerler de ekleyeceğiz.

Sıralamayı etkinleştirmek için modellerin hiçbirini değiştirmeye gerek yoktur. Görünümün ve denetleyicinin güncellenmesi gerekir. Ev kumandasını açarak başlayın.

### <a name="add-the-orderby-property-to-the-search-parameters"></a>OrderBy özelliğini arama parametrelerine ekleme

1. Tek bir sayısal özelliği temel alarak sonuçları sıralamak için gereken tek şey, **OrderBy** parametresini özelliğin adına ayarlamaktır. **Index(SearchData modeli)** yönteminde, arama parametrelerine aşağıdaki satırı ekleyin.

    ```cs
        OrderBy = new[] { "Rating desc" },
    ```

    >[!Note]
    > Bunu net yapmak için özelliğe **asc** ekleyebilirsiniz rağmen varsayılan sipariş yükseliyor. Azalan sıra **desc**eklenerek belirtilir.

2. Şimdi uygulamayı çalıştırın ve herhangi bir ortak arama terimi girin. Sonuçlar doğru sırada olabilir veya olmayabilir, geliştirici olarak ne de siz, kullanıcı, sonuçları doğrulamak için herhangi bir kolay yolu vardır!

3. Sonuçların reytinge göre sıralı olduğunu açıklığa kavuşturalım. İlk olarak, hotels.css dosyasındaki **box1** ve **box2** sınıflarını aşağıdaki sınıflarla değiştirin (bu sınıflar bu öğretici için ihtiyacımız olan yeni sınıflardır).

    ```html
    textarea.box1A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box1B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box2A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box2B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box3 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }
    ```

    >[!Tip]
    >Tarayıcılar genellikle css dosyalarını önbelleğe almakta ve bu durum eski bir css dosyasının kullanılmasına ve bunların göz ardı edilmesine neden olabilir. Bu iyi bir şekilde bu bağlantıya bir sürüm parametresi ile bir sorgu dizesi eklemektir. Örnek:
    >
    >```html
    >   <link rel="stylesheet" href="~/css/hotels.css?v1.1" />
    >```
    >
    >Tarayıcınız tarafından eski bir css dosyasının kullanıldığını düşünüyorsanız sürüm numarasını güncelleştirin.

4. **Index(SearchData modeli)** yönteminde **Derecelendirme** özelliğini **Seç** parametresine ekleyin.

    ```cs
    Select = new[] { "HotelName", "Description", "Rating"},
    ```

5. Görünümü açın (index.cshtml) ve işleme döngüsünün**&lt;(!-- Otel&gt;verilerini gösterin. --**) aşağıdaki kodla değiştirin.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

6. Derecelendirmenin hem ilk görüntülenen sayfada hem de sonsuz kaydırma yoluyla çağrılan sonraki sayfalarda kullanılabilir olması gerekir. Bu iki durumdan ikincisi için, hem denetleyicideki **Sonraki** eylemi hem de görünümdeki **kaydırılan** işlevi güncelleştirmemiz gerekir. Denetleyiciden başlayarak, **Sonraki** yöntemi aşağıdaki kodla değiştirin. Bu kod, derecelendirme metnini oluşturur ve iletır.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";

                // Add three strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

7. Şimdi, derecelendirme metnini görüntülemek için görünümde **kaydırılan** işlevi güncelleştirin.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 3) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 2] + '</textarea>';
                            }
                        });
                    }
                }
            </script>

    ```

8. Şimdi uygulamayı yeniden çalıştırın. "Wifi" gibi herhangi bir yaygın terimi arayın ve sonuçların azalan otel derecelendirmesi sırasına göre sıralı olduğunu doğrulayın.

    ![Derecelendirmeye göre sıralama](./media/tutorial-csharp-create-first-app/azure-search-orders-rating.png)

    Birkaç otelin aynı derecelendirmeye sahip olduğunu fark edeceksiniz ve bu nedenle ekrandaki görünümleri yine verilerin bulunduğu sıradır ve bu rasgelebir durumdur.

    İkinci bir sipariş düzeyi eklemeyi başlamadan önce, oda fiyatları aralığını görüntülemek için biraz kod ekleyelim. Biz hem karmaşık bir _tür_veri ayıklama göstermek için bu kodu ekliyoruz, ve aynı zamanda biz fiyat (en ucuz ilk belki) dayalı sipariş sonuçları tartışabilirsiniz.

### <a name="add-the-range-of-room-rates-to-the-view"></a>Görünüme oda fiyatları aralığını ekleyin

1. Hotel.cs modeline en ucuz ve en pahalı oda fiyatı içeren özellikleri ekleyin.

    ```cs
        // Room rate range
        public double cheapest { get; set; }
        public double expensive { get; set; }
    ```

2. **Ev denetleyicisinde, Index(SearchData modeli)** eyleminin sonundaki oda oranlarını hesaplayın. Geçici verilerin depolanmasından sonra hesaplamaları ekleyin.

    ```cs
                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;

                // Calculate the room rate ranges.
                for (int n = 0; n < model.resultList.Results.Count; n++)
                {
                    // Calculate room rates.
                    var cheapest = 0d;
                    var expensive = 0d;

                    for (var r = 0; r < model.resultList.Results[n].Document.Rooms.Length; r++)
                    {
                        var rate = model.resultList.Results[n].Document.Rooms[r].BaseRate;
                        if (rate < cheapest || cheapest == 0)
                        {
                            cheapest = (double)rate;
                        }
                        if (rate > expensive)
                        {
                            expensive = (double)rate;
                        }
                    }
                    model.resultList.Results[n].Document.cheapest = cheapest;
                    model.resultList.Results[n].Document.expensive = expensive;
                }
    ```

3. **Denetleyicinin** **Dizin (SearchData modeli)** eylem yöntemindeki **Seç** parametresine Odalar özelliğini ekleyin.

    ```cs
     Select = new[] { "HotelName", "Description", "Rating", "Rooms" },
    ```

4. Sonuçların ilk sayfasının fiyat aralığını görüntülemek için görünümdeki işleme döngüünü değiştirin.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"rates{i}" , rateText, new { @class = "box2A" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

5. Sonraki sonuçlar sayfaları için fiyat aralığını iletmek için ev denetleyicisinde **Sonraki** yöntemi değiştirin.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";
                var rateText = $"Rates from ${model.resultList.Results[n].Document.cheapest} to ${model.resultList.Results[n].Document.expensive}";

                // Add strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

6. Oda fiyatları metnini işlemek için görünümde **kaydırılan** işlevi güncelleştirin.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 4) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                            }
                        });
                    }
                }
            </script>
    ```

7. Uygulamayı çalıştırın ve oda fiyat aralıklarının görüntülendiğini doğrulayın.

    ![Oda fiyat aralıklarının görüntülenmesi](./media/tutorial-csharp-create-first-app/azure-search-orders-rooms.png)

Arama parametrelerinin **OrderBy** özelliği, odalar zaten fiyata göre sıralanmış olsa bile, en ucuz oda ücretini sağlamak için **Rooms.BaseRate** gibi bir girişi kabul etmez. Bu durumda, odalar oran sıralanmış değildir. Oda fiyatına göre sıralanan örnek veri setinde ki otelleri görüntülemek için, sonuçları ev denetleyicinizde sıralamanız ve bu sonuçları istediğiniz sırada görünüme göndermeniz gerekir.

## <a name="order-results-based-on-multiple-values"></a>Birden çok değere dayalı sipariş sonuçları

Şimdi soru, aynı derecelendirmeye sahip oteller arasında nasıl ayrım yapılacılığınız. Bir iyi yolu otel yenilenmiş son kez temelinde sipariş olacaktır. Başka bir deyişle, otel ne kadar yakın zamanda yenilenirse, sonuçlar da o kadar yüksek görünür.

1. İkinci bir sıralama düzeyi eklemek için, **Index(SearchData modeli)** yöntemindeki **OrderBy** ve **Select** özelliklerini **LastRenovationDate** özelliğini içerecek şekilde değiştirin.

    ```cs
    OrderBy = new[] { "Rating desc", "LastRenovationDate desc" },
    Select = new[] { "HotelName", "Description", "Rating", "Rooms", "LastRenovationDate" },
    ```

    >[!Tip]
    >Herhangi bir sayıda özellik **SiparişBy** listesine girilebilir. Oteller aynı derecelendirme ve yenileme tarihine sahipse, aralarında ayrım yapmak için üçüncü bir tesise girilebilir.

2. Yine, sadece sipariş doğru olduğundan emin olmak için, görünümünde yenileme tarihini görmek gerekir. Bir yenileme gibi bir şey için, muhtemelen sadece yıl gereklidir. Görünümdeki işleme döngüünü aşağıdaki kodla değiştirin.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                    var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"rates{i}" , rateText, new { @class = "box2A" })
                    @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

3. Son yenileme tarihinin yıl bileşenini iletmek için ev denetleyicisinde **Sonraki** yöntemi değiştirin.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";
                var rateText = $"Rates from ${model.resultList.Results[n].Document.cheapest} to ${model.resultList.Results[n].Document.expensive}";
                var lastRenovatedText = $"Last renovated: {model.resultList.Results[n].Document.LastRenovationDate.Value.Year}";

                // Add strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(lastRenovatedText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. Yenileme metnini görüntülemek için görünümde **kaydırılan** işlevi değiştirin.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 5) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                            }
                        });
                    }
                }
            </script>
    ```

5. Uygulamayı çalıştırın. "Havuz" veya "görünüm" gibi ortak bir terimüzerinde arama yapın ve aynı derecelendirmeye sahip otellerin artık azalan yenileme tarihinde görüntülendiğini doğrulayın.

    ![Yenileme tarihinde sipariş](./media/tutorial-csharp-create-first-app/azure-search-orders-renovation.png)

## <a name="filter-results-based-on-a-distance-from-a-geographical-point"></a>Sonuçları coğrafi bir noktadan uzaklık durumuna göre filtreleme

Derecelendirme ve yenileme tarihi, azalan sırada en iyi görüntülenen özelliklere örnektir. Alfabetik bir liste, artan sıranın iyi bir kullanımına bir örnek olacaktır (örneğin, sadece bir **OrderBy** özelliği varsa ve **HotelName** olarak ayarlanmışsa, alfabetik bir sıra görüntülenir). Ancak, örnek verilerimiz için, coğrafi bir noktadan uzaklık daha uygun olacaktır.

Sonuçları coğrafi uzaklık lara göre görüntülemek için birkaç adım gerekir.

1. Boylam, enlem ve yarıçap parametrelerine sahip bir filtre girerek, belirli bir yarıçapın dışında olan tüm otelleri belirli bir noktadan filtreleyin. Boylam ilk OLARAK POINT fonksiyonuna verilir. Yarıçapı kilometrelerce uzakta.

    ```cs
        // "Location" must match the field name in the Hotel class.
        // Distance (the radius) is in kilometers.
        // Point order is Longitude then Latitude.
        Filter = $"geo.distance(Location, geography'POINT({model.lon} {model.lat})') le {model.radius}",
    ```

2. Yukarıdaki filtre mesafeye göre sonuçları sipariş _etmez,_ sadece aykırı kaldırır. Sonuçları sıralamak için, geoDistance yöntemini belirten bir **OrderBy** ayarı girin.

    ```cs
    OrderBy = new[] { $"geo.distance(Location, geography'POINT({model.lon} {model.lat})') asc" },
    ```

3. Sonuçlar bir mesafe filtresi kullanılarak Azure Bilişsel Arama tarafından döndürülse de, verilerle belirtilen nokta arasındaki hesaplanan uzaklık _döndürülmez._ Sonuçlarda görüntülemek istiyorsanız, bu değeri görünümde veya denetleyicide yeniden hesaplayın.

    Aşağıdaki kod iki lat/lon noktası arasındaki mesafeyi hesaplar.

    ```cs
        const double EarthRadius = 6371;

        public static double Degrees2Radians(double deg)
        {
            return deg * Math.PI / 180;
        }

        public static double DistanceInKm( double lat1,  double lon1, double lat2, double lon2)
        {
            double dlon = Degrees2Radians(lon2 - lon1);
            double dlat = Degrees2Radians(lat2 - lat1);

            double a = (Math.Sin(dlat / 2) * Math.Sin(dlat / 2)) + Math.Cos(Degrees2Radians(lat1)) * Math.Cos(Degrees2Radians(lat2)) * (Math.Sin(dlon / 2) * Math.Sin(dlon / 2));
            double angle = 2 * Math.Atan2(Math.Sqrt(a), Math.Sqrt(1 - a));
            return angle * EarthRadius;
        }
    ```

4. Şimdi bu kavramları birbirine bağlamanız gerekiyor. Ancak, bu kod parçacıkları kadar bizim öğretici gider, bir harita tabanlı uygulama bina okuyucu için bir egzersiz olarak bırakılır. Bu örneği daha da ileri götürmek için, yarıçapı olan bir şehir adı girmeyi veya haritada bir noktayı bulmayı ve bir yarıçap seçmeyi düşünün. Bu seçenekleri daha fazla araştırmak için aşağıdaki kaynaklara bakın:

* [Azure Haritalar Belgeleri](https://docs.microsoft.com/azure/azure-maps/)
* [Azure Haritalar arama hizmetini kullanarak bir adres bulma](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address)

## <a name="order-results-based-on-a-scoring-profile"></a>Puanlama profiline dayalı sipariş sonuçları

Öğreticide şimdiye kadar verilen örnekler, _kesin_ bir sipariş süreci sağlayarak sayısal değerler (derecelendirme, yenileme tarihi, coğrafi uzaklık) hakkında nasıl sırayapılacağını göstermektedir. Ancak, bazı aramalar ve bazı veriler iki veri öğesi arasında bu kadar kolay bir karşılaştırma ya da bu tür bir karşılaştırmaya kendilerini ödünç vermezler. Azure Bilişsel Arama _puanlama_kavramını içerir. _Puanlama profilleri,_ önce hangilerinin görüntüleneceğine karar vermek için metin tabanlı verileri karşılaştırırken en değerli olan, daha karmaşık ve nitel karşılaştırmalar sağlamak için kullanılabilecek bir veri kümesi için belirtilebilir.

Puanlama profilleri kullanıcılar tarafından değil, genellikle bir veri kümesinin yöneticileri tarafından tanımlanır. Otel verilerinde çeşitli puanlama profilleri hazırlanmıştır. Puanlama profilinin nasıl tanımlandığına bakalım, sonra bunları aramak için kod yazmayı deneyelim.

### <a name="how-scoring-profiles-are-defined"></a>Puanlama profilleri nasıl tanımlanır?

Puanlama profillerinin üç örneğine bakalım ve her birinin sonuç sırasını nasıl _etkilemesi gerektiğini_ düşünelim. Bir uygulama geliştiricisi olarak, bu profilleri yazmayın, veri yöneticisi tarafından yazılır, ancak sözdizimine bakmak yararlıdır.

1. Bu, herhangi bir **OrderBy** veya **ScoringProfile** parametresi belirtmediğinizde kullanılan oteller veri kümesi için varsayılan puanlama profilidir. Bu profil, arama metni otel adı, açıklama veya etiket listesinde (olanaklar) mevcutsa, bir otelin _puanını_ artırır. Puanlamanın ağırlıklarının belirli alanları nasıl desteklediğine dikkat edin. Arama metni aşağıda listelenmemiş başka bir alanda görünüyorsa, ağırlığı 1 olacaktır. Açıkçası, yüksek puan, erken bir sonuç görünümünde görünür.

     ```cs
    {
            "name": "boostByField",
            "text": {
                "weights": {
                    "HotelName": 2,
                    "Description": 1.5,
                    "Description_fr": 1.5,
                    "Tags": 3
                }
            }
        }

    ```

2. Sağlanan bir parametre etiket listesinden birini veya birkaçını içeriyorsa (ki biz buna "olanaklar" diyoruz) aşağıdaki puanlama profili skoru önemli ölçüde artırır. Bu profilin temel noktası, metin içeren bir parametre nin sağlanması _gerektiğidir._ Parametre boşsa veya sağlanmazsa, bir hata atılır.
 
    ```cs
            {
            "name": "boostAmenities",
            "functions": [
                {
                    "type": "tag",
                    "fieldName": "Tags",
                    "boost": 5,
                    "tag": {
                        "tagsParameter": "amenities"
                    }
                }
            ]
        }
    ```

3. Bu üçüncü örnekte, derecelendirme skora önemli bir destek verir. Son yenilenen tarih de puanı artıracak, ancak bu veriler geçerli tarihten itibaren 730 gün (2 yıl) içinde kalırsa.

    ```cs
            {
            "name": "renovatedAndHighlyRated",
            "functions": [
                {
                    "type": "magnitude",
                    "fieldName": "Rating",
                    "boost": 20,
                    "interpolation": "linear",
                    "magnitude": {
                        "boostingRangeStart": 0,
                        "boostingRangeEnd": 5,
                        "constantBoostBeyondRange": false
                    }
                },
                {
                    "type": "freshness",
                    "fieldName": "LastRenovationDate",
                    "boost": 10,
                    "interpolation": "quadratic",
                    "freshness": {
                        "boostingDuration": "P730D"
                    }
                }
            ]
        }

    ```

    Şimdi, bu profiller düşündüğümüz gibi çalışıp çalışmadıklarını görelim!

### <a name="add-code-to-the-view-to-compare-profiles"></a>Profilleri karşılaştırmak için görünüme kod ekleme

1. index.cshtml dosyasını açın ve &lt;&gt; gövde bölümünü aşağıdaki kodla değiştirin.

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
                        Hotels Search - Order Results
                    </h1>
                </td>
            </tr>
            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it. -->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
                    </div>

                    <div class="searchBoxForm">
                        <b>&nbsp;Order:&nbsp;</b>
                        @Html.RadioButtonFor(m => m.scoring, "Default") Default&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "RatingRenovation") By numerical Rating&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "boostAmenities") By Amenities&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "renovatedAndHighlyRated") By Renovated date/Rating profile&nbsp;&nbsp;
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">

                        @if (Model != null && Model.facetText != null)
                        {
                            <h5 class="facetheader">Amenities:</h5>
                            <ul class="facetlist">
                                @for (var c = 0; c < Model.facetText.Length; c++)
                                {
                                    <li> @Html.CheckBoxFor(m => m.facetOn[c], new { @id = "check" + c.ToString() }) @Model.facetText[c] </li>
                                }

                            </ul>
                        }
                    </div>
                </td>
                <td>
                    @if (Model != null && Model.resultList != null)
                    {
                        // Show the total result count.
                        <p class="sampleText">
                            @Html.DisplayFor(m => m.resultList.Count) Results <br />
                        </p>

                        <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                            <!-- Show the hotel data. -->
                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                                var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                                var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);
                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel details.
                                @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                                @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                                @Html.TextArea($"rates{i}", rateText, new { @class = "box2A" })
                                @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
                            }
                        </div>

                        <script>
                            function scrolled() {
                                if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                                    $.getJSON("/Home/Next", function (data) {
                                        var div = document.getElementById('myDiv');

                                        // Append the returned data to the current list of hotels.
                                        for (var i = 0; i < data.length; i += 5) {
                                            div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                            div.innerHTML += '<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                            div.innerHTML += '<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                                        }
                                    });
                                }
                            }
                        </script>
                    }
                </td>
            </tr>
        </table>
    }
    </body>
    ```

2. SearchData.cs dosyasını açın ve **SearchData** sınıfını aşağıdaki kodla değiştirin.

    ```cs
    public class SearchData
    {
        public SearchData()
        {
        }

        // Constructor to initialize the list of facets sent from the controller.
        public SearchData(List<string> facets)
        {
            facetText = new string[facets.Count];

            for (int i = 0; i < facets.Count; i++)
            {
                facetText[i] = facets[i];
            }
        }

        // Array to hold the text for each amenity.
        public string[] facetText { get; set; }

        // Array to hold the setting for each amenitity.
        public bool[] facetOn { get; set; }

        // The text to search for.
        public string searchText { get; set; }

        // Record if the next page is requested.
        public string paging { get; set; }

        // The list of results.
        public DocumentSearchResult<Hotel> resultList;

        public string scoring { get; set; }       
    }
    ```

3. hotels.css dosyasını açın ve aşağıdaki HTML sınıflarını ekleyin.

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

### <a name="add-code-to-the-controller-to-specify-a-scoring-profile"></a>Puanlama profili belirtmek için denetleyiciye kod ekleme

1. Ev denetleyicisi dosyasını açın. Aşağıdaki **leri kullanarak** deyimi ekleyin (liste oluşturmaya yardımcı olmak için).

    ```cs
    using System.Linq;
    ```

2.  Bu örnekiçin, ilk görünümü döndürmekten biraz daha fazlasını yapmak için **Index'e** ilk çağrıyı yapmamız gerekir. Yöntem şimdi görünümde görüntülenecek en fazla 20 olanak arıyor.

    ```cs
        public async Task<ActionResult> Index()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search for up to 20 amenities.
                Facets = new List<string> { "Tags,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to a list that can be displayed in the client.
            List<string> facets = searchResult.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

            // Initiate a model with a list of facets for the first view.
            SearchData model = new SearchData(facets);

            // Save the facet text for the next view.
            SaveFacets(model, false);

            // Render the view including the facets.
            return View(model);
        }
    ```

3. Yönleri geçici depolamaya kaydetmek ve bunları geçici depolamadan kurtarmak ve bir modeli doldurmak için iki özel yönteme ihtiyacımız var.

    ```cs
        // Save the facet text to temporary storage, optionally saving the state of the check boxes.
        private void SaveFacets(SearchData model, bool saveChecks = false)
        {
            for (int i = 0; i < model.facetText.Length; i++)
            {
                TempData["facet" + i.ToString()] = model.facetText[i];
                if (saveChecks)
                {
                    TempData["faceton" + i.ToString()] = model.facetOn[i];
                }
            }
            TempData["facetcount"] = model.facetText.Length;
        }

        // Recover the facet text to a model, optionally recoving the state of the check boxes.
        private void RecoverFacets(SearchData model, bool recoverChecks = false)
        {
            // Create arrays of the appropriate length.
            model.facetText = new string[(int)TempData["facetcount"]];
            if (recoverChecks)
            {
                model.facetOn = new bool[(int)TempData["facetcount"]];
            }

            for (int i = 0; i < (int)TempData["facetcount"]; i++)
            {
                model.facetText[i] = TempData["facet" + i.ToString()].ToString();
                if (recoverChecks)
                {
                    model.facetOn[i] = (bool)TempData["faceton" + i.ToString()];
                }
            }
        }
    ```

4. **OrderBy** ve **ScoringProfile** parametrelerini gerektiği gibi ayarlamamız gerekir. Varolan **Index(SearchData modeli)** yöntemini aşağıdakilerle değiştirin.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Recover the facet text, and the facet check box settings.
                    RecoverFacets(model, true);

                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First search with text. 
                    // Recover the facet text, but ignore the check box settings, and use the current model settings.
                    RecoverFacets(model,false);

                    // First call. Check for valid text input, and valid scoring profile.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    if (model.scoring == null)
                    {
                        model.scoring = "Default";
                    }
                    page = 0;
                }

                // Set empty defaults for ordering and scoring parameters.
                var orderby = new List<string>();
                string profile = "";
                var scoringParams = new List<ScoringParameter>();

                // Set the ordering based on the user's radio button selection.
                switch (model.scoring)
                {
                    case "RatingRenovation":
                        orderby.Add("Rating desc");
                        orderby.Add("LastRenovationDate desc");
                        break;

                    case "boostAmenities":
                        {
                            profile = model.scoring;
                            var setAmenities = new List<string>();

                            // Create a string list of amenities that have been clicked.
                            for (int a = 0; a < model.facetOn.Length; a++)
                            {
                                if (model.facetOn[a])
                                {
                                    setAmenities.Add(model.facetText[a]);
                                }
                            }
                            if (setAmenities.Count > 0)
                            {
                                // Only set scoring parameters if there are any.
                                var sp = new ScoringParameter("amenities", setAmenities);
                                scoringParams.Add(sp);
                            }
                            else
                            {
                                // No amenities selected, so set profile back to default.
                                profile = "";
                            }
                        }
                        break;

                    case "renovatedAndHighlyRated":
                        profile = model.scoring;
                        break;

                    default:
                        break;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Set the ordering/scoring parameters.
                    OrderBy = orderby,
                    ScoringProfile = profile,
                    ScoringParameters = scoringParams,

                    // Select the data properties to be returned.
                    Select = new[] { "HotelName", "Description", "Tags", "Rooms", "Rating", "LastRenovationDate" },
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
                TempData["scoring"] = model.scoring;
                SaveFacets(model,true);

                // Calculate the room rate ranges.
                for (int n = 0; n < model.resultList.Results.Count; n++)
                {
                    var cheapest = 0d;
                    var expensive = 0d;

                    for (var r = 0; r < model.resultList.Results[n].Document.Rooms.Length; r++)
                    {
                        var rate = model.resultList.Results[n].Document.Rooms[r].BaseRate;
                        if (rate < cheapest || cheapest == 0)
                        {
                            cheapest = (double)rate;
                        }
                        if (rate > expensive)
                        {
                            expensive = (double)rate;
                        }
                    }
                    model.resultList.Results[n].Document.cheapest = cheapest;
                    model.resultList.Results[n].Document.expensive = expensive;
                }
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    **Geçiş** seçimlerinin her biri için yapılan açıklamaları okuyun.

5. Birden çok özelliğe dayalı sıralama yla ilgili önceki bölümün ek kodunu tamamladıysanız, **Sonraki** eylemde herhangi bir değişiklik yapmamız gerekmez.

### <a name="run-and-test-the-app"></a>Uygulamayı çalıştırın ve test edin

1. Uygulamayı çalıştırın. Görünümde tam bir olanaklar kümesi görmelisiniz.

2. Sipariş için, "Sayısal Derecelendirmeye Göre" seçeneğini seçmek, bu eğitimde uyguladığınız sayısal sıralamayı verecektir ve yenileme tarihi eşit dereceli oteller arasında karar verir.

![Derecelendirmeye göre "plaj" siparişi](./media/tutorial-csharp-create-first-app/azure-search-orders-beach.png)

3. Şimdi "Olanaklara göre" profilini deneyin. Çeşitli olanaklar için seçim yapın ve bu olanaklara sahip otellerin sonuç listesinin tanıtımını yaptığından emin olun.

![Profile göre "plaj" siparişi](./media/tutorial-csharp-create-first-app/azure-search-orders-beach-profile.png)

4. Beklediğiniz şeyi alıp almamanızı görmek için "By Renovated date/Rating profilini" deneyin. Sadece yakın zamanda yenilenen oteller _tazelik_ desteği almalıdır.

### <a name="resources"></a>Kaynaklar

Daha fazla bilgi için, bir Azure Bilişsel Arama dizinine aşağıdaki [puanlama profillerini ekleyin' e](https://docs.microsoft.com/azure/search/index-add-scoring-profiles)bakın.

## <a name="takeaways"></a>Paketler

Bu projeden aşağıdaki paketleri göz önünde bulundurun:

* Kullanıcılar, ilk önce en alakalı arama sonuçlarının sırayla yapılmasını beklerler.
* Veri, siparişin kolay olması için yapılandırılır. Veriler ek kod olmadan sipariş yapılmasını sağlayacak şekilde yapılandırılmadığımız için, ilk olarak "en ucuz" olarak kolayca sıralayabildik.
* Daha yüksek bir sıralama düzeyinde aynı değere sahip sonuçlar arasında ayrım yapmak için, sıralamanın birçok düzeyi olabilir.
* Bazı sonuçların artan sırada (örneğin, bir noktadan uzaklık) ve bazılarının azalan sırada (örneğin, misafirin reytingi) sıralanması doğaldır.
* Bir veri kümesi için sayısal karşılaştırmalar kullanılamadığında veya yeterince akıllı olmadığında puanlama profilleri tanımlanabilir. Her sonucu puanlama akıllıca sipariş ve görüntülemeye yardımcı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu C# öğretici serisini tamamladınız - Azure Bilişsel Arama API'leri hakkında değerli bilgiler edinmiş olmalısınız.

Daha fazla başvuru ve öğretici için, [Microsoft Learn](https://docs.microsoft.com/learn/browse/?products=azure)veya Azure Bilişsel [Arama Dokümantasyon'daki](https://docs.microsoft.com/azure/search/)diğer öğreticilere göz atmayı düşünün.
