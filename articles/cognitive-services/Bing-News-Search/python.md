---
title: 'Hızlı başlangıç: Python ve Bing Haber Arama REST API bir haber araması gerçekleştirin'
titleSuffix: Azure Cognitive Services
description: Python kullanarak Bing Haber Arama REST API isteği göndermek ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın.
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
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448487"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Hızlı başlangıç: Python ve Bing Haber Arama REST API kullanarak bir haber arama gerçekleştirme

Bing Haber Arama API'si ilk çağrısını yapmak ve bir JSON yanıtı almak için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması, API 'ye bir arama sorgusu gönderir ve sonuçları işler. Bu uygulama Python 'da yazıldığı sırada API, bir veya daha fazla programlama dili ile uyumlu bir Web hizmeti hizmetidir.

Bu kod örneğini, [myciltçi](https://mybinder.org) üzerinde bir Jupyter Not defteri olarak çalıştırabilirsiniz. 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py)' da de mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve istek modülünü içeri aktarın. Abonelik anahtarınız, uç nokta ve arama teriminiz için değişkenler oluşturun. Aşağıdaki genel uç noktayı veya kaynak için Azure portal görüntülenmiş [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

### <a name="create-parameters-for-the-request"></a>İstek için parametreler oluşturma

1. Anahtar olarak `"Ocp-Apim-Subscription-Key"` kullanarak abonelik anahtarınızı yeni bir sözlüğe ekleyin. Arama parametreleriniz için aynısını yapın.

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
    ```

## <a name="send-a-request-and-get-a-response"></a>İstek gönderme ve yanıt edinme

1. Abonelik anahtarınızı ve son adımda oluşturulan sözlük nesnelerini kullanarak Bing Görsel Arama API'si çağırmak için istekler kitaplığını kullanın.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

2. `search_results`, API 'den bir JSON nesnesi olarak yanıtı içerir. Yanıtta yer alan makalelerin açıklamalarına erişin.
    
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
