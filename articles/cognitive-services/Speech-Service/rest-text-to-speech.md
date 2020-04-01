---
title: Metinden konuşmaya API başvurusu (REST) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Metinden konuşmaya REST API'yi nasıl kullanacağınızı öğrenin. Bu makalede, yetkilendirme seçenekleri, sorgu seçenekleri, bir isteği yapılandırma ve yanıt alma hakkında bilgi edineceksiniz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: dapine
ms.openlocfilehash: 977c6ec9aa1cd6a8b8a545d123c5308bb8c51651
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409015"
---
# <a name="text-to-speech-rest-api"></a>Metin okuma REST API'si

Konuşma hizmeti, [metni sentezlenmiş konuşmaya dönüştürmenize](#convert-text-to-speech) ve bir bölge için [desteklenen seslerin bir listesini](#get-a-list-of-voices) rest API'leri kullanarak almanıza olanak tanır. Kullanılabilir her uç nokta bir bölgeyle ilişkilidir. Kullanmayı planladığınız bitiş noktası/bölge için bir abonelik anahtarı gereklidir.

Metinden konuşmaya REST API, her biri yerel olarak tanımlanan belirli bir dili ve lehçeyi destekleyen nöral ve standart metinden konuşmaya sesleri destekler.

* Seslerin tam listesi için [dil desteğine](language-support.md#text-to-speech)bakın.
* Bölgesel kullanılabilirlik hakkında bilgi için [bölgelere](regions.md#text-to-speech)bakın.

> [!IMPORTANT]
> Maliyetler standart, özel ve sinirsel seslere göre değişir. Daha fazla bilgi için [Fiyatlandırma'ya](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)bakın.

Bu API'yi kullanmadan önce şunları anlayın:

* Metinden konuşmaya REST API bir Yetkilendirme üstbilgigerektirir. Bu, hizmete erişmek için bir belirteç değişimini tamamlamanız gerektiği anlamına gelir. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="get-a-list-of-voices"></a>Seslerin listesini alın

Bitiş `voices/list` noktası, belirli bir bölge/bitiş noktası için seslerin tam listesini almanızı sağlar.

### <a name="regions-and-endpoints"></a>Bölgeler ve uç noktalar

| Bölge | Uç Nokta |
|--------|----------|
| Doğu Avustralya | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Güney Brezilya | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Orta Kanada | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Orta ABD | `https://centralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Doğu Asya | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Doğu ABD | `https://eastus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Doğu ABD 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Orta Fransa | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Hindistan Orta | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Doğu Japonya | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Güney Kore - Orta | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Orta Kuzey ABD | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Kuzey Avrupa | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Orta Güney ABD | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Güneydoğu Asya | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Güney Birleşik Krallık | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Batı Avrupa | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Batı ABD | `https://westus.tts.speech.microsoft.com/cognitiveservices/voices/list` |
| Batı ABD 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/voices/list` |

### <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, metinden konuşmaya istekleri için gerekli ve isteğe bağlı üstbilgi listelerini listeler.

| Üst bilgi | Açıklama | Gerekli / İsteğe Bağlı |
|--------|-------------|---------------------|
| `Authorization` | Sözcüğün `Bearer`önünde ki bir yetkilendirme belirteci. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Gerekli |

### <a name="request-body"></a>İstek gövdesi

Bu uç noktaya istekler için `GET` gövde gerekmez.

### <a name="sample-request"></a>Örnek istek

Bu istek yalnızca bir yetkilendirme üstbilgigerektirir.

```http
GET /cognitiveservices/voices/list HTTP/1.1

Host: westus.tts.speech.microsoft.com
Authorization: Bearer [Base64 access_token]
```

### <a name="sample-response"></a>Örnek yanıt

Bu yanıt, yanıtın yapısını göstermek için kesildi.

> [!NOTE]
> Ses kullanılabilirliği bölgeye/bitiş noktasına göre değişir.

```json
[
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)",
        "ShortName": "ar-EG-Hoda",
        "Gender": "Female",
        "Locale": "ar-EG",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)",
        "ShortName": "ar-SA-Naayf",
        "Gender": "Male",
        "Locale": "ar-SA",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)",
        "ShortName": "bg-BG-Ivan",
        "Gender": "Male",
        "Locale": "bg-BG",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)",
        "ShortName": "ca-ES-HerenaRUS",
        "Gender": "Female",
        "Locale": "ca-ES",
        "SampleRateHertz": "16000",
        "VoiceType": "Standard"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)",
        "ShortName": "zh-CN-XiaoxiaoNeural",
        "Gender": "Female",
        "Locale": "zh-CN",
        "SampleRateHertz": "24000",
        "VoiceType": "Neural"
    },

    ...
]
```

### <a name="http-status-codes"></a>HTTP durum kodu

Her yanıt ın HTTP durum kodu başarıyı veya yaygın hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| 200 | Tamam | İstek başarılı oldu. |
| 400 | Hatalı İstek | Gerekli bir parametre eksik, boş veya null. Veya, gerekli veya isteğe bağlı parametreye geçirilen değer geçersizdir. Yaygın bir sorun çok uzun bir üstbilgidir. |
| 401 | Yetkisiz | İstek yetkili değil. Abonelik anahtarınızın veya belirteçlerinizin geçerli olduğundan ve doğru bölgede olduğundan emin olun. |
| 429 | Çok Fazla İstek | Aboneliğiniz için izin verilen istek kotasını veya oranını aştınız. |
| 502 | Kötü Ağ Geçidi    | Ağ veya sunucu tarafı sorunu. Geçersiz üstbilgi de gösterebilir. |


## <a name="convert-text-to-speech"></a>Metin okumayı dönüştürme

Bitiş `v1` noktası, [Konuşma Sentezi İşaretleme Dili 'ni (SSML)](speech-synthesis-markup.md)kullanarak metinden konuşmaya dönüştürmenizi sağlar.

### <a name="regions-and-endpoints"></a>Bölgeler ve uç noktalar

Bu bölgeler, REST API'si kullanılarak metinden konuşmaya desteklenir. Abonelik bölgenizle eşleşen bitiş noktasını seçtiğinizden emin olun.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

### <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, metinden konuşmaya istekleri için gerekli ve isteğe bağlı üstbilgi listelerini listeler.

| Üst bilgi | Açıklama | Gerekli / İsteğe Bağlı |
|--------|-------------|---------------------|
| `Authorization` | Sözcüğün `Bearer`önünde ki bir yetkilendirme belirteci. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Gerekli |
| `Content-Type` | Sağlanan metnin içerik türünü belirtir. Kabul edilen `application/ssml+xml`değer: . | Gerekli |
| `X-Microsoft-OutputFormat` | Ses çıkış biçimini belirtir. Kabul edilen değerlerin tam listesi için [ses çıktılarına](#audio-outputs)bakın. | Gerekli |
| `User-Agent` | Uygulama adı. Sağlanan değer 255 karakterden az olmalıdır. | Gerekli |

### <a name="audio-outputs"></a>Ses çıkışları

Bu, her istekte üstbilgi olarak gönderilen desteklenen ses `X-Microsoft-OutputFormat` biçimlerinin listesidir. Her bir bitrate ve kodlama türü içerir. Konuşma hizmeti 24 kHz, 16 kHz ve 8 kHz ses çıkışını destekler.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-alaw` | `riff-8khz-8bit-mono-mulaw` |
| `riff-16khz-16bit-mono-pcm` | `audio-16khz-128kbitrate-mono-mp3` |
| `audio-16khz-64kbitrate-mono-mp3` | `audio-16khz-32kbitrate-mono-mp3` |
| `raw-24khz-16bit-mono-pcm` | `riff-24khz-16bit-mono-pcm` |
| `audio-24khz-160kbitrate-mono-mp3` | `audio-24khz-96kbitrate-mono-mp3` |
| `audio-24khz-48kbitrate-mono-mp3` | |

> [!NOTE]
> Seçtiğiniz ses ve çıkış biçimi farklı bit hızları varsa, ses gerektiğinde yeniden örneklenir. Ancak, 24 kHz sesler `audio-16khz-16kbps-mono-siren` `riff-16khz-16kbps-mono-siren` destek ve çıkış biçimleri yok.

### <a name="request-body"></a>İstek gövdesi

Her `POST` isteğin gövdesi [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)olarak gönderilir. SSML, metinden konuşmaya hizmeti tarafından döndürülen sentezlenmiş konuşmanın sesini ve dilini seçmenize olanak tanır. Desteklenen seslerin tam listesi için [dil desteğine](language-support.md#text-to-speech)bakın.

> [!NOTE]
> Özel bir ses kullanıyorsanız, bir isteğin gövdesi düz metin (ASCII veya UTF-8) olarak gönderilebilir.

### <a name="sample-request"></a>Örnek istek

Bu HTTP isteği, sesi ve dili belirtmek için SSML'yi kullanır. Vücut uzunluğu uzunsa ve ortaya çıkan ses 10 dakikayı aşarsa - 10 dakikaya kadar kesilir. Başka bir deyişle, ses uzunluğu 10 dakikayı geçemez.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='en-US-AriaRUS'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

Dile özel örnekler için hızlı başlangıçlarımıza bakın:

* [.NET Çekirdek, C #](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=dotnetcore)
* [Python](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-python)
* [Node.js](quickstart-nodejs-text-to-speech.md)

### <a name="http-status-codes"></a>HTTP durum kodu

Her yanıt ın HTTP durum kodu başarıyı veya yaygın hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| 200 | Tamam | İstek başarılı oldu; yanıt gövdesi bir ses dosyasıdır. |
| 400 | Hatalı İstek | Gerekli bir parametre eksik, boş veya null. Veya, gerekli veya isteğe bağlı parametreye geçirilen değer geçersizdir. Yaygın bir sorun çok uzun bir üstbilgidir. |
| 401 | Yetkisiz | İstek yetkili değil. Abonelik anahtarınızın veya belirteçlerinizin geçerli olduğundan ve doğru bölgede olduğundan emin olun. |
| 413 | Varlık Çok Büyük İstek | SSML girişi 1024 karakterden daha uzundur. |
| 415 | Desteklenmeyen Ortam Türü | Yanlış ın `Content-Type` sağlanmış olması mümkün. `Content-Type``application/ssml+xml`olarak ayarlanmalıdır. |
| 429 | Çok Fazla İstek | Aboneliğiniz için izin verilen istek kotasını veya oranını aştınız. |
| 502 | Kötü Ağ Geçidi    | Ağ veya sunucu tarafı sorunu. Geçersiz üstbilgi de gösterebilir. |

HTTP durumu ise, `200 OK`yanıtın gövdesi istenen biçimde bir ses dosyası içerir. Bu dosya aktarılırken, arabelleğe kaydedilirken veya bir dosyaya kaydedilirken çalınabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services)
- [Uzun formlu ses için asenkron sentez](quickstarts/text-to-speech/async-synthesis-long-form-audio.md)
- [Özel Ses’i kullanmaya başlama](how-to-custom-voice.md)
