---
title: 'Hızlı başlangıç: metin okuma, Node.js konuşma hizmetini dönüştürme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js ve metinden konuşmaya REST API kullanarak metin okumayı nasıl dönüştürebileceğinizi öğreneceksiniz. Bu kılavuza eklenen örnek metin, konuşma birleştirme biçimlendirme dili (SSML) olarak yapılandırılmıştır. Bu, konuşma yanıtının ses ve dilini seçmenizi sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/13/2020
ms.author: erhopf
ms.custom: devx-track-javascript
ms.openlocfilehash: 38f8df1e721d2e29d4871a05263d9f4559aaedc4
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406818"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Hızlı başlangıç: Node.js kullanarak metin okumayı dönüştürme

Bu hızlı başlangıçta, Node.js ve metinden konuşmaya REST API kullanarak metin okumayı nasıl dönüştürebileceğinizi öğreneceksiniz. Bu kılavuzdaki istek gövdesi [konuşma sen, biçimlendirme dili (SSML)](speech-synthesis-markup.md)olarak yapılandırılmıştır ve bu da yanıtın ses ve dilini seçmenizi sağlar.

Bu hızlı başlangıç, bir konuşma hizmeti kaynağı olan bir Azure bilişsel [Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* <a href="https://nodejs.org/en/" target="_blank">Node 8.12. x veya üzeri<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://visualstudio.microsoft.com/downloads/" target="_blank">Visual Studio <span class="docon docon-navigate-external x-hidden-focus"></span> </a>, <a href="https://code.visualstudio.com/download" target="_blank">Visual Studio Code <span class="docon docon-navigate-external x-hidden-focus"></span> </a>veya sık kullandığınız metin düzenleyiciniz
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz olarak bir tane alın!](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Proje oluşturma ve bağımlılıklar gerektirme

En sevdiğiniz IDE veya düzenleyiciyi kullanarak yeni bir Node.js projesi oluşturun. Ardından bu kod parçacığını projenizde `tts.js` adlı bir dosyaya kopyalayın.

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

Metin okuma REST API kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir Exchange gereklidir. Bu işlev, uç noktasını kullanarak bir erişim belirteci için konuşma hizmeti abonelik anahtarınızı değiştirir `issueToken` .

Bu örnek, konuşma hizmeti aboneliğinizin Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölge kullanıyorsanız, değerini güncelleştirin `uri` . Tam liste için bkz. [bölgeler](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

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
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteciyle kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

Sonraki bölümde, metin okuma API 'sini çağırmak ve sentezlenmiş konuşma yanıtını kaydetmek için işlevi oluşturacağız.

## <a name="make-a-request-and-save-the-response"></a>İstek yapın ve yanıtı kaydedin

Burada, metin okuma API 'sine yönelik isteği derleyeceksiniz ve konuşma yanıtını kaydetmelisiniz. Bu örnek Batı ABD uç noktasını kullandığınızı varsayar. Kaynağınız farklı bir bölgeye kayıtlıysa, ' yi güncelleştirdiğinizden emin olun `uri` . Daha fazla bilgi için bkz. [konuşma hizmeti bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Sonra, istek için gerekli üst bilgileri eklemeniz gerekir. `User-Agent`Kaynağınızın adıyla (Azure Portal bulunan) güncelleştirdiğinizden ve `X-Microsoft-OutputFormat` tercih ettiğiniz ses çıktılarınız için ayarlandığından emin olun. Çıkış biçimlerinin tam listesi için bkz. [Ses çıkışları](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

Sonra, konuşma birleştirme biçimlendirme dili (SSML) kullanarak istek gövdesini oluşturun. Bu örnek yapıyı tanımlar ve `text` daha önce oluşturduğunuz girişi kullanır.

>[!NOTE]
> Bu örnek, `JessaRUS` ses yazı tipini kullanır. Microsoft tarafından sunulan seslerin/dillerin tüm listesi için bkz. [dil desteği](language-support.md).
> Markanız için benzersiz ve tanınabilir bir ses oluşturmaya ilgileniyorsanız, bkz. [özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md).

Son olarak, hizmet için bir istek yaparsınız. İstek başarılı olursa ve 200 durum kodu döndürülürse, konuşma yanıtı olarak yazılır `TTSOutput.wav` .

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

Neredeyse bitti. Son adım zaman uyumsuz bir işlev oluşturmaktır. Bu işlev, bir ortam değişkeninden abonelik anahtarınızı okur, metin ister, belirteç alır, isteğin tamamlanmasını bekler, sonra metni konuşmaya dönüştürür ve sesi bir. WAV olarak kaydeder.

Ortam değişkenlerini tanımıyorsanız veya bir dize olarak abonelik anahtarınız ile test yapmayı tercih ediyorsanız, `process.env.SPEECH_SERVICE_KEY` bir dize olarak abonelik anahtarınızla değiştirin.

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

Bu, metinden konuşmaya örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumu), proje dizininize gidin ve şunu çalıştırın:

```console
node tts.js
```

İstendiğinde, metinden konuşmaya dönüştürmek istediğiniz her şeyi yazın. Başarılı olursa, konuşma dosyası proje klasörünüzde bulunur. En sevdiğiniz Medya yürütücünüzü kullanarak yürütün.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek uygulamanızın kaynak kodundaki tüm gizli bilgileri abonelik anahtarları gibi kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da Node.js örnekleri keşfet](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
* [Özel bir ses oluşturmak için ses örneklerini kaydetme](record-custom-voice-samples.md)
