---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 029cb1e59876eb01c609ef8b7ba0412ed35a2a1b
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940761"
---
Bu hızlı başlangıçta, konuşma hizmeti ve kıvrımlı kullanarak metin okumayı nasıl dönüştürebileceğinizi öğreneceksiniz.

Metin okuma kavramlarının üst düzey bir görünümü için bkz. [genel bakış](../../../text-to-speech.md) makalesi.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="convert-text-to-speech"></a>Metni konuşmaya Dönüştür

Komut isteminde aşağıdaki komutu çalıştırın. Komutuna aşağıdaki değerleri eklemeniz gerekir.
- Konuşma hizmeti abonelik anahtarınız.
- Konuşma hizmeti bölgeniz.

Ayrıca, aşağıdaki değerleri değiştirmek isteyebilirsiniz.
- `X-Microsoft-OutputFormat`Ses çıkış biçimini denetleyen üst bilgi değeri. Desteklenen ses çıkış biçimlerinin bir listesini, [metinden konuşmaya REST API başvurusunda](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech#audio-outputs)bulabilirsiniz.
- Çıktı sesi. Konuşma uç noktanız için kullanılabilen seslerin listesini almak için sonraki bölüme bakın.
- Çıkış dosyası. Bu örnekte, sunucusundan gelen yanıtı adlı bir dosyaya yönlendirdik `output.wav` .

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/text-to-speech.sh":::

## <a name="list-available-voices-for-your-speech-endpoint"></a>Konuşma uç noktanız için kullanılabilir seslerinizi listeleyin

Konuşma uç noktanız için kullanılabilir seslerinizi listelemek için aşağıdaki komutu çalıştırın.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/get-voices.sh" id="request":::

Aşağıdakine benzer bir yanıt almanız gerekir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/get-voices.sh" id="response":::
