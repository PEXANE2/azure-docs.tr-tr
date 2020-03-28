---
title: 'Hızlı Başlangıç: Python ile arama gerçekleştirme - Bing Web Araması API’si'
titleSuffix: Azure Cognitive Services
description: Python'u kullanarak Bing Web Search REST API'sine istek göndermek ve JSON yanıtı almak için bu hızlı başlatmayı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: c1af1142faca76cc58b6b3ca9a7106bc0433ea18
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74976375"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Hızlı Başlangıç: Bing Web Araması API’sini çağırmak için Python kullanma  

Bing Web Arama API'sine ilk aramanızı yapmak ve JSON yanıtını almak için bu hızlı başlangıcı kullanın. Bu Python uygulaması API'ye bir arama isteği gönderir ve yanıtı gösterir. Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

Bu örnek [MyBinder](https://mybinder.org) üzerinde bir Jupyter notebook olarak çalıştırılır. Başlatma bağlayıcı rozetini seçin:

[![Cilt](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Ön koşullar

* [Python 2.x veya 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Değişkenleri tanımlama

`subscription_key` değerini Azure hesabınızdan geçerli bir abonelik anahtarı ile değiştirin.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Bing Web Araması API’si uç noktasını tanımlayın. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

`search_term` için değeri değiştirerek arama sorgusunu değiştirebilirsiniz.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>İstekte bulunma

Bu blokta `requests` kitaplığı kullanılarak Bing Web Araması API'si çağrılır ve sonuçlar JSON nesnesi olarak döndürülür. API anahtarı `headers` dizininde, arama terimi ile sorgu parametreleri de `params` dizininde iletilir. Tüm seçeneklerin ve parametrelerin listesi için [Bing Web Araması API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) belgelerine bakın.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key": subscription_key}
params = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Yanıtı biçimlendirme ve görüntüleme

Nesne, `search_results` ilgili sorgular ve sayfalar gibi arama sonuçlarını ve meta verileri içerir. Bu kodda yanıtı biçimlendirmek ve tarayıcınızda görüntülemek için `IPython.display` kitaplığı kullanılır.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"], v["name"], v["snippet"])
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>GitHub'da örnek kod

Bu kodu yerel ortamda çalıştırmak isterseniz [GitHub'da örneğin tamamına ulaşabilirsiniz](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingWebSearchv7.py).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Web araması tek sayfalı uygulama öğreticisi](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
