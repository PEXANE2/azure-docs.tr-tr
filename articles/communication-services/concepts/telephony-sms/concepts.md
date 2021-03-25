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
ms.openlocfilehash: 0ddc9bfeb0df32614d835e0eaef9da52e917ee91
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105108443"
---
# <a name="sms-concepts"></a>SMS kavramları

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Iletişim Hizmetleri, Iletişim Hizmetleri SMS SDK 'larını kullanarak SMS metin iletilerini göndermenizi ve almanızı sağlar. Bu SDK 'lar müşteri hizmeti senaryolarını, randevu anımsatıcıları, iki öğeli kimlik doğrulaması ve diğer gerçek zamanlı iletişim ihtiyaçlarını desteklemek için kullanılabilir. İletişim Hizmetleri SMS, kampanyalarınızı çevreleyen teslimat ve yanıt oranı öngörülerini açığa çıkarırken iletileri güvenilir bir şekilde göndermenize olanak tanır.

Azure Iletişim Hizmetleri SMS SDK 'larının temel özellikleri şunlardır:

-  Uygulamalarınıza SMS özelliği eklemek için **basit** kurulum deneyimi.
- A2P (uygulamadan kişiye) için ücretsiz numaralar üzerinden **yüksek hız** iletisi desteği Birleşik Devletler kullanım örnekleri.
- Müşteri desteği, uyarılar ve randevu anımsatıcıları gibi senaryoları desteklemeye yönelik **iki yönlü** konuşmalar.
- Uygulamanızdan gönderilen iletiler için gerçek zamanlı teslim raporlarıyla **güvenilir teslim** .
- Kullanım modellerinizi ve müşteri katılımını izlemek için **analiz** .
- Ücretsiz numaraları otomatik olarak algılamak ve kabul etmek için, işleme desteğini **kabul etme** desteği. Ücretsiz telefon numaraları uygulanan ve ABD taşıyıcılar tarafından zorlanır.
  - Durdur-bir kısa mesaj alıcısı kabul etmek isterse, ücretsiz numaraya ' STOP ' gönderebilirler. Taşıyıcı durdurma için aşağıdaki varsayılan yanıtı gönderir: *"ağ msg: Bu sayıdan gönderilen tüm metinleri engelleyen" Durdur "sözcüğüyle yanıtladınız. İletileri yeniden almak için "Durdur" metnini geri alın. "*
  - Başlat/kaldır-alıcı ücretsiz bir sayıdan kısa mesajlardan metin iletilerine yeniden gönderim isterse, ücretsiz numarayı ' Başlat ' veya ' Durdur ' a GERI gönderebilirler. Taşıyıcı, Başlat/Durdur için şu varsayılan yanıtı gönderir: *"ağ iletisi:" durdurulmuş olursunuz "ve bu sayıdan yeniden ileti almaya başlayacaktır."*
  - Azure Iletişim Hizmetleri Durdur iletisini algılayacak ve alıcıya yapılan tüm iletileri engelleyecek. Teslim raporu, "verilen alıcı için gönderici engellendi" olarak durum iletisiyle başarısız bir teslim olduğunu gösterir.
  - Durdur, kaldır ve Başlat iletileri size geri alınacaktır. Azure Iletişim Hizmetleri, iletişimlerinizi kapatan alıcılara daha fazla ileti gönderme denemesi yapılmadığını sağlamak için bu kabul etme işlemini izlemenizi ve uygulamanızı önerir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SMS göndermeye başlama](../../quickstarts/telephony-sms/send.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- [SMS SDK 'sını](../telephony-sms/sdk-features.md) öğrenmeye
- SMS özellikli [telefon numarası](../../quickstarts/telephony-sms/get-phone-number.md) alın
- [Azure Iletişim hizmetlerindeki telefon numarası türleri](../telephony-sms/plan-solution.md)
