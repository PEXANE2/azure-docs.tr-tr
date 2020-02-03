---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 01/23/2019
ms.author: aahi
ms.openlocfilehash: b71c66c9025ea76d9f99f27537c0d4239ce93fdc
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717219"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-textanalytics/1.0.0b1/azure.ai.textanalytics.html) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics) | [paketi (pipy)](https://pypi.org/project/azure-ai-textanalytics/) | [örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

> [!NOTE]
> * Bu hızlı başlangıçta, iyileştirilmiş [yaklaşım Analizi](../../../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../../../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel bir önizleme içeren Metin Analizi istemci kitaplığının sürüm `3.0-preview` kullanılmaktadır.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Örneğin, `azure.ai.textanalytics.aio` ad alanından istemciyi içeri aktarma ve `azure.ai.textanalytics` ad alanından `analyze_sentiment()` yerine `analyze_sentiment()`çağırma.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Python 3. x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-text-analytics-azure-resource"></a>Metin Analizi Azure kaynağı oluşturma

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Python yükledikten sonra, ile istemci kitaplığını yükleyebilirsiniz:

```console
pip install azure-ai-textanalytics
```

### <a name="create-a-new-python-application"></a>Yeni bir Python uygulaması oluşturma

Yeni bir Python dosyası oluşturun ve kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info.md)]

```python
key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```


## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir `TextAnalyticsClient` nesnesidir. İstemci, toplu olarak metin çözümlemek için çeşitli yöntemler sağlar. Bu hızlı başlangıç, tek bir belgeyi hızlı bir şekilde göndermek için bir işlevler koleksiyonu kullanır.

Toplu işlem metni, API 'ye `id`, `text`ve `language` özniteliklerinin bir bileşimini içeren `dictionary` nesneler olan `documents`bir liste olarak gönderilir. `text` özniteliği, kaynak `language`analiz edilecek metni depolar ve `id` herhangi bir değer olabilir. Tek belgeleri işlerken aşağıdaki örneklerde görünebileceği gibi yalnızca bir `text` girişi gerekir.  

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Metin Analizi istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [Yaklaşım Analizi](#sentiment-analysis) (Genel Önizleme)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-public-preview) (Genel Önizleme)
* [Adlandırılmış varlık tanıma-kişisel bilgiler](#named-entity-recognition---personal-information-public-preview) (Genel Önizleme)
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="sentiment-analysis"></a>Yaklaşım analizi

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan yaklaşım Analizi v3 içindir.

Uç nokta ve anahtarı bağımsız değişken olarak alan `sentiment_analysis_example()` adlı yeni bir işlev oluşturun ve sonra `single_analyze_sentiment()` işlevini çağırır. Döndürülen yanıt nesnesi, tüm giriş belgesi için yaklaşım etiketini ve Puanını, her tümce için de yaklaşım analizini içerir.


```python
from azure.ai.textanalytics import single_analyze_sentiment

def sentiment_analysis_example(endpoint, key):

    document = "I had the best day of my life. I wish you were there with me."

    response = single_analyze_sentiment(endpoint=endpoint, credential=key, input_text=document)
    print("Document Sentiment: {}".format(response.sentiment))
    print("Overall scores: positive={0:.3f}; neutral={1:.3f}; negative={2:.3f} \n".format(
        response.document_scores.positive,
        response.document_scores.neutral,
        response.document_scores.negative,
    ))
    for idx, sentence in enumerate(response.sentences):
        print("[Offset: {}, Length: {}]".format(sentence.offset, sentence.length))
        print("Sentence {} sentiment: {}".format(idx+1, sentence.sentiment))
        print("Sentence score:\nPositive={0:.3f}\nNeutral={1:.3f}\nNegative={2:.3f}\n".format(
            sentence.sentence_scores.positive,
            sentence.sentence_scores.neutral,
            sentence.sentence_scores.negative,
        ))

            
sentiment_analysis_example(endpoint, key)
```

### <a name="output"></a>Çıktı

```console
Document Sentiment: positive
Overall scores: positive=0.999; neutral=0.001; negative=0.000 

[Offset: 0, Length: 30]
Sentence 1 sentiment: positive
Sentence score:
positive=0.999
neutral=0.001
negative=0.000

[Offset: 31, Length: 30]
Sentence 2 sentiment: neutral
Sentence score:
positive=0.212
neutral=0.771
negative=0.017
```

## <a name="language-detection"></a>Dil algılama

Uç nokta ve anahtarı bağımsız değişken olarak alan `language_detection_example()` adlı yeni bir işlev oluşturun ve sonra `single_detect_languages()` işlevini çağırır. Döndürülen yanıt nesnesi, başarılı olursa `detected_languages` algılanan dili ve değilse bir `error` içerecektir.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `country_hint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `country_hint : ""`olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. 

```python
from azure.ai.textanalytics import single_detect_language

def language_detection_example(endpoint, key):
    try:
        document = "Ce document est rédigé en Français."
        response = single_detect_language(endpoint=endpoint, credential=key, input_text= document)
        print("Language: ", response.primary_language.name)

    except Exception as err:
        print("Encountered exception. {}".format(err))
language_detection_example(endpoint, key)
```


### <a name="output"></a>Çıktı

```console
Language:  French
```

## <a name="named-entity-recognition-public-preview"></a>Adlandırılmış varlık tanıma (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan adlandırılmış varlık tanıma v3 içindir.

Uç nokta ve anahtarı bağımsız değişken olarak alan `entity_recognition_example` adlı yeni bir işlev oluşturun, sonra `single_recognize_entities()` işlevini çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa `entity` algılanan varlıkların listesini ve değilse bir `error` içerir. Algılanan her varlık için, türü ve alt türü varsa yazdırın.

```python
from azure.ai.textanalytics import single_recognize_entities

def entity_recognition_example(endpoint, key):

    try:
        document = "I had a wonderful trip to Seattle last week."
        result = single_recognize_entities(endpoint=endpoint, credential=key, input_text= document)
        
        print("Named Entities:\n")
        for entity in result.entities:
                print("\tText: \t", entity.text, "\tType: \t", entity.type, "\tSubType: \t", entity.subtype,
                      "\n\tOffset: \t", entity.offset, "\tLength: \t", entity.offset, 
                      "\tConfidence Score: \t", round(entity.score, 3), "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(endpoint, key)
```

### <a name="output"></a>Çıktı

```console
Named Entities:

    Text:    Seattle    Type:    Location   SubType:     None 
    Offset:      26     Length:      26     Confidence Score:    0.806 

    Text:    last week  Type:    DateTime   SubType:     DateRange 
    Offset:      34     Length:      34     Confidence Score:    0.8 
```

## <a name="named-entity-recognition---personal-information-public-preview"></a>Adlandırılmış varlık tanıma-kişisel bilgiler (Genel Önizleme)

> [!NOTE]
> Aşağıdaki kod, genel önizlemede olan adlandırılmış varlık tanıma v3 kullanan kişisel bilgileri tespit etmek içindir.

Uç nokta ve anahtarı bağımsız değişken olarak alan `entity_pii_example()` adlı yeni bir işlev oluşturun, sonra `single_recognize_pii_entities()` işlevini çağırır ve sonucu alır. Ardından sonuçları yineleyin ve varlıkları yazdırın.

```python
from azure.ai.textanalytics import single_recognize_pii_entities

def entity_pii_example(endpoint, key):

        document = "Insurance policy for SSN on file 123-12-1234 is here by approved."


        result = single_recognize_pii_entities(endpoint=endpoint, credential=key, input_text= document)
        
        print("Personally Identifiable Information Entities: ")
        for entity in result.entities:
            print("\tText: ",entity.text,"\tType: ", entity.type,"\tSub-Type: ", entity.subtype)
            print("\t\tOffset: ", entity.offset, "\tLength: ", entity.length, "\tScore: {0:.3f}".format(entity.score), "\n")
        
entity_pii_example(endpoint, key)
```

### <a name="output"></a>Çıktı

```console
Personally Identifiable Information Entities: 
    Text:  123-12-1234  Type:  U.S. Social Security Number (SSN)    Sub-Type:  
        Offset:  33     Length:  11     Score: 0.850 
```

## <a name="entity-linking"></a>Varlık Bağlama

Uç nokta ve anahtarı bağımsız değişken olarak alan `entity_linking_example()` adlı yeni bir işlev oluşturun, sonra `single_recognize_linked_entities()` işlevini çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa `entities` algılanan varlıkların listesini ve değilse bir `error` içerir. Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları bir `entity` nesnesi altında `match` nesnelerin listesi olarak gruplandırılır.

```python
from azure.ai.textanalytics import single_recognize_linked_entities

def entity_linking_example(endpoint, key):

    try:
        document = """Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, 
        to develop and sell BASIC interpreters for the Altair 8800. 
        During his career at Microsoft, Gates held the positions of chairman,
        chief executive officer, president and chief software architect, 
        while also being the largest individual shareholder until May 2014."""
        result = single_recognize_linked_entities(endpoint=endpoint, credential=key, input_text= document)

        print("Linked Entities:\n")
        for entity in result.entities:
            print("\tName: ", entity.name, "\tId: ", entity.id, "\tUrl: ", entity.url,
            "\n\tData Source: ", entity.data_source)
            print("\tMatches:")
            for match in entity.matches:
                print("\t\tText:", match.text)
                print("\t\tScore: {0:.3f}".format(match.score), "\tOffset: ", match.offset, 
                      "\tLength: {}\n".format(match.length))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(endpoint, key)
```

### <a name="output"></a>Çıktı

```console
Linked Entities:

    Name:  Altair 8800  Id:  Altair 8800    Url:  https://en.wikipedia.org/wiki/Altair_8800 
    Data Source:  Wikipedia
    Matches:
        Text: Altair 8800
        Score: 0.777    Offset:  116    Length: 11

    Name:  Bill Gates   Id:  Bill Gates     Url:  https://en.wikipedia.org/wiki/Bill_Gates 
    Data Source:  Wikipedia
    Matches:
        Text: Bill Gates
        Score: 0.555    Offset:  25     Length: 10

        Text: Gates
        Score: 0.555    Offset:  161    Length: 5

    Name:  Paul Allen   Id:  Paul Allen     Url:  https://en.wikipedia.org/wiki/Paul_Allen 
    Data Source:  Wikipedia
    Matches:
        Text: Paul Allen
        Score: 0.533    Offset:  40     Length: 10

    Name:  Microsoft    Id:  Microsoft  Url:  https://en.wikipedia.org/wiki/Microsoft 
    Data Source:  Wikipedia
    Matches:
        Text: Microsoft
        Score: 0.469    Offset:  0  Length: 9

        Text: Microsoft
        Score: 0.469    Offset:  150    Length: 9

    Name:  April 4  Id:  April 4    Url:  https://en.wikipedia.org/wiki/April_4 
    Data Source:  Wikipedia
    Matches:
        Text: April 4
        Score: 0.248    Offset:  54     Length: 7

    Name:  BASIC    Id:  BASIC  Url:  https://en.wikipedia.org/wiki/BASIC 
    Data Source:  Wikipedia
    Matches:
        Text: BASIC
        Score: 0.281    Offset:  89     Length: 5
```

## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

Uç nokta ve anahtarı bağımsız değişken olarak alan `key_phrase_extraction_example()` adlı yeni bir işlev oluşturun ve sonra `single_extract_key_phrases()` işlevini çağırır. Sonuç, başarılı olursa `key_phrases` algılanan anahtar tümceciklerin listesini ve değilse bir `error` içerir. Algılanan tüm anahtar tümceleri yazdır.

```python
from azure.ai.textanalytics import single_extract_key_phrases

def key_phrase_extraction_example(endpoint, key):

    try:
        document = "My cat might need to see a veterinarian."

        response = single_extract_key_phrases(endpoint=endpoint, credential=key, input_text= document)

        if not response.is_error:
            print("\tKey Phrases:")
            for phrase in response.key_phrases:
                print("\t\t", phrase)
        else:
            print(response.id, response.error)

    except Exception as err:
        print("Encountered exception. {}".format(err))
        
key_phrase_extraction_example(endpoint, key)
```


### <a name="output"></a>Çıktı

```console
    Key Phrases:
         cat
         veterinarian
```
