---
title: 'Quickstart: REST API ve Python kullanarak görüntü öngörüleri alın - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sine nasıl görüntü yükleyip bu konuda bilgi edineceklerini öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: b56f6743b642904349797ac5b6167194f7916b45
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75446599"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Quickstart: Bing Visual Search REST API ve Python'u kullanarak görüntü öngörüleri alın

Bing Görsel Arama API'sine ilk aramanızı yapmak ve sonuçları görüntülemek için bu hızlı başlangıcı kullanın. Bu Python uygulaması API'ye bir resim yükler ve döndürdgösterdiği bilgileri görüntüler. Bu uygulama Python'da yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python `import` dosyası oluşturun ve aşağıdaki ifadeyi ekleyin:

    ```python
    import requests, json
    ```

2. Abonelik anahtarınız, bitiş noktanız ve yüklediğiniz resme giden yol için değişkenler oluşturun. `BASE_URI`aşağıdaki genel bitiş noktası veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktası olabilir:

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```
    
    Yerel bir resim yüklediğinizde, form verilerinin üstbilgiiç `Content-Disposition` içermesi gerekir. Parametresini `name` "görüntü" olarak ayarlamanız gerekir ve `filename` parametreyi herhangi bir dize ayarlayabilirsiniz. Formun içeriği görüntünün ikili verilerini içerir. Yükleyebileceğiniz maksimum görüntü boyutu 1 MB'dır.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

3. İsteğinizin üstbilgi bilgilerini tutmak için bir sözlük nesnesi oluşturun. Abonelik anahtarınızı aşağıda gösterildiği `Ocp-Apim-Subscription-Key`gibi dize yle bağdın:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. İsteği gönderdiğinizde açılıp yüklenen resminizi içeren başka bir sözlük oluşturun:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Ayrışdırış JSON yanıtı

1. API yanıtını `print_json()` almak için çağrılan bir yöntem oluşturun ve JSON'u yazdırın:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>İsteği gönderme

1. Bing `requests.post()` Görsel Arama API'sine istek göndermek için kullanın. Bitiş noktanız, üstbilginiz ve dosya bilgileriniz için dizeyi ekleyin. `response.json()` Yazdır: `print_json()`

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
> [Görsel Arama tek sayfalık web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
