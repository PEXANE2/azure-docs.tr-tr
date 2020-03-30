---
title: include dosyası
description: include dosyası
services: notification-hubs
author: sethmanheim
ms.service: notification-hubs
ms.topic: include
ms.date: 11/07/2019
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: 520a0b4ec42b9a32fbd30c28c7ce311b5445f23d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74260909"
---
Şablon bildirimleri gönderdiğinizde, yalnızca bir dizi özellik sağlamanız gerekir. Bu senaryoda, özellikler kümesi geçerli haberlerin yerelleştirilmiş sürümünü içerir.

```json
{
    "News_English": "World News in English!",
    "News_French": "World News in French!",
    "News_Mandarin": "World News in Mandarin!"
}
```

### <a name="send-notifications-using-a-c-console-app"></a>C# konsol uytun uygulamasını kullanarak bildirim gönderme

Bu bölümde, bir konsol uygulamasını kullanarak bildirimlerin nasıl gönderilen gösteriliyor. Kod, bildirimleri hem Windows Mağazası'na hem de iOS aygıtlarına yayınlar. Daha önce oluşturduğunuz konsol uygulamasındaki `SendTemplateNotificationAsync` yöntemini aşağıdaki kodla değiştirin:

```csharp
private static async void SendTemplateNotificationAsync()
{
    // Define the notification hub.
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(
            "<connection string with full access>", "<hub name>");

    // Apple requires the apns-push-type header for all requests
    var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};

    // Sending the notification as a template notification. All template registrations that contain 
    // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
    // This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string, string> templateParams = new Dictionary<string, string>();

    // Create an array of breaking news categories.
    var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
    var locales = new string[] { "English", "French", "Mandarin" };

    foreach (var category in categories)
    {
        templateParams["messageParam"] = "Breaking " + category + " News!";

        // Sending localized News for each tag too...
        foreach( var locale in locales)
        {
            string key = "News_" + locale;

            // Your real localized news content would go here.
            templateParams[key] = "Breaking " + category + " News in " + locale + "!";
        }

        await hub.SendTemplateNotificationAsync(templateParams, category);
    }
}
```

SendTemplateNotificationAsync yöntemi, platformdan bağımsız olarak **tüm** aygıtlarınıza yerelleştirilmiş haber parçasını sunar. Bildirim merkeziniz, belirli bir etikete abone olan tüm aygıtlara doğru yerel yükü oluşturur ve sağlar.

### <a name="sending-notification-with-mobile-services"></a>Mobil Hizmetlerle bildirim gönderme

Mobil Hizmetler zamanlayıcısınızda aşağıdaki komut dosyasını kullanın:

```csharp
var azure = require('azure');
var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
var notification = {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin", "World News in Mandarin!"
}
notificationHubService.send('World', notification, function(error) {
    if (!error) {
        console.warn("Notification successful");
    }
});
```
