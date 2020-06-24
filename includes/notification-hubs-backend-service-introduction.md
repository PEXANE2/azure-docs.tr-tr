---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 98c9d83424e7d347bb6e1166a3b00304d39e0cce
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081522"
---
En son ve en iyi [yükleme](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#installations) yaklaşımını kullanarak istemci için [cihaz kaydını](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) Işlemek üzere bir [ASP.NET Core Web API](https://dotnet.microsoft.com/apps/aspnet/apis) arka ucu kullanılır. Hizmet, anında iletme bildirimlerini de platformlar arası bir şekilde gönderir. 

Bu işlemler, [arka uç işlemleri için NOTIFICATION HUBS SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)kullanılarak işlenir. Genel yaklaşımla ilgili daha fazla ayrıntı, [uygulama arka ucu belgelerinden kayıt](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#registration-management-from-a-backend) sırasında verilmiştir.
