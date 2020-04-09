---
title: Metinden konuşmaya kapsayıcı bitiş noktasını sorgula
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 6fe07da1e658efa8150c8cafb95bc5719c780aff
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879046"
---
Kapsayıcı [REST tabanlı uç nokta API'leri](../rest-text-to-speech.md)sağlar. Platform, çerçeve ve dil varyasyonları için birçok [örnek kaynak kodu projesi](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/) mevcuttur.

Standart *Metinden konuşmaya* kapsayıcısıyla, indirdiğiniz görüntü etiketinin yerel görüntüsüne ve sesine güvenmelisiniz. Örneğin, `latest` etiketi indirdiyseniz varsayılan yerel alan `en-US` ve `JessaRUS` sestir. Argüman `{VOICE_NAME}` daha sonra [`en-US-JessaRUS`](../language-support.md#standard-voices)olacaktır . Aşağıdaki Örnek SSML'ye bakın:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="en-US-JessaRUS">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Ancak, *Özel Metin-to-speech* için [özel ses portalıses](https://aka.ms/custom-voice-portal) **/ model** elde etmek gerekir. Özel model adı ses adı ile eş anlamlıdır. **Eğitim** sayfasına gidin ve bağımsız değişken olarak kullanmak `{VOICE_NAME}` üzere Ses / **modelini** kopyalayın.
<br><br>
:::image type="content" source="../media/custom-voice/custom-voice-model-voice-name.png" alt-text="Özel ses modeli - ses adı":::

Aşağıdaki Örnek SSML'ye bakın:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="custom-voice-model">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Birkaç üstbilgi ve veri yükü sağlayan bir HTTP POST isteği oluşturalım. Yer `{VOICE_NAME}` tutucuyu kendi değerinizle değiştirin.

```curl
curl -s -v -X POST http://localhost:5000/speech/synthesize/cognitiveservices/v1 \
 -H 'Accept: audio/*' \
 -H 'Content-Type: application/ssml+xml' \
 -H 'X-Microsoft-OutputFormat: riff-16khz-16bit-mono-pcm' \
 -d '<speak version="1.0" xml:lang="en-US"><voice name="{VOICE_NAME}">This is a test, only a test.</voice></speak>'
```

Şu komut:

* `speech/synthesize/cognitiveservices/v1` Bitiş noktası için bir HTTP POST isteği oluşturuyor.
* Üstbilgi `Accept` belirtir`audio/*`
* `Content-Type` Üstbilgi, `application/ssml+xml`daha fazla bilgi için istek [gövdesine](../rest-text-to-speech.md#request-body)bakın.
* Bir `X-Microsoft-OutputFormat` üstbilgi belirtir `riff-16khz-16bit-mono-pcm`, daha fazla seçenek için [ses çıkışı](../rest-text-to-speech.md#audio-outputs)bakın .
* Konuşma [Sentezi Biçimlendirme Dili (SSML)](../speech-synthesis-markup.md) isteğini bitiş noktasına gönderir. `{VOICE_NAME}`