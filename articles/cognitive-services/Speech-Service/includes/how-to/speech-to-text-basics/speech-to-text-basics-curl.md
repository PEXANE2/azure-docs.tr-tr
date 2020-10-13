---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 1ec0281145609f9ae06ad07e1ad2cfd2b7f9aba8
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940822"
---
Bu hızlı başlangıçta, konuşma hizmeti ve kıvrımlı kullanarak konuşmayı metne dönüştürmeyi öğreneceksiniz.

Konuşmadan metne yönelik kavramlar hakkında üst düzey bir bakış için bkz. [genel bakış](../../../speech-to-text.md) makalesi.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="convert-text-to-speech"></a>Metni konuşmaya Dönüştür

Komut isteminde aşağıdaki komutu çalıştırın. Komutuna aşağıdaki değerleri eklemeniz gerekir.
- Konuşma hizmeti abonelik anahtarınız.
- Konuşma hizmeti bölgeniz.
- Giriş ses dosyası yolu. [Metin okuma özelliğini](../../../get-started-text-to-speech.md)kullanarak ses dosyaları oluşturabilirsiniz.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="request":::

Aşağıdakine benzer bir yanıt almanız gerekir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="response":::

Daha fazla bilgi için [konuşmayı metne dönüştürme REST API başvurusuna](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text)bakın.
