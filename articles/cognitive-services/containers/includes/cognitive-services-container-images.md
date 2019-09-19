---
title: Kapsayıcı depoları ve görüntüleri
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Tüm bilişsel hizmet teklifleri için kapsayıcı kayıt defterleri, depolar ve görüntü adları temsil eden iki tablo.
ms.service: cognitive-services
ms.topic: include
ms.date: 09/06/2019
ms.author: dapine
ms.openlocfilehash: 181f4acd86dfacb15592ded6f2df3287e3dc13bf
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130154"
---
### <a name="container-repositories-and-images"></a>Kapsayıcı depoları ve görüntüleri

Aşağıdaki tablolar, Azure bilişsel hizmetler tarafından sunulan kullanılabilir kapsayıcı görüntülerinin kapsamlı bir listesidir.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>Ortak "ungated" (kapsayıcı kayıt defteri `mcr.microsoft.com`:)

Microsoft Container Registry bilişsel hizmetler için genel kullanıma açık olan "geçitli" kapsayıcıları barındırır.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Ortak "geçişli" Önizleme (kapsayıcı kayıt defteri `containerpreview.azurecr.io`:)

Kapsayıcı Önizleme kayıt defteri, bilişsel hizmetler için genel kullanıma açık olan "geçitli" kapsayıcıları barındırır. Bu kapsayıcılar, bunları kullanabilmesi için bir erişim için resmi bir istek gerektirir.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Anomali algılayıcısı](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomali Algılayıcısı | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | Metin Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text` |
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Yüz tanıma](../../face/face-how-to-install-containers.md) | Yüz | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md) | Konuşmayı Metne Dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md) | Metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
