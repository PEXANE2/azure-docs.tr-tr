---
title: Azure Iletişim hizmetlerinde SMS kavramları
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri SMS kavramları hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: bdba05dd75b6b6f4e32bf3f536e794e568fa13d7
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2021
ms.locfileid: "105642676"
---
# <a name="sms-concepts"></a>SMS kavramları
[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Iletişim Hizmetleri, Iletişim Hizmetleri SMS SDK 'larını kullanarak SMS metin iletilerini göndermenizi ve almanızı sağlar. Bu SDK 'lar müşteri hizmeti senaryolarını, randevu anımsatıcıları, iki öğeli kimlik doğrulaması ve diğer gerçek zamanlı iletişim ihtiyaçlarını desteklemek için kullanılabilir. İletişim Hizmetleri SMS, e-posta ve yanıt ölçümleri ortaya çıkarırken iletileri güvenilir bir şekilde göndermenize olanak tanır.

Azure Iletişim Hizmetleri SMS SDK 'larının temel özellikleri şunlardır:

-  Uygulamalarınıza SMS özelliği eklemek için **basit** kurulum deneyimi.
- A2P (uygulamadan kişiye) için ücretsiz numaralar üzerinden **yüksek hız** iletisi desteği Birleşik Devletler kullanım örnekleri.
- Tek seferde birden çok alıcıya ileti gönderilmesini etkinleştirmek için **toplu mesajlaşma** desteklenir.
- Müşteri desteği, uyarılar ve randevu anımsatıcıları gibi senaryoları desteklemeye yönelik **iki yönlü** konuşmalar.
- Uygulamanızdan gönderilen iletiler için gerçek zamanlı teslim raporlarıyla **güvenilir teslim** .
- SMS kullanım desenlerinizi izlemek için **analiz** .
- Ücretsiz numaraları otomatik olarak algılamak ve kabul etmek için, işleme desteğini **kabul etme** desteği. Ücretsiz telefon numaraları uygulanan ve ABD taşıyıcılar tarafından zorlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SMS göndermeye başlama](../../quickstarts/telephony-sms/send.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- [SMS SDK 'sını](../telephony-sms/sdk-features.md) öğrenmeye
- SMS özellikli [telefon numarası](../../quickstarts/telephony-sms/get-phone-number.md) alın
- [Azure Iletişim hizmetlerindeki telefon numarası türleri](../telephony-sms/plan-solution.md)
