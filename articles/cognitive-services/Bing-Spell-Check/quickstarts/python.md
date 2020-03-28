---
title: 'Quickstart: REST API ve Python ile yazım denetimi - Bing Yazım Denetimi'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçla yazım ve dilbilgisini denetlemek için Bing Yazım Denetimi REST API'sını kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: aahi
ms.openlocfilehash: 6b0977628f7c3d971804d8597f42425608028081
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448465"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-python"></a>Quickstart: Bing Yazım Denetimi REST API ve Python ile yazım denetimi

Bing Yazım Denetimi REST API'ye ilk aramanızı yapmak için bu hızlı başlangıcı kullanın. Bu basit Python uygulaması API'ye bir istek gönderir ve önerilen düzeltmelerin bir listesini döndürür. Bu uygulama Python ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py) kullanılabilir

## <a name="prerequisites"></a>Ön koşullar

* Python [3.x](https://www.python.org)

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki alma deyimini ekleyin.

   ```python
   import requests
   import json
   ```

2. Denetimi hecelemek istediğiniz metin, abonelik anahtarınız ve Bing Büyüsü Denetimi bitiş noktanız için değişkenler oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```python
    api_key = "<ENTER-KEY-HERE>"
    example_text = "Hollo, wrld" # the text to be spell-checked
    endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck"
    ```

## <a name="create-the-parameters-for-the-request"></a>İstek için parametreleri oluşturma

1. Anahtar `text` olarak yeni bir sözlük ve değer olarak metin oluşturun.

    ```python
    data = {'text': example_text}
    ```

2. İsteğiniz için parametreleri ekleyin. Sonra pazar kodunuzu `mkt=`ekle. Piyasa kodu, talepte bulunduğunuz ülkedir. Ayrıca, yazım denetimi modunuzu `&mode=`sonra ekleyin. Mod ya `proof` (çoğu yazım/dilbilgisi hatalarını yakalar) ya da `spell` (çoğu yazım ı yakalar, ancak o kadar çok dilbilgisi hatası yakalamaz).

    ```python
    params = {
        'mkt':'en-us',
        'mode':'proof'
        }
    ```

3. Üstbilgi `Content-Type` ve abonelik anahtarınızı üstbilgiye `Ocp-Apim-Subscription-Key` ekleyin.

    ```python
    headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Ocp-Apim-Subscription-Key': api_key,
        }
    ```

## <a name="send-the-request-and-read-the-response"></a>İsteği gönderin ve yanıtı okuyun

1. İstek kitaplığını kullanarak POST isteğini gönderin.

    ```python
    response = requests.post(endpoint, headers=headers, params=params, data=data)
    ```

2. JSON yanıtını alın ve yazdırın.

    ```python
    json_response = response.json()
    print(json.dumps(json_response, indent=4))
    ```


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Komut satırını kullanıyorsanız, uygulamayı çalıştırmak için aşağıdaki komutu kullanın.

```bash
python <FILE_NAME>.py
```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorials/spellcheck.md)

- [Bing Yazım Denetimi API’si nedir?](../overview.md)
- [Bing Yazım Denetimi API’si v7 Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
