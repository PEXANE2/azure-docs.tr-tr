---
title: Azure Iletişim Hizmetleri için SMS istemci kitaplığına genel bakış
titleSuffix: An Azure Communication Services concept document
description: SMS istemci kitaplığı ve teklifleri hakkında genel bakış sağlar.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 6f0b5325ff95d2c2f34572239967151986621bcb
ms.sourcegitcommit: dea56e0dd919ad4250dde03c11d5406530c21c28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96937194"
---
# <a name="sms-client-library-overview"></a>SMS istemci kitaplığına genel bakış

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]
[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Iletişim Hizmetleri SMS istemci kitaplıkları, uygulamalarınıza SMS iletileri eklemek için kullanılabilir.

## <a name="sms-client-library-capabilities"></a>SMS istemci kitaplığı özellikleri

Aşağıdaki liste, şu anda istemci kitaplıklarımızda bulunan özellikler kümesini gösterir.

| Özellik grubu | Özellik                                                                            | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------- | --- | ---- | ---- | ------ |
| Temel yetenekler | SMS iletileri gönderme ve alma </br> *Desteklenen Unicode emojıs*                        | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Gönderilen iletiler için teslim raporları alma                                            | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Tüm karakter kümeleri (dil/Unicode desteği)                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Uzun mesaj desteği (en fazla 2048 karakter)                                           | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Uzun mesajların otomatik olarak eklenmesi                                                   | ✔️   | ✔️    | ✔️    | ✔️      |
| Olaylar            | Web kancalarını gelen iletileri ve teslim raporlarını alacak şekilde yapılandırmak için Event Grid kullanın | ✔️   | ✔️    | ✔️    | ✔️      |
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
- [SMS çözümünüzü planlayın](../telephony-sms/plan-solution.md)
