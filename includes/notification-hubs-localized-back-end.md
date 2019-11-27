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
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260909"
---
Şablon bildirimleri gönderdiğinizde yalnızca bir özellikler kümesi sağlamanız gerekir. Bu senaryoda, özellikler kümesi geçerli haberlerin yerelleştirilmiş sürümünü içerir.

```json
{
    "News_English": "World News in English!",
    "News_French": "World News in French!",
    "News_Mandarin": "World News in Mandarin!"
}
```

### <a name="send-notifications-using-a-c-console-app"></a>C# Konsol uygulaması kullanarak bildirim gönderme

Bu bölüm, bir konsol uygulaması kullanarak bildirimlerin nasıl gönderileceğini gösterir. Kod hem Windows Mağazası hem de iOS cihazlarına bildirim yayınlar. Daha önce oluşturduğunuz konsol uygulamasındaki `SendTemplateNotificationAsync` yöntemini aşağıdaki kodla değiştirin:

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

Sendtemplatenocertificate bağımsız zaman uyumsuz yöntemi, platformdan bağımsız olarak yerelleştirilmiş haberleri **Tüm** cihazlarınıza gönderir. Bildirim Hub 'ınız, belirli bir etikete abone olan tüm cihazlara doğru yerel yük oluşturur ve gönderir.

### <a name="sending-notification-with-mobile-services"></a>Mobile Services bildirim gönderiliyor

Mobile Services Scheduler 'da aşağıdaki betiği kullanın:

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
