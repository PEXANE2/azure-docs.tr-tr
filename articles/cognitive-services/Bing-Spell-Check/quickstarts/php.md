---
title: 'Hızlı başlangıç: REST API ve PHP ile yazımı denetleme-Bing Yazım Denetimi'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıç, basit bir PHP uygulamasının Bing Yazım Denetimi API'si bir istek gönderdiğini ve önerilen düzeltmelerin bir listesini nasıl döndürdüğünü gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 05/21/2020
ms.author: aahi
ms.openlocfilehash: 44dc3f16128913f76369ad645a32e91f38272546
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83869869"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-php"></a>Hızlı başlangıç: Bing Yazım Denetimi REST API ve PHP ile yazım denetimi yapma

Bing Yazım Denetimi REST API ilk çağrlarınızı yapmak için bu hızlı başlangıcı kullanın. Bu basit PHP uygulaması, API 'ye bir istek gönderir ve önerilen düzeltmelerin bir listesini döndürür. 

Bu uygulama PHP 'de yazılmış olsa da, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

* [PHP 5.6. x](https://php.net/downloads.php)

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="get-bing-spell-check-rest-api-results"></a>Bing Yazım Denetimi REST API sonuçları al

1. Sık kullandığınız IDE ile yeni bir PHP projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
4. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.
5. Programı çalıştırın.
    
    ```php
    <?php
    
    // NOTE: Be sure to uncomment the following line in your php.ini file.
    // ;extension=php_openssl.dll
    
    // These properties are used for optional headers (see below).
    // define("CLIENT_ID", "<Client ID from Previous Response Goes Here>");
    // define("CLIENT_IP", "999.999.999.999");
    // define("CLIENT_LOCATION", "+90.0000000000000;long: 00.0000000000000;re:100.000000000000");
    
    $host = 'https://api.cognitive.microsoft.com';
    $path = '/bing/v7.0/spellcheck?';
    $params = 'mkt=en-us&mode=proof';
    
    $input = "Hollo, wrld!";
    
    $data = array (
        'text' => urlencode ($input)
    );
    
    // NOTE: Replace this example key with a valid subscription key.
    $key = 'ENTER KEY HERE';
    
    // The following headers are optional, but it is recommended
    // that they are treated as required. These headers will assist the service
    // with returning more accurate results.
    //'X-Search-Location' => CLIENT_LOCATION
    //'X-MSEdge-ClientID' => CLIENT_ID
    //'X-MSEdge-ClientIP' => CLIENT_IP
    
    $headers = "Content-type: application/x-www-form-urlencoded\r\n" .
        "Ocp-Apim-Subscription-Key: $key\r\n";
    
    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // https://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'POST',
            'content' => http_build_query ($data)
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path . $params, false, $context);
    
    if ($result === FALSE) {
        /* Handle error */
    }
    
    $json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
    echo $json;
    ?>
    ```


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Web sunucusunu başlatıp dosyanıza giderek uygulamanızı çalıştırın.

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
