---
title: 'Hızlı başlangıç: REST API ve Python ile yazım denetimi-Bing Yazım Denetimi'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıç ile yazım ve dilbilgisini denetlemek için Bing Yazım Denetimi REST API kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 05/21/2020
ms.author: aahi
ms.custom: devx-track-python
ms.openlocfilehash: e121aaf6725c179189b25dff6534b019c5de730c
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852744"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-python"></a>Hızlı başlangıç: Bing Yazım Denetimi REST API ve Python ile yazım denetimi yapma

Bing Yazım Denetimi REST API ilk çağrlarınızı yapmak için bu hızlı başlangıcı kullanın. Bu basit Python uygulaması, API 'ye bir istek gönderir ve önerilen düzeltmelerin bir listesini döndürür. 

Bu uygulama Python 'da yazılmış olsa da, API birçok programlama dili ile uyumlu olan bir yenilenmiş Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingEntitySearchv7.py) 'da kullanılabilir

## <a name="prerequisites"></a>Önkoşullar

* Python [3. x](https://www.python.org)

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimlerini ekleyin:

   ```python
   import requests
   import json
   ```

2. Yazım denetimi yapmak istediğiniz metin, abonelik anahtarınız ve Bing Yazım Denetimi uç noktanız için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```python
    api_key = "<ENTER-KEY-HERE>"
    example_text = "Hollo, wrld" # the text to be spell-checked
    endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck"
    ```

## <a name="create-the-parameters-for-the-request"></a>İstek için parametreleri oluşturma

1. Anahtar olarak ile yeni bir sözlük `text` ve değer olarak metninizi oluşturun.

    ```python
    data = {'text': example_text}
    ```

2. İsteğiniz için parametreleri ekleyin: 

   1. Pazar kodunuzu `mkt` parametreye, `=` işleçle atayın. Pazar kodu, isteği yaptığınız ülkenin/bölgenin kodudur. 

   1. `mode`Parametresini `&` işleçle ekleyin ve ardından yazım denetimi modunu atayın. Mod `proof` (en fazla yazım/dilbilgisi hatalarını yakalar) ya da `spell` (en fazla yazım hatalarını yakalar, ancak çok sayıda dilbilgisi hatası) olabilir. 
 
    ```python
    params = {
        'mkt':'en-us',
        'mode':'proof'
        }
    ```

3. Üst bilgiye `Content-Type` ve abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` .

    ```python
    headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Ocp-Apim-Subscription-Key': api_key,
        }
    ```

## <a name="send-the-request-and-read-the-response"></a>İsteği gönder ve yanıtı oku

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

Komut satırını kullanıyorsanız, uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

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
- [Bing Yazım Denetimi API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
