---
title: Azure Iletişim Hizmetleri için SMS SDK 'ya genel bakış
titleSuffix: An Azure Communication Services concept document
description: SMS SDK ve teklifleriyle ilgili genel bir bakış sunar.
author: mikben
manager: jken
services: azure-communication-services
ms.author: prakulka
ms.date: 03/26/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 2e9f782f0b8fa0be77e70ccdae8849caa4abb81e
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2021
ms.locfileid: "105643809"
---
# <a name="sms-sdk-overview"></a>SMS SDK genel bakış

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Iletişim Hizmetleri SMS SDK 'Ları, uygulamalarınıza SMS iletileri eklemek için kullanılabilir.

## <a name="sms-sdk-capabilities"></a>SMS SDK özellikleri

Aşağıdaki liste, SDK 'larımızda Şu anda kullanılabilir olan özellikler kümesini gösterir.

| Özellik grubu | Özellik                                                                            | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------- | --- | ---- | ---- | ------ |
| Temel yetenekler | SMS iletileri gönderme ve alma                                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Gönderilen iletiler için teslim raporlarını etkinleştir                                             | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Tüm karakter kümeleri (dil/Unicode desteği)                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Uzun mesaj desteği (2048 bayta kadar)                                          | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Uzun mesajların otomatik olarak eklenmesi                                                   | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Tek seferde birden çok alıcıya ileti gönderme                                        | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Idempotlik desteği                                                               | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | İletiler için özel etiketler.                                                             | ✔️   | ✔️    | ✔️    | ✔️      |
| Ekinlikler            | Web kancalarını gelen iletileri ve teslim raporlarını alacak şekilde yapılandırmak için Event Grid kullanın | ✔️   | ✔️    | ✔️    | ✔️      |
| Telefon Numarası      | Toll-Free numaraları                                                                     | ✔️   | ✔️    | ✔️    | ✔️      |
| PSTN çağırma      | SMS destekli ücretsiz numaraya PSTN çağırma özellikleri ekleyin                    | ✔️   | ✔️    | ✔️    | ✔️      |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SMS göndermeye başlama](../../quickstarts/telephony-sms/send.md)

Aşağıdaki belgeler sizin için ilginç olabilir:

- Genel [SMS kavramları](../telephony-sms/concepts.md) hakkında bilgi edinin
- SMS özellikli [telefon numarası](../../quickstarts/telephony-sms/get-phone-number.md) alın
- [Azure Iletişim hizmetlerindeki telefon numarası türleri](../telephony-sms/plan-solution.md)
