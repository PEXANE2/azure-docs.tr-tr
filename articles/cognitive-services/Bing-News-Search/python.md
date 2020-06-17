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
ms.date: 06/16/2020
ms.author: aahi
ms.custom: seodec2018, tracking-python
ms.openlocfilehash: 534310609b1d2f375feef81fe45147485216d5ec
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84883734"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Hızlı başlangıç: Python ve Bing Haber Arama REST API kullanarak bir haber arama gerçekleştirme

Bing Haber Arama API'si ilk çağrısını yapmak için bu hızlı başlangıcı kullanın. Bu basit Python uygulaması, API 'ye bir arama sorgusu gönderir ve JSON sonucunu işler. 

Bu uygulama Python 'da yazılmış olsa da, API birçok programlama dili ile uyumlu olan bir yenilenmiş Web hizmetidir.

Bu kod örneğini [Myciltçi](https://mybinder.org)üzerinde bir Jupyter Not defteri olarak çalıştırmak için, **cildi Başlat** rozetini seçin: 

[![cildi Başlat](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py)' da de mevcuttur.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve istek modülünü içeri aktarın. Abonelik anahtarınız, uç noktanız ve arama teriminiz için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

## <a name="create-parameters-for-the-request"></a>İstek için parametreler oluşturma

Abonelik anahtarınızı anahtar olarak kullanarak yeni bir sözlüğe ekleyin `Ocp-Apim-Subscription-Key` . Arama parametreleriniz için aynısını yapın.

```python
headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
```

## <a name="send-a-request-and-get-a-response"></a>İstek gönderme ve yanıt edinme

1. Abonelik anahtarınızla Bing Görsel Arama API'si çağırmak ve önceki adımda oluşturduğunuz sözlük nesneleri için istekler kitaplığını kullanın.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = json.dumps(response.json())
    ```

2. Bir JSON nesnesi olarak depolanan API 'den gelen yanıtta bulunan makalelerin açıklamalarına erişin `search_results` . 
    
    ```python
    descriptions = [article["description"] for article in search_results["value"]]
    ```

## <a name="display-the-results"></a>Sonuçları görüntüle

Bu açıklamalardan arama anahtar sözcüklerinin kalın yazıldığı bir tablo oluşturulabilir.

```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc)
                  for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](tutorial-bing-news-search-single-page-app.md)
