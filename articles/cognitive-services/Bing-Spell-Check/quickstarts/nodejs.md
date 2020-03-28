---
title: 'Quickstart: REST API ve Node.js ile yazım denetimi - Bing Yazım Denetimi'
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
ms.openlocfilehash: 69c391e6c3f93a998ade7c5721a528d895f8df76
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75382872"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-nodejs"></a>Quickstart: Bing Yazım Denetimi REST API ve Node.js ile yazım denetimi

Bing Yazım Denetimi REST API'ye ilk aramanızı yapmak için bu hızlı başlangıcı kullanın. Bu basit Düğüm uygulaması API'ye bir istek gönderir ve tanımadığı sözcüklerin listesini döndürür ve ardından önerilen düzeltmeler de vardır. Bu uygulama Düğüm.js olarak yazılmış olsa da, API çoğu programlama dili yle uyumlu bir RESTful Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingSpellCheckv7.js)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Düğüm.js 6](https://nodejs.org/en/download/) veya daha sonra.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun. Katılığı ayarlayın ve `https`. Ardından API uç noktanızın ana bilgisayarı, yolu ve abonelik anahtarınız için değişkenler oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```javascript
    'use strict';
    let https = require ('https');

    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/spellcheck';
    let key = '<ENTER-KEY-HERE>';
    ```

2. Arama parametreleriniz ve denetlemek istediğiniz metin için değişkenler oluşturun. Sonra pazar kodunuzu `mkt=`ekle. Piyasa kodu, talepte bulunduğunuz ülkedir. Ayrıca, yazım denetimi modunuzu `&mode=`sonra ekleyin. Mod ya `proof` (çoğu yazım/dilbilgisi hatalarını yakalar) ya da `spell` (çoğu yazım ı yakalar, ancak o kadar çok dilbilgisi hatası yakalamaz).

    ```javascript
    let mkt = "en-US";
    let mode = "proof";
    let text = "Hollo, wrld!";
    let query_string = "?mkt=" + mkt + "&mode=" + mode;
    ```

## <a name="create-the-request-parameters"></a>İstek parametrelerini oluşturma

`POST` Bir yöntemle yeni bir nesne oluşturarak istek parametrelerinizi oluşturun. Bitiş noktası yolunuzu ve sorgu dizenizi ekleyerek yolunuzu ekleyin. Abonelik anahtarınızı üstbilgiye `Ocp-Apim-Subscription-Key` ekleyin.

```javascript
let request_params = {
   method : 'POST',
   hostname : host,
   path : path + query_string,
   headers : {
   'Content-Type' : 'application/x-www-form-urlencoded',
   'Content-Length' : text.length + 5,
      'Ocp-Apim-Subscription-Key' : key,
   }
};
```

## <a name="create-a-response-handler"></a>Yanıt işleyici oluşturma

ApI'den `response_handler` JSON yanıtını almak için çağrılan bir işlev oluşturun ve yazdırın. Yanıt gövdesi için bir değişken oluşturun. Bir `data` bayrak alındığı zaman yanıtı ekle. `response.on()` Bir `end` bayrak aldığında, JSON gövdesini konsola yazdırın.

```javascript
let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        console.log (body_);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};
```

## <a name="send-the-request"></a>İsteği gönderme

İstek parametrelerinizle `https.request()` ve yanıt işleyicinizi kullanarak API'yi arayın. Metninizi API'ye yazın ve isteğinizi daha sonra sonlayın.

```javascript
let req = https.request (request_params, response_handler);
req.write ("text=" + text);
req.end ();
```


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizi oluşturun ve çalıştırın.

Komut satırını kullanıyorsanız, uygulamayı oluşturmak ve çalıştırmak için aşağıdaki komutları kullanın.

```bash
node <FILE_NAME>.js
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
