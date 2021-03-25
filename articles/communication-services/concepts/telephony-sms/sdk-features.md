---
title: Azure Iletişim Hizmetleri için SMS SDK 'ya genel bakış
titleSuffix: An Azure Communication Services concept document
description: SMS SDK ve teklifleriyle ilgili genel bir bakış sunar.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 25a5befab6ef141b059caaac7b1784d47f2f93cf
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105108120"
---
# <a name="sms-sdk-overview"></a>SMS SDK genel bakış

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Iletişim Hizmetleri SMS SDK 'Ları, uygulamalarınıza SMS iletileri eklemek için kullanılabilir.

## <a name="sms-sdk-capabilities"></a>SMS SDK özellikleri

Aşağıdaki liste, SDK 'larımızda Şu anda kullanılabilir olan özellikler kümesini gösterir.

| Özellik grubu | Özellik                                                                            | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------- | --- | ---- | ---- | ------ |
| Temel yetenekler | SMS iletileri gönderme ve alma </br> *Desteklenen Unicode emojıs*                        | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Gönderilen iletiler için teslim raporları alma                                            | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Tüm karakter kümeleri (dil/Unicode desteği)                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Uzun mesaj desteği (en fazla 2048 karakter)                                           | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Uzun mesajların otomatik olarak eklenmesi                                                   | ✔️   | ✔️    | ✔️    | ✔️      |
| Ekinlikler            | Web kancalarını gelen iletileri ve teslim raporlarını alacak şekilde yapılandırmak için Event Grid kullanın | ✔️   | ✔️    | ✔️    | ✔️      |
| Telefon Numarası      | Toll-Free numaraları                                                                     | ✔️   | ✔️    | ✔️    | ✔️      |
| Yasal        | Opt-Out Işleme                                                                      | ✔️   | ✔️    | ✔️    | ✔️      |
| İzleme        | Gönderilen ve alınan iletiler için kullanımı izleme                                          | ✔️   | ✔️    | ✔️    | ✔️      |
| PSTN çağırma      | SMS destekli ücretsiz numaraya PSTN çağırma özellikleri ekleyin                    | ✔️   | ✔️    | ✔️    | ✔️      |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SMS göndermeye başlama](../../quickstarts/telephony-sms/send.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- Genel [SMS kavramları](../telephony-sms/concepts.md) hakkında bilgi edinin
- SMS özellikli [telefon numarası](../../quickstarts/telephony-sms/get-phone-number.md) alın
- [Azure Iletişim hizmetlerindeki telefon numarası türleri](../telephony-sms/plan-solution.md)
