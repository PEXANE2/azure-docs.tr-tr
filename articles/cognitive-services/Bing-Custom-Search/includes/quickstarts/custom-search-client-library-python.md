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
ms.openlocfilehash: 51dcc0585117d2e3db97046a25a4fbae6cff0112
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94340979"
---
Python için Bing Özel Arama istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Bing Özel Arama API'si, ilgilendiğiniz konular için özel, reklam ücretsiz arama deneyimleri oluşturmanıza olanak sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py)' da bulunabilir.

Python için Bing Özel Arama istemci kitaplığını şu şekilde kullanın:
* Bing Özel Arama örneğinden Web 'de arama sonuçları bulun.

[Başvuru belgeleri](/python/api/azure-cognitiveservices-search-customsearch/?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-customsearch)  |  [Paket (Pypı)](https://pypi.org/project/azure-cognitiveservices-search-customsearch/)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/)


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

2. `CustomSearchClient`Abonelik anahtarıyla bir nesne kullanarak bir örneği oluşturun `CognitiveServicesCredentials` . 

    ```python
    client = CustomSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

3. İle bir arama isteği gönderin `client.custom_instance.search()` . Arama teriminizi `query` parametreye ekleyin ve `custom_config` arama örneğinizi kullanmak Için özel yapılandırma kimliğinize ayarlayın. **Üretim** sekmesine tıklayarak [Bing özel arama portalından](https://www.customsearch.ai/)kimliğiniz edinebilirsiniz.

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
