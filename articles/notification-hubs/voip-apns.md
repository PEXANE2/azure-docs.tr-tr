---
title: Azure Notification Hubs APNS VOıP bildirimleri gönderin
description: Azure Notification Hubs aracılığıyla APNS VOıP bildirimleri gönderme hakkında bilgi edinin (resmi olarak desteklenmez).
author: sethmanheim
ms.author: sethm
ms.date: 3/23/2020
ms.topic: how-to
ms.service: notification-hubs
ms.openlocfilehash: c99af881b8f93b75633741c2352dc5df17dd2963
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80146894"
---
# <a name="use-apns-voip-through-notification-hubs-not-officially-supported"></a>APNS VOıP 'i Notification Hubs aracılığıyla kullanma (resmi olarak desteklenmez)

APNS VOıP bildirimlerinin Azure Notification Hubs aracılığıyla kullanılması mümkündür; Ancak, bu senaryo için resmi bir destek yoktur.

## <a name="considerations"></a>Önemli noktalar

Notification Hubs aracılığıyla APNS VOıP bildirimleri gönderilmesini hala seçerseniz, aşağıdaki sınırlamalara dikkat edin:

- Bir VOıP bildirimi göndermek `apns-topic` için üst bilginin uygulama PAKETI Kimliği + sonek olarak ayarlanması gerekir `.voip` . Örneğin, paket KIMLIĞI olan bir örnek uygulama için `com.microsoft.nhubsample` `apns-topic` üst bilgi şu şekilde ayarlanmalıdır`com.microsoft.nhubsample.voip.`

   Uygulamanın paket KIMLIĞI, hub 'ın APNS kimlik bilgilerinin bir parçası olarak yapılandırılması gerektiğinden ve değer değiştirilemediğinden, bu yöntem Azure Notification Hubs ile iyi çalışmaz. Ayrıca, Notification Hubs `apns-topic` çalışma zamanında üstbilgideki değerin geçersiz kılınmasına izin vermez.

   VOıP bildirimleri göndermek için uygulama paket KIMLIĞIYLE ayrı bir Bildirim Hub 'ı yapılandırmanız gerekir `.voip` .

- Bir VOıP bildirimi göndermek `apns-push-type` için üstbilginin değere ayarlanması gerekir `voip` .

   Müşterilere iOS 13 ' e geçiş yapmaya yardımcı olmak için Notification Hubs üst bilgi için doğru değeri çıkarması gerekir `apns-push-type` . Çıkarım mantığı, standart bildirimlerin kesilmesini önlemek için özellikle basit bir çabadır. Ne yazık ki, bu yöntem VOıP bildirimleri ile sorunlara neden olur, çünkü Apple, VOıP bildirimlerini standart bildirimlerle aynı kurallara uymuyor özel bir durum olarak değerlendirir.

   VOıP bildirimleri göndermek için, üst bilgi için açık bir değer belirtmeniz gerekir `apns-push-type` .

- Notification Hubs, Apple tarafından belgelendiği gibi APNS yüklerini 4 KB 'ye kısıtlar. VOıP bildirimleri için Apple, 5 KB 'a kadar yükleri sağlar. Notification Hubs, standart ve VOıP bildirimlerini ayırt etmez; Bu nedenle, tüm bildirimler 4 KB ile sınırlıdır.

   VOıP bildirimleri göndermek için, 4 KB 'lık yük boyut sınırını aşmamalı.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [ `apns-topic` `apns-push-type` VoIP bildirimleri için özel durumlar da dahil olmak üzere belgeler ve üstbilgiler ve değerler](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns).

- [Yük boyutu sınırına yönelik belgeler](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification).

- [İOS 13 güncelleştirme Notification Hubs](push-notification-updates-ios-13.md#apns-push-type).
