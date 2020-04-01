---
title: 'Quickstart: Bing Image Search REST API ve Python kullanarak görüntüleri arayın'
titleSuffix: Azure Cognitive Services
description: Python'u kullanarak Bing Image Search REST API'sine görüntü arama istekleri göndermek ve JSON yanıtlarını almak için bu hızlı başlatmayı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 03/31/2020
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 6287b9b8e6129fd62a896e5ac1fcca29febbf01a
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478548"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-python"></a>Quickstart: Bing Image Search REST API ve Python kullanarak görüntüleri arayın

Bing Resim Arama API'sine arama istekleri göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu Python uygulaması API'ye bir arama sorgusu gönderir ve sonuçlardaki ilk resmin URL'sini görüntüler. Bu uygulama Python dilinde yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir.

Bu örneği başlatma Bağlayıcı rozetine tıklayarak [Bağlayıcım](https://mybinder.org)’da bir Jupyter not defteri olarak çalıştırabilirsiniz:

[![Cilt](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Bu örnek için kaynak kodu, ek hata işleme ve ek açıklamalar ile [GitHub kullanılabilir.](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py)


## <a name="prerequisites"></a>Ön koşullar

* [Python 2.x veya 3.x](https://www.python.org/)
* [Python Görüntüleme Kitaplığı (PIL)](https://pillow.readthedocs.io/en/stable/index.html)
* [matplotlib](https://matplotlib.org/) 

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki modülleri aktarın. Abonelik anahtarınız, arama bitiş noktanız ve arama teriminiz için bir değişken oluşturun. `search_url`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir.

    ```python
    import requests
    import matplotlib.pyplot as plt
    from PIL import Image
    from io import BytesIO
    
    subscription_key = "your-subscription-key"
    search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
    search_term = "puppies"
    ```

2. Sözlük oluşturarak ve `Ocp-Apim-Subscription-Key` anahtarı değer olarak ekleyerek abonelik anahtarınızı üstbilgiye ekleyin. 

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    ```

## <a name="create-and-send-a-search-request"></a>Arama isteği oluşturma ve gönderme

1. Arama isteğinin parametreleri için bir sözlük oluşturun. Arama teriminizi parametreye `q` ekleyin. Kamu malı görüntüleri `license` aramak için parametre için "ortak" kullanın. Yalnızca fotoğrafları aramak `imageType` için "fotoğraf" kullanın.

    ```python
    params  = {"q": search_term, "license": "public", "imageType": "photo"}
    ```

2. Bing `requests` Resim Arama API'sini aramak için kitaplığı kullanın. Üstbilginizi ve parametrelerinizi isteğe ekleyin ve yanıtı JSON nesnesi olarak döndürün. URL'leri yanıtın `thumbnailUrl` alanından birkaç küçük resim görüntüsüne getirin.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
    ```

## <a name="view-the-response"></a>Yanıtı görüntüleme

1. Matplotlib kitaplığını kullanarak dört sütunlu ve dört satırlı yeni bir şekil oluşturun. 

2. Şeklin satırlarını ve sütunlarını yineleyin ve her alana `Image.open()` bir resim küçük resmi eklemek için PIL kitaplığı yöntemini kullanın. 

3. Şekli `plt.show()` çizmek ve görüntüleri görüntülemek için kullanın.

    ```python
    f, axes = plt.subplots(4, 4)
    for i in range(4):
        for j in range(4):
            image_data = requests.get(thumbnail_urls[i+4*j])
            image_data.raise_for_status()
            image = Image.open(BytesIO(image_data.content))        
            axes[i][j].imshow(image)
            axes[i][j].axis("off")
    plt.show()
    ```


## <a name="example-json-response"></a>Örnek JSON yanıtı

Bing Resim Arama API'sinden yanıtlar JSON olarak döndürülür. Bu örnek yanıt, tek bir sonuç göstermek için kısaltıldı.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Resim Arama tek sayfalı uygulama öğreticisi](../tutorial-bing-image-search-single-page-app.md)

* [Bing Resim Arama API’si nedir?](../overview.md)  
* Bing Arama API'leri için [fiyatlandırma ayrıntıları.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) 
* [Ücretsiz bir Bilişsel Hizmetler erişim anahtarı alın](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Azure Bilişsel Hizmetler Belgeleri](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Resim Arama API’si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
