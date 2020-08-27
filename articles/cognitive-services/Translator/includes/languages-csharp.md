---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.custom: devx-track-csharp
ms.openlocfilehash: 9a69c0b7f204fb07e6d4ec94e8a2cecb0a404735
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88921384"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>.NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o languages-sample
cd languages-sample
```

İlk komut iki şeyi yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı bir dizin oluşturur `languages-sample` . İkinci komut, projenizin dizinine değişir.

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

## <a name="get-endpoint-information-from-an-environment-variable"></a>Bir ortam değişkeninden uç nokta bilgilerini al

Sınıfına aşağıdaki satırları ekleyin `Program` . Bu satırlar, ortam değişkenlerinden, abonelik anahtarınızı ve uç noktayı okur ve herhangi bir sorunla karşılaşırsanız bir hata oluşturur.

```csharp
private const string endpoint_var = "TRANSLATOR_TEXT_ENDPOINT";
private static readonly string endpoint = Environment.GetEnvironmentVariable(endpoint_var);

static Program()
{
    if (null == endpoint)
    {
        throw new Exception("Please set/export the environment variable: " + endpoint_var);
    }
}
```

## <a name="create-a-function-to-get-a-list-of-languages"></a>Dillerin listesini almak için bir işlev oluşturma

`Program`Sınıfında adlı bir işlev oluşturun `GetLanguages` . Bu sınıf, diller kaynağını çağırmak için kullanılan kodu kapsüller ve sonucu konsola yazdırır.

```csharp
static void GetLanguages()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-route"></a>Rotayı ayarlama

Bu satırları `GetLanguages` işleve ekleyin.

```csharp
string route = "/languages?api-version=3.0";
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
* Gerekli üst bilgileri ekle
* Zaman uyumsuz istek oluşturma
* Yanıtı yazdırma

Bu kodu öğesine ekleyin `HttpRequestMessage` :

```csharp
// Set the method to GET
request.Method = HttpMethod.Get;
// Construct the full URI
request.RequestUri = new Uri(endpoint + route);
// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;
// Pretty print the response
Console.WriteLine(PrettyPrint(jsonResponse));
Console.WriteLine("Press any key to continue.");
```

Bilişsel hizmetler çoklu hizmet aboneliği kullanıyorsanız, istek parametrelerinize de dahil etmeniz gerekir `Ocp-Apim-Subscription-Region` . [Multi-Service aboneliğiyle kimlik doğrulama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

Yanıtı "düzgün Yazdır" (Yanıt biçimlendirme) ile yazdırmak için, bu işlevi program sınıfınıza ekleyin:

```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım, işlevini çağırmak için kullanılır `GetLanguages()` `Main` . `static void Main(string[] args)`Şu satırları bulun ve ekleyin:

```csharp
GetLanguages();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumu), proje dizininize gidin ve şunu çalıştırın:

```console
dotnet run
```

## <a name="sample-response"></a>Örnek yanıt

Bu [diller listesinde](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)ülke/bölge kısaltmasını bulun.

```json
{
    "translation": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr"
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl"
        },
        ...
    },
    "transliteration": {
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "scripts": [
                {
                    "code": "Arab",
                    "name": "Arabic",
                    "nativeName": "العربية",
                    "dir": "rtl",
                    "toScripts": [
                        {
                            "code": "Latn",
                            "name": "Latin",
                            "nativeName": "اللاتينية",
                            "dir": "ltr"
                        }
                    ]
                },
                {
                    "code": "Latn",
                    "name": "Latin",
                    "nativeName": "اللاتينية",
                    "dir": "ltr",
                    "toScripts": [
                        {
                            "code": "Arab",
                            "name": "Arabic",
                            "nativeName": "العربية",
                            "dir": "rtl"
                        }
                    ]
                }
            ]
        },
      ...
    },
    "dictionary": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
      ...
    }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek uygulamanızın kaynak kodundaki tüm gizli bilgileri abonelik anahtarları gibi kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmenle gerçekleştirebileceğiniz her şeyi anlamak için API başvurusuna göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
