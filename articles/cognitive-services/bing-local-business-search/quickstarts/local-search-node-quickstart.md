---
title: Hızlı başlangıç-Node.js-Bing yerel Iş araması kullanarak API 'ye bir sorgu gönderme
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmeti olan Bing yerel Iş Arama API 'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 05/12/2020
ms.author: aahi
ms.openlocfilehash: 2b5de1c5bf99ff2516a0eb836d540fc2833d99cb
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611263"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Hızlı başlangıç: Node.js kullanarak Bing yerel Iş Arama API 'sine bir sorgu gönderin

Azure bilişsel hizmeti olan Bing yerel Iş Arama API 'sine istek gönderme hakkında bilgi edinmek için bu hızlı başlangıcı kullanın. Bu basit uygulama Node.js yazılmış olsa da, API, HTTP istekleri yapma ve JSON 'u ayrıştırma yeteneğine sahip olan herhangi bir programlama diliyle uyumlu olan bir yeniden takip eden Web hizmetidir.

Bu örnek uygulama, bir arama sorgusu için API 'den gelen yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.
* [JavaScript Istek kitaplığı](https://github.com/request/request).
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir Bing arama kaynağı oluşturun Bing arama bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.


## <a name="code-scenario"></a>Kod senaryosu

Aşağıdaki kod, aşağıdaki adımlarda uygulanan isteği tanımlar ve gönderir:

1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorguyu belirtin ve sorgu parametresini ekleyin.
3. Yanıt için bir işleyici işlevi oluşturma.
4. İsteği oluşturan ve üstbilgiyi ekleyen arama işlevini tanımlayın `Ocp-Apim-Subscription-Key` .
5. Search işlevini çalıştırın.


```javascript
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&mkt=" + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Sonraki adımlar

* [Yerel Iş arama C# hızlı başlangıç](local-quickstart.md)
* [Yerel Iş arama Java hızlı başlangıç](local-search-java-quickstart.md)
* [Yerel Iş arama Python hızlı başlangıç](local-search-python-quickstart.md)
