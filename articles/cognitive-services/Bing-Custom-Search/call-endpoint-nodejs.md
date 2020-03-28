---
title: 'Quickstart: Node.js kullanarak Bing Özel Arama bitiş noktasını arayın | Microsoft Dokümanlar'
titleSuffix: Azure Cognitive Services
description: Node.js kullanarak Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 1c7bd97de4e46e1c8da467840006fe2520851caf
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80238876"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-nodejs"></a>Quickstart: Node.js kullanarak Bing Özel Arama bitiş noktasını arayın

Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın. Bu uygulama JavaScript'te yazılıyken, Bing Özel Arama API'sı çoğu programlama diliyle uyumlu bir yeniden kullanımlı web hizmetidir. Bu örnek için kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingCustomSearchv7.js)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

- Bing Özel Arama örneği. Bkz. Hızlı Başlangıç: Daha fazla bilgi için [ilk Bing Özel Arama örneğini oluşturun.](quick-start.md)

- [Node.js](https://www.nodejs.org/)

- [JavaScript İstek Kitaplığı](https://github.com/request/request)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullanılan IDE veya düzenleyicinizde yeni bir JavaScript dosyası oluşturun ve istekkitaplığı için bir `require()` deyim ekleyin. Abonelik anahtarınız, Özel Yapılandırma Kimliğiniz ve arama teriminiz için değişkenler oluşturun. 

    ```javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderme ve alma 

1. İsteğinizde gönderilen bilgileri depolamak için bir değişken oluşturun. Arama teriminizi `q=` sorgu parametresine ve arama örneğinizin Özel Yapılandırma Kimliği'ne `customconfig=`ekleyerek istek URL'sini oluştur. parametreleri bir `&` karakterle ayırın. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

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

1. Bing Özel Arama örneğinize bir arama isteği göndermek ve sonuçları yla ilgili bilgileri (adı, url'si ve web sayfasının en son tarandığı tarih) yazdırmak için JavaScript İstek Kitaplığını kullanın.

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
> [Özel Arama web uygulaması oluşturma](./tutorials/custom-search-web-page.md)
