---
title: 'Hızlı başlangıç: Ruby için istemci kitaplığı Metin Analizi | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Azure bilişsel hizmetler 'den Ruby Metin Analizi istemci kitaplığı 'nı kullanarak dili algılayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: aahi
ms.openlocfilehash: 0d4d32a413dd22c55f1b2f01dce3a3df81f5f729
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77919677"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-ruby"></a>Hızlı başlangıç: Ruby için Metin Analizi istemci kitaplığını kullanma

Metin Analizi istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

Şunları yapmak için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar ifade ayıklama

> [!NOTE]
> Bu hızlı başlangıç yalnızca Metin Analizi sürüm 2,1 için geçerlidir. Şu anda Ruby için bir v3 istemci kitaplığı kullanılamıyor.

[Başvuru belge](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_textanalytics) | [paketi (RubyGems)](https://rubygems.org/gems/azure_cognitiveservices_textanalytics) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

<a name="HOLTop"></a>

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Ruby](https://www.ruby-lang.org/) 'nin geçerli sürümü
* Azure aboneliğiniz olduktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, anahtarınızı ve uç noktanızı almak için"  target="_blank">Azure Portal bir metin analizi kaynağı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> oluşturun metin analizi bir kaynak oluşturun. 
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu daha sonra hızlı başlangıçta yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-ruby-application"></a>Yeni bir Ruby uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. Daha sonra kodunuz için adlı `GemFile`bir dosya ve Ruby dosyası oluşturun.

```console
mkdir myapp && cd myapp
```

İçinde `GemFile`, istemci kitaplığını bir bağımlılık olarak eklemek için aşağıdaki satırları ekleyin.

```ruby
source 'https://rubygems.org'
gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
```

Ruby dosyanızda aşağıdaki paketleri içeri aktarın.

[!code-ruby[Import statements](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=includeStatement)]

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. 

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```ruby
const subscription_key = '<paste-your-text-analytics-key-here>'
const endpoint = `<paste-your-text-analytics-endpoint-here>`
```

## <a name="object-model"></a>Nesne modeli 

Metin Analizi istemcisi anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapar. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar. 

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

Adlı `TextAnalyticsClient`bir sınıf oluşturun. 

```ruby
class TextAnalyticsClient
  @textAnalyticsClient
  #...
end
```

Bu sınıfta, anahtarınızı ve uç noktanızı kullanarak `initialize` istemcinin kimliğini doğrulamak için adlı bir işlev oluşturun. 

[!code-ruby[initialize function for authentication](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=initialize)]

Sınıfının dışında, onun örneğini oluşturmak için istemcinin `new()` işlevini kullanın.

[!code-ruby[client creation](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=clientCreation)] 

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>Yaklaşım analizi

İstemci nesnesinde, daha sonra oluşturulacak giriş belgelerinin bir `AnalyzeSentiment()` listesini alan adlı bir işlev oluşturun. İstemcinin `sentiment()` işlevini çağırın ve sonucu alın. Ardından sonuçları yineleyin ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

[!code-ruby[client method for sentiment analysis](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=analyzeSentiment)] 

İstemci işlevinin dışında, daha önce oluşturulan `SentimentAnalysisExample()` `TextAnalyticsClient` nesneyi alan adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `MultiLanguageInput` içeren bir nesne listesi oluşturun. Her nesne bir `id` `Language` ve `text` özniteliği içerir. `text` Özniteliği çözümlenecek metni depolar, `language` belgenin dilidir ve `id` herhangi bir değer olabilir. Ardından, istemcinin `AnalyzeSentiment()` işlevini çağırın.

[!code-ruby[sentiment analysis document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=sentimentCall)] 

`SentimentAnalysisExample()` İşlevi çağırın.

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

İstemci nesnesinde, daha sonra oluşturulacak giriş belgelerinin bir `DetectLanguage()` listesini alan adlı bir işlev oluşturun. İstemcinin `detect_language()` işlevini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve algılanan dili yazdırın.

[!code-ruby[client method for language detection](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguage)] 

İstemci işlevinin dışında, daha önce oluşturulan `DetectLanguageExample()` `TextAnalyticsClient` nesneyi alan adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `LanguageInput` içeren bir nesne listesi oluşturun. Her nesne bir `id`ve `text` özniteliği içerir. `text` Özniteliği çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. Ardından, istemcinin `DetectLanguage()` işlevini çağırın.

[!code-ruby[language detection document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguageCall)] 

`DetectLanguageExample()` İşlevi çağırın.

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

İstemci nesnesinde, daha sonra oluşturulacak giriş belgelerinin bir `RecognizeEntities()` listesini alan adlı bir işlev oluşturun. İstemcinin `entities()` işlevini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve tanınan varlıkları yazdırın.

[!code-ruby[client method for entity recognition](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntities)]

İstemci işlevinin dışında, daha önce oluşturulan `RecognizeEntitiesExample()` `TextAnalyticsClient` nesneyi alan adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `MultiLanguageInput` içeren bir nesne listesi oluşturun. Her nesne `id`, `language`bir, ve bir `text` özniteliği içerir. `text` Özniteliği çözümlenecek metni depolar, `language` metnin dilidir ve `id` herhangi bir değer olabilir. Ardından, istemcinin `RecognizeEntities()` işlevini çağırın.

[!code-ruby[entity recognition documents and method call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntitiesCall)] 

`RecognizeEntitiesExample()` İşlevi çağırın.

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

İstemci nesnesinde, daha sonra oluşturulacak giriş belgelerinin bir `ExtractKeyPhrases()` listesini alan adlı bir işlev oluşturun. İstemcinin `key_phrases()` işlevini çağırın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin KIMLIĞINI ve ayıklanan anahtar tümceciklerini yazdırın.

[!code-ruby[key phrase extraction client method](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=extractKeyPhrases)] 

İstemci işlevinin dışında, daha önce oluşturulan `KeyPhraseExtractionExample()` `TextAnalyticsClient` nesneyi alan adlı yeni bir işlev oluşturun. Analiz etmek istediğiniz belgeleri `MultiLanguageInput` içeren bir nesne listesi oluşturun. Her nesne `id`, `language`bir, ve bir `text` özniteliği içerir. `text` Özniteliği çözümlenecek metni depolar, `language` metnin dilidir ve `id` herhangi bir değer olabilir. Ardından, istemcinin `ExtractKeyPhrases()` işlevini çağırın.

[!code-ruby[key phrase document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=keyPhrasesCall)]


`KeyPhraseExtractionExample()` İşlevi çağırın.

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
