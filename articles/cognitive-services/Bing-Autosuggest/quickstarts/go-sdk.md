---
title: 'Quickstart: Bing Autosuggest istemci kitaplığı için Git | Microsoft Dokümanlar'
description: Go için Bing Otomatik Öner istemci kitaplığını başlatın ve kısmi sorgu dizelerini temel alan arama önerileri alın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 59f704377695be0f3253bdd0d7a7b82b460c86d0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80239000"
---
# <a name="quickstart-bing-autosuggest-client-library-for-go"></a>Quickstart: Bing Otomatik öner istemci kitaplığı için Git

Go için Bing Autosuggest istemci kitaplığını başlatın. Kitaplığı yüklemek ve temel görevler için örneklerimizi denemek için aşağıdaki adımları izleyin. 

Kısmi sorgu dizelerine dayalı arama önerileri almak için Git için Bing Otomatik Öner istemci kitaplığını kullanın.

[Referans belgeleri](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/autosuggest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics) | [Örnek kod](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/BingAutoSuggest/BingAutoSuggestQuickstart.go)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Go'nun](https://golang.org/dl/) en son sürümü

## <a name="setting-up"></a>Ayarlama

### <a name="create-an-azure-resource"></a>Azure kaynağı oluşturma 

Azure kaynağı oluşturarak Bing Otomatik Öner istemci kitaplığını kullanmaya başlayın. Sizin için doğru olan kaynak türünü seçin:

[!INCLUDE [cognitive-services-bing-autosuggest-signup-requirements](../../../../includes/cognitive-services-bing-autosuggest-signup-requirements.md)]

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

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-new-go-project"></a>Yeni bir Go projesi oluşturun

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), Go projeniz için yeni bir çalışma alanı oluşturun ve projeye gidin. Çalışma alanınız üç klasör içerir: 

* **src** - Bu dizin kaynak kodu ve paketleri içerir. `go get` Komutu ile yüklü tüm paketler burada bulunacaktır.
* **pkg** - Bu dizin derlenmiş Go paket nesnelerini içerir. Bu dosyaların hepsinin `.a` bir uzantısı var.
* **bin** - Bu dizin çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerir.

> [!TIP]
> [Go çalışma alanının](https://golang.org/doc/code.html#Workspaces)yapısı hakkında daha fazla bilgi edinin. Bu kılavuz, ayar `$GOPATH` `$GOROOT`ve .

Bir çalışma alanı oluşturalım `my-app` ve gerekli alt `src`dizinler için , `pkg`, ve: `bin`

```
$ mkdir -p my-app/{src, bin, pkg}  
$ cd my-app
```

### <a name="install-the-client-library-for-go"></a>Go için istemci kitaplığını yükleme

Şimdi, Go için istemci kitaplığını yükleyelim: 

```bash
$ go get -u <library-location-or-url>
```

veya repo çalışmanızda dep kullanıyorsanız:

```bash
$ dep ensure -add <library-location-or-url>
```

### <a name="create-your-go-application"></a>Go uygulamanızı oluşturun

Sonra, adlı `src/sample-app.go`bir dosya oluşturalım:

```bash
$ cd src
$ touch sample-app.go
```

Paket `sample-app.go` adını açın ve ekleyin ve aşağıdaki kitaplıkları içe aktarın:

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

Adlı `main`bir işlev oluşturun. Ardından Bing Autosuggest anahtarınız ve bitiş noktanız için ortam değişkenleri oluşturun.

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

Bu kod örnekleri, Go için Bing Otomatik Öner istemci kitaplığını kullanarak temel görevleri nasıl tamamlayabileceğinizi gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [API isteği gönderme](#send-an-api-request)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE] 
> Bu hızlı başlatma, Bing otomatik öner anahtarınız için bir `BING_AUTOSUGGEST_SUBSCRIPTION_KEY` [ortam değişkeni oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar, adlı ve bitiş noktanız için .. `BING_AUTOSUGGEST_ENDPOINT`

İşlevde, `main()` son noktanız ve anahtarınızla bir istemciyi anında işaretedin. 

```go
// Get the context, which is required by the SDK methods.
ctx := context.Background()

client := autosuggest.New()
// Set the subscription key on the client.
client.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscription_key)
client.Endpoint = endpoint
```

## <a name="send-an-api-request"></a>API isteği gönderme

Aynı yöntemde, Bing'e sorgu göndermek için istemcinin [AutoSuggestMethodAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.autosuggestclientextensions.autosuggestmethodasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Search_AutoSuggest_AutoSuggestClientExtensions_AutoSuggestMethodAsync_Microsoft_Azure_CognitiveServices_Search_AutoSuggest_IAutoSuggestClient_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_String_System_Collections_Generic_IList_System_String__System_Threading_CancellationToken_) yöntemini kullanın. Ardından [Öneriler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.search.autosuggest.models.suggestions?view=azure-dotnet) yanıtını tekrarlayın ve ilk öneriyi yazdırın.

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

Uygulama dizininizdeki `go run [arguments]` komutla Go uygulamanızı çalıştırın.

```Go
go run sample-app.go
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Otomatik Öneri öğreticisi](../tutorials/autosuggest.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Bing Otomatik Öneri nedir?](../get-suggested-search-terms.md)
- [Bing Otomatik Öneri API’si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference)
