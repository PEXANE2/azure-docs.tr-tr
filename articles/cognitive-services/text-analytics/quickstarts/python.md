---
title: "Hızlı başlangıç: Metin Analizi API'sini çağırmak için Python kullanma"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Azure bilişsel hizmetler 'de Metin Analizi API'si kullanmaya hızlı bir şekilde başlamanıza yardımcı olacak bilgiler ve kod örneklerinin nasıl alınacağı gösterilmektedir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/06/2020
ms.author: aahi
ms.custom: tracking-python
ms.openlocfilehash: feababd3d6f6845142a13ccb3b2b31b78315704b
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027938"
---
# <a name="quickstart-using-the-python-rest-api-to-call-the-text-analytics-cognitive-service"></a>Hızlı başlangıç: Metin Analizi bilişsel hizmeti çağırmak için Python REST API kullanma 
<a name="HOLTop"></a>

Metin Analizi REST API ve Python ile dili çözümlemeye başlamak için bu hızlı başlangıcı kullanın. Bu makalede, [Dili algılama](#Detect), yaklaşımı [Çözümleme](#SentimentAnalysis), [anahtar tümceleri ayıklama](#KeyPhraseExtraction)ve [bağlantılı varlıkları belirleme](#Entities)işlemlerinin nasıl yapılacağı gösterilir.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.x](https://python.org)

* Python istekleri kitaplığı
    
    Kitaplığı şu komutla yükleyebilirsiniz:

    ```console
    pip install --upgrade requests
    ```

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]


## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

En sevdiğiniz düzenleyicide veya IDE 'de yeni bir Python uygulaması oluşturun. Aşağıdaki içeri aktarmaları dosyanıza ekleyin.

```python
import requests
# pprint is used to format the JSON response
from pprint import pprint
```

Kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun.
    
```python
import os

subscription_key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```

Aşağıdaki bölümlerde, API 'nin özelliklerinin her birinin nasıl çağrılacağını açıklamaktadır.

<a name="Detect"></a>

## <a name="detect-languages"></a>Dilleri algılama

`/text/analytics/v3.0/languages`Dil ALGıLAMA URL 'sini oluşturmak için metin analizi temel uç noktaya ekleyin. Örneğin, `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/languages`
    
```python
language_api_url = endpoint + "/text/analytics/v3.0/languages"
```

API 'nin yükü, `documents` ve özniteliğini içeren tanımlama grupları olan bir listesinden oluşur `id` `text` . `text`Özniteliği çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. 

```python
documents = {"documents": [
    {"id": "1", "text": "This is a document written in English."},
    {"id": "2", "text": "Este es un document escrito en Español."},
    {"id": "3", "text": "这是一个用中文写的文件"}
]}
```

API 'ye belge göndermek için Istekler kitaplığını kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin ve isteği ile gönderin `requests.post()` . 

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

### <a name="output"></a>Çıktı

```json
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "English",
                "iso6391Name": "en",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "2",
            "detectedLanguage": {
                "name": "Spanish",
                "iso6391Name": "es",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "3",
            "detectedLanguage": {
                "name": "Chinese_Simplified",
                "iso6391Name": "zh_chs",
                "confidenceScore": 1.0
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

Bir belge kümesinin yaklaşımını (pozitif veya negatif) algılamak için, `/text/analytics/v3.0/sentiment` dil ALGıLAMA URL 'sini oluşturmak üzere metin analizi temel uç noktasına ekleyin. Örneğin, `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment`
    
```python
sentiment_url = endpoint + "/text/analytics/v3.0/sentiment"
```

Dil algılama örneğinde olduğu gibi, bir `documents` belge listesinden oluşan anahtarı içeren bir sözlük oluşturun. Her belge, analiz edilecek `id` ve `text` ile metnin `language` öğesini içeren bir demettir. 

```python
documents = {"documents": [
    {"id": "1", "language": "en",
        "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
    {"id": "2", "language": "es",
        "text": "Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico."}
]}
```

API 'ye belge göndermek için Istekler kitaplığını kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin ve isteği ile gönderin `requests.post()` . 

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(sentiment_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

### <a name="output"></a>Çıktı

Bir belge için yaklaşım puanı, daha pozitif bir yaklaşım belirten daha yüksek bir puana sahip 0,0 ve 1,0 arasındadır.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "confidenceScores": {
                "positive": 1.0,
                "neutral": 0.0,
                "negative": 0.0
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "confidenceScores": {
                        "positive": 1.0,
                        "neutral": 0.0,
                        "negative": 0.0
                    },
                    "offset": 0,
                    "length": 102,
                    "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                }
            ],
            "warnings": []
        },
        {
            "id": "2",
            "sentiment": "negative",
            "confidenceScores": {
                "positive": 0.02,
                "neutral": 0.05,
                "negative": 0.93
            },
            "sentences": [
                {
                    "sentiment": "negative",
                    "confidenceScores": {
                        "positive": 0.02,
                        "neutral": 0.05,
                        "negative": 0.93
                    },
                    "offset": 0,
                    "length": 92,
                    "text": "Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico."
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Anahtar ifadeleri ayıklama
 
Bir belge kümesinden anahtar tümceleri ayıklamak için, `/text/analytics/v3.0/keyPhrases` dil ALGıLAMA URL 'sini oluşturmak üzere metin analizi temel uç noktaya ekleyin. Örneğin, `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/keyPhrases`
    
```python
keyphrase_url = endpoint + "/text/analytics/v3.0/keyphrases"
```

Bu belge koleksiyonu, yaklaşım analizi örneği için aynı şekilde kullanılır.

```python
documents = {"documents": [
    {"id": "1", "language": "en",
        "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
    {"id": "2", "language": "es",
        "text": "Si usted quiere comunicarse con Carlos, usted debe de llamarlo a su telefono movil. Carlos es muy responsable, pero necesita recibir una notificacion si hay algun problema."},
    {"id": "3", "language": "en",
        "text": "The Grand Hotel is a new hotel in the center of Seattle. It earned 5 stars in my review, and has the classiest decor I've ever seen."}
]}
```

API 'ye belge göndermek için Istekler kitaplığını kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin ve isteği ile gönderin `requests.post()` . 

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(keyphrase_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

### <a name="output"></a>Çıktı

```json
{
    "documents": [
        {
            "id": "1",
            "keyPhrases": [
                "HDR resolution",
                "new XBox",
                "clean look"
            ],
            "warnings": []
        },
        {
            "id": "2",
            "keyPhrases": [
                "Carlos",
                "notificacion",
                "algun problema",
                "telefono movil"
            ],
            "warnings": []
        },
        {
            "id": "3",
            "keyPhrases": [
                "new hotel",
                "Grand Hotel",
                "review",
                "center of Seattle",
                "classiest decor",
                "stars"
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

<a name="Entities"></a>

## <a name="identify-entities"></a>Varlıkları tanımla

Metin belgelerindeki iyi bilinen varlıkları (kişiler, konumlar ve şeyler) belirlemek için, `/text/analytics/v3.0/entities/recognition/general` dil ALGıLAMA URL 'sini oluşturmak üzere metin analizi temel uç noktaya ekleyin. Örneğin, `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`
    
```python
entities_url = endpoint + "/text/analytics/v3.0/entities/recognition/general/recognition/general"
```

Önceki örneklerde olduğu gibi bir belge koleksiyonu oluşturun. 

```python
documents = {"documents": [
    {"id": "1", "text": "Microsoft is an It company."}
]}
```

API 'ye belge göndermek için Istekler kitaplığını kullanın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin ve isteği ile gönderin `requests.post()` .

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(entities_url, headers=headers, json=documents)
entities = response.json()
pprint(entities)
```

### <a name="output"></a>Çıktı

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "text": "Microsoft",
                    "category": "Organization",
                    "offset": 0,
                    "length": 9,
                    "confidenceScore": 0.86
                },
                {
                    "text": "IT",
                    "category": "Skill",
                    "offset": 16,
                    "length": 2,
                    "confidenceScore": 0.8
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
