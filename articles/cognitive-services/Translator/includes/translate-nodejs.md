---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: db48917bfb219a85dcaf0075d0dc7072858a9e27
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69906730"
---
[!INCLUDE [Prerequisites](prerequisites-nodejs.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

En sevdiğiniz IDE veya düzenleyiciyi veya masaüstünüzde bir `translate-text.js` dosya içeren yeni bir klasör kullanarak yeni bir proje oluşturun. Ardından bu kod parçacıklarını projenize/dosyanıza kopyalayın:

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request uuidv4`.

Bu modüller HTTP isteği ve `'X-ClientTraceId'` üst bilgisi için benzersiz tanıtıcı oluşturmak için gereklidir.

## <a name="set-the-subscription-key-and-endpoint"></a>Abonelik anahtarını ve bitiş noktasını ayarlama

Bu örnek, çevirmen metin abonelik anahtarınızı ve bitiş noktanızı `TRANSLATOR_TEXT_SUBSCRIPTION_KEY` `TRANSLATOR_TEXT_ENDPOINT`bu ortam değişkenlerinden okumaya çalışır: ve . Ortam değişkenlerini bilmiyorsanız, koşullu ifadeleri `subscriptionKey` ayarlayabilir ve `endpoint` dizeleri olarak yorumlayabilirsiniz.

Bu kodu projenize kopyalayın:

```javascript
var key_var = 'TRANSLATOR_TEXT_SUBSCRIPTION_KEY';
if (!process.env[key_var]) {
    throw new Error('Please set/export the following environment variable: ' + key_var);
}
var subscriptionKey = process.env[key_var];
var endpoint_var = 'TRANSLATOR_TEXT_ENDPOINT';
if (!process.env[endpoint_var]) {
    throw new Error('Please set/export the following environment variable: ' + endpoint_var);
}
var endpoint = process.env[endpoint_var];
```

## <a name="configure-the-request"></a>İsteği yapılandırma

İstek modülü aracılığıyla kullanıma sunulan `request()` yöntemi HTTP yöntemi, URL, istek parametreleri, üst bilgileri ve JSON gövdesi bileşenlerini `options` nesnesi olarak geçirmemizi sağlar. Bu kod parçacığında isteği yapılandıracağız:

>[!NOTE]
> Uç noktalar, rotalar ve istek parametreleri hakkında daha fazla bilgi için bkz. [Translator Metin Çevirisi API’si 3.0: Çeviri](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate).

```javascript
let options = {
    method: 'POST',
    baseUrl: endpoint,
    url: 'translate',
    qs: {
      'api-version': '3.0',
      'to': ['de', 'it']
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'Hello World!'
    }],
    json: true,
};
```

Bir isteğin kimliğini doğrulamanın en kolay yolu, abonelik anahtarınızda bir `Ocp-Apim-Subscription-Key` üst bilgisi olarak geçirmektir. Bu örnekte bu yöntem kullanılır. Alternatif olarak, abonelik anahtarınızı bir erişim belirteciyle değiştirebilir ve isteğinizi doğrulamak için erişim belirtecini bir `Authorization` üst bilgisi olarak geçirebilirsiniz.

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek `Ocp-Apim-Subscription-Region` üstbilginize de eklemeniz gerekir.

Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>İstekte bulunma ve yanıtı yazdırma

Ardından, `request()` yöntemini kullanarak isteği oluşturacağız. Bu istek ilk bağımsız değişken olarak bir önceki bölümde oluşturduğumuz `options` nesnesini alır ve düzeltilmiş JSON yanıtını yazdırır.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> Bu örnekte HTTP isteğini `options` nesnesinde tanımlıyoruz. Ancak istek modülü `.post` ve `.get` gibi kullanışlı yöntemleri de destekler. Daha fazla bilgi için bkz. [kullanışlı yöntemler](https://github.com/request/request#convenience-methods).

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
node translate-text.js
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "text": "Hallo Welt!",
                "to": "de"
            },
            {
                "text": "Salve, mondo!",
                "to": "it"
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
