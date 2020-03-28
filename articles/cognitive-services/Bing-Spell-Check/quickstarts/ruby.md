---
title: 'Quickstart: REST API ve Ruby ile yazım denetimi - Bing Yazım Denetimi'
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
ms.openlocfilehash: 89a2a345e2a4e3ca1be31297e614e86f800e6316
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448425"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-ruby"></a>Quickstart: Bing Yazım Denetimi REST API ve Ruby ile yazım denetimi

Ruby'yi kullanarak Bing Spell Check REST API'ye ilk aramanızı yapmak için bu hızlı başlangıcı kullanın. Bu basit uygulama API'ye bir istek gönderir ve tanımadığı sözcüklerin listesini döndürür ve ardından önerilen düzeltmeler de vardır. Bu uygulama, Ruby ile yazılmış olmakla birlikte API, çoğu programlama diliyle uyumlu bir RESTful Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingSpellCheckv7.rb) kullanılabilir

## <a name="prerequisites"></a>Ön koşullar

* [Yakut 2.4](https://www.ruby-lang.org/en/downloads/) veya sonrası.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullanılan düzenleyicinizde veya IDE'nizde yeni bir Ruby dosyası oluşturun ve aşağıdaki gereksinimleri ekleyin. 

    ```ruby
    require 'net/http'
    require 'uri'
    require 'json'
    ```

2. Abonelik anahtarınız, bitiş noktası URI'niz ve yolunuz için değişkenler oluşturun. Parametreyi `mkt=` pazarınıza ve `&mode` `proof` prova moduna ekleyerek istek parametrelerinizi oluşturun. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

    ```ruby
    key = 'ENTER YOUR KEY HERE'
    uri = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/spellcheck?'
    params = 'mkt=en-us&mode=proof'
    ```

## <a name="send-a-spell-check-request"></a>Yazım denetimi isteği gönderme

1. Ana bilgisayar uri, yol ve parametreler dizenizden bir URI oluşturun. Sorgusunu denetimi hecelemek istediğiniz metni içerecek şekilde ayarlayın.

   ```ruby
   uri = URI(uri + path + params)
   uri.query = URI.encode_www_form({
      # Request parameters
   'text' => 'Hollo, wrld!'
   })
   ```

2. Yukarıda oluşturulmuş URI'yi kullanarak bir istek oluşturun. Üstbilgi için `Ocp-Apim-Subscription-Key` anahtarınızı ekleyin.

    ```ruby
    request = Net::HTTP::Post.new(uri)
    request['Content-Type'] = "application/x-www-form-urlencoded"
    request['Ocp-Apim-Subscription-Key'] = key
    ```

3. İsteği gönderin.

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request(request)
    end
    ```

4. JSON yanıtını alın ve konsola yazdırın. 

    ```ruby
    result = JSON.pretty_generate(JSON.parse(response.body))
    puts result
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizi oluşturun ve çalıştırın.

Komut satırını kullanıyorsanız, uygulamayı çalıştırmak için aşağıdaki komutu kullanın.

```bash
ruby <FILE_NAME>.rb
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
