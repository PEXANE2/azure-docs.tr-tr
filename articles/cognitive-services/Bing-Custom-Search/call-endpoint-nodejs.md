---
title: 'Hızlı başlangıç: node. js kullanarak Bing Özel Arama uç noktanızı çağırma | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Node. js kullanarak Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 1c7bd97de4e46e1c8da467840006fe2520851caf
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80238876"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-nodejs"></a>Hızlı başlangıç: node. js kullanarak Bing Özel Arama uç noktanızı çağırma

Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın. Bu uygulama JavaScript 'e yazılırken, Bing Özel Arama API'si çoğu programlama dili ile uyumlu olan bir yeniden yazılmış Web hizmetidir. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingCustomSearchv7.js)' da kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

- Bir Bing Özel Arama örneği. Daha fazla bilgi için bkz. [hızlı başlangıç: ilk Bing özel arama örneğinizi oluşturma](quick-start.md) .

- [Node.js](https://www.nodejs.org/)

- [JavaScript İstek Kitaplığı](https://github.com/request/request)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir JavaScript dosyası oluşturun ve istekler kitaplığı için bir `require()` ifade ekleyin. Abonelik anahtarınız, özel yapılandırma KIMLIĞINIZ ve bir arama terimi için değişkenler oluşturun. 

    ```javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderme ve alma 

1. İsteğiniz içinde gönderilmekte olan bilgileri depolamak için bir değişken oluşturun. Arama teriminizi `q=` sorgu parametresine ekleyerek istek URL 'sini oluşturun ve arama örneğinizin özel yapılandırma kimliği ' ni olarak `customconfig=`yapın. parametreleri bir `&` karakterle ayırın. Aşağıdaki genel uç noktayı veya kaynak için Azure portal görüntülenmiş [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```javascript
    var info = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
            'q=' + searchTerm + "&" +
            'customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    ```

1. Bing Özel Arama örneğinize bir arama isteği göndermek ve sonuçları hakkındaki bilgileri, adı, URL 'si ve Web sayfasının son gezinildiği tarihi de içerecek şekilde yazdırmak için JavaScript Istek kitaplığını kullanın.

    ```javascript
    request(info, function(error, response, body){
            var searchResponse = JSON.parse(body);
            for(var i = 0; i < searchResponse.webPages.value.length; ++i){
                var webPage = searchResponse.webPages.value[i];
                console.log('name: ' + webPage.name);
                console.log('url: ' + webPage.url);
                console.log('displayUrl: ' + webPage.displayUrl);
                console.log('snippet: ' + webPage.snippet);
                console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
                console.log();
            }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel bir arama Web uygulaması oluşturma](./tutorials/custom-search-web-page.md)
