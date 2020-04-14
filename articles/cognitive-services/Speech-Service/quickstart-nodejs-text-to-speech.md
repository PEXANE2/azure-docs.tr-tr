---
title: 'Quickstart: Metinden konuşmaya dönüştürme, Düğüm.js - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Düğüm.js ve Metinden Konuşmaya REST API'yi kullanarak metinden konuşmaya nasıl dönüştüreceğinizi öğreneceksiniz. Bu kılavuzda yer alan örnek metin Konuşma Sentezi Biçimlendirme Dili (SSML) olarak yapılandırılmıştır. Bu, konuşma yanıtının sesini ve dilini seçmenize olanak tanır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/13/2020
ms.author: erhopf
ms.openlocfilehash: 8cf9641a1b7a5d1aada13522d612458d5032f883
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258703"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Quickstart: Düğüm.js kullanarak metin den konuşmaya dönüştürme

Bu hızlı başlangıçta, Düğüm.js ve metinden konuşmaya REST API'yi kullanarak metinden konuşmaya nasıl dönüştüreceğinizi öğreneceksiniz. Bu kılavuzdaki istek gövdesi, yanıtın sesini ve dilini seçmenize olanak tanıyan [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)olarak yapılandırılmıştır.

Bu hızlı başlatma, Konuşma hizmeti kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* <a href="https://nodejs.org/en/" target="_blank">Düğüm 8.12.x veya sonrası<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://visualstudio.microsoft.com/downloads/" target="_blank">Visual <span class="docon docon-navigate-external x-hidden-focus"> </span>Studio </a>, <a href="https://code.visualstudio.com/download" target="_blank">Visual Studio Code <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>, veya en sevdiğiniz metin editörü
* Konuşma hizmeti için azure abonelik anahtarı. [Ücretsiz bir tane al!](get-started.md)

## <a name="create-a-project-and-require-dependencies"></a>Proje oluşturma ve bağımlılıklar gerektirme

En sevdiğiniz IDE veya düzenleyiciyi kullanarak yeni bir Düğüm.js projesi oluşturun. Ardından bu kod parçacığını projenizde `tts.js` adlı bir dosyaya kopyalayın.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write synthesized speech to a file
const fs = require('fs');
// Requires readline-sync to read command line inputs
const readline = require('readline-sync');
// Requires xmlbuilder to build the SSML body
const xmlbuilder = require('xmlbuilder');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request request-promise xmlbuilder readline-sync`.

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

Bir sonraki bölümde, metinden konuşmaya API'yi çağırmak ve sentezlenen konuşma yanıtını kaydetmek için işlevi oluşturacağız.

## <a name="make-a-request-and-save-the-response"></a>İstekte bulunun ve yanıtı kaydedin

Burada metinden konuşmaya API'ye istek oluşturacak ve konuşma yanıtını kaydedebilirsiniz. Bu örnek, Batı ABD bitiş noktasını kullandığınızı varsayar. Kaynağınız farklı bir bölgeye kayıtlıysa, 'yi `uri`güncelleştirdiğinizden emin olun Daha fazla bilgi için konuşma [hizmet bölgelerine](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech)bakın.

Ardından, istek için gerekli üstbilgi eklemeniz gerekir. Kaynağınızın adıyla `User-Agent` güncelleştirip (Azure portalında bulunan) ve tercih `X-Microsoft-OutputFormat` ettiğiniz ses çıktısına ayarladığınızdan emin olun. Çıktı biçimlerinin tam listesi için [Ses çıktılarına](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)bakın.

Daha sonra Konuşma Sentezi Biçimlendirme Dili (SSML) kullanarak istek gövdesini oluşturun. Bu örnek yapıyı tanımlar ve `text` daha önce oluşturduğunuz girişi kullanır.

>[!NOTE]
> Bu örnek `JessaRUS` ses yazı tipini kullanır. Microsoft tarafından sağlanan seslerin/dillerin tam listesi için [Dil desteğine](language-support.md)bakın.
> Markanız için benzersiz, tanınabilir bir ses oluşturmak istiyorsanız, [bkz.](how-to-customize-voice-font.md)

Son olarak, servise bir istekte bulunacaksınız. İstek başarılı olursa ve 200 durum kodu döndürülürse, `TTSOutput.wav`konuşma yanıtı .

```javascript
// Make sure to update User-Agent with the name of your resource.
// You can also change the voice and output formats. See:
// https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech
function textToSpeech(accessToken, text) {
    // Create the SSML request.
    let xml_body = xmlbuilder.create('speak')
        .att('version', '1.0')
        .att('xml:lang', 'en-us')
        .ele('voice')
        .att('xml:lang', 'en-us')
        .att('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
        .txt(text)
        .end();
    // Convert the XML into a string to send in the TTS request.
    let body = xml_body.toString();

    let options = {
        method: 'POST',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/v1',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'cache-control': 'no-cache',
            'User-Agent': 'YOUR_RESOURCE_NAME',
            'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
            'Content-Type': 'application/ssml+xml'
        },
        body: body
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('TTSOutput.wav'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım bir eşzamanlı fonksiyon oluşturmaktır. Bu işlev, abonelik anahtarınızı bir ortam değişkeninden okur, metin ister, bir belirteç alır, isteğin tamamlanmasını bekler, ardından metinden konuşmaya dönüştürür ve sesi .wav olarak kaydeder.

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
    // Prompts the user to input text.
    const text = readline.question('What would you like to convert to speech? ');

    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken, text);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, metinden konuşmaya örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), proje dizininize gidin ve çalıştırın:

```console
node tts.js
```

İstendiğinde, metinden konuşmaya dönüştürmek istediğiniz her şeyi yazın. Başarılı olursa, konuşma dosyası proje klasörünüzde bulunur. En sevdiğiniz medya oynatıcıkullanarak oynayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan gizli bilgileri kaldırdıklarından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Node.js örneklerini keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
* [Özel bir ses oluşturmak için ses örneklerini kaydetme](record-custom-voice-samples.md)
