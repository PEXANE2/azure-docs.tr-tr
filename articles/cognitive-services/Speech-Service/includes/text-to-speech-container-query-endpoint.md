---
title: Metin okuma kapsayıcısı uç noktası sorgula
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/31/2020
ms.author: aahi
ms.openlocfilehash: 50926074c09ef8f438b0abb53a4eb5f1813871b3
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89321063"
---
Kapsayıcı, [REST tabanlı uç nokta API 'leri](../rest-text-to-speech.md)sağlar. Platform, çerçeve ve dil çeşitlemeleri için birçok [örnek kaynak kodu projesi](https://github.com/Azure-Samples/Cognitive-Speech-TTS) mevcuttur.

Standart veya sinir metin okuma kapsayıcılarıyla, indirdiğiniz görüntü etiketinin yerel ayarına ve sesinize güvenmelisiniz. Örneğin, etiketini indirdiyseniz `latest` varsayılan yerel ayar `en-US` ve `AriaRUS` sestir. `{VOICE_NAME}`Daha sonra bağımsız değişken olur [`en-US-AriaRUS`](../language-support.md#standard-voices) . Aşağıda SSML örneğine bakın:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Ancak, *özel metin okuma* için [özel ses portalından](https://aka.ms/custom-voice-portal) **Ses/model** edinmeniz gerekir. Özel model adı, ses adı ile eşanlamlıdır. **Eğitim** sayfasına gidin ve bağımsız değişken olarak kullanılacak **ses/modeli** kopyalayın `{VOICE_NAME}` .
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

Birkaç üstbilgiyi ve bir veri yükünü sağlayan bir HTTP POST isteği oluşturalım. `{VOICE_NAME}`Yer tutucusunu kendi değeri ile değiştirin.

```curl
curl -s -v -X POST http://localhost:5000/speech/synthesize/cognitiveservices/v1 \
 -H 'Accept: audio/*' \
 -H 'Content-Type: application/ssml+xml' \
 -H 'X-Microsoft-OutputFormat: riff-16khz-16bit-mono-pcm' \
 -d '<speak version="1.0" xml:lang="en-US"><voice name="{VOICE_NAME}">This is a test, only a test.</voice></speak>'
```

Şu komut:

* Uç nokta için bir HTTP POST isteği oluşturur `speech/synthesize/cognitiveservices/v1` .
* Bir `Accept` üst bilgisini belirtir `audio/*`
* `Content-Type`Üst bilgisini belirtir `application/ssml+xml` , daha fazla bilgi için bkz. [İstek gövdesi](../rest-text-to-speech.md#request-body).
* `X-Microsoft-OutputFormat`Üst bilgisini belirtir `riff-16khz-16bit-mono-pcm` , daha fazla seçenek için [ses çıkışı](../rest-text-to-speech.md#audio-outputs)bölümüne bakın.
* Uç noktasına verilen [konuşma birleştirme biçimlendirme dili (SSML)](../speech-synthesis-markup.md) isteğini gönderir `{VOICE_NAME}` .