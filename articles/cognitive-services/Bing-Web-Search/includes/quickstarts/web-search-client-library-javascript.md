---
title: Bing Web Araması JavaScript istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/05/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: 20eaf6e6a9f0eee15e6ad6a5bd8f23bf8531545c
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376454"
---
Bing Web Araması istemci kitaplığı, Bing Web Araması Node.js uygulamanıza tümleştirmeyi kolaylaştırır. Bu hızlı başlangıçta istemci başlatmayı, istek göndermeyi ve yanıtı yazdırmayı öğreneceksiniz.

Kodu hemen görmek istiyor musunuz? [JavaScript için Bing arama istemci kitaplıklarının](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/tree/master/Samples) örnekleri GitHub ' da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı çalıştırmak için aşağıdakilere ihtiyacınız olacaktır:

* [Node.js 6](https://nodejs.org/en/download/) veya üzeri
* Abonelik anahtarı  

[!INCLUDE [bing-web-search-quickstart-signup](~/includes/bing-web-search-quickstart-signup.md)]


## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı ayarlama

Node.js projemiz için geliştirme ortamını ayarlayarak başlayalım.

1. Projeniz için yeni bir dizin oluşturun:

    ```console
    mkdir YOUR_PROJECT
    ```

1. Yeni bir paket dosyası oluşturun:

    ```console
    cd YOUR_PROJECT
    npm init
    ```

1. Şimdi, bazı Azure modüllerini yükleyelim ve bunları içine ekleyelim `package.json` :

    ```console
    npm install --save @azure/cognitiveservices-websearch
    npm install --save @azure/ms-rest-azure-js
    ```

## <a name="create-a-project-and-declare-required-modules"></a>Bir proje oluşturun ve gerekli modülleri bildirin

`package.json` ile aynı dizinde favori IDE ortamınızı veya düzenleyiciyi kullanarak yeni bir Node.js projesi oluşturun. Örneğin: `sample.js`.

Şimdi bu kodu projenize kopyalayın. Önceki bölümde yüklenen modülleri yükler.

```javascript
const CognitiveServicesCredentials = require('@azure/ms-rest-azure-js').CognitiveServicesCredentials;
const WebSearchAPIClient = require('@azure/cognitiveservices-websearch');
```

## <a name="instantiate-the-client"></a>İstemciyi başlatma

Bu kod `@azure/cognitiveservices-websearch` modünü kullanarak bir istemci başlatır. Devam etmeden önce Azure hesabınız için geçerli bir abonelik anahtarı girdiğinizden emin olun.

```javascript
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```

## <a name="make-a-request-and-print-the-results"></a>İstekte bulunma ve sonuçları yazdırma

İstemciyi kullanarak Bing Web Araması'na bir arama sorgusu gönderin. Yanıt `properties` dizisindeki öğeler için sonuç içerirse konsolda `result.value` yazılır.

```javascript
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})
```

## <a name="run-the-program"></a>Programı çalıştırma

Son adım programınızı çalıştırmaktır!

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu projeyi tamamladıktan sonra abonelik anahtarınızı program kodundan kaldırmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel hizmetler Node.js SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Node SDK başvurusu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-websearch/)
