---
title: Hızlı başlangıç-Node. js kullanarak API 'ye bir sorgu gönderme-Bing yerel Iş arama
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmeti olan Bing yerel Iş Arama API 'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: d366195f9cd72e6baa88c17203ae93cbbc6cbe6a
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80475527"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Hızlı başlangıç: node. js kullanarak Bing yerel Iş Arama API 'sine bir sorgu gönderme

Azure bilişsel hizmeti olan Bing yerel Iş Arama API 'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu basit uygulama Node. js ' de yazıldığı sırada, API, HTTP istekleri yapma ve JSON 'u ayrıştırma yeteneğine sahip olan herhangi bir programlama diliyle uyumlu olan bir yeniden takip eden Web hizmetidir.

Bu örnek uygulama, arama sorgusu `hotel in Bellevue`için API 'den gelen yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [JavaScript İstek Kitaplığı](https://github.com/request/request)

Bing API 'lerle bir bilişsel [Hizmetler API 'si hesabınızın](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme tarafından sunulan erişim anahtarını kullanın.  Ayrıca bkz. bilişsel [Hizmetler fiyatlandırması-BING arama API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="code-scenario"></a>Kod senaryosu

Aşağıdaki kod, isteği tanımlar ve gönderir. Aşağıdaki adımları izler:

1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorguyu belirtin ve sorgu parametresini ekleyin.
3. Yanıt için bir işleyici işlevi oluşturma.
4. İsteği oluşturan ve Ocp-Apim-Subscription-Key üst bilgisini ekleyen Search işlevini tanımlama.
5. Search işlevini çalıştırma.

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

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

* [Yerel Iş araması hızlı başlangıç](local-quickstart.md)
* [Yerel Iş arama Java hızlı başlangıç](local-search-java-quickstart.md)
* [Yerel Iş arama Python hızlı başlangıç](local-search-python-quickstart.md)
