---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 3d92d3f959e2ad44daa82d6b609b9357cee969c9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69906844"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Setup and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>Bir .NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o sentences-sample
cd sentences-sample
```

İlk komut iki şey yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı `sentences-sample`bir dizin oluşturur. İkinci komut, projenizin dizininde değişir.

Ardından, Json.Net yüklemeniz gerekir. Projenizin dizininden çalıştır:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>C# dil sürümünü seçin

Bu hızlı başlatma C# 7.1 veya daha sonra gerektirir. Projenizin C# sürümünü değiştirmenin birkaç yolu vardır. Bu kılavuzda, dosyayı nasıl ayarlayabileceğinizi `sentences-sample.csproj` göstereceğiz. Visual Studio'da dili değiştirme gibi tüm kullanılabilir seçenekler için [C# dil sürümünü seçin'e](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version)bakın.

Projenizi açın, `sentences-sample.csproj`sonra açın. 7.1 `LangVersion` veya daha sonra olarak ayarlandıklarına emin olun. Dil sürümü için bir özellik grubu yoksa, aşağıdaki satırları ekleyin:

```xml
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

## <a name="add-required-namespaces-to-your-project"></a>Projenize gerekli ad alanları ekleme

Daha `dotnet new console` önce çalıştırdığınız komut, `Program.cs`'. Bu dosya, uygulama kodunuzu koyacağınız yerdir. Varolan ifadeleri açın `Program.cs`ve değiştirin. Bu ifadeler, örnek uygulamayı oluşturmak ve çalıştırmak için gereken tüm türlere erişebildiğinizi sağlar.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
// Install Newtonsoft.Json with NuGet
using Newtonsoft.Json;
```

## <a name="create-classes-for-the-json-response"></a>JSON yanıtı için sınıflar oluşturma

Daha sonra, Çevirmen Metin API'si tarafından döndürülen JSON yanıtını deserializing'de kullanılan bir sınıf oluşturacağız.

```csharp
/// <summary>
/// The C# classes that represents the JSON returned by the Translator Text API.
/// </summary>
public class BreakSentenceResult
{
    public int[] SentLen { get; set; }
    public DetectedLanguage DetectedLanguage { get; set; }
}

public class DetectedLanguage
{
    public string Language { get; set; }
    public float Score { get; set; }
}
```

## <a name="get-subscription-information-from-environment-variables"></a>Çevre değişkenlerinden abonelik bilgileri alma

`Program` Sınıfa aşağıdaki satırları ekleyin. Bu satırlar, abonelik anahtarınızı ve bitiş noktanızı ortam değişkenlerinden okur ve herhangi bir sorunla karşılaştığınızda hata atar.

```csharp
private const string key_var = "TRANSLATOR_TEXT_SUBSCRIPTION_KEY";
private static readonly string subscriptionKey = Environment.GetEnvironmentVariable(key_var);

private const string endpoint_var = "TRANSLATOR_TEXT_ENDPOINT";
private static readonly string endpoint = Environment.GetEnvironmentVariable(endpoint_var);

static Program()
{
    if (null == subscriptionKey)
    {
        throw new Exception("Please set/export the environment variable: " + key_var);
    }
    if (null == endpoint)
    {
        throw new Exception("Please set/export the environment variable: " + endpoint_var);
    }
}
// The code in the next section goes here.
```

## <a name="create-a-function-to-determine-sentence-length"></a>Cümle uzunluğunu belirlemek için bir işlev oluşturma

`Program` Sınıfta, '. `BreakSentenceRequest()` Bu işlev dört `subscriptionKey`bağımsız `endpoint` `route`değişken `inputText`alır: , , ve .

```csharp
static public async Task BreakSentenceRequest(string subscriptionKey, string endpoint, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-break-sentence-request"></a>Kesme cümlesi isteğini serihale etme

Ardından, metni içeren JSON nesnesini oluşturmanız ve seri hale almanız gerekir. Unutmayın, `body` dizide birden fazla nesne geçirebilirsiniz.

```csharp
object[] body = new object[] { new { Text = inputText } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Müşteriyi anında anlayıp bir istekte bulunun

Bu satırlar anında `HttpClient` `HttpRequestMessage`ve:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>İsteği oluşturma ve yanıtı yazdırma

`HttpRequestMessage` İçinde:

* HTTP yöntemini bildirin
* İstek URI'yi oluşturma
* İstek gövdesini ekleme (serileştirilmiş JSON nesnesi)
* Gerekli üstbilgi ekleme
* Eşzamanlı istekte bulunun
* Yanıtı yazdırma

Bu kodu `HttpRequestMessage`ekleyin:

```csharp
// Build the request.
// Set the method to POST
request.Method = HttpMethod.Post;
// Construct the URI and add headers.
request.RequestUri = new Uri(endpoint + route);
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send the request and get response.
HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
// Read response as a string.
string result = await response.Content.ReadAsStringAsync();
// Deserialize the response using the classes created earlier.
BreakSentenceResult[] deserializedOutput = JsonConvert.DeserializeObject<BreakSentenceResult[]>(result);
foreach (BreakSentenceResult o in deserializedOutput)
{
    Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.", o.DetectedLanguage.Language, o.DetectedLanguage.Score);
    Console.WriteLine("The first sentence length is: {0}", o.SentLen[0]);
}
```

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek parametrelerinize `Ocp-Apim-Subscription-Region` de eklemeniz gerekir. [Çoklu hizmet aboneliği ile kimlik doğrulama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım `BreakSentenceRequest()` `Main` işlevi aramaktır. Bu `static void Main(string[] args)` kodu bulun ve değiştirin:

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    string route = "/breaksentence?api-version=3.0";
    // Feel free to use any string.
    string breakSentenceText = @"How are you doing today? The weather is pretty pleasant. Have you been to the movies lately?";
    await BreakSentenceRequest(subscriptionKey, endpoint, route, breakSentenceText);
    Console.WriteLine("Press any key to continue.");
    Console.ReadKey();
}
```

Bunu fark edeceksiniz `Main`ki, beyan ettiğiniz `subscriptionKey` `endpoint`, `route`, , ve `breakSentenceText`değerlendirmek için metin .

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
dotnet run
```

## <a name="sample-response"></a>Örnek yanıt

Örneği çalıştırdıktan sonra terminale yazdırılan aşağıdakileri görmeniz gerekir:

```bash
The detected language is \'en\'. Confidence is: 1.
The first sentence length is: 25
```

Bu mesaj ham JSON, bu gibi görünecektir inşa edilmiştir:

```json
[
    {
        "detectedLanguage":
        {
            "language":"en",
            "score":1.0
        },
        "sentLen":[25,32,35]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
