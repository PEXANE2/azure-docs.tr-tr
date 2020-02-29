---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: 8e3f08be70a2a6b932b849ff28dd762828abf229
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155506"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

v3 [başvurusu belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-ai-textanalytics/1.0.0b2/azure.ai.textanalytics.html) | v3 [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics) | [V3 paketi (Pıy)](https://pypi.org/project/azure-ai-textanalytics/) | [v3 örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

v2 [başvuru belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | v2 [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [v2 paketi (pipy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) | [v2 örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

---

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Python 3. x](https://www.python.org/)
* Azure aboneliğiniz olduktan sonra, anahtarınızı ve uç noktanızı almak için</a> Azure portal bir kaynak Metin Analizi <span class="docon docon-navigate-external x-hidden-focus"></span> oluşturun"  target="_blank">metin analizi bir kaynak oluşturun <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=". 
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bunu daha sonra hızlı başlangıçta yapacaksınız.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyebilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Python yükledikten sonra, ile istemci kitaplığını yükleyebilirsiniz:

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

```console
pip install azure-ai-textanalytics
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

```console
pip install --upgrade azure-cognitiveservices-language-textanalytics
```

---

### <a name="create-a-new-python-application"></a>Yeni bir Python uygulaması oluşturma

Yeni bir Python dosyası oluşturun ve kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```python
key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```


## <a name="object-model"></a>Nesne modeli

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir `TextAnalyticsClient` nesnesidir. İstemci, toplu olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Toplu işlem metni, API 'ye `id`, `text`ve `language` özniteliklerinin bir bileşimini içeren `dictionary` nesneler olan `documents`bir liste olarak gönderilir. `text` özniteliği, kaynak `language`analiz edilecek metni depolar ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [TextAnalyticsClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python) nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Metin, kullanılan yönteme bağlı olarak `id`, `text`ve `language` özniteliklerinin bir birleşimini içeren `dictionary` nesneler olan `documents`bir liste olarak API 'ye gönderilir. `text` özniteliği, kaynak `language`analiz edilecek metni depolar ve `id` herhangi bir değer olabilir. 

---

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Metin Analizi istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`key` ve yukarıda oluşturulan `endpoint` `TextAnalyticsClient` nesnesini oluşturmak için bir işlev oluşturun. Ardından yeni bir istemci oluşturun. 

```python
from azure.ai.textanalytics import TextAnalyticsClient, TextAnalyticsApiKeyCredential

def authenticate_client():
    ta_credential = TextAnalyticsApiKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, credential=ta_credential)
    return text_analytics_client

client = authenticate_client()
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[!code-python[imports statements](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=imports)]

`key` ve yukarıda oluşturulan `endpoint` `TextAnalyticsClient` nesnesini oluşturmak için bir işlev oluşturun. Ardından yeni bir istemci oluşturun. 

[!code-python[version 2 authentication](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=authentication)]

--- 

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

İstemciyi bağımsız değişken olarak alan `sentiment_analysis_example()` adlı yeni bir işlev oluşturun ve sonra `analyze_sentiment()` işlevini çağırır. Döndürülen yanıt nesnesi, tüm giriş belgesi için yaklaşım etiketini ve Puanını, her tümce için de yaklaşım analizini içerir.


```python
def sentiment_analysis_example(client):

    document = ["I had the best day of my life. I wish you were there with me."]
    response = client.analyze_sentiment(inputs=document)[0]
    print("Document Sentiment: {}".format(response.sentiment))
    print("Overall scores: positive={0:.3f}; neutral={1:.3f}; negative={2:.3f} \n".format(
        response.sentiment_scores.positive,
        response.sentiment_scores.neutral,
        response.sentiment_scores.negative,
    ))
    for idx, sentence in enumerate(response.sentences):
        print("[Offset: {}, Length: {}]".format(sentence.offset, sentence.length))
        print("Sentence {} sentiment: {}".format(idx+1, sentence.sentiment))
        print("Sentence score:\nPositive={0:.3f}\nNeutral={1:.3f}\nNegative={2:.3f}\n".format(
            sentence.sentiment_scores.positive,
            sentence.sentiment_scores.neutral,
            sentence.sentiment_scores.negative,
        ))

            
sentiment_analysis_example(client)
```

### <a name="output"></a>Çıktı

```console
Document Sentiment: positive
Overall scores: positive=1.000; neutral=0.000; negative=0.000 

[Offset: 0, Length: 30]
Sentence 1 sentiment: positive
Sentence score:
Positive=1.000
Neutral=0.000
Negative=0.000

[Offset: 31, Length: 30]
Sentence 2 sentiment: neutral
Sentence score:
Positive=0.210
Neutral=0.770
Negative=0.020
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

İstemci nesnesinin kimliğini doğrulayın ve [Sentiment ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#sentiment-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın. Sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

[!code-python[sentiment analysis](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=sentimentAnalysis)]

### <a name="output"></a>Çıktı

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

---

## <a name="language-detection"></a>Dil algılama

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

İstemciyi bağımsız değişken olarak alan `language_detection_example()` adlı yeni bir işlev oluşturun ve sonra `detect_language()` işlevini çağırır. Döndürülen yanıt nesnesi, başarılı olursa `primary_language` algılanan dili ve değilse bir `error` içerecektir.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. 2 harfli bir ülke kodu belirtmek için `country_hint` parametresini kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize `country_hint : ""`olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz. 

```python
def language_detection_example(client):
    try:
        document = ["Ce document est rédigé en Français."]
        response = client.detect_language(inputs = document, country_hint = 'us')[0]
        print("Language: ", response.primary_language.name)

    except Exception as err:
        print("Encountered exception. {}".format(err))
language_detection_example(client)
```


### <a name="output"></a>Çıktı

```console
Language:  French
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturulan istemciyi kullanarak [detect_language ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#detect-language-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve ilk döndürülen dili yazdırın.

[!code-python[language detection](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=languageDetection)]


### <a name="output"></a>Çıktı

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

> [!NOTE]
> Sürüm `3.0-preview`:
> * NER kişisel bilgileri algılamak için ayrı yöntemler içerir. 
> * Varlık bağlama, NER 'den ayrı bir istek.

İstemciyi bağımsız değişken olarak alan `entity_recognition_example` adlı yeni bir işlev oluşturun, sonra `recognize_entities()` işlevini çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa `entity` algılanan varlıkların listesini ve değilse bir `error` içerir. Algılanan her varlık için, varsa kategorisini ve alt kategorisini yazdırın.

```python
def entity_recognition_example(client):

    try:
        document = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(inputs= document)[0]

        print("Named Entities:\n")
        for entity in result.entities:
                print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                      "\n\tOffset: \t", entity.offset, "\tLength: \t", entity.offset, 
                      "\tConfidence Score: \t", round(entity.score, 3), "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Çıktı

```console
Named Entities:

    Text:    Seattle    Category:    Location   SubCategory:     GPE 
    Offset:      26     Length:      26     Confidence Score:    0.92 

    Text:    last week  Category:    DateTime   SubCategory:     DateRange 
    Offset:      34     Length:      34     Confidence Score:    0.8 
```

## <a name="using-ner-to-detect-personal-information"></a>Kişisel bilgileri algılamak için NER kullanma

İstemciyi bağımsız değişken olarak alan `entity_pii_example()` adlı yeni bir işlev oluşturun, sonra `recognize_pii_entities()` işlevini çağırır ve sonucu alır. Ardından sonuçları yineleyin ve varlıkları yazdırın.

```python
def entity_pii_example(client):

        document = ["Insurance policy for SSN on file 123-12-1234 is here by approved."]


        result = client.recognize_pii_entities(inputs= document)[0]
        
        print("Personally Identifiable Information Entities: ")
        for entity in result.entities:
            print("\tText: ",entity.text,"\tCategory: ", entity.category,"\tSubCategory: ", entity.subcategory)
            print("\t\tOffset: ", entity.offset, "\tLength: ", entity.length, "\tScore: {0:.3f}".format(entity.score), "\n")
        
entity_pii_example(client)
```

### <a name="output"></a>Çıktı

```console
Personally Identifiable Information Entities: 
    Text:  123-12-1234  Category:  U.S. Social Security Number (SSN)    SubCategory:  None
        Offset:  33     Length:  11     Score: 0.850 
```


## <a name="entity-linking"></a>Varlık Bağlama

İstemciyi bağımsız değişken olarak alan `entity_linking_example()` adlı yeni bir işlev oluşturun, sonra `recognize_linked_entities()` işlevini çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa `entities` algılanan varlıkların listesini ve değilse bir `error` içerir. Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları bir `entity` nesnesi altında `match` nesnelerin listesi olarak gruplandırılır.

```python
def entity_linking_example(client):

    try:
        document = ["""Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, 
        to develop and sell BASIC interpreters for the Altair 8800. 
        During his career at Microsoft, Gates held the positions of chairman,
        chief executive officer, president and chief software architect, 
        while also being the largest individual shareholder until May 2014."""]
        result = client.recognize_linked_entities(inputs= document)[0]

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
entity_linking_example(client)
```

### <a name="output"></a>Çıktı

```console
Linked Entities:

    Name:  Altair 8800  Id:  Altair 8800    Url:  https://en.wikipedia.org/wiki/Altair_8800 
    Data Source:  Wikipedia
    Matches:
        Text: Altair 8800
        Score: 0.777    Offset:  125    Length: 11

    Name:  Bill Gates   Id:  Bill Gates     Url:  https://en.wikipedia.org/wiki/Bill_Gates 
    Data Source:  Wikipedia
    Matches:
        Text: Bill Gates
        Score: 0.555    Offset:  25     Length: 10

        Text: Gates
        Score: 0.555    Offset:  179    Length: 5

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
        Score: 0.469    Offset:  168    Length: 9

    Name:  April 4  Id:  April 4    Url:  https://en.wikipedia.org/wiki/April_4 
    Data Source:  Wikipedia
    Matches:
        Text: April 4
        Score: 0.248    Offset:  54     Length: 7

    Name:  BASIC    Id:  BASIC  Url:  https://en.wikipedia.org/wiki/BASIC 
    Data Source:  Wikipedia
    Matches:
        Text: BASIC
        Score: 0.281    Offset:  98     Length: 5

```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

> [!NOTE]
> Sürüm 2,1 ' de, varlık bağlama, NER yanıtına dahil edilir.

Daha önce oluşturulan istemciyi kullanarak, [Entities ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#entities-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve içerdiği varlıkları yazdırın.

[!code-python[Entity recognition](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=entityRecognition)]

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

---

## <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama


#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

İstemciyi bağımsız değişken olarak alan `key_phrase_extraction_example()` adlı yeni bir işlev oluşturun ve sonra `extract_key_phrases()` işlevini çağırır. Sonuç, başarılı olursa `key_phrases` algılanan anahtar tümceciklerin listesini ve değilse bir `error` içerir. Algılanan tüm anahtar tümceleri yazdır.

```python
def key_phrase_extraction_example(client):

    try:
        document = ["My cat might need to see a veterinarian."]

        response = client.extract_key_phrases(inputs= document)[0]

        if not response.is_error:
            print("\tKey Phrases:")
            for phrase in response.key_phrases:
                print("\t\t", phrase)
        else:
            print(response.id, response.error)

    except Exception as err:
        print("Encountered exception. {}".format(err))
        
key_phrase_extraction_example(client)
```


### <a name="output"></a>Çıktı

```console
    Key Phrases:
         cat
         veterinarian
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturulan istemciyi kullanarak, [key_phrases ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#key-phrases-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın, her belge KIMLIĞINI ve içerdiği anahtar tümceleri yazdırın.

[!code-python[key phrase extraction](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=keyPhrases)]


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

---