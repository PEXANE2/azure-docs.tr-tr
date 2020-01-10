---
title: 'Hızlı başlangıç: .NET için Bing Otomatik Öneri istemci kitaplığı | Microsoft Docs'
description: .NET için Bing Otomatik Öneri istemci kitaplığı ile çalışmaya başlayın ve kısmi sorgu dizelerine göre arama önerileri alın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 12/20/2019
ms.author: aahi
ms.openlocfilehash: 6bcd89422188d8c0064547c37b4d425f47dd6496
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75473930"
---
# <a name="quickstart-bing-autosuggest-client-library-for-net"></a>Hızlı başlangıç: .NET için Bing Otomatik Öneri istemci kitaplığı

.NET için Bing Otomatik Öneri istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

Kısmi sorgu dizelerine göre arama önerileri almak için .NET için Bing Otomatik Öneri istemci Kitaplığı ' nı kullanın.

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingautosuggest?view=azure-dotnet) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingAutoSuggest) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.AutoSuggest/) | [örnek kod](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/BingAutoSuggest/Program.cs)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-an-azure-resource"></a>Azure kaynağı oluşturma

[!INCLUDE [cognitive-services-bing-autosuggest-signup-requirements](../../../../includes/cognitive-services-bing-autosuggest-signup-requirements.md)]

### <a name="create-an-environment-variable"></a>Ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan deneme olmayan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Oluşturduğunuz kaynaktan anahtarınızı ve uç noktayı kullanarak, kimlik doğrulama için iki ortam değişkeni oluşturun:
<!-- replace the below variable names with the names expected in the code sample.-->
* `AUTOSUGGEST_SUBSCRIPTION_KEY`-isteklerinizin kimliğini doğrulamak için kaynak anahtarı.
* `AUTOSUGGEST_ENDPOINT`-API isteklerinin gönderilmesi için kaynak uç noktası. Şöyle görünür: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiz için yönergeleri kullanın.
<!-- replace the below endpoint and key examples -->
#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx AUTOSUGGEST_SUBSCRIPTION_KEY <replace-with-your-autosuggest-api-key>
setx AUTOSUGGEST_ENDPOINT <replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

`.bash_profile`düzenleyin ve ortam değişkenini ekleyin:

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-new-c-application"></a>Yeni C# bir uygulama oluşturun

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `bing-autosuggest-quickstart`adlı yeni bir konsol uygulaması oluşturmak için `dotnet new` komutunu kullanın. Bu komut, tek bir kaynak dosyası olan C# basit bir "Merhaba Dünya" projesi oluşturur: *program.cs*. 

```console
dotnet new console -n bing-autosuggest-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdaki `using` yönergelerini ekleyin:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
```

`Program` sınıfında, kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

```csharp
private const string key_var = "AUTOSUGGEST_SUBSCRIPTION_KEY";
private static readonly string subscription_key = Environment.GetEnvironmentVariable(key_var);

// Note you must use the same region as you used to get your subscription key.
private const string endpoint_var = "AUTOSUGGEST_ENDPOINT";
private static readonly string endpoint = Environment.GetEnvironmentVariable(endpoint_var);
```

Uygulamanın `Main` yönteminde, daha sonra tanımlayacağınızı aşağıdaki Yöntem çağrılarını ekleyin.

```csharp
static void Main(string[] args)
{
    Task.WaitAll(RunQuickstart());
    Console.WriteLine("Press any key to exit.");
    Console.Read();
}
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için Bing Otomatik Öneri istemci Kitaplığı ' nı yüklemelisiniz:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Search.AutoSuggest --version 2.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Bing Otomatik Öneri istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Otomatik öneri isteği gönder](#send-an-autosuggest-request)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu hızlı başlangıçta, Bing Otomatik Öneri anahtarınız için `AUTOSUGGEST_SUBSCRIPTION_KEY`ve `AUTOSUGGEST_ENDPOINT`adlı uç nokta için bir [ortam değişkeni oluşturdunuz](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayılır.


Yeni bir zaman uyumsuz yöntemde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.apikeyserviceclientcredentials?view=azure-dotnet) nesnesi oluşturun ve bir [AutosuggestClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.autosuggestclient?view=azure-dotnet) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

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

## <a name="send-an-autosuggest-request"></a>Otomatik öneri isteği gönder

Aynı yöntemde, Bing 'e bir sorgu göndermek için istemcinin [AutoSuggestMethodAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.autosuggestclientextensions.autosuggestmethodasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Search_AutoSuggest_AutoSuggestClientExtensions_AutoSuggestMethodAsync_Microsoft_Azure_CognitiveServices_Search_AutoSuggest_IAutoSuggestClient_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_Collections_Generic_IList_System_String__System_Threading_CancellationToken_) yöntemini kullanın. Sonra [öneriler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.models.suggestions?view=azure-dotnet) yanıtı üzerinde yineleme yapın ve ilk öneriyi yazdırın.

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

Uygulamayı `dotnet run` komutuyla uygulama dizininizden çalıştırın.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Otomatik Öneri öğreticisi](../tutorials/autosuggest.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Otomatik Öneri nedir?](../get-suggested-search-terms.md)
- [Bing Otomatik Öneri DotNet başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingautosuggest?view=azure-dotnet)
