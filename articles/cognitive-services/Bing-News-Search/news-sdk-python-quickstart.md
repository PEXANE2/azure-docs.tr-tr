---
title: "Hızlı başlangıç: Python için SDK 'Yı kullanarak haber araması gerçekleştirme-Bing Haber Arama"
titleSuffix: Azure Cognitive Services
description: Python için Bing Haber Arama SDK 'yı kullanarak haberleri aramak ve yanıtı işlemek için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 12/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: b84b5ee8682007191953bef34579973c7c24ca45
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448506"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-python"></a>Hızlı başlangıç: Python için Bing Haber Arama SDK ile bir haber araması gerçekleştirme

Python için Bing Haber Arama SDK ile haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Python](https://www.python.org/) 2. x veya 3. x

Python geliştirme için [sanal bir ortam](https://docs.python.org/3/tutorial/venv.html) kullanmanız önerilir. [Venv modülü](https://pypi.python.org/pypi/virtualenv)ile sanal ortamı yükleyebilir ve başlatabilirsiniz. Python 2,7 için bir virtualenv yüklemelisiniz. İle sanal bir ortam oluşturabilirsiniz:

```console
python -m venv mytestenv
```

Bing Haber Arama SDK bağımlılıklarını şu komutla yükleyebilirsiniz:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki kitaplıkları içeri aktarın. Abonelik anahtarınız ve arama teriminiz için bir değişken oluşturun.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>İstemciyi başlatıp istek gönder

1. `CognitiveServicesCredentials` örneği oluşturun. İstemciyi başlatın:
    
    ```python
    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

2. Haber Arama API 'sine bir arama sorgusu gönderin, yanıtı depolayın.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Yanıtı ayrıştırma

Herhangi bir arama sonucu bulunursa, ilk Web sayfası sonucunu Yazdır:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(
        news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
