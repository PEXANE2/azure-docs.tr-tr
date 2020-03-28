---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: a7715577936b0e95392f2d561e4b492b20c9dbf5
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69906908"
---
[!INCLUDE [Prerequisites](prerequisites-csharp.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-net-core-project"></a>Bir .NET Core projesi oluşturma

Yeni bir komut istemi (veya terminal oturumu) açın ve şu komutları çalıştırın:

```console
dotnet new console -o languages-sample
cd languages-sample
```

İlk komut iki şey yapar. Yeni bir .NET konsol uygulaması oluşturur ve adlı `languages-sample`bir dizin oluşturur. İkinci komut, projenizin dizininde değişir.

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

## <a name="get-endpoint-information-from-an-environment-variable"></a>Bir ortam değişkeninden uç nokta bilgisi alma

`Program` Sınıfa aşağıdaki satırları ekleyin. Bu satırlar, abonelik anahtarınızı ve bitiş noktanızı ortam değişkenlerinden okur ve herhangi bir sorunla karşılaştığınızda hata atar.

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

## <a name="create-a-function-to-get-a-list-of-languages"></a>Dillerin listesini almak için işlev oluşturma

`Program` Sınıfta, adı verilen `GetLanguages`bir işlev oluşturun. Bu sınıf, Diller kaynağını aramak için kullanılan kodu kapsüller ve sonucu konsola yazdırır.

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

Bu satırları işleve `GetLanguages` ekleyin.

```csharp
string route = "/languages?api-version=3.0";
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
* Gerekli üstbilgi ekleme
* Eşzamanlı istekte bulunun
* Yanıtı yazdırma

Bu kodu `HttpRequestMessage`ekleyin:

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

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek parametrelerinize `Ocp-Apim-Subscription-Region` de eklemeniz gerekir. [Çoklu hizmet aboneliği ile kimlik doğrulama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)

Yanıtı "Pretty Print" (yanıt için biçimlendirme) ile yazdırmak için bu işlevi Program sınıfınıza ekleyin:

```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım `GetLanguages()` `Main` işlevi aramaktır. Bu `static void Main(string[] args)` satırları bulun ve ekleyin:

```csharp
GetLanguages();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
dotnet run
```

## <a name="sample-response"></a>Örnek yanıt

Bu dil listesinde ülke/bölge [kısaltmasını](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)bulun.

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

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
