---
title: Bing Autosuggest C# istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/06/2020
ms.author: aahi
ms.openlocfilehash: ac46eb0119ac95cf09e48823686a6c563d8d4d4a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887539"
---
.NET için Bing Autosuggest istemci kitaplığını başlatın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Kısmi sorgu dizelerine dayalı arama önerileri almak için .NET için Bing Otomatik Öner istemci kitaplığını kullanın.

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingautosuggest?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingAutoSuggest) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.AutoSuggest/) | Örnek[kodu](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/BingAutoSuggest/Program.cs)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="setting-up"></a>Ayarlama

### <a name="create-an-azure-resource"></a>Azure kaynağı oluşturma

[!INCLUDE [cognitive-services-bing-autosuggest-signup-requirements](~/includes/cognitive-services-bing-autosuggest-signup-requirements.md)]

### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019'dan sonra oluşturulan deneme dışı kaynakların bitiş noktaları, aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains)bakın. 

Oluşturduğunuz kaynaktan anahtar ve bitiş noktanızı kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:
<!-- replace the below variable names with the names expected in the code sample.-->
* `AUTOSUGGEST_SUBSCRIPTION_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `AUTOSUGGEST_ENDPOINT`- API isteklerini göndermek için kaynak bitiş noktası. Bu gibi görünecektir: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiziçin yönergeleri kullanın.
<!-- replace the below endpoint and key examples -->
#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx AUTOSUGGEST_SUBSCRIPTION_KEY <replace-with-your-autosuggest-api-key>
setx AUTOSUGGEST_ENDPOINT <replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[macOS](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `bing-autosuggest-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *program.cs.* 

```console
dotnet new console -n bing-autosuggest-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
dotnet build
```

Yapı çıktısı hiçbir uyarı veya hata içermemelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki *program.cs* dosyasını açın. Aşağıdaki `using` yönergeleri ekleyin:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
```

`Program` Sınıfta, kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

```csharp
private const string key_var = "AUTOSUGGEST_SUBSCRIPTION_KEY";
private static readonly string subscription_key = Environment.GetEnvironmentVariable(key_var);

// Note you must use the same region as you used to get your subscription key.
private const string endpoint_var = "AUTOSUGGEST_ENDPOINT";
private static readonly string endpoint = Environment.GetEnvironmentVariable(endpoint_var);
```

Uygulama `Main` yönteminde, daha sonra tanımlayacak aşağıdaki yöntem çağrıları ekleyin.

```csharp
static void Main(string[] args)
{
    Task.WaitAll(RunQuickstart());
    Console.WriteLine("Press any key to exit.");
    Console.Read();
}
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Uygulama dizininde ,.NET için Bing Autosuggest istemci kitaplığını aşağıdaki komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Search.AutoSuggest --version 2.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Bing Otomatik Öner istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Otomatik önerme isteği gönderme](#send-an-autosuggest-request)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlatma, Bing Autosuggest anahtarınız için bir [ortam değişkeni oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar, adlı `AUTOSUGGEST_SUBSCRIPTION_KEY`ve bitiş noktanız için .. `AUTOSUGGEST_ENDPOINT`


Yeni bir eşzamanlı yöntemle, son noktanız ve anahtarınızla istemciyi anında anında işaretedin. Anahtarınızla bir [ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.apikeyserviceclientcredentials?view=azure-dotnet) oluşturun ve bir [AutosuggestClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.autosuggestclient?view=azure-dotnet) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

```csharp
async static Task RunQuickstart()
{
    // Generate the credentials and create the client.
    var credentials = new Microsoft.Azure.CognitiveServices.Search.AutoSuggest.ApiKeyServiceClientCredentials(subscription_key);
    var client = new AutoSuggestClient(credentials, new System.Net.Http.DelegatingHandler[] { })
    {
        Endpoint = endpoint
    };
}
```

## <a name="send-an-autosuggest-request"></a>Otomatik önerme isteği gönderme

Aynı yöntemde, Bing'e sorgu göndermek için istemcinin [AutoSuggestMethodAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.autosuggestclientextensions.autosuggestmethodasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Search_AutoSuggest_AutoSuggestClientExtensions_AutoSuggestMethodAsync_Microsoft_Azure_CognitiveServices_Search_AutoSuggest_IAutoSuggestClient_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_Collections_Generic_IList_System_String__System_Threading_CancellationToken_) yöntemini kullanın. Ardından [Öneriler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.models.suggestions?view=azure-dotnet) yanıtını tekrarlayın ve ilk öneriyi yazdırın.

```csharp
var result = await client.AutoSuggestMethodAsync("xb");
var groups = result.SuggestionGroups;
if (groups.Count > 0) {
    var group = groups[0];
    Console.Write("First suggestion group: {0}\n", group.Name);
    var suggestions = group.SearchSuggestions;
    if (suggestions.Count > 0)
    {
        Console.WriteLine("First suggestion:");
        Console.WriteLine("Query: {0}", suggestions[0].Query);
        Console.WriteLine("Display text: {0}", suggestions[0].DisplayText);
    }
    else
    {
        Console.WriteLine("No suggestions found in this group.");
    }
}
else
{
    Console.WriteLine("No suggestions found.");
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki uygulamayı komutla `dotnet run` çalıştırın.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Otomatik Öneri öğreticisi](../../tutorials/autosuggest.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Otomatik Öneri nedir?](../../get-suggested-search-terms.md)
- [Bing Autosuggest dotnet başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingautosuggest?view=azure-dotnet)
