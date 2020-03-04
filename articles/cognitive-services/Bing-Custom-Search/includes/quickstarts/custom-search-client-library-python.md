---
title: Bing Özel Arama Python istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.author: aahi
ms.openlocfilehash: ec0ffdcf86e67a7126a3100c1e20b6e5c3474e35
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252887"
---
Python için Bing Özel Arama istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Bing Özel Arama API'si, ilgilendiğiniz konular için özel, reklam ücretsiz arama deneyimleri oluşturmanıza olanak sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py)' da bulunabilir.

Python için Bing Özel Arama istemci kitaplığını şu şekilde kullanın:
* Bing Özel Arama örneğinden Web 'de arama sonuçları bulun.

[Başvuru belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/customsearch?view=azure-python) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-customsearch) | [paketi (Pypı)](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/)


## <a name="prerequisites"></a>Önkoşullar

- Bir Bing Özel Arama örneği. Daha fazla bilgi için bkz. [hızlı başlangıç: ilk Bing özel arama örneğinizi oluşturma](../../quick-start.md) .
- Python [2. x veya 3. x](https://www.python.org/) 
- [Python için Bing özel arama SDK 'sı](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="install-the-python-client-library"></a>Python istemci kitaplığını 'nı yükler

Aşağıdaki komutla Bing Özel Arama istemci kitaplığı 'nı yüklersiniz.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

En sevdiğiniz düzenleyicide veya IDE 'de yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarmaları ekleyin.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Arama İstemcisi Oluşturma ve istek gönderme

1. Abonelik anahtarınız ve uç noktanız için bir değişken oluşturun.

    ```python
    subscription_key = 'your-subscription-key'
    endpoint = 'your-endpoint'
    ```

2. Abonelik anahtarı ile bir `CognitiveServicesCredentials` nesnesi kullanarak `CustomSearchClient`örneğini oluşturun. 

    ```python
    client = CustomSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

3. `client.custom_instance.search()`bir arama isteği gönderin. Arama teriminizi `query` parametresine ekleyin ve arama örneğinizi kullanmak için `custom_config` özel yapılandırma KIMLIĞINIZE ayarlayın. **Üretim** sekmesine tıklayarak [Bing özel arama portalından](https://www.customsearch.ai/)kimliğiniz edinebilirsiniz.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Arama sonuçlarını görüntüleme

Herhangi bir Web sayfası arama sonucu bulunursa, birinciden birini alın ve adını, URL 'sini ve bulunan toplam Web sayfalarını yazdırın.

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
> [Özel bir arama Web uygulaması oluşturma](../../tutorials/custom-search-web-page.md)
