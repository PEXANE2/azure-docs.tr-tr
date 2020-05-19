---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 3dc58b68270707eb5e92214def85ec8cf9cb3f5b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587075"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Setup and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>.NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o alternate-sample
cd alternate-sample
```

İlk komut iki şeyi yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı bir dizin oluşturur `alternate-sample` . İkinci komut, projenizin dizinine değişir.

Sonra, Json.Net yüklemeniz gerekir. Projenizin dizininden şunu çalıştırın:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Projenize gerekli ad alanlarını ekleyin

`dotnet new console`Daha önce çalıştırdığınız komut, dahil olmak üzere bir proje oluşturdu `Program.cs` . Bu dosya, uygulama kodunuzu koyacaksınız. `Program.cs`Öğesini açın ve var olan using deyimlerini değiştirin. Bu deyimler, örnek uygulamayı derlemek ve çalıştırmak için gereken tüm türlere erişebildiğinizden emin olmanızı sağlar.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="get-subscription-information-from-environment-variables"></a>Ortam değişkenlerinden abonelik bilgilerini al

Sınıfına aşağıdaki satırları ekleyin `Program` . Bu satırlar, ortam değişkenlerinden, abonelik anahtarınızı ve uç noktayı okur ve herhangi bir sorunla karşılaşırsanız bir hata oluşturur.

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

Sınıfı içinde `Program` adlı bir işlev oluşturun `AltTranslation` . Bu sınıf, sözlük kaynağını çağırmak için kullanılan kodu kapsüller ve sonucu konsola yazdırır.

```csharp
static void AltTranslation()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="construct-the-uri"></a>URI 'yi oluşturun

Bu satırları `AltTranslation` işleve ekleyin. İle birlikte, `api-version` iki ek parametre bildirildiğine dikkat edin. Bu parametreler, çeviri girişini ve çıkışını ayarlamak için kullanılır. Bu örnekte, bunlar Ingilizce ( `en` ) ve İspanyolca ( `es` ).

```csharp
string route = "/dictionary/lookup?api-version=3.0";
static string params_ = "from=en&to=es";
static string uri = endpoint + path + params_;
```

Ardından, çevirmek istediğiniz metni içeren JSON nesnesini oluşturup serileştirmemiz gerekir. Dikkat edin, dizide birden fazla nesne geçirebilirsiniz `body` .

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Elephants" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>İstemci örneği oluşturma ve bir istek oluşturma

Bu satırlar ve ' nin örneğini oluşturur `HttpClient` `HttpRequestMessage` :

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>İsteği oluşturun ve yanıtı yazdırın

İçinde şunları `HttpRequestMessage` yapabilirsiniz:

* HTTP yöntemini bildirin
* İstek URI 'sini oluşturun
* İstek gövdesini (serileştirilmiş JSON nesnesi) ekleme
* Gerekli üst bilgileri ekle
* Zaman uyumsuz istek oluşturma
* Yanıtı yazdırma

Bu kodu öğesine ekleyin `HttpRequestMessage` :

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

`PrettyPrint`JSON yanıtınıza biçimlendirme eklemek için ekleyin:
```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

Bilişsel hizmetler çoklu hizmet aboneliği kullanıyorsanız, istek parametrelerinize de dahil etmeniz gerekir `Ocp-Apim-Subscription-Region` . [Multi-Service aboneliğiyle kimlik doğrulama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım, işlevini çağırmak için kullanılır `AltTranslation()` `Main` . `static void Main(string[] args)`Şu satırları bulun ve ekleyin:

```csharp
AltTranslation();
Console.WriteLine("Press any key to continue.");
Console.ReadKey();
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumu), proje dizininize gidin ve şunu çalıştırın:

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

Örnek uygulamanızın kaynak kodundaki tüm gizli bilgileri abonelik anahtarları gibi kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmenle gerçekleştirebileceğiniz her şeyi anlamak için API başvurusuna göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
