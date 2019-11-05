---
title: Kapsayıcı depoları ve görüntüleri
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Tüm bilişsel hizmet teklifleri için kapsayıcı kayıt defterleri, depolar ve görüntü adları temsil eden iki tablo.
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2019
ms.author: dapine
ms.openlocfilehash: c1593cb3dad7ee1370a66747fa3fe47e93c19957
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499198"
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
| [Ünüzü](../../face/face-how-to-install-containers.md) | Yüz | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
< < < < < < < baş | [Konuşma hizmeti API 'si](../../speech-service/speech-container-howto.md?tab=stt) | Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` | | [Konuşma hizmeti API 'si](../../speech-service/speech-container-howto.md?tab=cstt) | Özel Konuşma Tanıma metin | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` | | [Konuşma hizmeti API 'si](../../speech-service/speech-container-howto.md?tab=tts) | Metinden konuşmaya | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` | | [Konuşma hizmeti API 'si](../../speech-service/speech-container-howto.md?tab=ctts) | Özel metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` | ======= | [Konuşma hizmeti API 'si](../../speech-service/speech-container-howto.md) | Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` | | [Konuşma hizmeti API 'si](../../speech-service/speech-container-howto.md) | Metinden konuşmaya | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` | | [Translator metin çevirisi](../../translator/how-to-install-containers.md) | Translator Metin Çevirisi | `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` |
>>>>>>> refs/uzaktan kumandalar/MicrosoftDocs/Master
