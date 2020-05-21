---
title: 'Hızlı Başlangıç: Küçük resim oluşturma - REST, Node.js'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js ile Görüntü İşleme API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5c3a6ba009771a879694fe869862f2c3e5836114
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83685084"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-nodejs"></a>Hızlı başlangıç: Görüntü İşleme REST API ve Node. js kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, Görüntü İşleme REST API kullanarak bir görüntüden küçük resim oluşturacaksınız. [Küçük resim al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yöntemiyle bir görüntünün küçük resmini oluşturabilirsiniz. Giriş görüntüsünün en boy oranından farklı olabilen bir yükseklik ve genişlik belirtirsiniz. Görüntü İşleme, ilgilendiğiniz alanı saptamak ve bu bölgeye göre kırpma koordinatları oluşturmak için akıllı kırpma kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- [Node.js](https://nodejs.org) 4.x veya sonraki bir sürüm yüklenmiş olmalıdır.
- [Npm](https://www.npmjs.com/) yüklenmiş olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)'den ücretsiz bir deneme anahtarı edinebilirsiniz. Ya da Görüntü İşleme abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Ardından, sırasıyla ve olarak adlandırılan anahtar ve hizmet uç noktası dizesi için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. NPM paketini yükler [`request`](https://www.npmjs.com/package/request) .
   1. Yönetici olarak bir komut istemi penceresini açın.
   1. Şu komutu çalıştırın:

      ```console
      npm install request
      ```

   1. Paket başarıyla yüklendikten sonra komut istemi penceresini kapatın.

1. Aşağıdaki kodu bir metin düzenleyicisine kopyalayın.
1. İsteğe bağlı olarak `imageUrl` değerini, analiz etmek istediğiniz başka bir görüntünün URL’si ile değiştirin.
1. Kodu, `.js` uzantısıyla bir dosya olarak kaydedin. Örneğin, `get-thumbnail.js`.
1. Bir komut istemi penceresi açın.
1. İstemde, dosyayı çalıştırmak için `node` komutunu kullanın. Örneğin, `node get-thumbnail.js`.

```javascript
'use strict';

const fs = require('fs');
const request = require('request').defaults({ encoding: null });

let subscriptionKey = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
let endpoint = process.env['COMPUTER_VISION_ENDPOINT']

var uriBase = endpoint + 'vision/v3.0/generateThumbnail';

const imageUrl = 'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
};

// Construct the request
const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
}

// Post the request and get the response (an image stream)
request.post(options, (error, response, body) => {
    // Write the stream to file
    var buf = Buffer.from(body, 'base64');
    fs.writeFile('thumbnail.png', buf, function (err) {
        if (err) throw err;
    });

    console.log('Image saved')
});
```

## <a name="examine-the-response"></a>Yanıtı inceleme

Küçük resmin bir açılır penceresi görüntülenir.
Küçük resmin görüntü verilerini temsil eden ikili veri halinde başarılı bir yanıt döndürülür. İstek başarısız olursa, yanıt konsol penceresinde görüntülenir. Başarısız isteğin yanıtı, neyin yanlış gittiğini belirlemeye yardımcı olması için bir hata kodu ve bir ileti içerir.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, bir görüntüyü çözümlemek, ünlüler ve yer işaretlerini saptamak, küçük resim oluşturmak ve yazdırılmış ve el yazısı metin ayıklamak için kullanılan Görüntü İşleme API'si keşfeder.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
