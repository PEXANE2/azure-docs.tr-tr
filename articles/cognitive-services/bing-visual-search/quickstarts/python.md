---
title: 'Hızlı başlangıç: REST API ve Python kullanarak görüntü öngörülerini alın-Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si bir görüntüyü karşıya yüklemeyi ve ilgili öngörüleri nasıl alabileceğinizi öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 6fafc35d9d74927789fee3f3fea3014ff3be5717
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383170"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Python kullanarak görüntü öngörülerini alın

Bing Görsel Arama API'si ilk çağrısını yapmak ve sonuçları görüntülemek için bu hızlı başlangıcı kullanın. Bu Python uygulaması, API 'ye bir görüntü yükler ve döndürdüğü bilgileri görüntüler. Bu uygulama Python 'da yazılmış olsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Yerel bir görüntüyü karşıya yüklediğinizde, form verileri `Content-Disposition` üst bilgisini içermelidir. `name` parametresini "image" olarak ayarlamanız gerekir ve `filename` parametresini herhangi bir dizeye ayarlayabilirsiniz. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz en büyük görüntü boyutu 1 MB 'tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Python 3. x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki `import` ifadesini ekleyin:

    ```python
    import requests, json
    ```

2. Abonelik anahtarınız, uç noktanız ve karşıya yüklediğiniz görüntünün yolu için değişkenler oluşturun:

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. İsteğiniz başlık bilgilerini tutmak için bir sözlük nesnesi oluşturun. Abonelik anahtarınızı aşağıda gösterildiği gibi dize `Ocp-Apim-Subscription-Key`bağlayın:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Görüntüsünü içeren ve isteği gönderdiğinizde açılan ve yüklenen bir başka sözlük oluşturun:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>JSON yanıtını Ayrıştır

1. API yanıtında gerçekleştirilecek `print_json()` adlı bir yöntem oluşturun ve JSON 'u yazdırın:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>İsteği gönder

1. Bing Görsel Arama API'si bir istek göndermek için `requests.post()` kullanın. Uç nokta, başlık ve dosya bilgileriniz için dize ekleyin. `print_json()``response.json()` Yazdır:

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
