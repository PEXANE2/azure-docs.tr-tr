---
title: Kapsayıcı depoları ve görüntüleri
services: cognitive-services
author: aahill
manager: nitinme
description: Tüm bilişsel hizmet teklifleri için kapsayıcı kayıt defterleri, depolar ve görüntü adları temsil eden iki tablo.
ms.service: cognitive-services
ms.topic: include
ms.date: 09/03/2020
ms.author: aahi
ms.openlocfilehash: 3f2611c9535b9721cccadf35e56bdd21a3020257
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92677456"
---
### <a name="container-repositories-and-images"></a>Kapsayıcı depoları ve görüntüleri

Aşağıdaki tablolarda, Azure bilişsel hizmetler tarafından sunulan kullanılabilir kapsayıcı görüntülerinin bir listesi verilmiştir. Tüm kullanılabilir kapsayıcı görüntüsü adlarının ve kullanılabilir etiketlerinin tüm listesi için bkz. bilişsel [Hizmetler kapsayıcısı görüntü etiketleri](../container-image-tags.md). 

#### <a name="generally-available"></a>Genel kullanıma sunuldu 

Microsoft Container Registry (MCR), bilişsel hizmetler için genel kullanıma sunulan kapsayıcıları Syndicates. Kapsayıcılar doğrudan [Docker Hub 'ından](https://hub.docker.com/_/microsoft-azure-cognitive-services)de kullanılabilir.

**LUIS**

| Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|
| LUIS | `mcr.microsoft.com/azure-cognitive-services/language/luis` |

Daha fazla bilgi için bkz. [lusıs kapsayıcılarını çalıştırma ve yüklemeyi öğrenin](../../LUIS/luis-container-howto.md) .

**Metin Analizi**

| Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|
| Yaklaşım Analizi v3 (Ingilizce) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-en` |
| Yaklaşım Analizi v3 (Ispanyolca) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-es` |
| Yaklaşım Analizi v3 (Fransızca) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-fr` |
| Yaklaşım Analizi v3 (Italyanca) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-it` |
| Yaklaşım Analizi v3 (Almanca) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-de` |
| Yaklaşım Analizi v3 (Çince-Basitleştirilmiş) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-zh` |
| Yaklaşım Analizi v3 (Çince-Geleneksel) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-zht` |
| Yaklaşım Analizi v3 (Japonca) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-ja` |
| Yaklaşım Analizi v3 (Portekizce) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-pt` |
| Yaklaşım Analizi v3 (Felemenkçe) | `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-nl` |

Daha fazla bilgi için bkz. [metin analizi kapsayıcılarını çalıştırma ve yüklemeyi öğrenin](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) .

**Anomali Algılayıcısı** 

| Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|
| Anomali algılayıcısı | `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector` |

Daha fazla bilgi için bkz. [anomali algılayıcı kapsayıcılarını çalıştırma ve yüklemeyi öğrenin](../../anomaly-detector/anomaly-detector-container-howto.md) .

**Konuşma hizmeti**

> [!NOTE]
> Konuşma kapsayıcılarını kullanmak için bir [çevrimiçi istek formu](https://aka.ms/csgate)doldurmanız gerekir.

| Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|
| [Konuşmayı metne dönüştürme](../../speech-service/speech-container-howto.md?tab=stt) | `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text` |
| [Özel Konuşma Tanıma metin](../../speech-service/speech-container-howto.md?tab=cstt) | `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text` |
| [Metin okuma](../../speech-service/speech-container-howto.md?tab=tts) | `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech` |

#### <a name="ungated-preview"></a>"Geçitli" Önizleme 

Aşağıdaki önizleme kapsayıcıları herkese açık bir şekilde kullanılabilir. Microsoft Container Registry (MCR), bilişsel hizmetler için tüm genel kullanıma açık Kapsayıcılı kapsayıcıları Syndicates. Kapsayıcılar doğrudan [Docker Hub 'ından](https://hub.docker.com/_/microsoft-azure-cognitive-services)de kullanılabilir.

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase` |
| [Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/textanalytics/language` |


#### <a name="gated-preview"></a>"Geçitli" Önizleme

Daha önce, geçitli önizleme kapsayıcıları `containerpreview.azurecr.io` depoda barındırılıyor. 22 2020 ' den itibaren, Bu kapsayıcılar (sistem durumu için metin analizi hariç) Microsoft Container Registry (MCR) üzerinde barındırılır ve indirme işlemi Docker Login komutunun kullanılmasını gerektirmez. Kapsayıcıyı kullanmak için şunları yapmanız gerekir:

1. Azure abonelik KIMLIĞINIZ ve Kullanıcı senaryosuyla bir [istek formunu](https://aka.ms/csgate) doldurun. 
2. Onay sonrasında, Mr 'den kapsayıcıyı indirin. 
3. Çalışma zamanında kapsayıcının kimliğini doğrulamak için uygun bir Azure kaynağından anahtar ve uç noktayı kullanın. 

| Hizmet | Kapsayıcı | Container Registry/depo/görüntü adı |
|--|--|--|
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | V 2.0 'ı oku | `mcr.microsoft.com/azure-cognitive-services/vision/read:2.0-preview` |
| [Görüntü İşleme](../../Computer-vision/computer-vision-how-to-install-containers.md) | V 3.1 'i oku | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview` |
| [Görüntü İşleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/spatial-analysis-container) | Uzamsal Analiz | `mcr.microsoft.com/azure-cognitive-services/vision/spatial-analysis` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=ctts) | Özel metin okuma | `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=lid) | Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection` |
| [Konuşma Hizmeti API’si](../../speech-service/speech-container-howto.md?tab=ntts) | Sinir metin okuma | `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech` |
| [Sistem durumu için Metin Analizi](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=health) | Sistem durumu için Metin Analizi | `containerpreview.azurecr.io/microsoft/cognitive-services-healthcare` |

