---
title: 'Hızlı Başlangıç: Node. js kullanarak Bing Özel Arama uç noktanızı çağırma | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Node. js kullanarak Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 3703658d6cab78592c50a23deaf9da2a426aebec
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405176"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-nodejs"></a>Hızlı Başlangıç: Node. js kullanarak Bing Özel Arama uç noktanızı çağırma

Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın. Bu uygulama JavaScript 'e yazılırken, Bing Özel Arama API'si çoğu programlama dili ile uyumlu olan bir yeniden yazılmış Web hizmetidir. Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingCustomSearchv7.js)’da mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

- Bir Bing Özel Arama örneği. Bkz [. hızlı başlangıç: Daha fazla bilgi için ilk](quick-start.md) Bing özel arama örneğinizi oluşturun.

- [Node.js](https://www.nodejs.org/)

- [JavaScript Istek kitaplığı](https://github.com/request/request)

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

1. İsteğiniz içinde gönderilmekte olan bilgileri depolamak için bir değişken oluşturun. Arama teriminizi `q=` sorgu parametresine ekleyerek istek URL 'sini oluşturun ve arama örneğinizin özel yapılandırma kimliği ' ni olarak `customconfig=`yapın. parametreleri bir `&` karakterle ayırın. 

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
