---
title: Konuşma kapsayıcılarını yükler
titleSuffix: Azure Cognitive Services
description: Konuşma şemsiye Held grafik yapılandırma seçeneklerini ayrıntılı olarak ayrıntılardır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: ed64412ccf9d192506fafe546b1ccee7941aa43a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "67717227"
---
### <a name="speech-umbrella-chart"></a>Konuşma (şemsiye grafik)

Üst düzey "şemsiye" grafiğindeki değerler ilgili alt grafik değerlerini geçersiz kılar. Bu nedenle, tüm şirket içi özelleştirilmiş değerler buraya eklenmelidir.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- | -- |
| `speechToText.enabled` | **Konuşmaya metin** hizmetinin etkinleştirilip etkinleştirilmeyeceğini belirtir. | `true` |
| `speechToText.verification.enabled` | **Konuşma-metin** hizmeti özelliğininetkinleştirilipetkinleştirilmeyeceğinibelirtir.`helm test` | `true` |
| `speechToText.verification.image.registry` | **Konuşma-metin** hizmetini test etmek `helm test` için kullanan Docker görüntü deposu. Hela, test için küme içinde ayrı Pod oluşturuyor ve bu kayıt defterinden *test kullanımı* görüntüsünü çeker. | `docker.io` |
| `speechToText.verification.image.repository` | **Konuşma-metin** hizmetini test etmek `helm test` için kullanan Docker görüntü deposu. Helmtest Pod, *sınama kullanımı* görüntüsünü çekmek için bu depoyu kullanır. | `antsu/on-prem-client` |
| `speechToText.verification.image.tag` | `helm test` **Konuşma-metin** hizmeti için ile kullanılan Docker Image etiketi. Helmtest Pod, *sınama kullanımı* görüntüsünü çekmek için bu etiketi kullanır. | `latest` |
| `speechToText.verification.image.pullByHash` | *Test kullanan* Docker görüntüsünün karma tarafından çekilip çekilmediğini belirtir. Varsa `true`,geçerli görüntü karma değeri ile eklenmelidir. `speechToText.verification.image.hash` | `false` |
| `speechToText.verification.image.arguments` | Testi yürütmek için kullanılan bağımsız değişkenler Docker görüntüsünü *kullanın* . Hele test Pod, çalışırken `helm test`bu bağımsız değişkenleri kapsayıcıya geçirir. | `"./speech-to-text-client"`<br/> `"./audio/whatstheweatherlike.wav"` <br/> `"--expect=What's the weather like"`<br/>`"--host=$(SPEECH_TO_TEXT_HOST)"`<br/>`"--port=$(SPEECH_TO_TEXT_PORT)"` |
| `textToSpeech.enabled` | **Metin okuma** hizmetinin etkin olup olmadığı. | `true` |
| `textToSpeech.verification.enabled` | **Konuşma-metin** hizmeti özelliğininetkinleştirilipetkinleştirilmeyeceğinibelirtir.`helm test` | `true` |
| `textToSpeech.verification.image.registry` | **Konuşma-metin** hizmetini test etmek `helm test` için kullanan Docker görüntü deposu. Hela, test için küme içinde ayrı Pod oluşturuyor ve bu kayıt defterinden *test kullanımı* görüntüsünü çeker. | `docker.io` |
| `textToSpeech.verification.image.repository` | **Konuşma-metin** hizmetini test etmek `helm test` için kullanan Docker görüntü deposu. Helmtest Pod, *sınama kullanımı* görüntüsünü çekmek için bu depoyu kullanır. | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | `helm test` **Konuşma-metin** hizmeti için ile kullanılan Docker Image etiketi. Helmtest Pod, *sınama kullanımı* görüntüsünü çekmek için bu etiketi kullanır. | `latest` |
| `textToSpeech.verification.image.pullByHash` | *Test kullanan* Docker görüntüsünün karma tarafından çekilip çekilmediğini belirtir. Varsa `true`,geçerli görüntü karma değeri ile eklenmelidir. `textToSpeech.verification.image.hash` | `false` |
| `textToSpeech.verification.image.arguments` | *Test-* Docker görüntüsünü kullanarak yürütülecek bağımsız değişkenler. Hele test Pod, çalışırken `helm test`bu bağımsız değişkenleri kapsayıcıya geçirir. | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |