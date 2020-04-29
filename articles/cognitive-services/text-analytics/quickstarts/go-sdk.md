---
title: 'Hızlı başlangıç: go için istemci kitaplığı Metin Analizi | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Azure bilişsel hizmetler 'den go Metin Analizi istemci kitaplığını kullanarak dili algılayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: aahi
ms.openlocfilehash: 0b4495616c750b2b3e8431e011d71ae8671af1ef
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77912658"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-go"></a>Hızlı başlangıç: go için Metin Analizi istemci kitaplığını kullanma

[Başvuru belge](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [paketi (GitHub)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

> [!NOTE]
> Bu hızlı başlangıç yalnızca Metin Analizi sürüm 2,1 için geçerlidir. Şu anda Go için bir v3 istemci kitaplığı kullanılamıyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* En son [Go](https://golang.org/dl/) sürümü
* Azure aboneliğiniz olduktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, anahtarınızı ve uç noktanızı almak için"  target="_blank">Azure Portal bir metin analizi kaynağı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> oluşturun metin analizi bir kaynak oluşturun. 
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu daha sonra hızlı başlangıçta yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-go-project"></a>Yeni bir go projesi oluştur

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), go projeniz için yeni bir çalışma alanı oluşturun ve bu projeye gidin. Çalışma alanınız üç klasör içerir: 

* **src** -bu dizin, kaynak kodu ve paketleri içerir. `go get` Komutuyla yüklenmiş tüm paketler burada yer alır.
* **pkg** -bu dizin, derlenmiş go paketi nesnelerini içerir. Bu dosyaların hepsi bir `.a` uzantısı vardır.
* **bin** -bu dizin, çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerir.

> [!TIP]
> [Go çalışma alanının](https://golang.org/doc/code.html#Workspaces)yapısı hakkında daha fazla bilgi edinin. Bu kılavuz, ve `$GOPATH` `$GOROOT`ayarları hakkında bilgi içerir.

, `pkg`Ve `bin`için `src`gereken `my-app` alt dizinler adlı bir çalışma alanı oluşturun:

```console
$ mkdir -p my-app/{src, bin, pkg}  
$ cd my-app
```

### <a name="install-the-text-analytics-client-library-for-go"></a>Go için Metin Analizi istemci kitaplığını yüklemesi

Go için istemci kitaplığını yükler: 

```console
$ go get -u <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

ya da DEP kullanıyorsanız, deponuzda çalıştırın:

```console
$ dep ensure -add <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

### <a name="create-your-go-application"></a>Go uygulamanızı oluşturma

Ardından, adlı `src/quickstart.go`bir dosya oluşturun:

```bash
$ cd src
$ touch quickstart.go
```

En `quickstart.go` sevdiğiniz IDE veya metin düzenleyicide açın. Ardından, paket adını ekleyin ve aşağıdaki kitaplıkları içeri aktarın:

[!code-go[Import statements](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=imports)]

## <a name="object-model"></a>Nesne modeli 

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir [Baseclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New) nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Metin, kullanılan yöntemine bağlı `documents`olarak `dictionary` `id`, `text`ve `language` özniteliklerinin bir birleşimini içeren nesneler olan bir listesi olarak API 'ye gönderilir. `text` Özniteliği, kaynak `language`olarak çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Metin Analizi istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Varlık tanıma](#entity-recognition)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama


Yeni bir işlevde, kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

Yeni bir [Baseclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New) nesnesi oluşturun. Anahtarınızı oto Rest 'e geçirin [. Newbiliveservicesauthorizer ()](https://godoc.org/github.com/Azure/go-autorest/autorest#NewCognitiveServicesAuthorizer) işlevi, daha sonra istemcinin `authorizer` özelliğine geçirilecek.

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

Adlı `SentimentAnalysis()` yeni bir işlev oluşturun ve daha önce oluşturulan `GetTextAnalyticsClient()` yöntemi kullanarak bir istemci oluşturun. Analiz etmek istediğiniz belgeleri içeren [Multilanguageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` `Language` ve `text` özniteliği içerir. `text` Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. 

İstemcinin [Sentiment ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Sentiment) işlevini çağırın ve sonucu alın. Ardından sonuçları yineleyin ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

[!code-go[Sentiment analysis sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=sentimentAnalysis)]

Projenizde `SentimentAnalysis()` çağırın.

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Dil algılama

Adlı `LanguageDetection()` yeni bir işlev oluşturun ve daha önce oluşturulan `GetTextAnalyticsClient()` yöntemi kullanarak bir istemci oluşturun. Analiz etmek istediğiniz belgeleri içeren [Languageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#LanguageInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` ve `text` özniteliği içerir. `text` Özniteliği çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. 

İstemcinin [DetectLanguage ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.DetectLanguage) öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan dili yazdırın.

[!code-go[Language detection sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=languageDetection)]

Projenizde `LanguageDetection()` çağırın.

### <a name="output"></a>Çıktı

```console
Document ID: 0 , Language: English 
Document ID: 1 , Language: Spanish
Document ID: 2 , Language: Chinese_Simplified
```

## <a name="entity-recognition"></a>Varlık tanıma

Adlı `ExtractEntities()` yeni bir işlev oluşturun ve daha önce oluşturulan `GetTextAnalyticsClient()` yöntemi kullanarak bir istemci oluşturun. Analiz etmek istediğiniz belgeleri içeren [Multilanguageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id`, `language`ve bir `text` özniteliği içerecektir. `text` Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. 

İstemcinin [varlıklarını ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Entities) çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın, her belge KIMLIĞINI ve ayıklanan varlık Puanını yazdırın.

[!code-go[entity recognition sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=entityRecognition)]

Projenizde `ExtractEntities()` çağırın.

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

Adlı `ExtractKeyPhrases()` yeni bir işlev oluşturun ve daha önce oluşturulan `GetTextAnalyticsClient()` yöntemi kullanarak bir istemci oluşturun. Analiz etmek istediğiniz belgeleri içeren [Multilanguageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id`, `language`ve bir `text` özniteliği içerecektir. `text` Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir.

İstemcinin [KeyPhrases ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.KeyPhrases) öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve ayıklanan anahtar tümceleri yazdırın.

[!code-go[key phrase extraction sample](~/azure-sdk-for-go-samples/cognitiveservices/textanalytics.go?name=keyPhrases)]

Projenizde `ExtractKeyPhrases()` çağırın.

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
