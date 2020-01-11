---
title: Kapsayıcı depoları ve görüntüleri
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Tüm bilişsel hizmet teklifleri için kapsayıcı kayıt defterleri, depolar ve görüntü adları temsil eden iki tablo.
ms.service: cognitive-services
ms.topic: include
ms.date: 01/10/2020
ms.author: dapine
ms.openlocfilehash: 7c229ea306d89f85fb37a68fc84e3e4f6770e5ad
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866928"
---
### <a name="container-repositories-and-images"></a>Kapsayıcı depoları ve görüntüleri

Aşağıdaki tablolarda, Azure bilişsel hizmetler tarafından sunulan kullanılabilir kapsayıcı görüntülerinin bir listesi verilmiştir. Tüm kullanılabilir kapsayıcı görüntüsü adlarının ve kullanılabilir etiketlerinin tüm listesi için bkz. bilişsel [Hizmetler kapsayıcısı görüntü etiketleri](../container-image-tags.md). Şu anda genel kullanıma açık olan bilişsel hizmet kapsayıcıları yoktur (GA). Bu süre boyunca, daha fazla duyuru yapılıncaya kadar, kapsayıcılar *ortak olmayan* veya *genel geçitli önizleme*olarak kullanılabilir.

 - *Ortak ungated*: kapsayıcılar, bir başarı mekanizması olmadan herkese açık bir şekilde kullanılabilir.
 - *Ortak geçişli önizleme*: kapsayıcılar herkese açık bir şekilde kullanılabilir, ancak önce kapsayıcı kayıt defterine erişmek için biçimsel isteğin olması gerekir.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>Ortak "ungated" (kapsayıcı kayıt defteri: `mcr.microsoft.com`)

Microsoft Container Registry (MCR), bilişsel hizmetler için tüm genel kullanıma açık "olmayan" kapsayıcıları Syndicates. Kapsayıcılar doğrudan [Docker Hub 'ından](https://hub.docker.com/_/microsoft-azure-cognitive-services)de kullanılabilir.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Anahtar Tümcecik Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Ortak "geçişli" Önizleme (kapsayıcı kayıt defteri: `containerpreview.azurecr.io`)

Kapsayıcı Önizleme kayıt defteri, bilişsel hizmetler için genel kullanıma açık olan "geçitli" kapsayıcıları barındırır. Bu kapsayıcılar, kapsayıcı kayıt defteri aracılığıyla bunlara erişmek için resmi bir istek gerektirir.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Anomali algılayıcısı](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomali Algılayıcısı | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Yüz tanıma](../../face/face-how-to-install-containers.md) | Yüz Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=stt) | Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=cstt) | Özel Konuşma Tanıma metin | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=tts) | Metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=ctts) | Özel metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
