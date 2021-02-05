---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: df6f7311613057c445ae714b8b11240d0d5be14b
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99569466"
---
### <a name="standard-and-neural-voices"></a>Standart ve sinir sesleri

Bölgeye/uç noktaya göre standart ve sinir sesinden kullanılabilirliği öğrenmek için bu tabloyu kullanın:

| Region | Uç Nokta | Standart sesler | Sinir sesleri |
|--------|----------|-----------------|---------------|
| Doğu Avustralya | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Güney Brezilya | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Orta Kanada | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Central US | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Doğu Asya | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Doğu ABD | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Doğu ABD 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Orta Fransa | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Hindistan Orta | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Doğu Japonya | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Batı Japonya | `https://japanwest.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Güney Kore - Orta | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Orta Kuzey ABD | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Kuzey Avrupa | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Orta Güney ABD | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Güneydoğu Asya | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Güney Birleşik Krallık | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| West Europe | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Batı ABD | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Batı ABD 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |

> [!TIP]
> [Önizlemede bulunan sesler](../articles/cognitive-services/Speech-Service/language-support.md#neural-voices-in-preview) yalnızca şu 3 bölgede kullanılabilir: Doğu ABD, Batı Avrupa ve Güneydoğu Asya.

### <a name="custom-voices"></a>Özel sesler

Özel bir ses yazı tipi oluşturduysanız, oluşturduğunuz uç noktayı kullanın. Aşağıda listelenen uç noktaları, `{deploymentId}` Ses modelinize ait DAĞıTıM kimliği ile değiştirerek de kullanabilirsiniz.

| Region | Uç Nokta |
|--------|----------|
| Doğu Avustralya | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Güney Brezilya | `https://brazilsouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Kanada | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Central US | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu Asya | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu ABD | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu ABD 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Fransa | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Hindistan Orta | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu Japonya | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı Japonya | `https://japanwest.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Güney Kore - Orta | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Kuzey ABD | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Kuzey Avrupa | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Güney ABD | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Güneydoğu Asya | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Güney Birleşik Krallık | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| West Europe | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı ABD | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı ABD 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |

### <a name="custom-neural-voice"></a>Özel sinir sesi

Aşağıdaki tabloda, özel sinir ses özellikleri için bölgesel destek ayrıntıları verilmiştir.

| Öne çıkan özelliği | Desteklenen bölgeler |
|---|---|
| Ses modeli barındırma | Doğu ABD, Batı ABD 2, Orta Güney ABD, Güneydoğu Asya, UK Güney, Batı Avrupa Avustralya Doğu |
| Gerçek zamanlı karakterler | Doğu ABD, Batı ABD 2, Orta Güney ABD, Güneydoğu Asya, UK Güney, Batı Avrupa Avustralya Doğu |
| Uzun ses karakterleri | Doğu ABD, Batı Avrupa, UK Güney, Güneydoğu Asya, Hindistan Orta |
| Özel sinir eğitimi | Doğu ABD, UK Güney |
