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
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-python
ms.openlocfilehash: 7c76ec8600ade26098206a5d18559e795db53178
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853084"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Python kullanarak görüntü öngörülerini alın

Bing Görsel Arama API'si ilk çağrısını yapmak için bu hızlı başlangıcı kullanın. Bu Python uygulaması, API 'ye bir görüntü yükler ve döndürdüğü bilgileri görüntüler. Bu uygulama Python 'da yazılmış olsa da, API birçok programlama dili ile uyumlu olan bir yenilenmiş Web hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki `import` ifadeyi ekleyin:

    ```python
    import requests, json
    ```

2. Abonelik anahtarınız, uç noktanız ve karşıya yüklediğiniz görüntünün yolu için değişkenler oluşturun. Değeri için `BASE_URI` aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure Portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```
    
3. Yerel bir görüntüyü karşıya yüklediğinizde, form verileri `Content-Disposition` üstbilgiyi içermelidir. `name`Parametresini "Image" olarak ayarlayın ve bu `filename` parametreyi görüntünüzün dosya adına ayarlayın. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz en büyük görüntü boyutu 1 MB 'tır.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

4. İsteğiniz başlık bilgilerini tutmak için bir sözlük nesnesi oluşturun. Abonelik anahtarınızı dizeye bağlayın `Ocp-Apim-Subscription-Key` .

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

5. Görüntüsünü içeren ve isteği gönderdiğinizde açılan ve yüklenen bir sözlük oluşturun.

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>JSON yanıtını Ayrıştır

`print_json()`API yanıtını kabul etmek ve JSON 'u yazdırmak için adlı bir yöntem oluşturun.

```python
def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
```

## <a name="send-the-request"></a>İsteği gönder

`requests.post()`Bing Görsel Arama API'si bir istek göndermek için kullanın. Uç nokta, başlık ve dosya bilgileriniz için dize ekleyin. `response.json()`İle Yazdır `print_json()` .

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
