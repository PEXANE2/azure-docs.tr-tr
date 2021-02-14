---
author: aahill
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 01/20/2021
ms.author: aahi
ms.openlocfilehash: fc66cf75101d38ed1deac3b5368ee823dfdab821
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100514913"
---
<a name="HOLTop"></a>

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

[v 3.1 başvuru belgeleri](/python/api/azure-ai-textanalytics/azure.ai.textanalytics)  |  [v 3.1 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics)  |  [v 3.1 paketi (PIG)](https://pypi.org/project/azure-ai-textanalytics/)  |  [v 3.1 örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

[v3 başvuru belgeleri](/python/api/azure-ai-textanalytics/azure.ai.textanalytics)  |  [v3 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics)  |  [V3 paketi (Pıy)](https://pypi.org/project/azure-ai-textanalytics/)  |  [v3 örnekleri](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[v2 başvuru belgeleri](/python/api/overview/azure/cognitiveservices/textanalytics)  |  [v2 kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics)  |  [v2 paketi (PiPy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/)  |  [v2 örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

---

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
* Azure aboneliğiniz olduktan sonra, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir metin analizi kaynağı oluşturun metin analizi bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
    * Uygulamanızı Metin Analizi API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Çözümle özelliğini kullanmak için standart fiyatlandırma katmanına sahip bir Metin Analizi kaynağına ihtiyacınız olacaktır.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Python yükledikten sonra, ile istemci kitaplığını yükleyebilirsiniz:

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

```console
pip install azure-ai-textanalytics --pre
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py)bulabilirsiniz. 

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

```console
pip install --upgrade azure-ai-textanalytics
```

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py)bulabilirsiniz. 


# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

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

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

Metin Analizi istemcisi, Azure 'da `TextAnalyticsClient` kimlik doğrulayan bir nesnedir. İstemci, metin çözümlemek için çeşitli yöntemler sağlar. 

İşleme metni, bir `documents` dize listesi, bir dict benzeri gösterim listesi veya bir listesi olarak olan bir liste olarak API 'ye gönderilir `TextDocumentInput/DetectLanguageInput` . Bir `dict-like` nesnesi,, ve birleşimini `id` içerir `text` `language/country_hint` . `text`Özniteliği, kaynak olarak çözümlenecek metni depolar `country_hint` ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

Metin Analizi istemcisi, `TextAnalyticsClient` anahtarınızı kullanarak Azure 'da kimlik doğrulayan bir nesnedir. İstemci, toplu olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Toplu işlem metni, API 'ye bir listesi olarak gönderildiğinde, `documents` `dictionary` `id` `text` `language` kullanılan yönteme bağlı olarak, ve özniteliklerinin bir bileşimini içeren nesneler olan. `text`Özniteliği, kaynak olarak çözümlenecek metni depolar `language` ve `id` herhangi bir değer olabilir. 

Yanıt nesnesi, her belge için çözümlenen bilgileri içeren bir listesidir. 

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [TextAnalyticsClient](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient) nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar. 

Metin, `documents` `dictionary` `id` `text` `language` kullanılan yöntemine bağlı olarak, ve özniteliklerinin bir birleşimini içeren nesneler olan bir listesi olarak API 'ye gönderilir. `text`Özniteliği, kaynak olarak çözümlenecek metni depolar `language` ve `id` herhangi bir değer olabilir. 

---

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Metin Analizi istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis)
* [Görüşün madenciliği](#opinion-mining)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner) 
* [Kişisel olarak tanımlanabilir bilgi tanıma](#personally-identifiable-information-recognition) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)


# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım Analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Adlandırılmış varlık tanıma](#named-entity-recognition-ner) 
* [Varlık bağlama](#entity-linking)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)

---

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

`TextAnalyticsClient` `key` Yukarıda oluşturduğunuz ve oluşturduğunuz nesneyi başlatmak için bir işlev oluşturun `endpoint` . Ardından yeni bir istemci oluşturun. 

```python
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, 
            credential=ta_credential)
    return text_analytics_client

client = authenticate_client()
```

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

`TextAnalyticsClient` `key` Yukarıda oluşturduğunuz ve oluşturduğunuz nesneyi başlatmak için bir işlev oluşturun `endpoint` . Ardından yeni bir istemci oluşturun. `api_version=TextAnalyticsApiVersion.V3_0`Sürüm 3,0 ' i kullanmak için tanımlanması gerektiğini unutmayın.

```python
# use this code if you're using SDK version is 5.0.0
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, 
            credential=ta_credential) 
    return text_analytics_client

client = authenticate_client()
```

Kullanarak istemci kitaplığının v 5.1.0 sürümünü yüklediyseniz `pip install azure-ai-textanalytics --pre` , metin Analizi API'si v 3.0 ' ı istemcinin `api_version` parametresiyle belirtebilirsiniz. Yalnızca `authenticate_client()` istemciniz v 5.1.0 veya üzeri ise aşağıdaki yöntemi kullanın.

```python
# Only use the following code sample if you're using v5.1.0 of the client library, 
# and are looking to specify v3.0 of the Text Analytics API for your client
from azure.ai.textanalytics import TextAnalyticsClient, TextAnalyticsApiVersion
from azure.core.credentials import AzureKeyCredential
def authenticate_client():
   ta_credential = AzureKeyCredential(key)
   text_analytics_client = TextAnalyticsClient(
     endpoint=endpoint,
     credential=ta_credential,
     api_version=TextAnalyticsApiVersion.V3_0
   )
   
client = authenticate_client()
```

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[!code-python[imports statements](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=imports)]

`TextAnalyticsClient` `key` Yukarıda oluşturduğunuz ve oluşturduğunuz nesneyi başlatmak için bir işlev oluşturun `endpoint` . Ardından yeni bir istemci oluşturun. 

[!code-python[version 2 authentication](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=authentication)]

--- 

## <a name="sentiment-analysis"></a>Yaklaşım analizi

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

`sentiment_analysis_example()`İstemciyi bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `analyze_sentiment()` işlevi çağırır. Döndürülen yanıt nesnesi, tüm giriş belgesi için yaklaşım etiketini ve Puanını, her tümce için de yaklaşım analizini içerir.


```python
def sentiment_analysis_example(client):

    documents = ["I had the best day of my life. I wish you were there with me."]
    response = client.analyze_sentiment(documents=documents)[0]
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

## <a name="opinion-mining"></a>Görüşün madenciliği

Fikrinizi inceleme ile yaklaşım analizi yapmak için, `sentiment_analysis_with_opinion_mining_example()` istemciyi bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `analyze_sentiment()` işlevi seçenek bayrağıyla çağırır `show_opinion_mining=True` . Döndürülen yanıt nesnesi, her tümce için yaklaşım analiziyle birlikte tüm giriş belgesi ve açıklama düzeyi yaklaşım analizini değil yalnızca yaklaşım etiketini ve Puanını içermez.


```python
def sentiment_analysis_with_opinion_mining_example(client):

    documents = [
        "The food and service were unacceptable, but the concierge were nice"
    ]

    result = client.analyze_sentiment(documents, show_opinion_mining=True)
    doc_result = [doc for doc in result if not doc.is_error]

    positive_reviews = [doc for doc in doc_result if doc.sentiment == "positive"]
    negative_reviews = [doc for doc in doc_result if doc.sentiment == "negative"]

    positive_mined_opinions = []
    mixed_mined_opinions = []
    negative_mined_opinions = []

    for document in doc_result:
        print("Document Sentiment: {}".format(document.sentiment))
        print("Overall scores: positive={0:.2f}; neutral={1:.2f}; negative={2:.2f} \n".format(
            document.confidence_scores.positive,
            document.confidence_scores.neutral,
            document.confidence_scores.negative,
        ))
        for sentence in document.sentences:
            print("Sentence: {}".format(sentence.text))
            print("Sentence sentiment: {}".format(sentence.sentiment))
            print("Sentence score:\nPositive={0:.2f}\nNeutral={1:.2f}\nNegative={2:.2f}\n".format(
                sentence.confidence_scores.positive,
                sentence.confidence_scores.neutral,
                sentence.confidence_scores.negative,
            ))
            for mined_opinion in sentence.mined_opinions:
                aspect = mined_opinion.aspect
                print("......'{}' aspect '{}'".format(aspect.sentiment, aspect.text))
                print("......Aspect score:\n......Positive={0:.2f}\n......Negative={1:.2f}\n".format(
                    aspect.confidence_scores.positive,
                    aspect.confidence_scores.negative,
                ))
                for opinion in mined_opinion.opinions:
                    print("......'{}' opinion '{}'".format(opinion.sentiment, opinion.text))
                    print("......Opinion score:\n......Positive={0:.2f}\n......Negative={1:.2f}\n".format(
                        opinion.confidence_scores.positive,
                        opinion.confidence_scores.negative,
                    ))
            print("\n")
        print("\n")
          
sentiment_analysis_with_opinion_mining_example(client)
```

### <a name="output"></a>Çıktı

```console
Document Sentiment: positive
Overall scores: positive=0.84; neutral=0.00; negative=0.16

Sentence: The food and service were unacceptable, but the concierge were nice
Sentence sentiment: positive
Sentence score:
Positive=0.84
Neutral=0.00
Negative=0.16

......'negative' aspect 'food'
......Aspect score:
......Positive=0.01
......Negative=0.99

......'negative' opinion 'unacceptable'
......Opinion score:
......Positive=0.01
......Negative=0.99

......'negative' aspect 'service'
......Aspect score:
......Positive=0.01
......Negative=0.99

......'negative' opinion 'unacceptable'
......Opinion score:
......Positive=0.01
......Negative=0.99

......'positive' aspect 'concierge'
......Aspect score:
......Positive=1.00
......Negative=0.00

......'positive' opinion 'nice'
......Opinion score:
......Positive=1.00
......Negative=0.00





Press any key to continue . . .

```

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

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

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

İstemci nesnesinin kimliğini doğrulayın ve [Sentiment ()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#sentiment-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın. Sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

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

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

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

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

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

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturulan istemciyi kullanarak [detect_language ()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#detect-language-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) öğesini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve ilk döndürülen dili yazdırın.

[!code-python[language detection](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=languageDetection)]


### <a name="output"></a>Çıktı

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

> [!NOTE]
> Şu sürümde `3.1` : 
> * Varlık bağlama, NER 'den ayrı bir istek.

`entity_recognition_example`İstemciyi bir bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `recognize_entities()` işlevi çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa ' da algılanan varlıkların listesini içerir `entity` ve bunu `error` değildir. Algılanan her varlık için kategorisini ve varsa Sub-Category yazdırın.

```python
def entity_recognition_example(client):

    try:
        documents = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(documents = documents)[0]

        print("Named Entities:\n")
        for entity in result.entities:
            print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                    "\n\tConfidence Score: \t", round(entity.confidence_score, 2), "\tLength: \t", entity.length, "\tOffset: \t", entity.offset, "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Çıktı

```console
Named Entities:

        Text:    trip   Category:        Event  SubCategory:     None
        Confidence Score:        0.61   Length:          4      Offset:          18

        Text:    Seattle        Category:        Location       SubCategory:     GPE
        Confidence Score:        0.82   Length:          7      Offset:          26

        Text:    last week      Category:        DateTime       SubCategory:     DateRange
        Confidence Score:        0.8    Length:          9      Offset:          34
```

### <a name="entity-linking"></a>Varlık Bağlama

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
                print("\t\tOffset: {}".format(match.offset))
                print("\t\tLength: {}".format(match.length))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(client)
```

### <a name="output"></a>Çıktı

```console
Linked Entities:

        Name:  Microsoft        Id:  Microsoft  Url:  https://en.wikipedia.org/wiki/Microsoft
        Data Source:  Wikipedia
        Matches:
                Text: Microsoft
                Confidence Score: 0.55
                Offset: 0
                Length: 9
                Text: Microsoft
                Confidence Score: 0.55
                Offset: 168
                Length: 9
        Name:  Bill Gates       Id:  Bill Gates         Url:  https://en.wikipedia.org/wiki/Bill_Gates
        Data Source:  Wikipedia
        Matches:
                Text: Bill Gates
                Confidence Score: 0.63
                Offset: 25
                Length: 10
                Text: Gates
                Confidence Score: 0.63
                Offset: 179
                Length: 5
        Name:  Paul Allen       Id:  Paul Allen         Url:  https://en.wikipedia.org/wiki/Paul_Allen
        Data Source:  Wikipedia
        Matches:
                Text: Paul Allen
                Confidence Score: 0.60
                Offset: 40
                Length: 10
        Name:  April 4  Id:  April 4    Url:  https://en.wikipedia.org/wiki/April_4
        Data Source:  Wikipedia
        Matches:
                Text: April 4
                Confidence Score: 0.32
                Offset: 54
                Length: 7
        Name:  BASIC    Id:  BASIC      Url:  https://en.wikipedia.org/wiki/BASIC
        Data Source:  Wikipedia
        Matches:
                Text: BASIC
                Confidence Score: 0.33
                Offset: 98
                Length: 5
        Name:  Altair 8800      Id:  Altair 8800        Url:  https://en.wikipedia.org/wiki/Altair_8800
        Data Source:  Wikipedia
        Matches:
                Text: Altair 8800
                Confidence Score: 0.88
                Offset: 125
                Length: 11
```

### <a name="personally-identifiable-information-recognition"></a>Kişisel olarak tanımlanabilir bilgi tanıma

`pii_recognition_example`İstemciyi bir bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `recognize_pii_entities()` işlevi çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa ' da algılanan varlıkların listesini içerir `entity` ve bunu `error` değildir. Algılanan her varlık için kategorisini ve varsa Sub-Category yazdırın.

```python
def pii_recognition_example(client):
    documents = [
        "The employee's SSN is 859-98-0987.",
        "The employee's phone number is 555-555-5555."
    ]
    response = client.recognize_pii_entities(documents, language="en")
    result = [doc for doc in response if not doc.is_error]
    for doc in result:
        print("Redacted Text: {}".format(doc.redacted_text))
        for entity in doc.entities:
            print("Entity: {}".format(entity.text))
            print("\tCategory: {}".format(entity.category))
            print("\tConfidence Score: {}".format(entity.confidence_score))
            print("\tOffset: {}".format(entity.offset))
            print("\tLength: {}".format(entity.length))
pii_recognition_example(client)
```

### <a name="output"></a>Çıktı

```console
Redacted Text: The employee's SSN is ***********.
Entity: 859-98-0987
        Category: U.S. Social Security Number (SSN)
        Confidence Score: 0.65
        Offset: 22
        Length: 11
Redacted Text: The employee's phone number is ************.
Entity: 555-555-5555
        Category: Phone Number
        Confidence Score: 0.8
        Offset: 31
        Length: 12
```

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

> [!NOTE]
> Şu sürümde `3.0` : 
> * Varlık bağlama, NER 'den ayrı bir istek.

`entity_recognition_example`İstemciyi bir bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `recognize_entities()` işlevi çağırır ve sonuçlar boyunca yinelenir. Döndürülen yanıt nesnesi, başarılı olursa ' da algılanan varlıkların listesini içerir `entity` ve bunu `error` değildir. Algılanan her varlık için kategorisini ve varsa Sub-Category yazdırın.

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

### <a name="entity-linking"></a>Varlık Bağlama

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

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

> [!NOTE]
> Sürüm 2,1 ' de, varlık bağlama, NER yanıtına dahil edilir.

Daha önce oluşturulan istemciyi kullanarak, [Entities ()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#entities-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve içerdiği varlıkları yazdırın.

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

### <a name="key-phrase-extraction"></a>Anahtar ifade ayıklama

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

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

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

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

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Daha önce oluşturulan istemciyi kullanarak, [key_phrases ()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#key-phrases-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) işlevini çağırın ve sonucu alın. Sonra sonuçlar arasında yineleme yapın, her belge KIMLIĞINI ve içerdiği anahtar tümceleri yazdırın.

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

## <a name="use-the-api-asynchronously-with-the-analyze-operation"></a>Çözümleme işlemi ile API 'YI zaman uyumsuz olarak kullanma

# <a name="version-31-preview"></a>[Sürüm 3,1 Önizleme](#tab/version-3-1)

[!INCLUDE [Analyze operation pricing](../analyze-operation-pricing-caution.md)]

`analyze_example()`İstemciyi bağımsız değişken olarak alan adlı yeni bir işlev oluşturun, sonra `begin_analyze()` işlevi çağırır. Sonuç, sonuçlar için yoklamak üzere uzun süren bir işlem olacaktır.

```python
    def analyze_example(client):
        documents = [
            "Microsoft was founded by Bill Gates and Paul Allen."
        ]

        poller = text_analytics_client.begin_analyze(
            documents,
            display_name="Sample Text Analysis",
            entities_recognition_tasks=[EntitiesRecognitionTask()]
        )

        result = poller.result()

        for page in result:
            for task in page.entities_recognition_results:
                print("Results of Entities Recognition task:")
                
                docs = [doc for doc in task.results if not doc.is_error]
                for idx, doc in enumerate(docs):
                    print("\nDocument text: {}".format(documents[idx]))
                    for entity in doc.entities:
                        print("Entity: {}".format(entity.text))
                        print("...Category: {}".format(entity.category))
                        print("...Confidence Score: {}".format(entity.confidence_score))
                        print("...Offset: {}".format(entity.offset))
                    print("------------------------------------------")

analyze_example(client)
```

### <a name="output"></a>Çıktı

```console
Results of Entities Recognition task:
Document text: Microsoft was founded by Bill Gates and Paul Allen.
Entity: Microsoft
...Category: Organization
...Confidence Score: 0.83
...Offset: 0
Entity: Bill Gates
...Category: Person
...Confidence Score: 0.85
...Offset: 25
Entity: Paul Allen
...Category: Person
...Confidence Score: 0.9
...Offset: 40
------------------------------------------
```

PII ve anahtar tümceciği ayıklamayı algılamak için Çözümle işlemini de kullanabilirsiniz. GitHub 'daki [Çözümle örneği](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples/async_samples) ' ne bakın.

# <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

Bu özellik 3,0 sürümünde kullanılamaz.

# <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

Bu özellik 2,1 sürümünde kullanılamaz.

---
