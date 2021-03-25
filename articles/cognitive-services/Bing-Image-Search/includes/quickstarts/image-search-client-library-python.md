---
title: Bing Resim Arama Python istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2020
ms.author: aahi
ms.openlocfilehash: 338e4f0c1c47a7786e21ad98bb3e56a1505f9bb2
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105455"
---
API için bir sarmalayıcı olan ve aynı özellikleri içeren Bing Resim Arama istemci kitaplığını kullanarak ilk görüntünüzü aramanızı sağlamak için bu hızlı başlangıcı kullanın. Bu basit Python uygulaması, bir görüntü arama sorgusu gönderir, JSON yanıtını ayrıştırır ve döndürülen ilk görüntünün URL’sini görüntüler.

Bu örneğe ilişkin kaynak kodu, ek hata işleme ve ek açıklama ile [GitHub 'da](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image-search-quickstart.py) kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

* [Python 2,7 veya 3.6 +](https://www.python.org/).

* Python için [Azure resim arama istemci kitaplığı](https://pypi.org/project/azure-cognitiveservices-search-imagesearch/)
    * `pip install azure-cognitiveservices-search-imagesearch` kullanarak yükleme

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Python betiği ve aşağıdaki içeri aktarımları oluşturun:

    ```python
    from azure.cognitiveservices.search.imagesearch import ImageSearchClient
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız ve arama teriminiz için değişkenler oluşturun.

    ```python
    subscription_key = "Enter your key here"
    subscription_endpoint = "Enter your endpoint here"
    search_term = "canadian rockies"
    ```

## <a name="create-the-image-search-client"></a>Görüntü arama istemcisi oluşturma

1. `CognitiveServicesCredentials` örneği oluşturun ve istemcinin bir örneğini başlatmak için bunu kullanın:

    ```python
    client = ImageSearchClient(endpoint=subscription_endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```
1. Bing Resim Arama API’sine arama sorgusu gönderme:
    ```python
    image_results = client.images.search(query=search_term)
    ```
   ## <a name="process-and-view-the-results"></a>Sonuçları işleme ve görüntüleme

Yanıtta döndürülen resim sonuçlarını ayrıştırın.


Yanıt arama sonuçları içeriyorsa, ilk sonucu depolayın ve döndürülen toplam görüntü sayısının yanı sıra bu ilk sonucun küçük resim URL'si, asıl URL gibi ayrıntılarını yazdırın.  

```python
if image_results.value:
    first_image_result = image_results.value[0]
    print("Total number of images returned: {}".format(len(image_results.value)))
    print("First image thumbnail url: {}".format(
        first_image_result.thumbnail_url))
    print("First image content url: {}".format(first_image_result.content_url))
else:
    print("No image results returned!")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](../../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama nedir?](../../overview.md)  
* [Çevrimiçi etkileşimli bir tanıtımı deneyin](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Azure Bilişsel Hizmetler SDK’sı için Python örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)  
* [Azure bilişsel hizmetler belgeleri](../../../index.yml)
* [Bing Resim Arama API’si başvurusu](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)