---
title: Zamanlanmış bildirimler nasıl gönderilir | Microsoft Dokümanlar
description: Bu konu, Azure Bildirim Hub'ları ile Zamanlanmış Bildirimler'i kullanmayı açıklar.
services: notification-hubs
documentationcenter: .net
keywords: push bildirimleri, push bildirimi, anında iletme bildirimlerini zamanlama
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: ef5eedaa903480ae670f9bc48d0af89744a99d22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71213008"
---
# <a name="how-to-send-scheduled-notifications"></a>Nasıl YapılSın: Zamanlanmış bildirimler gönderme

Gelecekte bir noktada bildirim göndermek istediğiniz bir senaryonuz varsa, ancak bildirimi göndermek için arka uç kodunuzu uyandırmanın kolay bir yolu yoksa. Standart katman bildirim hub'ları, bildirimleri gelecekte yedi güne kadar zamanlamanızı sağlayan bir özelliği destekler.


## <a name="schedule-your-notifications"></a>Bildirimlerinizi zamanlama
Bildirim gönderirken, aşağıdaki örnekte gösterildiği gibi Bildirim Hub'ları SDK'daki [ `ScheduledNotification` sınıfı](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) kullanmanız yeterlidir:

```csharp
Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));
```

## <a name="cancel-scheduled-notifications"></a>Zamanlanmış bildirimleri iptal etme
Ayrıca, bildirimId'ini kullanarak önceden zamanlanmış bir bildirimi iptal edebilirsiniz:

```csharp
await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);
```

Gönderebileceğiniz zamanlanmış bildirim sayısında sınır yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki öğreticilere bakın:

 - [Tüm kayıtlı cihazlara anında bildirim](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
 - [Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)
 - [Yerelleştirilmiş anında iletme bildirimleri gönderme](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
 - [Belirli kullanıcılara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) 
 - [Konum temelli anında iletme bildirimleri gönderme](notification-hubs-push-bing-spatial-data-geofencing-notification.md)
