---
title: 'Quickstart: Python ve Bing Haberler Arama REST API ile bir haber araması gerçekleştirin'
titleSuffix: Azure Cognitive Services
description: Python'u kullanarak Bing Haberler Arama REST API'sine istek göndermek ve JSON yanıtı almak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 12/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 1c424c75a4df193ec412355607c68abeda0560a5
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448487"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Quickstart: Python ve Bing Haberler Arama REST API kullanarak bir haber araması gerçekleştirin

Bing Haberler Arama API'sine ilk aramanızı yapmak ve Bir JSON yanıtı almak için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması API'ye bir arama sorgusu gönderir ve sonuçları işler. Bu uygulama Python'da yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

Bu kod örneğini [MyBinder'da](https://mybinder.org) Jupyter dizüstü bilgisayar olarak başlat Bağlayıcı rozetine tıklayarak çalıştırabilirsiniz: 

[![Cilt](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

Bu örnek için kaynak kodu [github'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py)da mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve istek modüllerini içe aktarın. Abonelik anahtarınız, bitiş noktanız ve arama teriminiz için değişkenler oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

### <a name="create-parameters-for-the-request"></a>İstek için parametreler oluşturma

1. Anahtar olarak kullanarak `"Ocp-Apim-Subscription-Key"` abonelik anahtarınızı yeni bir sözlük için ekleyin. Arama parametreleriniz için de aynısını yapın.

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
    ```

## <a name="send-a-request-and-get-a-response"></a>İstek gönderme ve yanıt alma

1. Abonelik anahtarınızı kullanarak Bing Görsel Arama API'sını ve son adımda oluşturulan sözlük nesnelerini aramak için istek kitaplığını kullanın.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

2. `search_results`Bir JSON nesnesi olarak API yanıtı içerir. Yanıtta yer alan makalelerin açıklamalarına erişin.
    
    ```python
    descriptions = [article["description"] for article in search_results["value"]]
    ```

## <a name="displaying-the-results"></a>Sonuçları görüntüleme

Bu açıklamalardan arama anahtar sözcüklerinin **kalın** yazıldığı bir tablo oluşturulabilir.

```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc)
                  for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
