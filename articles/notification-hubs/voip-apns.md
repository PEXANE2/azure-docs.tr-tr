---
title: Azure Notification Hubs APNS VOıP bildirimleri gönderin
description: Azure Notification Hubs aracılığıyla APNS VOıP bildirimleri gönderme hakkında bilgi edinin (resmi olarak desteklenmez).
author: sethmanheim
ms.author: sethm
ms.date: 3/23/2020
ms.topic: how-to
ms.service: notification-hubs
ms.openlocfilehash: c99af881b8f93b75633741c2352dc5df17dd2963
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80146894"
---
# <a name="use-apns-voip-through-notification-hubs-not-officially-supported"></a>APNS VOıP 'i Notification Hubs aracılığıyla kullanma (resmi olarak desteklenmez)

APNS VOıP bildirimlerinin Azure Notification Hubs aracılığıyla kullanılması mümkündür; Ancak, bu senaryo için resmi bir destek yoktur.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Notification Hubs aracılığıyla APNS VOıP bildirimleri gönderilmesini hala seçerseniz, aşağıdaki sınırlamalara dikkat edin:

- Bir VOıP bildirimi göndermek için `apns-topic` üst bilginin uygulama paketi kimliği + `.voip` sonek olarak ayarlanması gerekir. Örneğin, paket KIMLIĞI `com.microsoft.nhubsample`olan bir örnek uygulama için `apns-topic` üst bilgi şu şekilde ayarlanmalıdır`com.microsoft.nhubsample.voip.`

   Uygulamanın paket KIMLIĞI, hub 'ın APNS kimlik bilgilerinin bir parçası olarak yapılandırılması gerektiğinden ve değer değiştirilemediğinden, bu yöntem Azure Notification Hubs ile iyi çalışmaz. Ayrıca, Notification Hubs çalışma zamanında `apns-topic` üstbilgideki değerin geçersiz kılınmasına izin vermez.

   VOıP bildirimleri göndermek için `.voip` uygulama paket kimliğiyle ayrı bir Bildirim Hub 'ı yapılandırmanız gerekir.

- Bir VOıP bildirimi göndermek için `apns-push-type` üstbilginin değere `voip`ayarlanması gerekir.

   Müşterilere iOS 13 ' e geçiş yapmaya yardımcı olmak için Notification Hubs `apns-push-type` üst bilgi için doğru değeri çıkarması gerekir. Çıkarım mantığı, standart bildirimlerin kesilmesini önlemek için özellikle basit bir çabadır. Ne yazık ki, bu yöntem VOıP bildirimleri ile sorunlara neden olur, çünkü Apple, VOıP bildirimlerini standart bildirimlerle aynı kurallara uymuyor özel bir durum olarak değerlendirir.

   VOıP bildirimleri göndermek için, `apns-push-type` üst bilgi için açık bir değer belirtmeniz gerekir.

- Notification Hubs, Apple tarafından belgelendiği gibi APNS yüklerini 4 KB 'ye kısıtlar. VOıP bildirimleri için Apple, 5 KB 'a kadar yükleri sağlar. Notification Hubs, standart ve VOıP bildirimlerini ayırt etmez; Bu nedenle, tüm bildirimler 4 KB ile sınırlıdır.

   VOıP bildirimleri göndermek için, 4 KB 'lık yük boyut sınırını aşmamalı.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [VoIP bildirimleri `apns-topic` için `apns-push-type` özel durumlar da dahil olmak üzere belgeler ve üstbilgiler ve değerler](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns).

- [Yük boyutu sınırına yönelik belgeler](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification).

- [İOS 13 güncelleştirme Notification Hubs](push-notification-updates-ios-13.md#apns-push-type).
