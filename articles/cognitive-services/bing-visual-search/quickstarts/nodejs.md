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
ms.date: 12/17/2019
ms.author: scottwhi
ms.openlocfilehash: 373d6fa5402ba703cbebe88ad562974ba97f3391
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75379717"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Hızlı başlangıç: Bing Görsel Arama REST API ve Node. js kullanarak görüntü öngörülerini alın

Bing Görsel Arama API'si ilk çağrısını yapmak ve arama sonuçlarını görüntülemek için bu hızlı başlangıcı kullanın. Bu basit JavaScript uygulaması, API 'ye bir görüntü yükler ve onunla ilgili olarak döndürülen bilgileri görüntüler. Bu uygulama JavaScript 'e yazılırken, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)
* JavaScript için Istek modülü. Modülünü yüklemek için `npm install request` komutunu kullanabilirsiniz.
* Form veri modülü. Modülünü yüklemek için `npm install form-data` komutunu kullanabilirsiniz. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Uygulamayı başlatma

1. En sevdiğiniz IDE veya düzenleyicide bir JavaScript dosyası oluşturun ve aşağıdaki gereksinimleri ayarlayın:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. API uç noktanız, abonelik anahtarınız ve görüntünüzün yolu için değişkenler oluşturun. `baseUri`, aşağıdaki genel uç nokta veya kaynak için Azure portal görüntülenecek [özel alt etki alanı](../../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktası olabilir:

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

Yerel bir görüntü karşıya yüklenirken, form verileri `Content-Disposition` üst bilgisini içermelidir. `name` parametresini "image" olarak ayarlamanız gerekir ve `filename` parametresi herhangi bir dizeye ayarlanabilir. Formun içeriği görüntünün ikili verilerini içerir. Karşıya yükleyebileceğiniz maksimum görüntü boyutu 1 MB’tır.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

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
