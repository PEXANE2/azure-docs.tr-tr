---
title: "Hızlı Başlangıç: Go SDK 'sını kullanarak Metin Analizi hizmetini çağırma"
titleSuffix: Azure Cognitive Services
description: Microsoft bilişsel hizmetler 'deki Metin Analizi API'si kullanmaya hızlı bir şekilde başlamanıza yardımcı olması için bilgi ve kod örnekleri alın.
services: cognitive-services
author: laramume
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: d3644022e1877369368953b9f147c64aaae2d459
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697638"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-go-sdk"></a>Hızlı Başlangıç: Go SDK 'sını kullanarak Metin Analizi hizmetini çağırma 
<a name="HOLTop"></a>

Go için Metin Analizi SDK ile dili çözümlemeye başlamak için bu hızlı başlangıcı kullanın. Bu makalede, dili algılama, yaklaşımı çözümleme, anahtar tümceleri ayıklama ve bağlantılı varlıkları belirleme işlemlerinin nasıl yapılacağı gösterilir. REST API çoğu programlama dili ile uyumlu olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/cognitiveservices)' da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Go için metin analizi SDK 'sı](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

## <a name="set-up-a-new-project"></a>Yeni bir proje ayarlama

En sevdiğiniz kod düzenleyicide veya IDE 'de yeni bir go projesi oluşturun. Ardından Go dosyasına aşağıdaki import ifadesini ekleyin.

```golang
import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics"
    "github.com/Azure/go-autorest/autorest"
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

## <a name="create-text-analytics-client-and-authenticate-credentials"></a>Metin Analizi Istemcisi oluşturma ve kimlik bilgilerini kimlik doğrulama

Projenizin ana işlevinde yeni `TextAnalytics` bir nesne oluşturun. Metin Analizi aboneliğiniz için doğru Azure bölgesini kullanın. Örneğin: `https://eastus.api.cognitive.microsoft.com` Deneme anahtarı kullanıyorsanız, konumu güncelleştirmeniz gerekmez.

```golang
//Replace 'eastus' with the correct region for your Text Analytics subscription
textAnalyticsClient := textanalytics.New("https://eastus.api.cognitive.microsoft.com")
```

Anahtarınız için bir değişken oluşturun ve daha sonra `autorest.NewCognitiveServicesAuthorizer` `authorizer` istemcinin özelliğine geçirilecek olan işleve geçirin.

```golang
subscriptionKey := "<<subscriptionKey>>"
textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscriptionKey)
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Daha önce oluşturulmuş istemciyi alan `SentimentAnalysis()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `MultiLanguageInput` içeren bir nesne listesi oluşturun. Her nesne bir `id` `Language` ve `text` özniteliği içerir. Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. `text` 

```golang
func SentimentAnalysis(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("I had the best day of my life."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("1"),
            Text:StringPointer("This was a waste of my time. The speaker put me to sleep."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("2"),
            Text:StringPointer("No tengo dinero ni nada que dar..."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("it"),
            ID:StringPointer("3"),
            Text:StringPointer("L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, sonucunu çağırın `textAnalyticsclient.Sentiment()` ve elde edin. Ardından sonuçları yineleyin ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

```golang
result, _ := textAnalyticsclient.Sentiment(ctx, BoolPointer(false), &batchInput)
batchResult := textanalytics.SentimentBatchResult{}
jsonString, _ := json.Marshal(result.Value)
json.Unmarshal(jsonString, &batchResult)

// Printing sentiment results
for _,document := range *batchResult.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Sentiment Score: %f\n",*document.Score)
}

// Printing document errors
fmt.Println("Document Errors")
for _,error := range *batchResult.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
```

Projenizin ana işlevinde, çağırın `SentimentAnalysis()`.

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="language-detection"></a>Dil algılama

Daha önce oluşturulmuş istemciyi alan `LanguageDetection()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `LanguageInput` içeren bir nesne listesi oluşturun. Her nesne bir `id` `text` ve özniteliği içerir. Özniteliği çözümlenecek metni depolar `id` ve herhangi bir değer olabilir. `text` 

```golang
func LanguageDetection(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.LanguageInput {
        textanalytics.LanguageInput {
            ID:StringPointer("0"),
            Text:StringPointer("This is a document written in English."),
        },
        textanalytics.LanguageInput {
            ID:StringPointer("1"),
            Text:StringPointer("Este es un document escrito en Español."),
        },
        textanalytics.LanguageInput {
            ID:StringPointer("2"),
            Text:StringPointer("这是一个用中文写的文件"),
        },
    }

    batchInput := textanalytics.LanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, sonucunu çağırın `textAnalyticsclient.DetectLanguage()` ve elde edin. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan dili yazdırın.

```golang
result, _ := textAnalyticsclient.DetectLanguage(ctx, BoolPointer(false), &batchInput)

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
fmt.Println("Document Errors")
for _,error := range *result.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
```

Projenizin ana işlevinde, çağırın `LanguageDetection()`.

### <a name="output"></a>Output

```console
Document ID: 0 Detected Languages with Score: English 1.000000,
Document ID: 1 Detected Languages with Score: Spanish 1.000000,
Document ID: 2 Detected Languages with Score: Chinese_Simplified 1.000000,
```

## <a name="entity-recognition"></a>Varlık tanıma

Daha önce oluşturulmuş istemciyi alan `ExtractEntities()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `MultiLanguageInput` içeren bir nesne listesi oluşturun. Her nesne bir `id`, `language`ve bir `text` özniteliği içerecektir. Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. `text` 

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("1"),
            Text:StringPointer("La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde ve sonucu elde `call textAnalyticsclient.Entities()` edin. Sonra sonuçlar arasında yineleme yapın, her belge KIMLIĞINI ve ayıklanan varlık Puanını yazdırın.

```golang
    result, _ := textAnalyticsclient.Entities(ctx, BoolPointer(false), &batchInput)

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
    fmt.Println("Document Errors")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
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

Document ID: 1
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 21  Length: 9   Score: 0.999756
        Name: Redmond (Washington)  Type: Location
            Offset: 60  Length: 7   Score: 0.991128
        Name: 21 kilómetros Type: Quantity  Sub-Type: Dimension

            Offset: 71  Length: 13  Score: 0.800000
        Name: Seattle   Type: Location
            Offset: 88  Length: 7   Score: 0.999878
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Daha önce oluşturulmuş istemciyi alan `ExtractKeyPhrases()` adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `MultiLanguageInput` içeren bir nesne listesi oluşturun. Her nesne bir `id`, `language`ve bir `text` özniteliği içerecektir. Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. `text`

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("ja"),
            ID:StringPointer("0"),
            Text:StringPointer("猫は幸せ"),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("de"),
            ID:StringPointer("1"),
            Text:StringPointer("Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("2"),
            Text:StringPointer("My cat might need to see a veterinarian."),
        },
        textanalytics.MultiLanguageInput {
            Language: StringPointer("es"),
            ID:StringPointer("3"),
            Text:StringPointer("A mi me encanta el fútbol!"),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Aynı işlevde, textAnalyticsclient. KeyPhrases () öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve ayıklanan anahtar tümceleri yazdırın.

```golang
    result, _ := textAnalyticsclient.KeyPhrases(ctx, BoolPointer(false), &batchInput)

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
    fmt.Println("Document Errors")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
```

Projenizin ana işlevinde, çağırın `ExtractKeyPhrases()`.

### <a name="output"></a>Output

```console
Document ID: 0
    Extracted Key Phrases:
        幸せ

Document ID: 1
    Extracted Key Phrases:
        Stuttgart
        Hotel
        Fahrt
        Fu

Document ID: 2
    Extracted Key Phrases:
        cat
        veterinarian

Document ID: 3
    Extracted Key Phrases:
        fútbol
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Metin analizi genel bakış](../overview.md) [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
