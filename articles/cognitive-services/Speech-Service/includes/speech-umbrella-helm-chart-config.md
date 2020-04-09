---
title: Konuşma kapsayıcılarını yükleme
titleSuffix: Azure Cognitive Services
description: Konuşma şemsiyesi dümen grafiği yapılandırma seçeneklerini ayrıntılarıyla anlatır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 0257f3af44cc85d0a3656472db224ae5a7e19161
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874372"
---
### <a name="speech-umbrella-chart"></a>Konuşma (şemsiye grafiği)

Üst düzey "şemsiye" grafiğindeki değerler karşılık gelen alt grafik değerlerini geçersiz kılar. Bu nedenle, tüm şirket içi özelleştirilmiş değerler buraya eklenmelidir.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- | -- |
| `speechToText.enabled` | **Konuşmadan metne** hizmetin etkin olup olmadığı. | `true` |
| `speechToText.verification.enabled` | `helm test` **Konuşmadan metne** hizmet olanağının etkin olup olmadığı. | `true` |
| `speechToText.verification.image.registry` | **Konuşmadan metne** hizmeti `helm test` test etmek için kullanan docker görüntü deposu. Miğfer, test için kümenin içinde ayrı bir bölme oluşturur ve *test kullanım* görüntüsünü bu kayıt defterinden çeker. | `docker.io` |
| `speechToText.verification.image.repository` | **Konuşmadan metne** hizmeti `helm test` test etmek için kullanan docker görüntü deposu. Miğfer test pod *test kullanımı* görüntü çekmek için bu deposu kullanır. | `antsu/on-prem-client` |
| `speechToText.verification.image.tag` | **Konuşmadan metne** `helm test` hizmet için kullanılan docker resim etiketi. Miğfer test pod *test kullanımı* görüntü çekmek için bu etiketi kullanır. | `latest` |
| `speechToText.verification.image.pullByHash` | Test *kullanım* docker görüntü karma tarafından çekilen olup olmadığını. Eğer `true` `speechToText.verification.image.hash` , geçerli görüntü karma değeri ile eklenmelidir. | `false` |
| `speechToText.verification.image.arguments` | *Test kullanım* docker görüntü yürütmek için kullanılan bağımsız değişkenler. Miğfer test pod çalışırken `helm test`kapsayıcı ya da bu bağımsız değişkenleri geçer. | `"./speech-to-text-client"`<br/> `"./audio/whatstheweatherlike.wav"` <br/> `"--expect=What's the weather like"`<br/>`"--host=$(SPEECH_TO_TEXT_HOST)"`<br/>`"--port=$(SPEECH_TO_TEXT_PORT)"` |
| `textToSpeech.enabled` | **Metinden konuşmaya** hizmetin etkin olup olmadığı. | `true` |
| `textToSpeech.verification.enabled` | `helm test` **Konuşmadan metne** hizmet olanağının etkin olup olmadığı. | `true` |
| `textToSpeech.verification.image.registry` | **Konuşmadan metne** hizmeti `helm test` test etmek için kullanan docker görüntü deposu. Miğfer, test için kümenin içinde ayrı bir bölme oluşturur ve *test kullanım* görüntüsünü bu kayıt defterinden çeker. | `docker.io` |
| `textToSpeech.verification.image.repository` | **Konuşmadan metne** hizmeti `helm test` test etmek için kullanan docker görüntü deposu. Miğfer test pod *test kullanımı* görüntü çekmek için bu deposu kullanır. | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | **Konuşmadan metne** `helm test` hizmet için kullanılan docker resim etiketi. Miğfer test pod *test kullanımı* görüntü çekmek için bu etiketi kullanır. | `latest` |
| `textToSpeech.verification.image.pullByHash` | Test *kullanım* docker görüntü karma tarafından çekilen olup olmadığını. Eğer `true` `textToSpeech.verification.image.hash` , geçerli görüntü karma değeri ile eklenmelidir. | `false` |
| `textToSpeech.verification.image.arguments` | *Test kullanan* docker görüntüsüyle yürütülecek bağımsız değişkenler. Dümen test pod çalışırken `helm test`kapsayıcı bu bağımsız değişkenler geçer. | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |