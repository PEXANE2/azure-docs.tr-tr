---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: 750ce2d19334f549242c37494a11a1f683469f12
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88926158"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programmming-language-csharp)
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Azure Blob 'a kaynak dosya yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

İlk adım, projenizin Visual Studio 'da açık olduğundan emin olmak.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve açın `Program.cs` .

## <a name="add-a-reference-to-newtonsoftjson"></a>Newtonsoft.Jsiçin bir başvuru ekleyin

1. Çözüm Gezgini, **HelloWorld** projesine sağ tıklayın ve ardından NuGet Paket Yöneticisi ' ni görüntülemek Için **NuGet Paketlerini Yönet** ' i seçin.
1. Sağ üst köşede, **paket kaynağı** açılan kutusunu bulun ve ' nin seçili olduğundan emin olun **`nuget.org`** .
1. Sol üst köşede, **Araştır**' ı seçin.
1. Arama kutusuna *newtonsoft.js* yazın ve **ENTER**' u seçin.
1. Arama sonuçlarından, paketteki [**Newtonsoft.Js**](https://www.nuget.org/packages/Newtonsoft.Json) seçin ve ardından en son kararlı sürümü yüklemek için **yüklenir** ' i seçin.
1. Yüklemeyi başlatmak için tüm anlaşmaları ve lisansları kabul edin.
   Paket yüklendikten sonra, **Paket Yöneticisi konsol** penceresinde bir onay görüntülenir.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

```csharp
class Program
{
    // Replace with your subscription key
    const string SubscriptionKey = "YourSubscriptionKey";

    // Update with your service region
    const string Region = "YourServiceRegion";
    const int Port = 443;
 
    // Recordings and locale
    const string Locale = "en-US";
    const string RecordingsBlobUri = "YourFileUrl";
 
    // Name and description
    const string Name = "Simple transcription";
    const string Description = "Simple transcription description";
 
    const string SpeechToTextBasePath = "api/speechtotext/v2.0/";
 
    static async Task Main()
    {
        // Cognitive Services follows security best practices.
        // If you experience connectivity issues, see:
        // https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls
 
        await TranscribeAsync();
    }
 
    static async Task TranscribeAsync()
    {
        Console.WriteLine("Starting transcriptions client...");
    }
}
```

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>JSON sarmalayıcıları

REST API JSON biçiminde istek alma ve ayrıca JSON 'daki sonuçları geri döndürme gibi yalnızca dizeleri kullanarak etkileşime girebiliriz ancak bu önerilmez.
İsteklerin ve yanıtların yönetimini kolaylaştırmak için, JSON 'u serileştirmek/seri durumdan çıkarmak için kullanmak üzere birkaç sınıf tanımlayacağız.

Devam edin ve bildirimlerini daha sonra yerleştirin `TranscribeAsync` .

```csharp
public class ModelIdentity
{
    ModelIdentity(Guid id) => Id = id;

    public Guid Id { get; private set; }

    public static ModelIdentity Create(Guid Id) => new ModelIdentity(Id);
}

public class Transcription
{
    [JsonConstructor]
    Transcription(
        Guid id,
        string name,
        string description,
        string locale,
        DateTime createdDateTime,
        DateTime lastActionDateTime,
        string status,
        Uri recordingsUrl,
        IReadOnlyDictionary<string, string> resultsUrls)
    {
        Id = id;
        Name = name;
        Description = description;
        CreatedDateTime = createdDateTime;
        LastActionDateTime = lastActionDateTime;
        Status = status;
        Locale = locale;
        RecordingsUrl = recordingsUrl;
        ResultsUrls = resultsUrls;
    }

    public string Name { get; set; }

    public string Description { get; set; }

    public string Locale { get; set; }

    public Uri RecordingsUrl { get; set; }

    public IReadOnlyDictionary<string, string> ResultsUrls { get; set; }

    public Guid Id { get; set; }

    public DateTime CreatedDateTime { get; set; }

    public DateTime LastActionDateTime { get; set; }

    public string Status { get; set; }

    public string StatusMessage { get; set; }
}

public class TranscriptionDefinition
{
    TranscriptionDefinition(
        string name,
        string description,
        string locale,
        Uri recordingsUrl,
        IEnumerable<ModelIdentity> models)
    {
        Name = name;
        Description = description;
        RecordingsUrl = recordingsUrl;
        Locale = locale;
        Models = models;
        Properties = new Dictionary<string, string>
        {
            ["PunctuationMode"] = "DictatedAndAutomatic",
            ["ProfanityFilterMode"] = "Masked",
            ["AddWordLevelTimestamps"] = "True"
        };
    }

    public string Name { get; set; }

    public string Description { get; set; }

    public Uri RecordingsUrl { get; set; }

    public string Locale { get; set; }

    public IEnumerable<ModelIdentity> Models { get; set; }

    public IDictionary<string, string> Properties { get; set; }

    public static TranscriptionDefinition Create(
        string name,
        string description,
        string locale,
        Uri recordingsUrl)
        => new TranscriptionDefinition(name, description, locale, recordingsUrl, new ModelIdentity[0]);
}
```

## <a name="create-and-configure-an-http-client"></a>Http Istemcisi oluşturma ve yapılandırma
İhtiyaç duyduğumuz ilk şey, doğru temel URL ve kimlik doğrulama kümesine sahip bir http Istemcidir.
Bu kodu içine ekleyin `TranscribeAsync` .

```csharp
var client = new HttpClient
{
    Timeout = TimeSpan.FromMinutes(25),
    BaseAddress = new UriBuilder(Uri.UriSchemeHttps, $"{Region}.cris.ai", Port).Uri,
    DefaultRequestHeaders =
    {
        { "Ocp-Apim-Subscription-Key", SubscriptionKey }
    }
};
```

## <a name="generate-a-transcription-request"></a>Döküm isteği oluştur
Ardından, döküm isteğini oluşturacağız. Bu kodu öğesine ekleyin `TranscribeAsync` .

```csharp
var transcriptionDefinition =
    TranscriptionDefinition.Create(
        Name,
        Description,
        Locale,
        new Uri(RecordingsBlobUri));

var res = JsonConvert.SerializeObject(transcriptionDefinition);
var sc = new StringContent(res);
sc.Headers.ContentType = JsonMediaTypeFormatter.DefaultMediaType;
```

## <a name="send-the-request-and-check-its-status"></a>İsteği gönder ve durumunu denetle
Artık isteği konuşma hizmetine gönderiyoruz ve ilk yanıt kodunu denetliyoruz. Bu yanıt kodu yalnızca hizmetin isteği aldığını gösterir. Hizmet, yanıt üst bilgilerinde, döküm durumunu depolayacağı konuma bir URL döndürür.

```csharp
Uri transcriptionLocation = null;
using (var response = await client.PostAsync($"{SpeechToTextBasePath}Transcriptions/", sc))
{
    if (!response.IsSuccessStatusCode)
    {
        Console.WriteLine("Error {0} starting transcription.", response.StatusCode);
        return;
    }

    transcriptionLocation = response.Headers.Location;
}
```

## <a name="wait-for-the-transcription-to-complete"></a>Döküm işleminin tamamlanmasını bekleyin
Hizmet, dökümü zaman uyumsuz olarak işlediğinden, her ne kadar her durumda yoklama yapması gerekir. Her 5 saniyede bir kontrol edeceğiz.

İstek gönderildiğinde elde ettiğimiz URL 'deki içeriği alarak durumu kontrol edebilirsiniz. İçeriği geri edindiğimiz zaman, ile etkileşime geçmesini kolaylaştırmak için yardımcı sınıfımızın bir üyesi olarak seri durumdan çıkardık.

Başarılı bir tamamlama haricinde her şey için durum görüntüleme ile yoklama kodu aşağıda verilmiştir.

```csharp
Console.WriteLine($"Created transcription at location {transcriptionLocation}.");
Console.WriteLine("Checking status.");

var completed = false;

// Check for the status of our transcriptions periodically
while (!completed)
{
    Transcription transcription = null;
    using (var response = await client.GetAsync(transcriptionLocation.AbsolutePath))
    {
        var contentType = response.Content.Headers.ContentType;
        if (response.IsSuccessStatusCode &&
            string.Equals(contentType.MediaType, "application/json", StringComparison.OrdinalIgnoreCase))
        {
            transcription = await response.Content.ReadAsAsync<Transcription>();
        }
        else
        {
            Console.WriteLine("Error with status {0} getting transcription result", response.StatusCode);
            continue;
        }
    }

    switch (transcription.Status)
    {
        case "Failed":
            completed = true;
            Console.WriteLine("Transcription failed. Status: {0}", transcription.StatusMessage);
            break;

        case "Succeeded":
            break;

        case "Running":
            Console.WriteLine("Transcription is still running.");
            break;

        case "NotStarted":
            Console.WriteLine("Transcription has not started.");
            break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}

Console.WriteLine("Press any key...");
Console.ReadKey();
```

## <a name="display-the-transcription-results"></a>Döküm sonuçlarını görüntüleme
Hizmet başarıyla tamamlandığında, sonuçlar durum yanıtından elde ettiğimiz başka bir URL 'de depolanır. Burada, bu sonuçları okumadan ve serisini kaldırmadan önce geçici bir dosyaya indirmek için bir istek yaptık.
Sonuçlar yüklendikten sonra bunları konsola yazdırabiliriz. Etiketine aşağıdaki kodu ekleyin `case "Succeeded":` .

```csharp
completed = true;
var webClient = new WebClient();
var filename = Path.GetTempFileName();
webClient.DownloadFile(transcription.ResultsUrls["channel_0"], filename);
var results = File.ReadAllText(filename);
Console.WriteLine($"Transcription succeeded. Results: {Environment.NewLine}{results}");
File.Delete(filename);
```

## <a name="check-your-code"></a>Kodunuzu denetleyin
Bu noktada, kodunuz şuna benzemelidir: (Bu sürüme bazı açıklamalar ekledik)

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Threading.Tasks;
using System.Net.Http;
using System.Net.Http.Formatting;

namespace BatchClient
{
    class Program
    {
        // Replace with your subscription key
        const string SubscriptionKey = "YourSubscriptionKey";

        // Update with your service region
        const string Region = "YourServiceRegion";
        const int Port = 443;

        // Recordings and locale
        const string Locale = "en-US";
        const string RecordingsBlobUri = "YourFileUrl";

        // Name and description
        const string Name = "Simple transcription";
        const string Description = "Simple transcription description";

        const string SpeechToTextBasePath = "api/speechtotext/v2.0/";

        static async Task Main()
        {
            // For non-Windows 10 users.
            ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;

            await TranscribeAsync();
        }

        static async Task TranscribeAsync()
        {
            Console.WriteLine("Starting transcriptions client...");

            // Create the client object and authenticate
            var client = new HttpClient
            {
                Timeout = TimeSpan.FromMinutes(25),
                BaseAddress = new UriBuilder(Uri.UriSchemeHttps, $"{Region}.cris.ai", Port).Uri,
                DefaultRequestHeaders =
                {
                    { "Ocp-Apim-Subscription-Key", SubscriptionKey }
                }
            };

            var transcriptionDefinition =
                TranscriptionDefinition.Create(
                    Name,
                    Description,
                    Locale,
                    new Uri(RecordingsBlobUri));

            var res = JsonConvert.SerializeObject(transcriptionDefinition);
            var sc = new StringContent(res);
            sc.Headers.ContentType = JsonMediaTypeFormatter.DefaultMediaType;

            Uri transcriptionLocation = null;

            using (var response = await client.PostAsync($"{SpeechToTextBasePath}Transcriptions/", sc))
            {
                if (!response.IsSuccessStatusCode)
                {
                    Console.WriteLine("Error {0} starting transcription.", response.StatusCode);
                    return;
                }

                transcriptionLocation = response.Headers.Location;
            }

            Console.WriteLine($"Created transcription at location {transcriptionLocation}.");
            Console.WriteLine("Checking status.");

            var completed = false;

            // Check for the status of our transcriptions periodically
            while (!completed)
            {
                Transcription transcription = null;

                // Get all transcriptions for the user
                using (var response = await client.GetAsync(transcriptionLocation.AbsolutePath))
                {
                    var contentType = response.Content.Headers.ContentType;
                    if (response.IsSuccessStatusCode &&
                        string.Equals(contentType.MediaType, "application/json", StringComparison.OrdinalIgnoreCase))
                    {
                        transcription = await response.Content.ReadAsAsync<Transcription>();
                    }
                    else
                    {
                        Console.WriteLine("Error with status {0} getting transcription result", response.StatusCode);
                        continue;
                    }
                }

                // For each transcription in the list we check the status
                switch (transcription.Status)
                {
                    case "Failed":
                        completed = true;
                        Console.WriteLine("Transcription failed. Status: {0}", transcription.StatusMessage);
                        break;

                    case "Succeeded":
                        completed = true;
                        var webClient = new WebClient();
                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(transcription.ResultsUrls["channel_0"], filename);
                        var results = File.ReadAllText(filename);
                        Console.WriteLine($"Transcription succeeded. Results: {Environment.NewLine}{results}");
                        File.Delete(filename);
                        break;

                    case "Running":
                        Console.WriteLine("Transcription is still running.");
                        break;

                    case "NotStarted":
                        Console.WriteLine("Transcription has not started.");
                        break;
                }

                await Task.Delay(TimeSpan.FromSeconds(5));
            }

            Console.WriteLine("Press any key...");
            Console.ReadKey();
        }
    }

    public class ModelIdentity
    {
        ModelIdentity(Guid id) => Id = id;

        public Guid Id { get; private set; }

        public static ModelIdentity Create(Guid Id) => new ModelIdentity(Id);
    }

    public class Transcription
    {
        [JsonConstructor]
        Transcription(
            Guid id,
            string name,
            string description,
            string locale,
            DateTime createdDateTime,
            DateTime lastActionDateTime,
            string status,
            Uri recordingsUrl,
            IReadOnlyDictionary<string, string> resultsUrls)
        {
            Id = id;
            Name = name;
            Description = description;
            CreatedDateTime = createdDateTime;
            LastActionDateTime = lastActionDateTime;
            Status = status;
            Locale = locale;
            RecordingsUrl = recordingsUrl;
            ResultsUrls = resultsUrls;
        }

        public string Name { get; set; }

        public string Description { get; set; }

        public string Locale { get; set; }

        public Uri RecordingsUrl { get; set; }

        public IReadOnlyDictionary<string, string> ResultsUrls { get; set; }

        public Guid Id { get; set; }

        public DateTime CreatedDateTime { get; set; }

        public DateTime LastActionDateTime { get; set; }

        public string Status { get; set; }

        public string StatusMessage { get; set; }
    }

    public class TranscriptionDefinition
    {
        TranscriptionDefinition(
            string name,
            string description,
            string locale,
            Uri recordingsUrl,
            IEnumerable<ModelIdentity> models)
        {
            Name = name;
            Description = description;
            RecordingsUrl = recordingsUrl;
            Locale = locale;
            Models = models;
            Properties = new Dictionary<string, string>
            {
                ["PunctuationMode"] = "DictatedAndAutomatic",
                ["ProfanityFilterMode"] = "Masked",
                ["AddWordLevelTimestamps"] = "True"
            };
        }

        public string Name { get; set; }

        public string Description { get; set; }

        public Uri RecordingsUrl { get; set; }

        public string Locale { get; set; }

        public IEnumerable<ModelIdentity> Models { get; set; }

        public IDictionary<string, string> Properties { get; set; }

        public static TranscriptionDefinition Create(
            string name,
            string description,
            string locale,
            Uri recordingsUrl)
            => new TranscriptionDefinition(name, description, locale, recordingsUrl, new ModelIdentity[0]);
    }
}
```

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü **çubuğundan derleme**  >  **Build Solution**' ı seçin.
2. **Uygulamanızı başlatın** -menü çubuğundan hata **Debug**  >  **ayıklamayı Başlat hata** Ayıkla ' yı seçin veya **F5**tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
