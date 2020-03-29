---
title: Azure Bildirim Hub'larında yönlendirme ve etiketleme ifadeleri
description: Azure Bildirim Hub'ları için ifadeleri nasıl yönlendirecek ve etiketleyeceklerini öğrenin.
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
ms.openlocfilehash: 2432ac41645e373ea3a87ff7e69ef02a4e30c81d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062305"
---
# <a name="routing-and-tag-expressions"></a>Yönlendirme ve etiket ifadeleri

## <a name="overview"></a>Genel Bakış

Etiket ifadeleri, Bildirim Hub'ları aracılığıyla anında iletme bildirimi gönderirken belirli aygıt kümelerini veya daha özel kayıtları hedeflemenizi sağlar.

## <a name="targeting-specific-registrations"></a>Belirli kayıtları hedefleme

Belirli bildirim kayıtlarını hedeflemenin tek yolu, etiketleri onlarla ilişkilendirmek ve ardından bu etiketleri hedeflemektir. [Kayıt Yönetimi'nde](notification-hubs-push-notification-registration-management.md)de belirtildiği gibi, anında iletme bildirimleri almak için bir uygulamanın bir bildirim hub'ına bir aygıt tutamacı kaydetmesi gerekir. Uygulama bir bildirim hub'ında bir kayıt oluşturduktan sonra, uygulama arka ucu ona anında iletme bildirimleri gönderebilir. Uygulama arka ucu aşağıdaki yollarla belirli bir bildirim ile hedeflemek için kayıtları seçebilirsiniz:

1. **Yayın**: bildirim merkezindeki tüm kayıtlar bildirimi alır.
2. **Etiket**: belirtilen etiketi içeren tüm kayıtlar bildirimi alır.
3. **Etiket ifadesi**: etiket kümesi belirtilen ifadeyle eşleşen tüm kayıtlar bildirimi alır.

## <a name="tags"></a>Etiketler

Bir etiket herhangi bir dize olabilir, en fazla 120 karakter, alfanümerik ve aşağıdaki`_`alfanümerik`#`olmayan karakterler`.`içeren: '`-`', ',`@`', ', ', ',`:`', ', '. Aşağıdaki örnekte, belirli müzik grupları hakkında tost bildirimleri alabileceğiniz bir uygulama gösterilmektedir. Bu senaryoda, bildirimleri yönlendirmenin basit bir yolu, aşağıdaki şekilde olduğu gibi, kayıtları farklı bantları temsil eden etiketlerle etiketlemektir:

![Etiketlere genel bakış](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags.png)

Şekilde, **Beatles** ile etiketlenmiş mesaj etiketi **Beatles**ile kayıtlı sadece tablet ulaşır.

Etiketler için kayıt oluşturma hakkında daha fazla bilgi için Kayıt [Yönetimi'ne](notification-hubs-push-notification-registration-management.md)bakın.

[Microsoft Azure Bildirim Hub'ları](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK'da `Microsoft.Azure.NotificationHubs.NotificationHubClient` sınıfın bildirim gönderme yöntemlerini kullanarak etiketlere bildirim gönderebilirsiniz. Ayrıca Node.js veya Push BildirimlerREST API'leri kullanabilirsiniz.  Burada SDK kullanarak bir örnek.

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

Etiketler önceden sağlanmamalıdır ve birden çok uygulamaya özgü kavramlara başvurabilir. Örneğin, bu örnek uygulamanın kullanıcıları gruplar hakkında yorum yapabilir ve yalnızca sevdikleri gruplardaki yorumlar için değil, aynı zamanda yorum yaptıkları grup ne olursa olsun arkadaşlarının tüm yorumları için de tost almak isterler. Aşağıdaki şekilde bu senaryonun bir örneğini vurgulamaktadır:

![Etiketler arkadaşlar](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags2.png)

Bu örnekte, Alice Beatles güncellemeleri ile ilgilenir ve Bob Wailers için güncelleştirmelerle ilgilenir. Bob ayrıca Charlie'nin yorumlarıyla, Charlie ise Wailer'larla ilgilenmektedir. Charlie'nin Beatles hakkındaki yorumu için bir bildirim gönderildiğinde, Bildirim Hub'ları bu bildirimi hem Alice'e hem de Bob'a gönderir.

Etiketlerde birden çok özendişeyi kodlayabilirken (örneğin, `band_Beatles` etiketler `follows_Charlie`basit dizeleri ve değerleri olan özellikler değildir. Kayıt yalnızca belirli bir etiketin varlığı veya yokluğuyla eşleşir.

İlgi gruplarına göndermek için etiketlerin nasıl kullanılacağı yla ilgili adım adım tam bir öğretici için [Son Dakika Haberleri'ne](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)bakın.

> [!NOTE]
> Azure Bildirim Hub'ları kayıt başına en fazla 60 etiketi destekler.

## <a name="using-tags-to-target-users"></a>Kullanıcıları hedeflemek için etiketleri kullanma

Etiketleri kullanmanın başka bir yolu da belirli bir kullanıcıyla ilişkili tüm aygıtları tanımlamaktır. Bir Kaydı, aşağıdaki şekilde olduğu gibi kullanıcı kimliğini içeren bir etiketle etiketleyebilirsiniz:

![Etiket kullanıcıları](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags3.png)

Şekilde, ileti etiketli `user_Alice` tüm cihazlara `user_Alice`ulaşır .

## <a name="tag-expressions"></a>Etiket ifadeleri

Bildirimlerin tek bir etiketle değil, etiketleri kullanan boolean ifadesiyle tanımlanan bir dizi kaydı hedeflemesi gereken durumlar vardır.

Red Sox ve Cardinals arasında bir oyun hakkında Boston herkese bir hatırlatma gönderir bir spor uygulaması düşünün. İstemci uygulaması takımlara ve konuma olan ilgiyle ilgili etiketleri kaydederse, bildirim Boston'da Red Sox veya Cardinals ile ilgilenen herkese hedeflenmelidir. Bu durum aşağıdaki Boolean ifadesi ile ifade edilebilir:

```csharp
(follows_RedSox || follows_Cardinals) && location_Boston
```

![Etiket ifadeleri](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags4.png)

Etiket ifadeleri gibi yaygın Boolean `AND` `&&`işleçleri `NOT` destekler`!`( ), `OR` (`||`), ve ( parantez de içerebilirler. Yalnızca `OR` işleçleri kullanarak etiket ifadeleri 20 etikete başvuruyapabilir; işleçleri `AND` ile `OR` ifade ama hiçbir operatörler 10 etiketleri başvurum olabilir; aksi takdirde, etiket ifadeleri 6 etiketle sınırlıdır.

SDK'yı kullanarak etiket ifadeleri içeren bildirimler göndermek için bir örnek aşağıda verilmiştir:

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
