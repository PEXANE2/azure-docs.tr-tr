---
title: Konuşma kapsayıcılarını yükleme
titleSuffix: Azure Cognitive Services
description: Konuşma şemsiyesi dümen grafiği yapılandırma seçeneklerini ayrıntılarıyla anlatır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: ed64412ccf9d192506fafe546b1ccee7941aa43a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73524093"
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