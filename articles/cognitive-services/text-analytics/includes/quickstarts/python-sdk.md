---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 04/8/2020
ms.author: aahi
ms.openlocfilehash: 2693fd6a84c221fdcbbe2d3511490805316a156b
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275417"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

[v3 Referans dokümantasyon](https://aka.ms/azsdk-python-textanalytics-ref-docs) | [v3 Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics) | [v3 Paketi (PiPy)](https://pypi.org/project/azure-ai-textanalytics/) | [v3 Örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[v2 Referans dokümantasyon](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [v2 Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [v2 Paketi (PiPy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) | [v2 Örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

---

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun. Dağıtıladıktan sonra **kaynağa git'i**tıklatın.
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Anahtarınızı ve bitiş noktanızı daha sonra hızlı başlatmada aşağıdaki koda yapıştıracaksınız.
    * Hizmeti denemek için ücretsiz`F0`fiyatlandırma katmanını kullanabilir ve daha sonra üretim için ücretli bir katmana yükseltebilirsiniz.

## <a name="setting-up"></a>Ayarlama

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Python'u yükledikten sonra istemci kitaplığını aşağıdakilerle yükleyebilirsiniz:

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

```console
pip install azure-ai-textanalytics==1.0.0b3
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py)bulabilirsiniz. 

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

```console
pip install --upgrade azure-cognitiveservices-language-textanalytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek ister misiniz? Bu hızlı başlatmada kod örneklerini içeren [GitHub'da](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py)bulabilirsiniz. 

---

### <a name="create-a-new-python-application"></a>Yeni bir python uygulaması oluşturma

Yeni bir Python dosyası oluşturun ve kaynağınızın Azure bitiş noktası ve abonelik anahtarı için değişkenler oluşturun.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```python
key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```


## <a name="object-model"></a>Nesne modeli

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Text Analytics istemcisi, anahtarınızı kullanarak Azure'a doğrulayan bir `TextAnalyticsClient` nesnedir. İstemci, metni toplu olarak çözümleme için çeşitli yöntemler sağlar. 

Toplu işleme metni, kullanılan yönteme bağlı `documents`olarak , `dictionary` , ve `id` `text` `language` özniteliklerin birleşimini içeren nesneler listesi olarak API'ye gönderildiğinde. Öznitelik, `text` metnin kaynağında `language`çözümlenecek metni depolar ve herhangi bir değer `id` olabilir. 

Yanıt nesnesi, her belge için çözümleme bilgilerini içeren bir listedir. 

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Text Analytics istemcisi, anahtarınızı kullanarak Azure'a doğrulayan bir [TextAnalyticsClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python) nesnesidir. İstemci, metni tek bir dize veya toplu iş olarak çözümleme için çeşitli yöntemler sağlar. 

Metin, kullanılan yönteme bağlı olarak `documents`, `dictionary` , ve `id` `text` `language` özniteliklerin birleşimini içeren nesneler listesi olarak API'ye gönderilir. Öznitelik, `text` metnin kaynağında `language`çözümlenecek metni depolar ve herhangi bir değer `id` olabilir. 

---

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Text Analytics istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış Varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümcecik çıkarma](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

Yukarıda oluşturulmuş ve `TextAnalyticsClient` `key` `endpoint` nesneyi anında anımsamak için bir işlev oluşturun. Sonra yeni bir istemci oluşturun. 

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

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[!code-python[imports statements](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=imports)]

Yukarıda oluşturulmuş ve `TextAnalyticsClient` `key` `endpoint` nesneyi anında anımsamak için bir işlev oluşturun. Sonra yeni bir istemci oluşturun. 

[!code-python[version 2 authentication](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=authentication)]

--- 

## <a name="sentiment-analysis"></a>Yaklaşım analizi

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

İstemciyi bağımsız `sentiment_analysis_example()` değişken olarak alan, ardından `analyze_sentiment()` işlevi çağıran yeni bir işlev oluşturun. Döndürülen yanıt nesnesi, tüm giriş belgesinin duyarlılık etiketini ve puanını ve her cümle için bir duygu analizini içerir.


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
        print("[Length: {}]".format(sentence.grapheme_length))
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

[Length: 30]
Sentence 1 sentiment: positive
Sentence score:
Positive=1.00
Neutral=0.00
Negative=0.00

[Length: 30]
Sentence 2 sentiment: neutral
Sentence score:
Positive=0.21
Neutral=0.77
Negative=0.02
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

İstemci nesnesini doğrula ve [duyarlılık()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#sentiment-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın. Sonuçları yineleyin ve her belgenin kimliğini ve duyarlılık puanını yazdırın. 0'a yakın bir puan olumsuz bir duyguyu gösterirken, 1'e yakın bir puan olumlu bir duyguyu gösterir.

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

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

İstemciyi bağımsız `language_detection_example()` değişken olarak alan, ardından `detect_language()` işlevi çağıran yeni bir işlev oluşturun. Döndürülen yanıt nesnesi, `primary_language` başarılı olursa algılanan `error` dili ve başarısız olursa içermez.

> [!Tip]
> Bazı durumlarda girişe göre dilleri ayrıştırmak zor olabilir. Parametreyi `country_hint` 2 harfli ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak API varsayılan countryHint olarak "ABD" kullanıyor, bu davranışı kaldırmak için bu değeri boş `country_hint : ""`dize için ayarlayarak bu parametreyi sıfırlayabilirsiniz. 

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

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Daha önce oluşturulan istemciyi [kullanarak, detect_language()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#detect-language-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) numaralı telefonu arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve ilk döndürülen dili yazdırın.

[!code-python[language detection](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=languageDetection)]


### <a name="output"></a>Çıktı

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık tanıma (NER)

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

> [!NOTE]
> Sürümde `3.0-preview`: 
> * Varlık bağlama NER'den ayrı bir istektir.

İstemciyi bağımsız `entity_recognition_example` değişken olarak alan, ardından `recognize_entities()` işlevi çağıran ve sonuçları yineleyen yeni bir işlev oluşturun. Döndürülen yanıt nesnesi, başarılı `entity` olursa algılanan varlıkların `error` listesini ve başarısız olursa listesini içerir. Algılanan her varlık için varsa Kendi Kategorisini ve Alt Kategorisini yazdırın.

```python
def entity_recognition_example(client):

    try:
        documents = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(documents = documents)[0]

        print("Named Entities:\n")
        for entity in result.entities:
            print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                    "\n\tLength: \t", entity.grapheme_length, "\tConfidence Score: \t", round(entity.confidence_score, 2), "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Çıktı

```console
Named Entities:

    Text:    Seattle        Category:        Location       SubCategory:     GPE
    Length:          7      Confidence Score:        0.92

    Text:    last week      Category:        DateTime       SubCategory:     DateRange
    Length:          9      Confidence Score:        0.8
```

## <a name="entity-linking"></a>Varlık Bağlama

İstemciyi bağımsız `entity_linking_example()` değişken olarak alan, ardından `recognize_linked_entities()` işlevi çağıran ve sonuçları yineleyen yeni bir işlev oluşturun. Döndürülen yanıt nesnesi, başarılı `entities` olursa algılanan varlıkların `error` listesini ve başarısız olursa listesini içerir. Bağlı varlıklar benzersiz olarak tanımlandığından, aynı varlığın oluşumları `entity` `match` nesnelerin listesi olarak bir nesne altında gruplandırılır.

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
                print("\t\tConfidence Score: {0:.2f}".format(match.confidence_score), "\tLength: {}\n".format(match.grapheme_length))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(client)
```

### <a name="output"></a>Çıktı

```console
Linked Entities:

    Name:  Altair 8800     Id:  Altair 8800     Url:  https://en.wikipedia.org/wiki/Altair_8800 
    Data Source:  Wikipedia
    Matches:
        Text: Altair 8800
        Confidence Score: 0.00     Length: 11

    Name:  Bill Gates     Id:  Bill Gates     Url:  https://en.wikipedia.org/wiki/Bill_Gates 
    Data Source:  Wikipedia
    Matches:
        Text: Bill Gates
        Confidence Score: 0.00     Length: 10

        Text: Gates
        Confidence Score: 0.00     Length: 5

    Name:  Paul Allen     Id:  Paul Allen     Url:  https://en.wikipedia.org/wiki/Paul_Allen 
    Data Source:  Wikipedia
    Matches:
        Text: Paul Allen
        Confidence Score: 0.00     Length: 10

    Name:  Microsoft     Id:  Microsoft     Url:  https://en.wikipedia.org/wiki/Microsoft 
    Data Source:  Wikipedia
    Matches:
        Text: Microsoft
        Confidence Score: 0.00     Length: 9

        Text: Microsoft
        Confidence Score: 0.00     Length: 9

    Name:  April 4     Id:  April 4     Url:  https://en.wikipedia.org/wiki/April_4 
    Data Source:  Wikipedia
    Matches:
        Text: April 4
        Confidence Score: 0.00     Length: 7

    Name:  BASIC     Id:  BASIC     Url:  https://en.wikipedia.org/wiki/BASIC 
    Data Source:  Wikipedia
    Matches:
        Text: BASIC
        Confidence Score: 0.00     Length: 5
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

> [!NOTE]
> Sürüm 2.1'de, varlık bağlantısı NER yanıtına dahildir.

Daha önce oluşturulan istemciyi [kullanarak, varlıkları()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#entities-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve içinde bulunan varlıkları yazdırın.

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


#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

İstemciyi bağımsız `key_phrase_extraction_example()` değişken olarak alan, ardından `extract_key_phrases()` işlevi çağıran yeni bir işlev oluşturun. Sonuç, başarılı `key_phrases` olursa algılanan anahtar tümceciklerin listesini `error` ve başarısız olursa listesini içerir. Algılanan anahtar tümcecikleri yazdırın.

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

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

Daha önce oluşturulan istemciyi [kullanarak, key_phrases()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#key-phrases-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini arayın ve sonucu alın. Ardından sonuçları yineleyin ve her belgenin kimliğini ve içinde bulunan anahtar tümcecikleri yazdırın.

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