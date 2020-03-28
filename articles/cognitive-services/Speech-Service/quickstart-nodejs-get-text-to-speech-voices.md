---
title: 'Hızlı başlangıç: Metinden konuşmaya sesleri, Düğüm.js -Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js kullanarak bir bölge/bitiş noktası için standart ve sinirsel seslerin tam listesini nasıl alacağınızı öğreneceksiniz. Liste JSON olarak döndürülür ve ses kullanılabilirliği bölgeye göre değişir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: erhopf
ms.openlocfilehash: 7a929794ffaea4f863ffaef7227e58c7ccf901f0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74976579"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-nodejs"></a>Quickstart: Düğüm.js kullanarak metinden konuşmaya seslerin listesini alın

Bu hızlı başlangıçta, Node.js kullanarak bir bölge/bitiş noktası için standart ve sinirsel seslerin tam listesini nasıl alacağınızı öğreneceksiniz. Liste JSON olarak döndürülür ve ses kullanılabilirliği bölgeye göre değişir. Desteklenen bölgelerin listesi için [bkz.](regions.md)

Bu hızlı başlatma, Konuşma hizmeti kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* [Node 8.12.x veya üzeri](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin editörü
* Konuşma hizmeti için azure abonelik anahtarı. [Ücretsiz bir tane al!](get-started.md)

## <a name="create-a-project-and-require-dependencies"></a>Proje oluşturma ve bağımlılıklar gerektirme

En sevdiğiniz IDE veya düzenleyiciyi kullanarak yeni bir Düğüm.js projesi oluşturun. Ardından bu kod parçacığını projenizde `get-voices.js` adlı bir dosyaya kopyalayın.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write the list of languages to a file
const fs = require('fs');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request request-promise`.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Metinden konuşmaya REST API kimlik doğrulaması için bir erişim belirteci gerektirir. Bir erişim jetonu almak için bir değişim gereklidir. Bu işlev, bitiş noktasını kullanarak bir erişim belirteci için Konuşma hizmeti abonelik anahtarınızı `issueToken` değiştirir.

Bu örnek, Konuşma hizmeti aboneliğinizin Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölge kullanıyorsanız, `uri`değeri güncelleştirin. Tam liste için [Bölgeler'e](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis)bakın.

Bu kodu projenize kopyalayın:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Kimlik doğrulama hakkında daha fazla bilgi için [erişim belirteciyle Kimlik Doğrulaması'na](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token)bakın.

Bir sonraki bölümde, seslerin listesini almak ve JSON çıktısını dosyaya kaydetmek için işlevi oluşturacağız.

## <a name="make-a-request-and-save-the-response"></a>İstekte bulunun ve yanıtı kaydedin

Burada isteği oluşturup iade edilen seslerin listesini kaydedebilirsiniz. Bu örnek, Batı ABD bitiş noktasını kullandığınızı varsayar. Kaynağınız farklı bir bölgeye kayıtlıysa, 'yi `uri`güncelleştirdiğinizden emin olun Daha fazla bilgi için konuşma [hizmet bölgelerine](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech)bakın.

Ardından, istek için gerekli üstbilgi ekleyin. Son olarak, servise bir istekte bulunacaksınız. İstek başarılı olursa ve 200 durum kodu döndürülürse, yanıt dosyaya yazılır.

```javascript
function textToSpeech(accessToken) {
    let options = {
        method: 'GET',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/voices/list',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json'
        }
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('voices.json'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım bir eşzamanlı fonksiyon oluşturmaktır. Bu işlev, abonelik anahtarınızı bir ortam değişkeninden okuyacak, bir belirteç alacak, isteğin tamamlanmasını bekleyecek ve ardından Dosyaya JSON yanıtını yazacaktır.

Ortam değişkenlerini bilmiyorsanız veya abonelik anahtarınızı dize olarak kodlanmış olarak test `process.env.SPEECH_SERVICE_KEY` etmeyi tercih ediyorsanız, bir dize olarak abonelik anahtarınızla değiştirin.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
node get-voices.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Node.js örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
* [Özel bir ses oluşturmak için ses örneklerini kaydetme](record-custom-voice-samples.md)
