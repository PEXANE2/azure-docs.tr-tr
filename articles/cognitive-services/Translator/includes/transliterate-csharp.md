---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 81fb599ca4987adccdb91baa7a74c33ae3af48d4
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69906505"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>Bir .NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o transliterate-sample
cd transliterate-sample
```

İlk komut iki şey yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı `transliterate-sample`bir dizin oluşturur. İkinci komut, projenizin dizininde değişir.

Ardından, Json.Net yüklemeniz gerekir. Projenizin dizininden çalıştır:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>C# dil sürümünü seçin

Bu hızlı başlatma C# 7.1 veya daha sonra gerektirir. Projenizin C# sürümünü değiştirmenin birkaç yolu vardır. Bu kılavuzda, dosyayı nasıl ayarlayabileceğinizi `transliterate-sample.csproj` göstereceğiz. Visual Studio'da dili değiştirme gibi tüm kullanılabilir seçenekler için [C# dil sürümünü seçin'e](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version)bakın.

Projenizi açın, `transliterate-sample.csproj`sonra açın. 7.1 `LangVersion` veya daha sonra olarak ayarlandıklarına emin olun. Dil sürümü için bir özellik grubu yoksa, aşağıdaki satırları ekleyin:

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
public class TransliterationResult
{
    public string Text { get; set; }
    public string Script { get; set; }
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

## <a name="create-a-function-to-transliterate-text"></a>Metni çevirmek için bir işlev oluşturma

`Program` Sınıf içinde, adı verilen `TransliterateTextRequest()`bir eşzamanlı işlev oluşturun. Bu işlev dört `subscriptionKey`bağımsız `endpoint` `route`değişken `inputText`alır: , , ve .

```csharp
static public async Task TransliterateTextRequest(string subscriptionKey, string endpoint, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-translation-request"></a>Çeviri isteğini serihale etme

Ardından, çevirmek istediğiniz metni içeren JSON nesnesini oluşturmamız ve seri hale vermemiz gerekir. Unutmayın, birden fazla nesne `body`geçirebilirsiniz.

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
// Set the method to Post.
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
TransliterationResult[] deserializedOutput = JsonConvert.DeserializeObject<TransliterationResult[]>(result);
// Iterate over the deserialized results.
foreach (TransliterationResult o in deserializedOutput)
{
    Console.WriteLine("Transliterated to {0} script: {1}", o.Script, o.Text);
}
```

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek parametrelerinize `Ocp-Apim-Subscription-Region` de eklemeniz gerekir. [Çoklu hizmet aboneliği ile kimlik doğrulama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım `TransliterateTextRequest()` `Main` işlevi aramaktır. Bu örnekte, Japonca'dan Latin alfabesine çeviriyoruz. Bu `static void Main(string[] args)` kodu bulun ve değiştirin:

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    // https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate
    string route = "/transliterate?api-version=3.0&language=ja&fromScript=jpan&toScript=latn";
    string textToTransliterate = @"こんにちは";
    await TransliterateTextRequest(subscriptionKey, endpoint, route, textToTransliterate);
    Console.WriteLine("Press any key to continue.");
    Console.ReadKey();
}
```

Fark edeceksiniz `Main`ki, "ve senaryoyu `subscriptionKey` `endpoint`çeviriye çevirdiğini" beyan `route` `textToTransliterate`ediyorsun.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
dotnet run
```

## <a name="sample-response"></a>Örnek yanıt

Örneği çalıştırdıktan sonra terminale yazdırılan aşağıdakileri görmeniz gerekir:

```bash
Transliterated to latn script: Kon\'nichiwa
```

Bu mesaj ham JSON, bu gibi görünecektir inşa edilmiştir:

```json
[
    {
        "script": "latn",
        "text": "konnichiwa"
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
