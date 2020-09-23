---
title: Azure Iletişim Hizmetleri için SMS istemci kitaplığına genel bakış
titleSuffix: An Azure Communication Services concept document
description: SMS istemci kitaplığı ve teklifleri hakkında genel bakış sağlar.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 2d81749e7023bdbf5353e5c8da633674ea8e8ce9
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947916"
---
# <a name="sms-client-library-overview"></a>SMS istemci kitaplığına genel bakış

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

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
| Telefon Numarası      | Ücretsiz numaralar                                                                     | ✔️   | ✔️    | ✔️    | ✔️      |
| Yasal        | Geri çevirme Işlemi                                                                      | ✔️   | ✔️    | ✔️    | ✔️      |
| İzleme        | Gönderilen ve alınan iletiler için kullanımı izleme                                          | ✔️   | ✔️    | ✔️    | ✔️      |
| PSTN çağırma      | SMS etkin telefon numaranız için PSTN çağırma özellikleri ekleyin                    | ✔️   | ✔️    | ✔️    | ✔️      |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SMS göndermeye başlama](../../quickstarts/telephony-sms/send.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- Genel [SMS kavramları](../telephony-sms/concepts.md) hakkında bilgi edinin
- SMS özellikli [telefon numarası](../../quickstarts/telephony-sms/get-phone-number.md) alın
- [SMS çözümünüzü planlayın](../telephony-sms/plan-solution.md)