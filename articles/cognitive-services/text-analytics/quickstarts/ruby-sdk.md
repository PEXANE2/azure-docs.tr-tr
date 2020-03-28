---
title: 'Quickstart: Ruby için Text Analytics istemci kitaplığı | Microsoft Dokümanlar'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Azure Bilişsel Hizmetleri'nin Ruby Text Analytics istemci kitaplığını kullanarak dili algılayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: aahi
ms.openlocfilehash: 0d4d32a413dd22c55f1b2f01dce3a3df81f5f729
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77919677"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-ruby"></a>Quickstart: Ruby için Text Analytics istemci kitaplığını kullanın

Text Analytics istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Gerçekleştirmek için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar ifade ayıklama

> [!NOTE]
> Bu hızlı başlangıç yalnızca Text Analytics sürüm 2.1 için geçerlidir. Şu anda, Ruby için bir v3 istemci kitaplığı kullanılamıyor.

[Referans belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_textanalytics) | [Paketi (RubyGems)](https://rubygems.org/gems/azure_cognitiveservices_textanalytics) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

<a name="HOLTop"></a>

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Ruby'nin](https://www.ruby-lang.org/) geçerli sürümü
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun. 
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Bunu daha sonra hızlı bir şekilde yapacaksınız.
    * Hizmeti denemek için ücretsiz fiyatlandırma katmanını kullanabilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-new-ruby-application"></a>Yeni bir Ruby uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. Ardından, kodunuzu `GemFile`adında bir dosya ve Ruby dosyası oluşturun.

```console
mkdir myapp && cd myapp
```

Sizin `GemFile`, bir bağımlılık olarak istemci kitaplığı eklemek için aşağıdaki satırları ekleyin.

```ruby
source 'https://rubygems.org'
gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
```

Ruby dosyanızda, aşağıdaki paketleri içe aktarın.

[!code-ruby[Import statements](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=includeStatement)]

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. 

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```ruby
const subscription_key = '<paste-your-text-analytics-key-here>'
const endpoint = `<paste-your-text-analytics-endpoint-here>`
```

## <a name="object-model"></a>Nesne modeli 

Text Analytics istemcisi anahtarınızı kullanarak Azure'a doğrular. İstemci, metni tek bir dize veya toplu iş olarak çözümleme için çeşitli yöntemler sağlar. 

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

Adlı `TextAnalyticsClient`bir sınıf oluşturun. 

```ruby
class TextAnalyticsClient
  @textAnalyticsClient
  #...
end
```

Bu sınıfta, anahtar ve `initialize` bitiş noktanızı kullanarak istemcinin kimliğini doğrulamak için bir işlev oluşturun. 

[!code-ruby[initialize function for authentication](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=initialize)]

Sınıfın dışında, istemcinin `new()` işlevini kullanarak anında kullanın.

[!code-ruby[client creation](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=clientCreation)] 

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>Yaklaşım analizi

İstemci nesnesinde, daha `AnalyzeSentiment()` sonra oluşturulacak giriş belgelerinin listesini alan bir işlev oluşturun. İstemcinin `sentiment()` işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve duyarlılık puanını yazdırın. 0'a yakın bir puan olumsuz bir duyguyu gösterirken, 1'e yakın bir puan olumlu bir duyguyu gösterir.

[!code-ruby[client method for sentiment analysis](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=analyzeSentiment)] 

İstemci işlevinin dışında, daha `SentimentAnalysisExample()` önce `TextAnalyticsClient` oluşturulan nesneyi alan yeni bir işlev oluşturun. Çözümlemek istediğiniz `MultiLanguageInput` belgeleri içeren nesnelerin bir listesini oluşturun. Her nesne bir `id` `Language` ve `text` bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. O zaman istemcinin `AnalyzeSentiment()` işlevini arayın.

[!code-ruby[sentiment analysis document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=sentimentCall)] 

`SentimentAnalysisExample()` İşlevi ara.

```ruby
SentimentAnalysisExample(textAnalyticsClient)
```

### <a name="output"></a>Çıktı

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>Dil algılama

İstemci nesnesinde, daha `DetectLanguage()` sonra oluşturulacak giriş belgelerinin listesini alan bir işlev oluşturun. İstemcinin `detect_language()` işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini yazdırın ve algılanan dili yazdırın.

[!code-ruby[client method for language detection](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguage)] 

İstemci işlevinin dışında, daha `DetectLanguageExample()` önce `TextAnalyticsClient` oluşturulan nesneyi alan yeni bir işlev oluşturun. Çözümlemek istediğiniz `LanguageInput` belgeleri içeren nesnelerin bir listesini oluşturun. Her nesne bir `id`ve `text` bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. O zaman istemcinin `DetectLanguage()` işlevini arayın.

[!code-ruby[language detection document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguageCall)] 

`DetectLanguageExample()` İşlevi ara.

```ruby
DetectLanguageExample(textAnalyticsClient)
```

### <a name="output"></a>Çıktı

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>Varlık tanıma

İstemci nesnesinde, daha `RecognizeEntities()` sonra oluşturulacak giriş belgelerinin listesini alan bir işlev oluşturun. İstemcinin `entities()` işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve tanınan varlıkları yazdırın.

[!code-ruby[client method for entity recognition](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntities)]

İstemci işlevinin dışında, daha `RecognizeEntitiesExample()` önce `TextAnalyticsClient` oluşturulan nesneyi alan yeni bir işlev oluşturun. Çözümlemek istediğiniz `MultiLanguageInput` belgeleri içeren nesnelerin bir listesini oluşturun. Her nesne bir `id`, `language`a `text` , ve bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar, `language` metnin dilidir ve `id` herhangi bir değer olabilir. O zaman istemcinin `RecognizeEntities()` işlevini arayın.

[!code-ruby[entity recognition documents and method call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntitiesCall)] 

`RecognizeEntitiesExample()` İşlevi ara.

```ruby
RecognizeEntitiesExample(textAnalyticsClient)
```

### <a name="output"></a>Çıktı

```console
===== ENTITY RECOGNITION =====
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

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

İstemci nesnesinde, daha `ExtractKeyPhrases()` sonra oluşturulacak giriş belgelerinin listesini alan bir işlev oluşturun. İstemcinin `key_phrases()` işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve ayıklanan anahtar tümcecikleri yazdırın.

[!code-ruby[key phrase extraction client method](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=extractKeyPhrases)] 

İstemci işlevinin dışında, daha `KeyPhraseExtractionExample()` önce `TextAnalyticsClient` oluşturulan nesneyi alan yeni bir işlev oluşturun. Çözümlemek istediğiniz `MultiLanguageInput` belgeleri içeren nesnelerin bir listesini oluşturun. Her nesne bir `id`, `language`a `text` , ve bir öznitelik içerir. Öznitelik, `text` çözümlenecek metni depolar, `language` metnin dilidir ve `id` herhangi bir değer olabilir. O zaman istemcinin `ExtractKeyPhrases()` işlevini arayın.

[!code-ruby[key phrase document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=keyPhrasesCall)]


`KeyPhraseExtractionExample()` İşlevi ara.

```ruby
KeyPhraseExtractionExample(textAnalyticsClient)
```

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
                rock
Document ID: 4
         Key phrases:
                fútbol
```
