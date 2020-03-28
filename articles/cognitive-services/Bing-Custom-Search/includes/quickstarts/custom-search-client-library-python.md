---
title: Bing Özel Arama Python istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.author: aahi
ms.openlocfilehash: ec0ffdcf86e67a7126a3100c1e20b6e5c3474e35
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78252887"
---
Python için Bing Özel Arama istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Bing Özel Arama API'si, önem verdiğiniz konular için özel, reklamsız arama deneyimleri oluşturmanıza olanak tanır. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py)bulunabilir.

Python için Bing Özel Arama istemci kitaplığını kullanın:
* Bing Özel Arama örneğinden web'de arama sonuçlarını bulun.

[Referans belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/customsearch?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-customsearch) | [Paketi (PyPi)](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/)


## <a name="prerequisites"></a>Ön koşullar

- Bing Özel Arama örneği. Bkz. Hızlı Başlangıç: Daha fazla bilgi için [ilk Bing Özel Arama örneğini oluşturun.](../../quick-start.md)
- Python [2.x veya 3.x](https://www.python.org/) 
- [Python için Bing Özel Arama SDK](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="install-the-python-client-library"></a>Python istemci kitaplığını yükleme

Bing Özel Arama istemci kitaplığını aşağıdaki komutla yükleyin.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

Sık kullanılan düzenleyicinizde veya IDE'nizde yeni bir Python dosyası oluşturun ve aşağıdaki içeri almaları ekleyin.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Arama istemcisi oluşturma ve istek gönderme

1. Abonelik anahtarınız ve bitiş noktanız için bir değişken oluşturun.

    ```python
    subscription_key = 'your-subscription-key'
    endpoint = 'your-endpoint'
    ```

2. Abonelik anahtarıyla `CustomSearchClient`bir `CognitiveServicesCredentials` nesne kullanarak bir örnek oluşturun. 

    ```python
    client = CustomSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

3. `client.custom_instance.search()`Bir arama isteği gönder. Arama teriminizi `query` parametreye ekleyip, `custom_config` arama örneğini kullanmak üzere Özel Yapılandırma Kimliğinize ayarlayın. Bing [Özel Arama portalından,](https://www.customsearch.ai/) **Üretim** sekmesini tıklayarak kimliğinizi alabilirsiniz.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Arama sonuçlarını görüntüleme

Herhangi bir web sayfası arama sonuçları bulunduysa, ilkini alın ve adını, URL'sini ve bulunan toplam web sayfalarını yazdırın.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Arama web uygulaması oluşturma](../../tutorials/custom-search-web-page.md)
