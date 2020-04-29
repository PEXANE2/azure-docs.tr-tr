---
title: Metin okuma kapsayıcısı uç noktası sorgula
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: b2a621a23a81e4fb4f47e7c99d780211973e30a0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81275444"
---
Kapsayıcı, [REST tabanlı uç nokta API 'leri](../rest-text-to-speech.md)sağlar. Platform, çerçeve ve dil çeşitlemeleri için birçok [örnek kaynak kodu projesi](https://github.com/Azure-Samples/Cognitive-Speech-TTS) mevcuttur.

*Standart metin okuma* kapsayıcında, indirdiğiniz görüntü etiketinin yerel ve sesinden yararlanmalısınız. Örneğin, `latest` etiketini indirdiyseniz varsayılan yerel ayar ve `en-US` `JessaRUS` sestir. Daha `{VOICE_NAME}` sonra bağımsız değişken olur [`en-US-JessaRUS`](../language-support.md#standard-voices). Aşağıda SSML örneğine bakın:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="en-US-JessaRUS">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Ancak, *özel metin okuma* için [özel ses portalından](https://aka.ms/custom-voice-portal) **Ses/model** edinmeniz gerekir. Özel model adı, ses adı ile eşanlamlıdır. **Eğitim** sayfasına gidin ve `{VOICE_NAME}` bağımsız değişken olarak kullanılacak **ses/modeli** kopyalayın.
<br><br>
:::image type="content" source="../media/custom-voice/custom-voice-model-voice-name.png" alt-text="Özel ses modeli-ses adı":::

Aşağıda SSML örneğine bakın:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="custom-voice-model">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Birkaç üstbilgiyi ve bir veri yükünü sağlayan bir HTTP POST isteği oluşturalım. `{VOICE_NAME}` Yer tutucusunu kendi değeri ile değiştirin.

```curl
curl -s -v -X POST http://localhost:5000/speech/synthesize/cognitiveservices/v1 \
 -H 'Accept: audio/*' \
 -H 'Content-Type: application/ssml+xml' \
 -H 'X-Microsoft-OutputFormat: riff-16khz-16bit-mono-pcm' \
 -d '<speak version="1.0" xml:lang="en-US"><voice name="{VOICE_NAME}">This is a test, only a test.</voice></speak>'
```

Şu komut:

* `speech/synthesize/cognitiveservices/v1` Uç nokta IÇIN BIR http post isteği oluşturur.
* Bir `Accept` üst bilgisini belirtir`audio/*`
* `Content-Type` Üst bilgisini belirtir `application/ssml+xml`, daha fazla bilgi için bkz. [İstek gövdesi](../rest-text-to-speech.md#request-body).
* `X-Microsoft-OutputFormat` Üst bilgisini belirtir `riff-16khz-16bit-mono-pcm`, daha fazla seçenek için [ses çıkışı](../rest-text-to-speech.md#audio-outputs)bölümüne bakın.
* Uç noktasına verilen `{VOICE_NAME}` [konuşma birleştirme biçimlendirme dili (SSML)](../speech-synthesis-markup.md) isteğini gönderir.