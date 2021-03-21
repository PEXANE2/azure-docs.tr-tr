---
title: C# öğreticisi, push API 'SI ile dizin oluşturmayı iyileştirme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'nin Push API 'sini kullanarak verileri verimli bir şekilde dizine alma hakkında bilgi edinin. Bu öğretici ve örnek kod C# ' de bulunur.
manager: luisca
author: dereklegenzoff
ms.author: delegenz
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 1/29/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: f97a99bf2d055805ee665ab51aff8cff12dc5a69
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99094201"
---
# <a name="tutorial-optimize-indexing-with-the-push-api"></a>Öğretici: gönderme API 'SI ile dizin oluşturmayı Iyileştirme

Azure Bilişsel Arama, verileri bir arama dizinine aktarmaya yönelik [iki temel yaklaşımı](search-what-is-data-import.md) *destekler: verilerinizi* dizine programlamayla gönderme veya desteklenen bir veri kaynağında [Azure bilişsel arama Dizin oluşturucuyu](search-indexer-overview.md) işaret eden verileri *çekme* .

Bu öğreticide, istek toplu işleme ve bir üstel geri alma yeniden deneme stratejisi kullanılarak [anında iletme modeli](search-what-is-data-import.md#pushing-data-to-an-index) kullanılarak verilerin nasıl verimli bir şekilde dizininin oluşturulduğu açıklanmaktadır. [Örnek uygulamayı indirebilir ve çalıştırabilirsiniz](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/optimize-data-indexing). Bu makalede, verilerin dizininin oluşturulması sırasında göz önünde bulundurulması gereken uygulamanın ve faktörlerin temel yönleri açıklanmaktadır.

Bu öğretici aşağıdaki görevleri gerçekleştirmek için C# ve [.NET SDK](/dotnet/api/overview/azure/search) kullanır:

> [!div class="checklist"]
> * Dizin oluşturma
> * En etkili boyutu anlamak için çeşitli toplu iş boyutlarını test edin
> * Toplu işleri zaman uyumsuz olarak oluştur
> * Dizin oluşturma hızlarını artırmak için birden çok iş parçacığı kullanın
> * Hatalı belgeleri yeniden denemek için bir üstel geri alma yeniden deneme stratejisi kullanın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için aşağıdaki hizmetler ve araçlar gereklidir.

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/), tüm sürüm. Örnek kod ve yönergeler ücretsiz topluluk sürümünde sınanmıştır.

+ Geçerli aboneliğinizde [bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) .

<a name="get-service-info"></a>

## <a name="download-files"></a>Dosyaları indirme

Bu öğreticinin kaynak kodu, [Azure-Samples/Azure-Search-DotNet-Samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) GitHub deposundaki [optimzize-Data-Indexing/v11](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/optimize-data-indexing/v11) klasöründedir.

## <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular

Verileri bir dizine gönderdiğinizde, dizin oluşturma hızlarını etkileyen bazı önemli noktalar vardır. Bu faktörlerle ilgili daha fazla bilgiyi, [büyük veri kümelerinde Dizin](search-howto-large-index.md)bölümünde bulabilirsiniz.

Dikkate alınması gereken altı temel etken şunlardır:

+ **Hizmet katmanı ve bölüm/çoğaltma sayısı** -bölüm ekleme ve katmanınızı artırma, her ikisi de dizin oluşturma hızlarını arttırır.
+ **Dizin şeması** -alanları ekleme ve alanlara ek özellikler ekleme ( *aranabilir*, çok *yönlü tablo* veya *filtrelenebilir* gibi) her ikisi de dizin oluşturma hızlarını azaltır.
+ **Toplu iş boyutu** -en iyi toplu iş boyutu Dizin şemanıza ve veri kümenize göre değişir.
+ **İş parçacığı/çalışan sayısı** -tek bir iş parçacığı dizin oluşturma hızlarından tam olarak yararlanmaz
+ **Yeniden deneme stratejisi** -Dizin oluşturmayı iyileştirmek için bir üstel geri alma yeniden deneme stratejisi kullanılmalıdır.
+ **Ağ verileri aktarım hızları** -veri aktarım hızları sınırlandırma faktörü olabilir. Veri aktarım hızlarını artırmak için Azure ortamınızdaki verileri dizin içinde yapın.


## <a name="1---create-azure-cognitive-search-service"></a>1-Azure Bilişsel Arama hizmeti oluşturma

Bu öğreticiyi tamamlayabilmeniz için [portalda oluşturabileceğiniz](search-create-service-portal.md)bir Azure bilişsel arama hizmetine ihtiyacınız vardır. Dizin oluşturma hızlarını doğru şekilde sınayabilmeniz ve iyileştirebilmeniz için üretimde kullanmayı planladığınız aynı katmanı kullanmanızı öneririz.

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama yönelik bir yönetici API anahtarı ve URL 'SI alın

API çağrıları, hizmet URL 'SI ve erişim anahtarı gerektirir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar**  >  **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

   ![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-rest/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

## <a name="2---set-up-your-environment"></a>2-ortamınızı ayarlama

1. Visual Studio 'Yu başlatın ve **Optimizedataındexing. sln**' i açın.
1. Çözüm Gezgini, bağlantı bilgilerini sağlamak için **appsettings.js** açın.

```json
{
  "SearchServiceUri": "https://{service-name}.search.windows.net",
  "SearchServiceAdminApiKey": "",
  "SearchIndexName": "optimize-indexing"
}
```

## <a name="3---explore-the-code"></a>3-kodu keşfet

*appsettings.js* güncelleştirme yaptıktan sonra, **Optimizedataındexing. sln** dosyasındaki örnek program, derleme ve çalıştırmaya hazır olmalıdır.

Bu kod, [C# hızlı](search-get-started-dotnet.md)başlangıcının türetilir. Bu makaledeki .NET SDK ile çalışmanın temelleri hakkında daha ayrıntılı bilgi edinebilirsiniz.

Bu basit C#/.NET konsol uygulaması aşağıdaki görevleri gerçekleştirir:

+ C# otel sınıfının veri yapısına dayalı yeni bir dizin oluşturur (Bu da adres sınıfına de başvurur).
+ En etkili boyutu belirlemede çeşitli toplu iş boyutlarını sınar
+ Verileri zaman uyumsuz olarak endeksler
    + Dizin oluşturma hızlarını artırmak için birden çok iş parçacığı kullanma
    + Hatalı öğeleri yeniden denemek için üstel geri alma yeniden deneme stratejisi kullanma

 Programı çalıştırmadan önce, bu örneğe ilişkin kodu ve Dizin tanımlarını incelemek için bir dakikanızı alın. İlgili kod birkaç dosyada mevcuttur:

  + **Otel. cs** ve **Address. cs** , dizini tanımlayan şemayı içerir
  + **DataGenerator. cs** , büyük miktarlarda otel verileri oluşturmayı kolaylaştıran basit bir sınıf içerir
  + **Üs Albackoff. cs** , dizin oluşturma işlemini aşağıda açıklandığı şekilde iyileştirmek için kod içerir
  + **Program. cs** , Azure bilişsel arama dizini oluşturup silen, veri yığınlarını dizinleyen ve farklı toplu iş boyutlarını test eden işlevler içerir

### <a name="creating-the-index"></a>Dizin oluşturuluyor

Bu örnek program, bir Azure Bilişsel Arama dizini tanımlamak ve oluşturmak için .NET SDK 'sını kullanır. `FieldBuilder`C# veri modeli sınıfından dizin yapısı oluşturmak için sınıfından faydalanır.

Veri modeli, ayrıca adres sınıfına başvurular içeren Otel Sınıfı tarafından tanımlanır. FieldBuilder, dizin için karmaşık bir veri yapısı oluşturmak üzere birden fazla sınıf tanımı aracılığıyla ayrıntıya gider. Meta veri etiketleri, her alanın, aranabilir veya sıralanabilir olup olmadığı gibi özniteliklerini tanımlamak için kullanılır.

**Otel. cs** dosyasındaki aşağıdaki kod parçacıkları, tek bir alanın ve başka bir veri modeli sınıfına yapılan başvurunun nasıl belirtime olduğunu gösterir.

```csharp
. . .
[SearchableField(IsSortable = true)]
public string HotelName { get; set; }
. . .
public Address Address { get; set; }
. . .
```

**Program. cs** dosyasında, dizin, yöntemi tarafından oluşturulan bir ad ve alan koleksiyonuyla tanımlanır `FieldBuilder.Build(typeof(Hotel))` ve sonra aşağıdaki gibi oluşturulur:

```csharp
private static async Task CreateIndexAsync(string indexName, SearchIndexClient indexClient)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address class is referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    FieldBuilder builder = new FieldBuilder();
    var definition = new SearchIndex(indexName, builder.Build(typeof(Hotel)));

    await indexClient.CreateIndexAsync(definition);
}
```

### <a name="generating-data"></a>Veri oluşturma

Basit bir sınıf, test için veri oluşturmak üzere **DataGenerator. cs** dosyasında uygulanır. Bu sınıfın tek amacı, dizin oluşturma için benzersiz bir KIMLIĞE sahip çok sayıda belge oluşturmayı kolaylaştırmaktadır.

Benzersiz kimliklere sahip 100.000 oteller listesini almak için aşağıdaki kod satırlarını çalıştırırsınız:

```csharp
long numDocuments = 100000;
DataGenerator dg = new DataGenerator();
List<Hotel> hotels = dg.GetHotels(numDocuments, "large");
```

Bu örnekte test için iki adet otel boyutu mevcuttur: **küçük** ve  **büyük**.

Dizinlerinizin şeması, dizin oluşturma hızları üzerinde önemli bir etkiye sahip olabilir. Bu etki nedeniyle, bu öğreticiyi çalıştırdıktan sonra bu sınıfı amaçlanan Dizin şemanıza uygun veri oluşturacak şekilde dönüştürmek mantıklı olur.

## <a name="4---test-batch-sizes"></a>4-test toplu işlem boyutları

Azure Bilişsel Arama, bir dizine tek veya birden çok belge yüklemek için aşağıdaki API 'Leri destekler:

+ [Belge Ekleme, Güncelleştirme veya Silme (REST API)](/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [Indexdocumentsaction sınıfı](/dotnet/api/azure.search.documents.models.indexdocumentsaction) veya [ındexdocumentsbatch sınıfı](/dotnet/api/azure.search.documents.models.indexdocumentsbatch)

Belgelerin toplu olarak dizinlemesi, dizin oluşturma performansını önemli ölçüde iyileştirir. Bu toplu işlemler en fazla 1000 belge veya toplu işlem başına en fazla 16 MB olabilir.

Verilerinizin en iyi toplu iş boyutunu belirlemek, dizin oluşturma hızlarını iyileştirmek için önemli bir bileşendir. En iyi toplu iş boyutunu etkileyen iki birincil etken şunlardır:

+ Dizininizin şeması
+ Verilerinizin boyutu

En iyi toplu iş boyutu dizininize ve verilerinize bağlı olduğundan, senaryonuza en hızlı Dizin oluşturma hızlarındaki sonuçları belirlemek için farklı toplu iş boyutlarını test etmek en iyi yaklaşımdır.

Aşağıdaki işlev, toplu iş boyutlarını test etmek için basit bir yaklaşımı gösterir.

```csharp
public static async Task TestBatchSizesAsync(SearchClient searchClient, int min = 100, int max = 1000, int step = 100, int numTries = 3)
{
    DataGenerator dg = new DataGenerator();

    Console.WriteLine("Batch Size \t Size in MB \t MB / Doc \t Time (ms) \t MB / Second");
    for (int numDocs = min; numDocs <= max; numDocs += step)
    {
        List<TimeSpan> durations = new List<TimeSpan>();
        double sizeInMb = 0.0;
        for (int x = 0; x < numTries; x++)
        {
            List<Hotel> hotels = dg.GetHotels(numDocs, "large");

            DateTime startTime = DateTime.Now;
            await UploadDocumentsAsync(searchClient, hotels).ConfigureAwait(false);
            DateTime endTime = DateTime.Now;
            durations.Add(endTime - startTime);

            sizeInMb = EstimateObjectSize(hotels);
        }

        var avgDuration = durations.Average(timeSpan => timeSpan.TotalMilliseconds);
        var avgDurationInSeconds = avgDuration / 1000;
        var mbPerSecond = sizeInMb / avgDurationInSeconds;

        Console.WriteLine("{0} \t\t {1} \t\t {2} \t\t {3} \t {4}", numDocs, Math.Round(sizeInMb, 3), Math.Round(sizeInMb / numDocs, 3), Math.Round(avgDuration, 3), Math.Round(mbPerSecond, 3));

        // Pausing 2 seconds to let the search service catch its breath
        Thread.Sleep(2000);
    }

    Console.WriteLine();
}
```

Tüm belgeler aynı boyutta olmadığından (Bu örnekte olsalar da), arama hizmetine gönderdiğimiz verilerin boyutunu tahmin ediyoruz. Bunu aşağıdaki işlevini kullanarak yaptık, önce nesneyi JSON 'a dönüştürür ve sonra boyutunu bayt olarak belirler. Bu teknik, MB/s dizin oluşturma hızları açısından hangi toplu iş boyutlarının en verimli olduğunu belirlememize olanak tanır.

```csharp
// Returns size of object in MB
public static double EstimateObjectSize(object data)
{
    // converting object to byte[] to determine the size of the data
    BinaryFormatter bf = new BinaryFormatter();
    MemoryStream ms = new MemoryStream();
    byte[] Array;

    // converting data to json for more accurate sizing
    var json = JsonSerializer.Serialize(data);
    bf.Serialize(ms, json);
    Array = ms.ToArray();

    // converting from bytes to megabytes
    double sizeInMb = (double)Array.Length / 1000000;

    return sizeInMb;
}
```

İşlev `SearchClient` için, her bir toplu iş boyutu için test etmek istediğiniz deneme sayısı ve ek olarak gereklidir. Her toplu iş için dizin oluşturma saatlerinde bir değişkenlik olabileceğinden, sonuçların daha istatistiksel olarak önemli olmasını sağlamak için her toplu işi varsayılan olarak üç kez deneyeceğiz.

```csharp
await TestBatchSizesAsync(searchClient, numTries: 3);
```

İşlevini çalıştırdığınızda, konsolunuza aşağıda olduğu gibi bir çıktı görmeniz gerekir:

   ![Test toplu iş boyutu işlevinin çıkışı](media/tutorial-optimize-data-indexing/test-batch-sizes.png "Test toplu iş boyutu işlevinin çıkışı")

Hangi toplu iş boyutunun en verimli olduğunu ve ardından bu toplu iş boyutunu öğreticinin sonraki adımında kullanın. Farklı toplu iş boyutlarında MB/sn cinsinden bir platoya görebilirsiniz.

## <a name="5---index-data"></a>5-dizin verileri

Kullanmayı planladığımız toplu iş boyutunu tanımladığımıza göre, bir sonraki adım, verileri dizine almayı başlıyoruz. Verileri verimli bir şekilde indekslemek için bu örnek:

+ Birden çok iş parçacığını/çalışanı kullanır.
+ Üstel geri alma yeniden deneme stratejisi uygular.

### <a name="use-multiple-threadsworkers"></a>Birden çok iş parçacığı veya çalışan kullanma

Azure Bilişsel Arama 'in dizin oluşturma hızlarından tam olarak yararlanmak için, çok sayıda iş parçacığını kullanarak toplu işlem dizini oluşturma isteklerini hizmete eşzamanlı olarak gönderebilirsiniz.  

Yukarıda bahsedilen önemli önemli noktalar, en iyi iş parçacığı sayısını etkiler. Senaryonuz için en iyi iş parçacığı sayısını öğrenmek için bu örneği değiştirebilir ve farklı iş parçacığı sayısı ile test edebilirsiniz. Bununla birlikte, eşzamanlı olarak çalışan birkaç iş parçacığı olduğu sürece, verimlilik kazanmalarının avantajlarından faydalanabilirsiniz.

Arama hizmetine vurur istekleri artırdığınız için, isteğin tam olarak başarılı olmadığını belirten [http durum kodları](/rest/api/searchservice/http-status-codes) ile karşılaşabilirsiniz. Dizin oluşturma sırasında iki ortak HTTP durum kodu şunlardır:

+ **503 Hizmet kullanılamıyor** -bu hata, sistem ağır yükün altında ve isteğiniz şu anda işlenemediği anlamına gelir.
+ **207 çok durum** -bu hata, bazı belgelerin başarılı olduğu, ancak en az bir başarısız olduğu anlamına gelir.

### <a name="implement-an-exponential-backoff-retry-strategy"></a>Üstel geri alma yeniden deneme stratejisi uygulama

Bir hata oluşursa, istekler [üstel geri alma yeniden deneme stratejisi kullanılarak yeniden](/dotnet/architecture/microservices/implement-resilient-applications/implement-retries-exponential-backoff)denenmelidir.

Azure Bilişsel Arama .NET SDK, 503s ve diğer başarısız istekleri otomatik olarak yeniden dener, ancak 20 7s 'yi yeniden denemek için kendi mantığınızı uygulamanız gerekir. Yeniden deneme stratejisi uygulamak için, [Polly](https://github.com/App-vNext/Polly) gibi açık kaynaklı araçlar da kullanılabilir.

Bu örnekte, kendi üstel geri alma stratejinizi uyguladık. Bu stratejiyi uygulamak için, `maxRetryAttempts` `delay` başarısız bir istek için ve başlangıcı dahil bazı değişkenler tanımlayarak başlayacağız:

```csharp
// Create batch of documents for indexing
var batch = IndexDocumentsBatch.Upload(hotels);

// Create an object to hold the result
IndexDocumentsResult result = null;

// Define parameters for exponential backoff
int attempts = 0;
TimeSpan delay = delay = TimeSpan.FromSeconds(2);
int maxRetryAttempts = 5;
```

Dizin oluşturma işleminin sonuçları değişkeninde depolanır `IndexDocumentResult result` . Bu değişken önemlidir çünkü toplu işteki herhangi bir belgenin aşağıda gösterildiği gibi başarısız olup olmadığını kontrol etmenizi sağlar. Kısmi bir hata varsa, başarısız belgelerin KIMLIĞI temel alınarak yeni bir toplu iş oluşturulur.

`RequestFailedException` Ayrıca, isteğin tamamen başarısız olduğunu ve ayrıca yeniden deneneceğini belirten özel durumlar yakalanmalıdır.

```csharp
// Implement exponential backoff
do
{
    try
    {
        attempts++;
        result = await searchClient.IndexDocumentsAsync(batch).ConfigureAwait(false);

        var failedDocuments = result.Results.Where(r => r.Succeeded != true).ToList();

        // handle partial failure
        if (failedDocuments.Count > 0)
        {
            if (attempts == maxRetryAttempts)
            {
                Console.WriteLine("[MAX RETRIES HIT] - Giving up on the batch starting at {0}", id);
                break;
            }
            else
            {
                Console.WriteLine("[Batch starting at doc {0} had partial failure]", id);
                Console.WriteLine("[Retrying {0} failed documents] \n", failedDocuments.Count);

                // creating a batch of failed documents to retry
                var failedDocumentKeys = failedDocuments.Select(doc => doc.Key).ToList();
                hotels = hotels.Where(h => failedDocumentKeys.Contains(h.HotelId)).ToList();
                batch = IndexDocumentsBatch.Upload(hotels);

                Task.Delay(delay).Wait();
                delay = delay * 2;
                continue;
            }
        }

        return result;
    }
    catch (RequestFailedException ex)
    {
        Console.WriteLine("[Batch starting at doc {0} failed]", id);
        Console.WriteLine("[Retrying entire batch] \n");

        if (attempts == maxRetryAttempts)
        {
            Console.WriteLine("[MAX RETRIES HIT] - Giving up on the batch starting at {0}", id);
            break;
        }

        Task.Delay(delay).Wait();
        delay = delay * 2;
    }
} while (true);
```

Buradan, kolayca çağrılabilmesi için üstel geri alma kodunu bir işleve sarıyoruz.

Daha sonra etkin iş parçacıklarını yönetmek için başka bir işlev oluşturulur. Kolaylık olması için bu işlev buraya dahil değildir ancak [üs Albackoff. cs](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/optimize-data-indexing/v11/OptimizeDataIndexing/ExponentialBackoff.cs)dosyasında bulunabilir. İşlevi, `hotels` yüklemek istediğimiz veriler nerede, `1000` toplu iş boyutu ve `8` eşzamanlı iş parçacığı sayısı olan aşağıdaki komutla çağrılabilir:

```csharp
await ExponentialBackoff.IndexData(indexClient, hotels, 1000, 8);
```

İşlevini çalıştırdığınızda, aşağıdaki gibi bir çıktı görmeniz gerekir:

![Dizin verisi işlevinin çıkışı](media/tutorial-optimize-data-indexing/index-data-start.png "Dizin verisi işlevinin çıkışı")

Bir dizi belge başarısız olduğunda, hatayı belirten ve toplu işlemin yeniden denenmekte olduğu bir hata görüntülenir:

```
[Batch starting at doc 6000 had partial failure]
[Retrying 560 failed documents]
```

İşlevin çalışması bittikten sonra, tüm belgelerin dizine eklendiğini doğrulayabilirsiniz.

## <a name="6---explore-index"></a>6-dizini keşfet

Program program aracılığıyla çalıştırdıktan sonra veya portalda [**Arama Gezgini**](search-explorer.md) 'ni kullanarak doldurulmuş arama dizinini keşfedebilirsiniz.

### <a name="programatically"></a>Program aracılığıyla

Bir dizindeki belge sayısını denetlemeye yönelik iki ana seçenek vardır: [belgeleri say API 'si](/rest/api/searchservice/count-documents) ve [Get Index STATISTICS API](/rest/api/searchservice/get-index-statistics). Her iki yol da bazı ek saat gerektirebilir, bu nedenle döndürülen belge sayısı başlangıçta beklediğinizden daha düşükse uyarıda yok.

#### <a name="count-documents"></a>Belge sayısı

Belgeleri say işlemi, bir arama dizinindeki belge sayısı sayısını alır:

```csharp
long indexDocCount = await searchClient.GetDocumentCountAsync();
```

#### <a name="get-index-statistics"></a>Dizin Istatistiklerini al

Dizin Istatistiklerini al işlemi, geçerli dizin için bir belge sayısı ve depolama kullanımı döndürür. Dizin istatistikleri Güncelleştirilecek belge sayısından daha uzun sürer.

```csharp
var indexStats = await indexClient.GetIndexStatisticsAsync(indexName);
```

### <a name="azure-portal"></a>Azure portalı

Azure portal ' de, arama hizmeti **genel bakış** sayfasını açın ve **dizinler** listesinde **en iyileştirme dizin oluşturma** dizinini bulun.

  ![Azure Bilişsel Arama dizinlerinin listesi](media/tutorial-optimize-data-indexing/portal-output.png "Azure Bilişsel Arama dizinlerinin listesi")

*Belge sayısı* ve *depolama boyutu* , [Get Index STATISTICS API 'sine](/rest/api/searchservice/get-index-statistics) dayanır ve güncelleştirilmesi birkaç dakika sürebilir.

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmede erken deneysel aşamalarda, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama silmek ve kodunuzun bunları yeniden oluşturması için izin verir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticinin örnek kodu var olan dizinleri denetler ve kodunuzu yeniden çalıştırabilmeniz için bunları siler.

Ayrıca, dizinleri silmek için portalını de kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir projenin sonunda kendi aboneliğinizde çalışırken, artık ihtiyaç duyulmadığınızda kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Verileri verimli bir şekilde alma kavramı hakkında bilgi sahibi olduğunuza göre, artık Lucene sorgu mimarisine ve tam metin aramasının Azure Bilişsel Arama 'de nasıl çalıştığını daha yakından inceleyelim.

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)