---
title: 'Quickstart: Go için Text Analytics istemci kitaplığı | Microsoft Dokümanlar'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Azure Bilişsel Hizmetler'den Go Text Analytics istemci kitaplığını kullanarak dili algılayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: aahi
ms.openlocfilehash: 0b4495616c750b2b3e8431e011d71ae8671af1ef
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77912658"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-go"></a>Hızlı başlangıç: Go için Text Analytics istemci kitaplığını kullanın

[Referans belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [Paketi (GitHub)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

> [!NOTE]
> Bu hızlı başlangıç yalnızca Text Analytics sürüm 2.1 için geçerlidir. Şu anda, Go için bir v3 istemci kitaplığı kullanılamıyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Go'nun](https://golang.org/dl/) en son sürümü
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun. 
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Bunu daha sonra hızlı bir şekilde yapacaksınız.
    * Hizmeti denemek için ücretsiz fiyatlandırma katmanını kullanabilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-new-go-project"></a>Yeni bir Go projesi oluşturun

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), Go projeniz için yeni bir çalışma alanı oluşturun ve projeye gidin. Çalışma alanınız üç klasör içerir: 

* **src** - Bu dizin kaynak kodu ve paketleri içerir. `go get` Komutu ile yüklü tüm paketler burada bulunacaktır.
* **pkg** - Bu dizin derlenmiş Go paket nesnelerini içerir. Bu dosyaların hepsinin `.a` bir uzantısı var.
* **bin** - Bu dizin çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerir.

> [!TIP]
> [Go çalışma alanının](https://golang.org/doc/code.html#Workspaces)yapısı hakkında daha fazla bilgi edinin. Bu kılavuz, ayar `$GOPATH` `$GOROOT`ve .

Adlı `my-app` bir çalışma alanı ve için `src`gerekli `pkg`alt `bin`dizinler oluşturun , , ve:

```console
$ mkdir -p my-app/{src, bin, pkg}  
$ cd my-app
```

### <a name="install-the-text-analytics-client-library-for-go"></a>Go için Text Analytics istemci kitaplığını yükleme

Go için istemci kitaplığını yükleyin: 

```console
$ go get -u <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

veya repo çalışmanızda dep kullanıyorsanız:

```console
$ dep ensure -add <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

### <a name="create-your-go-application"></a>Go uygulamanızı oluşturun

Ardından, adlı `src/quickstart.go`bir dosya oluşturun:

```bash
$ cd src
$ touch quickstart.go
```

En `quickstart.go` sevdiğiniz IDE veya metin düzenleyicisinde açın. Ardından paket adını ekleyin ve aşağıdaki kitaplıkları içe aktarın:

[!code-go[Import statements](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=imports)]

## <a name="object-model"></a>Nesne modeli 

Text Analytics istemcisi, anahtarınızı kullanarak Azure'a doğrulayan bir [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New) nesnesidir. İstemci, metni tek bir dize veya toplu iş olarak çözümleme için çeşitli yöntemler sağlar. 

Metin, kullanılan yönteme bağlı olarak `documents`, `dictionary` , ve `id` `text` `language` özniteliklerin birleşimini içeren nesneler listesi olarak API'ye gönderilir. Öznitelik, `text` metnin kaynağında `language`çözümlenecek metni depolar ve herhangi bir değer `id` olabilir. 

Yanıt nesnesi, her belge için çözümleme bilgilerini içeren bir listedir. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Python için Text Analytics istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Duygusallık Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Varlık tanıma](#entity-recognition)
* [Anahtar tümcecik çıkarma](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula


Yeni bir işlevde, kaynağınızın Azure bitiş noktası ve abonelik anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

Yeni bir [BaseClient nesnesi](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New) oluşturun. Anahtarınızı [otosuna iletin. Daha](https://godoc.org/github.com/Azure/go-autorest/autorest#NewCognitiveServicesAuthorizer) sonra istemcinin `authorizer` özelliğine geçirilecek NewCognitiveServicesAuthorizer() işlevi.

```go
func GetTextAnalyticsClient() textanalytics.BaseClient {
    var key string = "<paste-your-text-analytics-key-here>"
    var endpoint string = "<paste-your-text-analytics-endpoint-here>"

    textAnalyticsClient := textanalytics.New(endpoint)
    textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(key)

    return textAnalyticsClient
}
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Çağrılan `SentimentAnalysis()` yeni bir işlev oluşturun ve `GetTextAnalyticsClient()` daha önce oluşturulan yöntemi kullanarak bir istemci oluşturun. Çözümlemek istediğiniz belgeleri içeren [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` `Language` ve `text` bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. 

Müşterinin [Sentiment()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Sentiment) işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve duyarlılık puanını yazdırın. 0'a yakın bir puan olumsuz bir duyguyu gösterirken, 1'e yakın bir puan olumlu bir duyguyu gösterir.

[!code-go[Sentiment analysis sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=sentimentAnalysis)]

projenizi arayın. `SentimentAnalysis()`

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Dil algılama

Çağrılan `LanguageDetection()` yeni bir işlev oluşturun ve `GetTextAnalyticsClient()` daha önce oluşturulan yöntemi kullanarak bir istemci oluşturun. Çözümlemek istediğiniz belgeleri içeren [LanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#LanguageInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` ve `text` bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. 

İstemcinin [DetectLanguage()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.DetectLanguage) numaralı telefonlarını arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini yazdırın ve algılanan dili yazdırın.

[!code-go[Language detection sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=languageDetection)]

Projenizi arayın. `LanguageDetection()`

### <a name="output"></a>Çıktı

```console
Document ID: 0 , Language: English 
Document ID: 1 , Language: Spanish
Document ID: 2 , Language: Chinese_Simplified
```

## <a name="entity-recognition"></a>Varlık tanıma

Çağrılan `ExtractEntities()` yeni bir işlev oluşturun ve `GetTextAnalyticsClient()` daha önce oluşturulan yöntemi kullanarak bir istemci oluşturun. Çözümlemek istediğiniz belgeleri içeren [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` `language`, ve `text` bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. 

İstemcinin [Varlıklarını Arayın()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Entities) ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini yazdırın ve ayıklanan varlıklar puanı yazdırın.

[!code-go[entity recognition sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=entityRecognition)]

projenizi arayın. `ExtractEntities()`

### <a name="output"></a>Çıktı

```console
Document ID: 1
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
    Offset: 0, Length: 9,   Score: 1.0
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
    Offset: 25, Length: 10, Score: 0.999847412109375
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
    Offset: 40, Length: 10, Score: 0.9988409876823425
    Name: April 4,  Type: Other,    Sub-Type: N/A
    Offset: 54, Length: 7,  Score: 0.8
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
    Offset: 54, Length: 13, Score: 0.8
    Name: BASIC,    Type: Other,    Sub-Type: N/A
    Offset: 89, Length: 5,  Score: 0.8
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
    Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
    Offset: 21, Length: 9,  Score: 0.999755859375
    Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
    Offset: 60, Length: 7,  Score: 0.9911284446716309
    Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
    Offset: 71, Length: 13, Score: 0.8
    Name: Seattle,  Type: Location, Sub-Type: N/A
    Offset: 88, Length: 7,  Score: 0.9998779296875
```

## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

Çağrılan `ExtractKeyPhrases()` yeni bir işlev oluşturun ve `GetTextAnalyticsClient()` daha önce oluşturulan yöntemi kullanarak bir istemci oluşturun. Çözümlemek istediğiniz belgeleri içeren [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` `language`, ve `text` bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir.

İstemcinin [Anahtar İfadelerini()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.KeyPhrases) arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini yazdırın ve anahtar tümcecikleri ayıklayın.

[!code-go[key phrase extraction sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=keyPhrases)]

Projenizi arayın. `ExtractKeyPhrases()`

### <a name="output"></a>Çıktı

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                veterinarian
Document ID: 4
         Key phrases:
                fútbol
```
