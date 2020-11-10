---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 8a877e1773431053c5ad7344209076cb868a0ee3
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94424659"
---
Bu hızlı başlangıçta, konuşma hizmeti ve kıvrımlı kullanarak metin okumayı nasıl dönüştürebileceğinizi öğreneceksiniz.

Metin okuma kavramlarının üst düzey bir görünümü için bkz. [genel bakış](../../../text-to-speech.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="convert-text-to-speech"></a>Metni konuşmaya Dönüştür

Komut isteminde aşağıdaki komutu çalıştırın. Komutuna aşağıdaki değerleri eklemeniz gerekir.
- Konuşma hizmeti abonelik anahtarınız.
- Konuşma hizmeti bölgeniz.

Ayrıca, aşağıdaki değerleri değiştirmek isteyebilirsiniz.
- `X-Microsoft-OutputFormat`Ses çıkış biçimini denetleyen üst bilgi değeri. Desteklenen ses çıkış biçimlerinin bir listesini, [metinden konuşmaya REST API başvurusunda](../../../rest-text-to-speech.md#audio-outputs)bulabilirsiniz.
- Çıktı sesi. Konuşma uç noktanız için kullanılabilen seslerin listesini almak için sonraki bölüme bakın.
- Çıkış dosyası. Bu örnekte, sunucusundan gelen yanıtı adlı bir dosyaya yönlendirdik `output.wav` .

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/text-to-speech.sh":::

## <a name="list-available-voices-for-your-speech-endpoint"></a>Konuşma uç noktanız için kullanılabilir seslerinizi listeleyin

Konuşma uç noktanız için kullanılabilir seslerinizi listelemek için aşağıdaki komutu çalıştırın.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/get-voices.sh" id="request":::

Aşağıdakine benzer bir yanıt almanız gerekir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/get-voices.sh" id="response":::