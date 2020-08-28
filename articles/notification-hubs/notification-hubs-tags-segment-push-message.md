---
title: Azure Notification Hubs yönlendirme ve etiket ifadeleri
description: Azure Notification Hubs için ifadelerin nasıl yönlendirileceğini ve etiketleyeceğinizi öğrenin.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 12/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 4ff8225522e79e2be40682fb5e4823777dde2aa0
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88998313"
---
# <a name="routing-and-tag-expressions"></a>Yönlendirme ve etiket ifadeleri

## <a name="overview"></a>Genel Bakış

Etiket ifadeleri, Notification Hubs aracılığıyla anında iletme bildirimi gönderirken belirli cihaz kümelerini veya daha fazla özel kaydı hedeflemesini sağlar.

## <a name="targeting-specific-registrations"></a>Belirli kayıtları hedefleme

Belirli bildirim kayıtlarını hedeflemek için tek yol, etiketleri bunlarla ilişkilendirmekte ve bu etiketleri hedeflemelidir. [Kayıt yönetiminde](notification-hubs-push-notification-registration-management.md)anlatıldığı gibi anında iletme bildirimleri almak için bir uygulamanın bir Bildirim Hub 'ına bir cihaz tanıtıcısı kaydetmesi gerekir. Uygulama bir Bildirim Hub 'ında kayıt oluşturduğunda, uygulama arka ucu buna anında iletme bildirimleri gönderebilir. Uygulama arka ucu, belirli bir bildirimle hedeflenen kayıtları aşağıdaki yollarla seçebilir:

1. **Yayın**: Bildirim Hub 'ındaki tüm kayıtlar bildirimi alır.
2. **Etiket**: belirtilen etiketi içeren tüm kayıtlar bildirimi alır.
3. **Etiket ifadesi**: etiket kümesi belirtilen ifadeyle eşleşen tüm kayıtlar bildirimi alır.

## <a name="tags"></a>Etiketler

Etiket, en fazla 120 karakter uzunluğunda olabilir ve alfasayısal karakterler ve şu alfasayısal olmayan karakterleri içerir: ' `_` ', ' `@` ', ' `#` ', ' `.` ', ' `:` ', ' `-` '. Aşağıdaki örnekte, belirli müzik grupları hakkında bildirim almak için kullanabileceğiniz bir uygulama gösterilmektedir. Bu senaryoda, bildirimleri yönlendirmeye yönelik basit bir yol, kayıtları aşağıdaki şekilde farklı bantları temsil eden etiketlerle etiketleyebilir:

![Etiketlere genel bakış](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags.png)

Şekilde, **Beatles** ile etiketlenmiş Ileti yalnızca **Beatles**etiketiyle kayıtlı olan tabletle ulaşır.

Etiketlere yönelik kayıtlar oluşturma hakkında daha fazla bilgi için bkz. [kayıt yönetimi](notification-hubs-push-notification-registration-management.md).

`Microsoft.Azure.NotificationHubs.NotificationHubClient` [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK 'sında sınıfının bildirim gönder yöntemlerini kullanarak etiketlere bildirim gönderebilirsiniz. Node.js veya anında Iletme bildirimleri REST API 'Lerini de kullanabilirsiniz.  SDK kullanarak bir örnek aşağıda verilmiştir.

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You requested a Beatles notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You requested a Wailers notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");
```

Etiketler önceden sağlanmamalıdır ve uygulamaya özgü birden çok kavrama başvurabilir. Örneğin, bu örnek uygulamanın kullanıcıları bantlara yorum yapabilir ve yalnızca en sevdiğiniz bantların açıklamalarını değil, kendi Arkadaşlarındaki açıklamaları değil, bunlara yorum eklemek istedikleri bant ne olursa olsun, bunlara ait olan açıklamaları almak isteyebilir. Aşağıdaki şekilde bu senaryonun bir örneği vurgulanmıştır:

![Etiketler arkadaşlar](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags2.png)

Bu örnekte, Çiğdem, Beatles güncelleştirmeleriyle ilgileniyor ve Bob, Wailers güncelleştirmeleriyle ilgileniyor. Emre Ayrıca, Charlie 'in yorumlarıyla ilgileniyor ve Charlie Wailers ile ilgileniyor. Beatles 'de Charlie 'in yorumu için bir bildirim gönderildiğinde Notification Hubs hem gamze hem de Bob 'a gönderir.

Etiketlerde birden çok endişeleri kodlayabilmeniz (örneğin, `band_Beatles` veya `follows_Charlie` ), Etiketler basit dizelerdir ve değerlerle Özellikler değildir. Kayıt yalnızca belirli bir etiketin varlığı veya yokluğunda eşleşir.

İlgi alanı gruplarına göndermek için etiketlerin nasıl kullanılacağına ilişkin bir adım adım öğretici için bkz. [son haberler](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

> [!NOTE]
> Azure Notification Hubs, kayıt başına en fazla 60 etiketi destekler.

## <a name="using-tags-to-target-users"></a>Kullanıcıları hedeflemek için etiketleri kullanma

Etiketleri kullanmanın başka bir yolu da belirli bir kullanıcıyla ilişkili tüm cihazları tanımlamaktır. Bir kaydı, aşağıdaki şekilde kullanıcı KIMLIĞINI içeren bir etiketle etiketleyebilir:

![Kullanıcıları etiketleme](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags3.png)

Şekilde, etiketli ileti, `user_Alice` etiketli tüm cihazlara ulaşır `user_Alice` .

## <a name="tag-expressions"></a>Etiket ifadeleri

Bildirimlerin tek bir etiket tarafından değil tanımlanmış bir kayıt kümesini hedeflemesi, ancak etiketleri kullanan bir Boole ifadesi olması gerektiği durumlar vardır.

Red Sox ve Cardınals arasındaki bir oyun hakkında Boston 'da herkese anımsatıcı gönderen bir spor uygulaması düşünün. İstemci uygulaması, ekiplerde ve konumda ilgi çekici Etiketler kaydederse, bu bildirimin Red Sox veya Cardınals ile ilgilendiği Boston 'daki herkese hedeflenmiş olması gerekir. Bu durum aşağıdaki Boolean ifadesiyle ifade edilebilir:

```csharp
(follows_RedSox || follows_Cardinals) && location_Boston
```

![Etiket ifadeleri](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags4.png)

Etiket ifadeleri (), () ve () gibi yaygın Boole işleçlerini destekler `AND` `&&` `OR` `||` `NOT` `!` ; Ayrıca parantez de içerebilir. Yalnızca işleçleri kullanan etiket ifadeleri `OR` 20 etikete başvurabilir; `AND` işleçle ifade, ancak hiçbir `OR` işleç 10 etikete başvuramaz; Aksi takdirde, etiket ifadeleri 6 etiketle sınırlandırılmıştır.

SDK kullanarak etiket ifadeleriyle bildirim göndermek için bir örnek aşağıda verilmiştir:

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

String userTag = "(location_Boston && !follows_Cardinals)";

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
```
