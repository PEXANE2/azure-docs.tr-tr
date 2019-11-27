---
title: 'Hızlı başlangıç: REST API ve Node. js kullanarak görüntü öngörülerini alın-Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si bir görüntüyü karşıya yüklemeyi ve ilgili öngörüleri nasıl alabileceğinizi öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: ecfe341fa050e693f919f35c29c8120c687c88f8
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383199"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Node. js kullanarak görüntü öngörülerini alın

Bing Görsel Arama API'si ilk çağrısını yapmak ve arama sonuçlarını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması, API 'ye bir görüntü yükler ve onunla ilgili olarak döndürülen bilgileri görüntüler. Bu uygulama JavaScript 'e yazılırken, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Yerel bir görüntü karşıya yüklenirken, form verileri `Content-Disposition` üst bilgisini içermelidir. `name` parametresini "image" olarak ayarlamanız gerekir ve `filename` parametresi herhangi bir dizeye ayarlanabilir. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz resim boyutu üst sınırı 1 MB'tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)
* JavaScript için Istek modülü. Modülünü yüklemek için `npm install request` komutunu kullanabilirsiniz.
* Form veri modülü. Modülünü yüklemek için `npm install form-data` komutunu kullanabilirsiniz. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ayarlayın:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API uç noktanız, abonelik anahtarınız ve görüntünüzün yolu için değişkenler oluşturun:

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. API 'den yanıtı yazdırmak için `requestCallback()` adlı bir işlev oluşturun:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Arama isteğini oluşturun ve gönderin

1. `FormData()`kullanarak yeni bir **FormData** nesnesi oluşturun ve `fs.createReadStream()`kullanarak görüntü yolunuza ekleyin:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Görüntüyü karşıya yüklemek için istek kitaplığını kullanın ve yanıtı yazdırmak için `requestCallback()` çağırın. Abonelik anahtarınızı istek üstbilgisine eklediğinizden emin olun:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel Arama tek sayfalı Web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
