---
title: 'Hızlı Başlangıç: Go için istemci kitaplığı Metin Analizi | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'de Metin Analizi API'si kullanmaya hızlı bir şekilde başlamanıza yardımcı olması için bilgi ve kod örnekleri alın.
services: cognitive-services
author: laramume
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: aahi
ms.openlocfilehash: 1ba2ec6b5c0c59be7b7264f7558fbb393adcc2d8
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142800"
---
# <a name="quickstart-text-analytics-client-library-for-go"></a>Hızlı Başlangıç: Go için metin analizi istemci kitaplığı

Go için Metin Analizi istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. 

Git için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar tümcecik ayıklama

[Başvuru belge](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [paketi (GitHub)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* En son [Go](https://golang.org/dl/) sürümü

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-text-analytics-azure-resource"></a>Metin Analizi Azure kaynağı oluşturma 

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak metin analizi için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* [Deneme anahtarını](https://azure.microsoft.com/try/cognitive-services/#decision) ücretsiz olarak 7 gün boyunca geçerli olacak şekilde öğrenin. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)mevcut olacaktır.
* [Azure Portal](https://portal.azure.com)kaynağı görüntüleyin.

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra adlı `TEXT_ANALYTICS_SUBSCRIPTION_KEY`anahtar için [bir ortam değişkeni oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-go-project"></a>Yeni bir go projesi oluştur

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), go projeniz için yeni bir çalışma alanı oluşturun ve bu projeye gidin. Çalışma alanınız üç klasör içerir: 

* **src** -bu dizin, kaynak kodu ve paketleri içerir. `go get` Komutuyla yüklenmiş tüm paketler burada yer alır.
* **pkg** -bu dizin, derlenmiş go paketi nesnelerini içerir. Bu dosyaların hepsi bir `.a` uzantısı vardır.
* **bin** -bu dizin, çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerir.

> [!TIP]
> [Go çalışma alanının](https://golang.org/doc/code.html#Workspaces)yapısı hakkında daha fazla bilgi edinin. Bu kılavuz, ve `$GOPATH` `$GOROOT`ayarları hakkında bilgi içerir.

`my-app` `src`, Ve`bin`içingereken alt dizinler adlı bir çalışma alanı oluşturun: `pkg`

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

```golang
import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics"
    "github.com/Azure/go-autorest/autorest"
    "log"
    "os"    
)
```

Bu hızlı başlangıç için parametrelerin ve özelliklerin çoğu dize ve bool işaretçilerini beklediği sürece projenize aşağıdaki işlevleri ekleyin.

```golang
// returns a pointer to the string value passed in.
func StringPointer(v string) *string {
    return &v
}

// returns a pointer to the bool value passed in.
func BoolPointer(v bool) *bool {
    return &v
}
```

Uygulamanızın `main` işlevinde, kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun. Bu değerleri TEXT_ANALYTICS_SUBSCRIPTION_KEY ve TEXT_ANALYTICS_ENDPOINT ortam değişkenleriyle elde edin. Uygulamayı düzenleme başladıktan sonra bu ortam değişkenlerini oluşturduysanız, değişkenlere erişmek için kullandığınız düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```golang
var subscriptionKeyVar string = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if "" == os.Getenv(subscriptionKeyVar) {
    log.Fatal("Please set/export the environment variable " + subscriptionKeyVar + ".")
}
var subscriptionKey string = os.Getenv(subscriptionKeyVar)
var endpointVar string = "TEXT_ANALYTICS_ENDPOINT"
if "" == os.Getenv(endpointVar) {
    log.Fatal("Please set/export the environment variable " + endpointVar + ".")
}
var endpoint string = os.Getenv(endpointVar)
```

## <a name="object-model"></a>Nesne modeli 

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir [Baseclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New) nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Metin, kullanılan yöntemine bağlı olarak `documents` `id`, `dictionary` `text` ve`language` özniteliklerinin bir birleşimini içeren nesneler olan bir listesi olarak API 'ye gönderilir. Özniteliği, kaynak `language`olarak çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. `text` 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Metin Analizi istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Varlık tanıma](#entity-recognition)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Projenizin ana işlevinde yeni bir [Baseclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New) nesnesi oluşturun. Anahtarınızı oto Rest 'e geçirin [. Newbiliveservicesauthorizer ()](https://godoc.org/github.com/Azure/go-autorest/autorest#NewCognitiveServicesAuthorizer) işlevi, daha sonra istemcinin `authorizer` özelliğine geçirilecek.

```golang
textAnalyticsClient := textanalytics.New(endpoint)
textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscriptionKey)
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Daha önce oluşturulmuş istemciyi alan `SentimentAnalysis()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri içeren [Multilanguageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` `Language` ve `text` özniteliği içerir. Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. `text` 

```golang
func SentimentAnalysis(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("I had the best day of my life."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, istemcinin [Sentiment ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Sentiment) işlevini çağırın ve sonucu alın. Ardından sonuçları yineleyin ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

```golang
result, err := textAnalyticsclient.Sentiment(ctx, BoolPointer(false), &batchInput)
if err != nil { log.Fatal(err) }

batchResult := textanalytics.SentimentBatchResult{}
jsonString, _ := json.Marshal(result.Value)
json.Unmarshal(jsonString, &batchResult)

// Printing sentiment results
for _,document := range *batchResult.Documents {
    fmt.Printf("Document ID: %s\n", *document.ID)
    fmt.Printf("Sentiment Score: %f\n",*document.Score)
}

// Printing document errors
fmt.Println("Document Errors:")
for _,error := range *batchResult.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
fmt.Println()
```

Projenizin ana işlevinde, çağırın `SentimentAnalysis()`.

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
```

## <a name="language-detection"></a>Dil algılama

Daha önce oluşturulmuş istemciyi alan `LanguageDetection()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri içeren [Languageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#LanguageInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id` `text` ve özniteliği içerir. Özniteliği çözümlenecek metni depolar `id` ve herhangi bir değer olabilir. `text` 

```golang
func LanguageDetection(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.LanguageInput {
        textanalytics.LanguageInput {
            ID:StringPointer("0"),
            Text:StringPointer("This is a document written in English."),
        },
    }

    batchInput := textanalytics.LanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, istemcinin [DetectLanguage ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.DetectLanguage) öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan dili yazdırın.

```golang
result, err := textAnalyticsclient.DetectLanguage(ctx, BoolPointer(false), &batchInput)
if err != nil { log.Fatal(err) }

// Printing language detection results
for _,document := range *result.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Detected Languages with Score: ")
    for _,language := range *document.DetectedLanguages{
        fmt.Printf("%s %f,",*language.Name, *language.Score)
    }
    fmt.Println()
}

// Printing document errors
fmt.Println("Document Errors:")
for _,error := range *result.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
fmt.Println()
```

Projenizin ana işlevinde, çağırın `LanguageDetection()`.

### <a name="output"></a>Output

```console
Document ID: 0 Detected Languages with Score: English 1.000000
```

## <a name="entity-recognition"></a>Varlık tanıma

Daha önce oluşturulmuş istemciyi alan `ExtractEntities()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri içeren [Multilanguageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id`, `language`ve bir `text` özniteliği içerecektir. Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. `text` 

```golang
func ExtractEntities(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, istemcinin [varlıklarını ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Entities) çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın, her belge KIMLIĞINI ve ayıklanan varlık Puanını yazdırın.

```golang
    result, err := textAnalyticsclient.Entities(ctx, BoolPointer(false), &batchInput)
    if err != nil { log.Fatal(err) }

    // Printing extracted entities results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Entities:\n")
        for _,entity := range *document.Entities{
            fmt.Printf("\t\tName: %s\tType: %s",*entity.Name, *entity.Type)
            if entity.SubType != nil{
                fmt.Printf("\tSub-Type: %s\n", *entity.SubType)
            }
            fmt.Println()
            for _,match := range *entity.Matches{
                fmt.Printf("\t\t\tOffset: %v\tLength: %v\tScore: %f\n", *match.Offset, *match.Length, *match.EntityTypeScore)
            }
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors:")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
    fmt.Println()
```

Projenizin ana işlevinde, çağırın `ExtractEntities()`.

### <a name="output"></a>Output

```console
Document ID: 0
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 0   Length: 9   Score: 1.000000
        Name: Bill Gates    Type: Person
            Offset: 25  Length: 10  Score: 0.999847
        Name: Paul Allen    Type: Person
            Offset: 40  Length: 10  Score: 0.998841
        Name: April 4   Type: Other
            Offset: 54  Length: 7   Score: 0.800000
        Name: April 4, 1975 Type: DateTime  Sub-Type: Date

            Offset: 54  Length: 13  Score: 0.800000
        Name: BASIC Type: Other
            Offset: 89  Length: 5   Score: 0.800000
        Name: Altair 8800   Type: Other
            Offset: 116 Length: 11  Score: 0.800000
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Daha önce oluşturulmuş istemciyi alan `ExtractKeyPhrases()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri içeren [Multilanguageınput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput) nesnelerinin bir listesini oluşturun. Her nesne bir `id`, `language`ve bir `text` özniteliği içerecektir. Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. `text`

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("My cat might need to see a veterinarian."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, istemcinin [KeyPhrases ()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.KeyPhrases) öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve ayıklanan anahtar tümceleri yazdırın.

```golang
    result, err := textAnalyticsclient.KeyPhrases(ctx, BoolPointer(false), &batchInput)
    if err != nil { log.Fatal(err) }

    // Printing extracted key phrases results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Key Phrases:\n")
        for _,keyPhrase := range *document.KeyPhrases{
            fmt.Printf("\t\t%s\n",keyPhrase)
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors:")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
    fmt.Println()
```

Projenizin ana işlevinde, çağırın `ExtractKeyPhrases()`.

### <a name="output"></a>Output

```console
Document ID: 0
    Extracted Key Phrases:
        cat
        veterinarian
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar


> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)


* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algıla](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/cognitiveservices)' da bulunabilir.
