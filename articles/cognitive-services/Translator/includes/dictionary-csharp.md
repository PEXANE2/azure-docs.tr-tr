---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 55ad3591a8c2e7d5de6d1efe255e0f3a4b3c11bd
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69907092"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Setup and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>Bir .NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o alternate-sample
cd alternate-sample
```

İlk komut iki şey yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı `alternate-sample`bir dizin oluşturur. İkinci komut, projenizin dizininde değişir.

Ardından, Json.Net yüklemeniz gerekir. Projenizin dizininden çalıştır:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Projenize gerekli ad alanları ekleme

Daha `dotnet new console` önce çalıştırdığınız komut, `Program.cs`'. Bu dosya, uygulama kodunuzu koyacağınız yerdir. Varolan ifadeleri açın `Program.cs`ve değiştirin. Bu ifadeler, örnek uygulamayı oluşturmak ve çalıştırmak için gereken tüm türlere erişebildiğinizi sağlar.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
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

## <a name="create-a-function-to-get-alternate-translations"></a>Alternatif çeviriler almak için bir işlev oluşturma

`Program` Sınıf içinde, adı verilen `AltTranslation`bir işlev oluşturun. Bu sınıf, Sözlük kaynağını aramak için kullanılan kodu kapsüller ve sonucu konsola yazdırır.

```csharp
static void AltTranslation()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="construct-the-uri"></a>URI'yi inşa edin

Bu satırları işleve `AltTranslation` ekleyin. İki ek parametrenin `api-version`birlikte beyan edildiğini fark edeceksiniz. Bu parametreler çeviri giriş ve çıktısını ayarlamak için kullanılır. Bu örnekte, bunlar`en`İngilizce (`es`) ve İspanyolca ( )

```csharp
string route = "/dictionary/lookup?api-version=3.0";
static string params_ = "from=en&to=es";
static string uri = endpoint + path + params_;
```

Ardından, çevirmek istediğiniz metni içeren JSON nesnesini oluşturmamız ve seri hale vermemiz gerekir. Unutmayın, `body` dizide birden fazla nesne geçirebilirsiniz.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Elephants" } };
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
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(uri);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(PrettyPrint(jsonResponse));
Console.WriteLine("Press any key to continue.");
```

JSON yanıtınıza biçimlendirme eklemek için ekleyin: `PrettyPrint`
```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek parametrelerinize `Ocp-Apim-Subscription-Region` de eklemeniz gerekir. [Çoklu hizmet aboneliği ile kimlik doğrulama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım `AltTranslation()` `Main` işlevi aramaktır. Bu `static void Main(string[] args)` satırları bulun ve ekleyin:

```csharp
AltTranslation();
Console.WriteLine("Press any key to continue.");
Console.ReadKey();
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
dotnet run
```

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
