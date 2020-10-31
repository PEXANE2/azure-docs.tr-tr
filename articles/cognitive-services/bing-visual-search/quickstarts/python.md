---
title: 'Hızlı başlangıç: REST API ve Python kullanarak görüntü öngörülerini alın-Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si ve Python kullanarak bir görüntüyü karşıya yüklemeyi öğrenin ve ardından görüntüyle ilgili öngörüleri alın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-python
ms.openlocfilehash: 712f86eeaa49c1afe281ad5ede7a6cf2cc0ada4b
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93074989"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Python kullanarak görüntü öngörülerini alın

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

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
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
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
