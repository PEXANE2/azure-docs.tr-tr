---
title: Quickstart - Node.js kullanarak API'ye sorgu gönderme - Bing Yerel İş Arama
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmeti olan Bing Yerel İş Arama API'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: d366195f9cd72e6baa88c17203ae93cbbc6cbe6a
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475527"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Hızlı başlatma: Node.js kullanarak Bing Yerel İş Arama API'sine sorgu gönderme

Azure Bilişsel Hizmeti olan Bing Yerel İş Arama API'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu basit uygulama Node.js olarak yazılmış olsa da, API, HTTP isteklerini yapma ve JSON ayrıştırma yeteneğine sahip herhangi bir programlama dili ile uyumlu bir RESTful Web hizmetidir.

Bu örnek uygulama, arama sorgusu `hotel in Bellevue`için API'den yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Ön koşullar

* [Node.js](https://nodejs.org/en/download/)'in en son sürümü.

* [JavaScript İstek Kitaplığı](https://github.com/request/request)

Bing API'leri ile [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olmalıdır. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme sürümü tarafından sağlanan erişim anahtarını kullanın.  Ayrıca bakınız [Bilişsel Hizmetler Fiyatlandırma - Bing Arama API.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="code-scenario"></a>Kod senaryosu

Aşağıdaki kod tanımlar ve isteği gönderir. Aşağıdaki adımları izler:

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

* [Yerel İş Arama sıcağınızda](local-quickstart.md)
* [Yerel İş Arama Java quickstart](local-search-java-quickstart.md)
* [Yerel İş Arama Python quickstart](local-search-python-quickstart.md)
