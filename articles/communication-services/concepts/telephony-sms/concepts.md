---
title: Azure Iletişim hizmetlerinde SMS kavramları
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri SMS kavramları hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: e5cfc1e27bae10a1c67e4506afe9db825664785f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947920"
---
# <a name="sms-concepts"></a>SMS kavramları

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Azure Iletişim Hizmetleri, Iletişim Hizmetleri SMS istemci kitaplıklarını kullanarak SMS metin iletilerini göndermenizi ve almanızı sağlar. Bu istemci kitaplıkları, müşteri hizmeti senaryolarını, randevu anımsatıcıları, iki öğeli kimlik doğrulaması ve diğer gerçek zamanlı iletişim ihtiyaçlarını desteklemek için kullanılabilir. İletişim Hizmetleri SMS, kampanyalarınızı çevreleyen teslimat ve yanıt oranı öngörülerini açığa çıkarırken iletileri güvenilir bir şekilde göndermenize olanak tanır.

Azure Iletişim Hizmetleri SMS istemci kitaplıklarının temel özellikleri şunlardır:

-  Uygulamalarınıza SMS özelliği eklemek için **basit** kurulum deneyimi.
- A2P (uygulamadan kişiye) için ücretsiz numaralar üzerinden **yüksek hız** iletisi desteği Birleşik Devletler kullanım örnekleri.
- Müşteri desteği, uyarılar ve randevu anımsatıcıları gibi senaryoları desteklemeye yönelik **iki yönlü** konuşmalar.
- Uygulamanızdan gönderilen iletiler için gerçek zamanlı teslim raporlarıyla **güvenilir teslim** .
- Kullanım modellerinizi ve müşteri katılımını izlemek için **analiz** .
- Ücretsiz numaraları otomatik olarak algılamak ve kabul etmek için, işleme desteğini **kabul etme** desteği. İletişim Hizmetleri durdurma ve başlatma iletilerini algılar ve son kullanıcılara aşağıdaki varsayılan yanıtları gönderir: 
  - Durdur- *"Bu sayıdan iletileri başarıyla kaldırdınız. Yeniden başlatmaya başlamak için yanıtlayın. "*
  - Başlat- *"Bu sayıdan iletilere başarıyla yeniden abone oldunuz. Aboneliğinizi kaldırmak için durdurun. "*
  - DURDURMA ve başlatma iletileri size geri alınacaktır. Azure Iletişim Hizmetleri, iletişimlerinizi kapatan alıcılara daha fazla ileti gönderilmemesini sağlamak için bu kabul etme işlemini izlemenizi ve uygulamanızı önerir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SMS göndermeye başlama](../../quickstarts/telephony-sms/send.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- [SMS istemci kitaplığı](../telephony-sms/sdk-features.md) hakkında bilgi edinin
- SMS özellikli [telefon numarası](../../quickstarts/telephony-sms/get-phone-number.md) alın
- [SMS çözümünüzü planlayın](../telephony-sms/plan-solution.md)
