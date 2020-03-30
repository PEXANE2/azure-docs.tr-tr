---
title: Azure Bildirim Hub'ları ile APNS VOIP bildirimleri gönderme
description: APNS VOIP bildirimlerini Azure Bildirim Hub'ları üzerinden nasıl göndereceğinizi öğrenin (resmi olarak desteklenmez).
author: sethmanheim
ms.author: sethm
ms.date: 3/23/2020
ms.topic: how-to
ms.service: notification-hubs
ms.openlocfilehash: c99af881b8f93b75633741c2352dc5df17dd2963
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80146894"
---
# <a name="use-apns-voip-through-notification-hubs-not-officially-supported"></a>Bildirim Hub'ları aracılığıyla APNS VOIP'yi kullanma (resmi olarak desteklenmez)

APNS VOIP bildirimlerini Azure Bildirim Hub'ları aracılığıyla kullanmak mümkündür; ancak, bu senaryo için resmi bir destek yoktur.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bildirim Hub'ları aracılığıyla APNS VOIP bildirimleri göndermeyi seçerseniz, aşağıdaki sınırlamalara dikkat edin:

- VOIP bildirimi göndermek, `apns-topic` üstbilginin uygulama paketi kimliğine `.voip` + soneke ayarlanmasını gerektirir. Örneğin, paket kimliğine `com.microsoft.nhubsample`sahip örnek bir `apns-topic` uygulama için üstbilgi`com.microsoft.nhubsample.voip.`

   Uygulamanın paket kimliği hub'ın APNS kimlik bilgilerinin bir parçası olarak yapılandırılmalı ve değer değiştirilemedığından, bu yöntem Azure Bildirim Hub'larında iyi çalışmaz. Ayrıca, Bildirim Hub'ları `apns-topic` üstbilginin değerinin çalışma zamanında geçersiz kılınmasını sağlamaz.

   VOIP bildirimleri göndermek için, `.voip` uygulama paketi kimliğiyle ayrı bir bildirim hub'ı yapılandırmanız gerekir.

- VOIP bildirimi göndermek, `apns-push-type` üstbilginin değere `voip`ayarlatılmasını gerektirir.

   Bildirim Hub'ları, iOS 13'e geçişte müşterilere yardımcı olmak `apns-push-type` için üstbilgi için doğru değeri çıkartmaya çalışır. Çıkarım mantığı, standart bildirimleri kırmamak amacıyla kasıtlı olarak basittir. Apple, VOIP bildirimlerini standart bildirimlerle aynı kurallara uymayan özel bir durum olarak ele aldığı ndan, bu yöntem VOIP bildirimlerinde sorunlara neden olur.

   VOIP bildirimleri göndermek için `apns-push-type` üstbilgi için açık bir değer belirtmeniz gerekir.

- Bildirim Hub'ları, Apple tarafından belgelenen APNS yüklerini 4 KB ile sınırlar. VOIP bildirimleri için Apple, 5 KB'ye kadar taşıma kapasitesine izin verir. Bildirim Hub'ları standart ve VOIP bildirimleri arasında ayrım yapmaz; bu nedenle, tüm bildirimler 4 KB ile sınırlıdır.

   VOIP bildirimleri göndermek için 4-KB yük boyutu sınırını aşmamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [VOIP `apns-topic` `apns-push-type` bildirimleri için özel durumlar da dahil olmak üzere, üstbilgi ve değerler için belgeler.](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns)

- [Yük boyutu sınırı için belgeler.](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)

- [iOS 13 için Bildirim Hub güncelleştirmeleri](push-notification-updates-ios-13.md#apns-push-type).
