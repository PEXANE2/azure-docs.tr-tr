---
title: Bing Otomatik Öneri istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/06/2020
ms.author: aahi
ms.openlocfilehash: b352e785673d7c4ed3a9b346758ef0d1fa68b36d
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80887546"
---
Go için Bing Otomatik Öneri istemci kitaplığı ile çalışmaya başlayın. Kitaplığı yüklemek için bu adımları izleyin ve temel görevler için örneklerimizi deneyin. 

Kısmi sorgu dizelerine göre arama önerilerini almak için Go için Bing Otomatik Öneri istemci Kitaplığı ' nı kullanın.

[Başvuru belge](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/autosuggest) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics) | [örnek kodu](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/BingAutoSuggest/BingAutoSuggestQuickstart.go)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* En son [Go](https://golang.org/dl/) sürümü

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-an-azure-resource"></a>Azure kaynağı oluşturma 

Azure kaynağı oluşturarak Bing Otomatik Öneri istemci kitaplığını kullanmaya başlayın. Sizin için doğru olan kaynak türünü seçin:

[!INCLUDE [cognitive-services-bing-autosuggest-signup-requirements](~/includes/cognitive-services-bing-autosuggest-signup-requirements.md)]

### <a name="create-an-environment-variable"></a>Ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan deneme olmayan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Oluşturduğunuz kaynaktan anahtarınızı ve uç noktayı kullanarak, kimlik doğrulama için iki ortam değişkeni oluşturun:
<!-- replace the below variable names with the names expected in the code sample.-->
* `AUTOSUGGEST_SUBSCRIPTION_KEY`-İsteklerinizin kimliğini doğrulamak için kaynak anahtarı.
* `AUTOSUGGEST_ENDPOINT`-API istekleri göndermek için kaynak uç noktası. Şöyle görünür: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiz için yönergeleri kullanın.
<!-- replace the below endpoint and key examples -->
#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx BING_AUTOSUGGEST_SUBSCRIPTION_KEY <replace-with-your-autosuggest-api-key>
setx BING_AUTOSUGGEST_ENDPOINT <replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[macOS](#tab/unix)

Hesabınızı `.bash_profile`düzenleyin ve ortam değişkenini ekleyin:

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-new-go-project"></a>Yeni bir go projesi oluştur

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), go projeniz için yeni bir çalışma alanı oluşturun ve bu projeye gidin. Çalışma alanınız üç klasör içerir: 

* **src** -bu dizin, kaynak kodu ve paketleri içerir. `go get` Komutuyla yüklenmiş tüm paketler burada yer alır.
* **pkg** -bu dizin, derlenmiş go paketi nesnelerini içerir. Bu dosyaların hepsi bir `.a` uzantısı vardır.
* **bin** -bu dizin, çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerir.

> [!TIP]
> [Go çalışma alanının](https://golang.org/doc/code.html#Workspaces)yapısı hakkında daha fazla bilgi edinin. Bu kılavuz, ve `$GOPATH` `$GOROOT`ayarları hakkında bilgi içerir.

Adlı `my-app` bir çalışma alanı ve, ve `src` `pkg` `bin`için gereken alt dizinleri oluşturalım:

```
$ mkdir -p my-app/{src, bin, pkg}  
$ cd my-app
```

### <a name="install-the-client-library-for-go"></a>Go için istemci kitaplığını yükler

Şimdi Go için istemci kitaplığını yükleyelim: 

```bash
$ go get -u <library-location-or-url>
```

ya da DEP kullanıyorsanız, deponuzda çalıştırın:

```bash
$ dep ensure -add <library-location-or-url>
```

### <a name="create-your-go-application"></a>Go uygulamanızı oluşturma

Sonra, şunu adlı `src/sample-app.go`bir dosya oluşturalım:

```bash
$ cd src
$ touch sample-app.go
```

Paket `sample-app.go` adını açın ve ekleyin ve aşağıdaki kitaplıkları içeri aktarın:

```Go
package main

import (
    "context"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/autosuggest"
    "github.com/Azure/go-autorest/autorest"
    "log"
    "os"
)
```

Adlı `main`bir işlev oluşturun. Ardından Bing Otomatik Öneri anahtarınız ve uç noktanız için ortam değişkenleri oluşturun.

```go
func main() {
    // Add your Bing Autosuggest subscription key to your environment variables.
    if "" == os.Getenv("BING_AUTOSUGGEST_SUBSCRIPTION_KEY") {
        log.Fatal("Please set/export the environment variable BING_AUTOSUGGEST_SUBSCRIPTION_KEY.")
    }
    // Add your Bing Autosuggest endpoint to your environment variables.
    var subscription_key string = os.Getenv("BING_AUTOSUGGEST_ENDPOINT")
    if "" == os.Getenv("BING_AUTOSUGGEST_ENDPOINT") {
        log.Fatal("Please set/export the environment variable BING_AUTOSUGGEST_ENDPOINT.")
    }
    var endpoint string = os.Getenv("BING_AUTOSUGGEST_ENDPOINT")
}
```

## <a name="code-examples"></a>Kod örnekleri

Bu kod örnekleri, Go için Bing Otomatik Öneri istemci kitaplığını kullanarak temel görevleri nasıl tamamlayakullanacağınızı gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [API isteği gönder](#send-an-api-request)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE] 
> Bu hızlı başlangıçta `BING_AUTOSUGGEST_SUBSCRIPTION_KEY`, adlı Bing otomatik öneri anahtarınız ve adlı `BING_AUTOSUGGEST_ENDPOINT`uç nokta için bir [ortam değişkeni oluşturdunuz](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

`main()` İşlevinde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. 

```go
// Get the context, which is required by the SDK methods.
ctx := context.Background()

client := autosuggest.New()
// Set the subscription key on the client.
client.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscription_key)
client.Endpoint = endpoint
```

## <a name="send-an-api-request"></a>API isteği gönder

Aynı yöntemde, Bing 'e bir sorgu göndermek için istemcinin [AutoSuggestMethodAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.autosuggestclientextensions.autosuggestmethodasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Search_AutoSuggest_AutoSuggestClientExtensions_AutoSuggestMethodAsync_Microsoft_Azure_CognitiveServices_Search_AutoSuggest_IAutoSuggestClient_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_Collections_Generic_IList_System_String__System_Threading_CancellationToken_) yöntemini kullanın. Sonra [öneriler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.models.suggestions?view=azure-dotnet) yanıtı üzerinde yineleme yapın ve ilk öneriyi yazdırın.

```Go
// This should return the query suggestion "xbox."
result, err := client.AutoSuggest (ctx, "xb", "", "", "", "", "", "", "", "", "", "", []autosuggest.ResponseFormat{"Json"})
if nil != err {
    log.Fatal(err)
}

groups := *result.SuggestionGroups
if len(groups) > 0 {
    group, _ := groups[0].AsSuggestionsSuggestionGroup()
    fmt.Printf ("First suggestion group: %s\n", (*group).Name)
    suggestions := *(*group).SearchSuggestions
    if len(suggestions) > 0 {
        fmt.Println("First suggestion:")
        fmt.Printf("Query: %s\n", *suggestions[0].Query)
        fmt.Printf("Display text: %s\n", *suggestions[0].DisplayText)
    } else {
        fmt.Println("No suggestions found in this group.")
    }
} else {
    fmt.Println("No suggestions found.")
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki `go run [arguments]` komutuyla go uygulamanızı çalıştırın.

```Go
go run sample-app.go
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Otomatik Öneri öğreticisi](../../tutorials/autosuggest.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Otomatik Öneri nedir?](../../get-suggested-search-terms.md)
- [Bing Otomatik Öneri API’si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference)
