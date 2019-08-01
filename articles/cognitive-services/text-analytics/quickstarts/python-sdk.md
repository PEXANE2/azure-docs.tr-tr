---
title: "Hızlı Başlangıç: Python SDK 'sını kullanarak Metin Analizi hizmetini çağırma"
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'de Metin Analizi API'si kullanmaya hızlı bir şekilde başlamanıza yardımcı olması için bilgi ve kod örnekleri alın.
services: cognitive-services
author: ctufts
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: 82f0313a237358fcaa1ae52e92821abef2b52af7
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697313"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-python-sdk"></a>Hızlı Başlangıç: Python SDK 'sını kullanarak Metin Analizi hizmetini çağırma 
<a name="HOLTop"></a>

Python için Metin Analizi SDK ile dili çözümlemeye başlamak için bu hızlı başlangıcı kullanın. Metin Analizi REST API çoğu programlama dili ile uyumlu olsa da SDK, JSON serileştirilmeden ve seri durumdan çıkarılırken hizmeti uygulamalarınıza tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py)' da bulunabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Python 3. x](https://www.python.org/)

* [Python için metin analizi SDK 'sı](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) ile paketi yükleyebilirsiniz:

    `pip install --upgrade azure-cognitiveservices-language-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

En sevdiğiniz düzenleyicide veya IDE 'de yeni bir Python uygulaması oluşturun. Ardından, dosyanıza aşağıdaki import deyimlerini ekleyin.

```python
from azure.cognitiveservices.language.textanalytics import TextAnalyticsClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="authenticate-your-credentials"></a>Kimlik bilgilerinizi doğrulama

> [!Tip]
> Üretim sistemlerinde parolaların güvenli dağıtımı için [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net)kullanmanızı öneririz.
>

Metin analizi abonelik anahtarınız için bir değişken oluşturduktan sonra, onunla bir `CognitiveServicesCredentials` nesne örneğini oluşturun.

```python
subscription_key = "enter-your-key-here"
credentials = CognitiveServicesCredentials(subscription_key)
```

## <a name="create-a-text-analytics-client"></a>Metin Analizi istemcisi oluşturma

Parametresi olarak ve `TextAnalyticsClient` `credentials` ile`text_analytics_url` yeni bir nesne oluşturun. Metin Analizi aboneliğiniz için doğru Azure bölgesini kullanın (örneğin `westcentralus`).

```
text_analytics_url = "https://westcentralus.api.cognitive.microsoft.com/"
text_analytics = TextAnalyticsClient(endpoint=text_analytics_url, credentials=credentials)
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

API 'nin yükü, bir `documents` `id` ve `text` özniteliği içeren sözlükler olan bir listesinden oluşur. Özniteliği çözümlenecek metni depolar `id` ve herhangi bir değer olabilir. `text` 

```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "I had the best day of my life."
    },
    {
        "id": "2",
        "language": "en",
        "text": "This was a waste of my time. The speaker put me to sleep."
    },
    {
        "id": "3",
        "language": "es",
        "text": "No tengo dinero ni nada que dar..."
    },
    {
        "id": "4",
        "language": "it",
        "text": "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."
    }
]
```

`sentiment()` İşlevi çağırın ve sonucu alın. Ardından sonuçları yineleyin ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

```python
response = text_analytics.sentiment(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id, ", Sentiment Score: ",
          "{:.2f}".format(document.score))
```

### <a name="output"></a>Output

```console
Document Id:  1 , Sentiment Score:  0.87
Document Id:  2 , Sentiment Score:  0.11
Document Id:  3 , Sentiment Score:  0.44
Document Id:  4 , Sentiment Score:  1.00
```

## <a name="language-detection"></a>Dil algılama

Her biri çözümlemek istediğiniz belgeyi içeren sözlüklerin bir listesini oluşturun. Özniteliği çözümlenecek metni depolar `id` ve herhangi bir değer olabilir. `text` 

```python
documents = [
    {
        'id': '1',
        'text': 'This is a document written in English.'
    },
    {
        'id': '2',
        'text': 'Este es un document escrito en Español.'
    },
    {
        'id': '3',
        'text': '这是一个用中文写的文件'
    }
]
```

Daha önce oluşturulan istemciyi kullanarak, sonucunu `detect_language()` çağırın ve elde edin. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve ilk döndürülen dili yazdırın.

```python
response = text_analytics.detect_language(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id, ", Language: ",
          document.detected_languages[0].name)
```

### <a name="output"></a>Output

```console
Document Id:  1 , Language:  English
Document Id:  2 , Language:  Spanish
Document Id:  3 , Language:  Chinese_Simplified
```

## <a name="entity-recognition"></a>Varlık tanıma

Çözümlemek istediğiniz belgeleri içeren sözlüklerin bir listesini oluşturun. Özniteliği çözümlenecek metni depolar `id` ve herhangi bir değer olabilir. `text` 


```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."
    },
    {
        "id": "2",
        "language": "es",
        "text": "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."
    }
]
```

Daha önce oluşturulan istemciyi kullanarak işlevini çağırın `entities()` ve sonucu alın. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve içerdiği varlıkları yazdırın.

```python
response = text_analytics.entities(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Entities:")
    for entity in document.entities:
        print("\t\t", "NAME: ", entity.name, "\tType: ",
              entity.type, "\tSub-type: ", entity.sub_type)
        for match in entity.matches:
            print("\t\t\tOffset: ", match.offset, "\tLength: ", match.length, "\tScore: ",
                  "{:.2f}".format(match.entity_type_score))
```


### <a name="output"></a>Output

```console
Document Id:  1
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  0  Length:  9  Score:  1.00
         NAME:  Bill Gates  Type:  Person   Sub-type:  None
            Offset:  25     Length:  10     Score:  1.00
         NAME:  Paul Allen  Type:  Person   Sub-type:  None
            Offset:  40     Length:  10     Score:  1.00
         NAME:  April 4     Type:  Other    Sub-type:  None
            Offset:  54     Length:  7  Score:  0.80
         NAME:  April 4, 1975   Type:  DateTime     Sub-type:  Date
            Offset:  54     Length:  13     Score:  0.80
         NAME:  BASIC   Type:  Other    Sub-type:  None
            Offset:  89     Length:  5  Score:  0.80
         NAME:  Altair 8800     Type:  Other    Sub-type:  None
            Offset:  116    Length:  11     Score:  0.80
Document Id:  2
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  21     Length:  9  Score:  1.00
         NAME:  Redmond (Washington)    Type:  Location     Sub-type:  None
            Offset:  60     Length:  7  Score:  0.99
         NAME:  21 kilómetros   Type:  Quantity     Sub-type:  Dimension
            Offset:  71     Length:  13     Score:  0.80
         NAME:  Seattle     Type:  Location     Sub-type:  None
            Offset:  88     Length:  7  Score:  1.00
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Çözümlemek istediğiniz belgeleri içeren sözlüklerin bir listesini oluşturun. Özniteliği çözümlenecek metni depolar `id` ve herhangi bir değer olabilir. `text` 


```python
documents = [
    {
        "id": "1",
        "language": "ja",
        "text": "猫は幸せ"
    },
    {
        "id": "2",
        "language": "de",
        "text": "Fahrt nach Stuttgart und dann zum Hotel zu Fu."
    },
    {
        "id": "3",
        "language": "en",
        "text": "My cat might need to see a veterinarian."
    },
    {
        "id": "4",
        "language": "es",
        "text": "A mi me encanta el fútbol!"
    }
]
```

Daha önce oluşturulan istemciyi kullanarak işlevini çağırın `key_phrases()` ve sonucu alın. Sonra sonuçlar arasında yineleme yapın, her belge KIMLIĞINI ve içerdiği anahtar tümceleri yazdırın.

```python
response = text_analytics.key_phrases(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Phrases:")
    for phrase in document.key_phrases:
        print("\t\t", phrase)
```

### <a name="output"></a>Output

```console
Document Id:  1
    Phrases:
         幸せ
Document Id:  2
    Phrases:
         Stuttgart
         Hotel
         Fahrt
         Fu
Document Id:  3
    Phrases:
         cat
         veterinarian
Document Id:  4
    Phrases:
         fútbol
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si nedir?](../overview.md)
* [Örnek kullanıcı senaryoları](../text-analytics-user-scenarios.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
