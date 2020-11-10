---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2020
ms.author: v-jawe
ms.openlocfilehash: 0964872d03d6e321d25d51a18edbb4a6f0be8a4f
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425484"
---
Bu hızlı başlangıçta, konuşma hizmeti ve kıvrımlı kullanarak konuşmayı metne dönüştürmeyi öğreneceksiniz.

Konuşmadan metne yönelik kavramlar hakkında üst düzey bir bakış için bkz. [genel bakış](../../../speech-to-text.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="convert-speech-to-text"></a>Konuşmayı metne dönüştürme

Komut isteminde aşağıdaki komutu çalıştırın. Komutuna aşağıdaki değerleri eklemeniz gerekir.
- Konuşma hizmeti abonelik anahtarınız.
- Konuşma hizmeti bölgeniz.
- Giriş ses dosyası yolu. [Metin okuma özelliğini](../../../get-started-text-to-speech.md)kullanarak ses dosyaları oluşturabilirsiniz.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="request":::

Aşağıdakine benzer bir yanıt almanız gerekir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speech-to-text.sh" id="response":::

Daha fazla bilgi için [konuşmayı metne dönüştürme REST API başvurusuna](../../../rest-speech-to-text.md)bakın.