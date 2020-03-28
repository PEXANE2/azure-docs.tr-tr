---
title: Bing Haber Arama Python istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.openlocfilehash: c1bd0d86a3fd9d19d67d84b9b05955421373e01e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79503884"
---
Python için Bing Haberler Arama istemci kitaplığı ile haber aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Haber Aramaçoğu programlama diliyle uyumlu bir REST API'si olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Piton](https://www.python.org/) 2.x veya 3.x

Python geliştirmeniz için sanal bir [ortam](https://docs.python.org/3/tutorial/venv.html) kullanmanız önerilir. [Venv modülü](https://pypi.python.org/pypi/virtualenv)ile sanal ortamı kurabilir ve başlatabilirsiniz. Python 2.7 için bir virtualenv yüklemeniz gerekir. Şularla sanal bir ortam oluşturabilirsiniz:

```console
python -m venv mytestenv
```

Bing Haberler Arama istemci kitaplığı bağımlılıklarını bu komutla yükleyebilirsiniz:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki kitaplıkları aktarın. Abonelik anahtarınız ve arama teriminiz için bir değişken oluşturun.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchClient
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>İstemciyi başlatma ve istek gönderme

1. `CognitiveServicesCredentials` örneği oluşturun.
    
    ```python
    client = NewsSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

2. Haber Arama API'sine bir arama sorgusu gönderin, yanıtı depolayın.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Yanıtı ayrıştırma

Herhangi bir arama sonucu bulunursa, ilk web sayfası sonucunu yazdırın:

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
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-news-search-single-page-app.md)
