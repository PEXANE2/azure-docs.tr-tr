---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: a1de313245e826511a7e9c864165ac44bcb48530
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80234172"
---
### <a name="standard-and-neural-voices"></a>Standart ve sinir sesleri

Bölgeye/uç noktaya göre standart ve sinir sesinden kullanılabilirliği öğrenmek için bu tabloyu kullanın:

| Bölge | Uç Nokta | Standart sesler | Sinir sesleri |
|--------|----------|-----------------|---------------|
| Doğu Avustralya | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Güney Brezilya | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Orta Kanada | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Orta ABD | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
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
| Batı Avrupa | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |
| Batı ABD | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Hayır |
| Batı ABD 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Yes | Yes |

### <a name="custom-voices"></a>Özel sesler

Özel bir ses yazı tipi oluşturduysanız, oluşturduğunuz uç noktayı kullanın. Aşağıda listelenen uç noktaları, ses modelinize ait dağıtım KIMLIĞI `{deploymentId}` ile değiştirerek de kullanabilirsiniz.

| Bölge | Uç Nokta |
|--------|----------|
| Doğu Avustralya | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Güney Brezilya | `https://brazilsouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Kanada | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta ABD | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
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
| Batı Avrupa | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı ABD | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı ABD 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
