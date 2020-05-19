---
title: Kapsayıcı depoları ve görüntüleri
services: cognitive-services
author: aahill
manager: nitinme
description: Tüm bilişsel hizmet teklifleri için kapsayıcı kayıt defterleri, depolar ve görüntü adları temsil eden iki tablo.
ms.service: cognitive-services
ms.topic: include
ms.date: 04/24/2020
ms.author: aahi
ms.openlocfilehash: 9025e016725a966061c557f16b610d8897c04c11
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83590681"
---
### <a name="container-repositories-and-images"></a>Kapsayıcı depoları ve görüntüleri

Aşağıdaki tablolarda, Azure bilişsel hizmetler tarafından sunulan kullanılabilir kapsayıcı görüntülerinin bir listesi verilmiştir. Tüm kullanılabilir kapsayıcı görüntüsü adlarının ve kullanılabilir etiketlerinin tüm listesi için bkz. bilişsel [Hizmetler kapsayıcısı görüntü etiketleri](../container-image-tags.md). 

#### <a name="generally-available"></a>Genel kullanıma sunuldu 

Microsoft Container Registry (MCR), bilişsel hizmetler için genel kullanıma sunulan kapsayıcıları Syndicates. Kapsayıcılar doğrudan [Docker Hub 'ından](https://hub.docker.com/_/microsoft-azure-cognitive-services)de kullanılabilir.

#### <a name="luis"></a>[LUIS](#tab/luis)

| LUSıS kapsayıcısı | Container Registry/depo/görüntü adı |
|--|--|
| LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |

Daha fazla bilgi için bkz. [lusıs kapsayıcılarını çalıştırma ve yüklemeyi öğrenin](../../LUIS/luis-container-howto.md) .

#### <a name="text-analytics"></a>[Metin Analizi](#tab/text-analytics)

| Metin Analizi kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|
| Yaklaşım Analizi v3 (Ingilizce) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en` |
| Yaklaşım Analizi v3 (Ispanyolca) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-es` |
| Yaklaşım Analizi v3 (Fransızca) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-fr` |
| Yaklaşım Analizi v3 (Italyanca) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-it` |
| Yaklaşım Analizi v3 (Almanca) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-de` |
| Yaklaşım Analizi v3 (Çince-Basitleştirilmiş) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-zh` |
| Yaklaşım Analizi v3 (Çince-Geleneksel) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-zht` |
| Yaklaşım Analizi v3 (Japonca) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-ja` |
| Yaklaşım Analizi v3 (Portekizce) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-pt` |
| Yaklaşım Analizi v3 (Felemenkçe) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-nl` |

Daha fazla bilgi için bkz. [metin analizi kapsayıcılarını çalıştırma ve yüklemeyi öğrenin](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) .

---

#### <a name="public-ungated-preview-container-registry-mcrmicrosoftcom"></a>Ortak "açılmamış" Önizleme (kapsayıcı kayıt defteri: `mcr.microsoft.com` )

Aşağıdaki önizleme kapsayıcıları herkese açık bir şekilde kullanılabilir. Microsoft Container Registry (MCR), bilişsel hizmetler için tüm genel kullanıma açık Kapsayıcılı kapsayıcıları Syndicates. Kapsayıcılar doğrudan [Docker Hub 'ından](https://hub.docker.com/_/microsoft-azure-cognitive-services)de kullanılabilir.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Anomali algılayıcısı](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomali Algılayıcısı | `mcr.microsoft.com/azure-cognitive-services/anomaly-detector` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Ortak "geçişli" Önizleme (kapsayıcı kayıt defteri: `containerpreview.azurecr.io` )

Aşağıdaki geçişli önizleme kapsayıcıları, kapsayıcı önizlemesi kayıt defterinde barındırılır ve bir uygulamanın erişmesini gerektirir. Daha fazla bilgi için bkz. [bilişsel hizmetler geçişi işlemi](../../cognitive-services-gating-process.md) .

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | Okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Yüz Tanıma](../../face/face-how-to-install-containers.md) | Yüz Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=stt) | Konuşmayı metne dönüştürme | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=cstt) | Özel Konuşma Tanıma metin | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=tts) | Metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=ctts) | Özel metin okuma | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
