---
title: 'Hızlı başlangıç: REST API ve Ruby-Bing Yazım Denetimi ile yazım denetimi yapma'
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
ms.openlocfilehash: fae466124244f5d2b04ad6e59681011b9c5ba974
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83993533"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-ruby"></a>Hızlı başlangıç: Bing Yazım Denetimi REST API ve Ruby ile yazım denetimi yapma

Ruby kullanarak Bing Yazım Denetimi REST API ilk çağrlarınızı yapmak için bu hızlı başlangıcı kullanın. Bu basit uygulama, API 'ye bir istek gönderir ve önerilen düzeltmelerin bir listesini döndürür. 

Bu uygulama Ruby dilinde yazılmış olsa da, API çoğu programlama dili ile uyumlu olan bir yenilenmiş Web hizmetidir. Bu uygulamanın kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingSpellCheckv7.rb) 'da kullanılabilir

## <a name="prerequisites"></a>Ön koşullar

* [Ruby 2,4](https://www.ruby-lang.org/en/downloads/) veya üzeri.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz düzenleyicide veya IDE 'de yeni bir Ruby dosyası oluşturun ve aşağıdaki gereksinimleri ekleyin: 

    ```ruby
    require 'net/http'
    require 'uri'
    require 'json'
    ```

2. Abonelik anahtarınız, uç nokta URI 'SI ve yolu için değişkenler oluşturun. Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz. İstek parametrelerinizi oluşturun:

   1. Pazar kodunuzu `mkt` parametreye, `=` işleçle atayın. Pazar kodu, isteği yaptığınız ülkenin/bölgenin kodudur. 

   1. `mode`Parametresini `&` işleçle ekleyin ve ardından yazım denetimi modunu atayın. Mod `proof` (en fazla yazım/dilbilgisi hatalarını yakalar) ya da `spell` (en fazla yazım hatalarını yakalar, ancak çok sayıda dilbilgisi hatası) olabilir. 

    ```ruby
    key = 'ENTER YOUR KEY HERE'
    uri = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/spellcheck?'
    params = 'mkt=en-us&mode=proof'
    ```

## <a name="send-a-spell-check-request"></a>Yazım denetimi isteği gönder

1. Konak Uri 'si, yolu ve parametre dizinizden bir URI oluşturun. Sorgu, yazım denetimi yapmak istediğiniz metni içerecek şekilde ayarlanır.

   ```ruby
   uri = URI(uri + path + params)
   uri.query = URI.encode_www_form({
      # Request parameters
   'text' => 'Hollo, wrld!'
   })
   ```

2. Daha önce oluşturulmuş URI 'yi kullanarak bir istek oluşturun. Anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

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

4. JSON yanıtını alın ve konsolunda yazdırın. 

    ```ruby
    result = JSON.pretty_generate(JSON.parse(response.body))
    puts result
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizi derleyin ve çalıştırın. Komut satırını kullanıyorsanız, uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

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
- [Bing Yazım Denetimi API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
