---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: aahi
ms.openlocfilehash: b842084d00c1ce8ec347994371a55c97b89ba54f
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140776"
---
<a name="HOLTop"></a>

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

[v3 başvuru belgeleri](https://aka.ms/azsdk-python-textanalytics-ref-docs)  |  [v3 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics)  |  [V3 paketi (Pıy)](https://pypi.org/project/azure-ai-textanalytics/)  |  [v3 örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[v2 başvuru belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python)  |  [v2 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics)  |  [v2 paketi (PiPy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/)  |  [v2 örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* Azure aboneliğiniz olduktan sonra, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir metin analizi kaynağı oluşturun metin analizi bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Python yükledikten sonra, ile istemci kitaplığını yükleyebilirsiniz:

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

```console
pip install azure-ai-textanalytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py)bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

```console
pip install --upgrade azure-cognitiveservices-language-textanalytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py)bulabilirsiniz. 

---

### <a name="create-a-new-python-application"></a>Yeni bir Python uygulaması oluşturma

Yeni bir Python dosyası oluşturun ve kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```python
key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```


## <a name="object-model"></a>Nesne modeli

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

Metin Analizi istemcisi, `TextAnalyticsClient` anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir nesnedir. İstemci, toplu olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Toplu işlem metni, API 'ye bir listesi olarak gönderildiğinde, `documents` `dictionary` `id` `text` `language` kullanılan yönteme bağlı olarak, ve özniteliklerinin bir bileşimini içeren nesneler olan. `text`Özniteliği, kaynak olarak çözümlenecek metni depolar `language` ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [TextAnalyticsClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python) nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Metin, `documents` `dictionary` `id` `text` `language` kullanılan yöntemine bağlı olarak, ve özniteliklerinin bir birleşimini içeren nesneler olan bir listesi olarak API 'ye gönderilir. `text`Özniteliği, kaynak olarak çözümlenecek metni depolar `language` ve `id` herhangi bir değer olabilir. 

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

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

`TextAnalyticsClient` `key` Yukarıda oluşturduğunuz ve oluşturduğunuz nesneyi başlatmak için bir işlev oluşturun `endpoint` . Ardından yeni bir istemci oluşturun. 

```python
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, credential=ta_credential)
    return text_analytics_client

client = authenticate_client()
```

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[!code-python[imports statements](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=imports)]

`TextAnalyticsClient` `key` Yukarıda oluşturduğunuz ve oluşturduğunuz nesneyi başlatmak için bir işlev oluşturun `endpoint` . Ardından yeni bir istemci oluşturun. 

[!code-python[version 2 authentication](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=authentication)]

--- 

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

`sentiment_analysis_example()`İstemciyi bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `analyze_sentiment()` işlevi çağırır. Döndürülen yanıt nesnesi, tüm giriş belgesi için yaklaşım etiketini ve Puanını, her tümce için de yaklaşım analizini içerir.


```python
def sentiment_analysis_example(client):

    documents = ["I had the best day of my life. I wish you were there with me."]
    response = client.analyze_sentiment(documents = documents)[0]
    print("Document Sentiment: {}".format(response.sentiment))
    print("Overall scores: positive={0:.2f}; neutral={1:.2f}; negative={2:.2f} \n".format(
        response.confidence_scores.positive,
        response.confidence_scores.neutral,
        response.confidence_scores.negative,
    ))
    for idx, sentence in enumerate(response.sentences):
        print("Sentence: {}".format(sentence.text))
        print("Sentence {} sentiment: {}".format(idx+1, sentence.sentiment))
        print("Sentence score:\nPositive={0:.2f}\nNeutral={1:.2f}\nNegative={2:.2f}\n".format(
            sentence.confidence_scores.positive,
            sentence.confidence_scores.neutral,
            sentence.confidence_scores.negative,
        ))
          
sentiment_analysis_example(client)
```

### <a name="output"></a>Çıktı

```console
Document Sentiment: positive
Overall scores: positive=1.00; neutral=0.00; negative=0.00 

Sentence: I had the best day of my life.
Sentence 1 sentiment: positive
Sentence score:
Positive=1.00
Neutral=0.00
Negative=0.00

Sentence: I wish you were there with me.
Sentence 2 sentiment: neutral
Sentence score:
Positive=0.21
Neutral=0.77
Negative=0.02
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

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

`language_detection_example()`İstemciyi bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `detect_language()` işlevi çağırır. Döndürülen yanıt nesnesi, başarılı olursa ' da algılanan dili içerir `primary_language` ve bunu `error` değildir.

> [!Tip]
> Bazı durumlarda, girişi temel alarak dilleri ayırt etmek zor olabilir. `country_hint`Parametresini 2 harfli bir ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak, API varsayılan Countryipucu olarak "US" kullanıyor, bu davranışı kaldırmak için bu değeri boş dize olarak ayarlayarak bu parametreyi sıfırlayabilirsiniz `country_hint : ""` . 

```python
def language_detection_example(client):
    try:
        documents = ["Ce document est rédigé en Français."]
        response = client.detect_language(documents = documents, country_hint = 'us')[0]
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

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

> [!NOTE]
> Şu sürümde `3.0` : 
> * Varlık bağlama, NER 'den ayrı bir istek.

`entity_recognition_example`İstemciyi bir bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `recognize_entities()` işlevi çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa ' da algılanan varlıkların listesini içerir `entity` ve bunu `error` değildir. Algılanan her varlık için, varsa kategorisini ve alt kategorisini yazdırın.

```python
def entity_recognition_example(client):

    try:
        documents = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(documents = documents)[0]

        print("Named Entities:\n")
        for entity in result.entities:
            print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                    "\n\tConfidence Score: \t", round(entity.confidence_score, 2), "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Çıktı

```console
Named Entities:

        Text:    trip   Category:        Event  SubCategory:     None
        Confidence Score:        0.61

        Text:    Seattle        Category:        Location       SubCategory:     GPE
        Confidence Score:        0.82

        Text:    last week      Category:        DateTime       SubCategory:     DateRange
        Confidence Score:        0.8
```

## <a name="entity-linking"></a>Varlık Bağlama

`entity_linking_example()`İstemciyi bir bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `recognize_linked_entities()` işlevi çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa ' da algılanan varlıkların listesini içerir `entities` ve bunu `error` değildir. Bağlantılı varlıklar benzersiz olarak tanımlandıklarından, aynı varlığın oluşumları `entity` nesne listesi olarak bir nesne altında gruplandırılır `match` .

```python
def entity_linking_example(client):

    try:
        documents = ["""Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, 
        to develop and sell BASIC interpreters for the Altair 8800. 
        During his career at Microsoft, Gates held the positions of chairman,
        chief executive officer, president and chief software architect, 
        while also being the largest individual shareholder until May 2014."""]
        result = client.recognize_linked_entities(documents = documents)[0]

        print("Linked Entities:\n")
        for entity in result.entities:
            print("\tName: ", entity.name, "\tId: ", entity.data_source_entity_id, "\tUrl: ", entity.url,
            "\n\tData Source: ", entity.data_source)
            print("\tMatches:")
            for match in entity.matches:
                print("\t\tText:", match.text)
                print("\t\tConfidence Score: {0:.2f}".format(match.confidence_score))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(client)
```

### <a name="output"></a>Çıktı

```console
Linked Entities:

        Name:  Altair 8800      Id:  Altair 8800        Url:  https://en.wikipedia.org/wiki/Altair_8800
        Data Source:  Wikipedia
        Matches:
                Text: Altair 8800
                Confidence Score: 0.88
        Name:  Bill Gates       Id:  Bill Gates         Url:  https://en.wikipedia.org/wiki/Bill_Gates
        Data Source:  Wikipedia
        Matches:
                Text: Bill Gates
                Confidence Score: 0.63
                Text: Gates
                Confidence Score: 0.63
        Name:  Paul Allen       Id:  Paul Allen         Url:  https://en.wikipedia.org/wiki/Paul_Allen
        Data Source:  Wikipedia
        Matches:
                Text: Paul Allen
                Confidence Score: 0.60
        Name:  Microsoft        Id:  Microsoft  Url:  https://en.wikipedia.org/wiki/Microsoft
        Data Source:  Wikipedia
        Matches:
                Text: Microsoft
                Confidence Score: 0.55
                Text: Microsoft
                Confidence Score: 0.55
        Name:  April 4  Id:  April 4    Url:  https://en.wikipedia.org/wiki/April_4
        Data Source:  Wikipedia
        Matches:
                Text: April 4
                Confidence Score: 0.32
        Name:  BASIC    Id:  BASIC      Url:  https://en.wikipedia.org/wiki/BASIC
        Data Source:  Wikipedia
        Matches:
                Text: BASIC
                Confidence Score: 0.33
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


#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

`key_phrase_extraction_example()`İstemciyi bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `extract_key_phrases()` işlevi çağırır. Sonuç, başarılı olursa ' de algılanan anahtar tümceciklerin listesini içerir `key_phrases` ve bunu `error` değildir. Algılanan tüm anahtar tümceleri yazdır.

```python
def key_phrase_extraction_example(client):

    try:
        documents = ["My cat might need to see a veterinarian."]

        response = client.extract_key_phrases(documents = documents)[0]

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
