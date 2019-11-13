---
title: Kapsayıcı depoları ve görüntüleri
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Tüm bilişsel hizmet teklifleri için kapsayıcı kayıt defterleri, depolar ve görüntü adları temsil eden iki tablo.
ms.service: cognitive-services
ms.topic: include
ms.date: 11/11/2019
ms.author: dapine
ms.openlocfilehash: ae11fe785237a02652af1f16aaba1f1bac5ecd62
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73961897"
---
### <a name="container-repositories-and-images"></a>Kapsayıcı depoları ve görüntüleri

Aşağıdaki tablolar, Azure bilişsel hizmetler tarafından sunulan kullanılabilir kapsayıcı görüntülerinin kapsamlı bir listesidir.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>Ortak "ungated" (kapsayıcı kayıt defteri: `mcr.microsoft.com`)

Microsoft Container Registry bilişsel hizmetler için genel kullanıma açık olan "geçitli" kapsayıcıları barındırır.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Ortak "geçişli" Önizleme (kapsayıcı kayıt defteri: `containerpreview.azurecr.io`)

Kapsayıcı Önizleme kayıt defteri, bilişsel hizmetler için genel kullanıma açık olan "geçitli" kapsayıcıları barındırır. Bu kapsayıcılar, bunları kullanabilmesi için bir erişim için resmi bir istek gerektirir.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Anomali Algılayıcısı ](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomali Algılayıcısı | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Yüz tanıma](../../face/face-how-to-install-containers.md) | Yüz | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=stt) | Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=cstt) | Özel Konuşma Tanıma metin | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=tts) | Metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=ctts) | Özel metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
