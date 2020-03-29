---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: a1de313245e826511a7e9c864165ac44bcb48530
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234172"
---
### <a name="standard-and-neural-voices"></a>Standart ve sinirsel sesler

Standart ve nöral seslerin bölgeye/uç noktaya göre kullanılabilirliğini belirlemek için bu tabloyu kullanın:

| Bölge | Uç Nokta | Standart Sesler | Nöral Sesler |
|--------|----------|-----------------|---------------|
| Doğu Avustralya | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Güney Brezilya | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Orta Kanada | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Orta ABD | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Doğu Asya | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Doğu ABD | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Doğu ABD 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Orta Fransa | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Hindistan Orta | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Doğu Japonya | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Batı Japonya | `https://japanwest.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Güney Kore - Orta | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Orta Kuzey ABD | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Kuzey Avrupa | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Orta Güney ABD | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Güneydoğu Asya | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Güney Birleşik Krallık | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Batı Avrupa | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Batı ABD | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Batı ABD 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |

### <a name="custom-voices"></a>Özel sesler

Özel bir ses yazı tipi oluşturduysanız, oluşturduğunuz bitiş noktasını kullanın. Ses modelinizin dağıtım kimliğini değiştirerek `{deploymentId}` aşağıda listelenen uç noktaları da kullanabilirsiniz.

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
