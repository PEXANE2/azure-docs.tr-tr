---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 675ad278cb8bdc0ced4eff3bd77572f44c9808fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68857522"
---
Bu bölümde, her yeni öğe ekleninde anında iletme bildirimi göndermek için mevcut Mobil Uygulamalar arka uç projenizdeki kodu güncellersiniz. Bu işlem, platformlar arası itmelere olanak tanıyan Azure Bildirim Hub'larının [şablon](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) özelliğinden güç içerir. Çeşitli istemciler şablonları kullanarak anında iletme bildirimleri için kaydedilir ve tek bir evrensel itme tüm istemci platformlarına ulaşabilirsiniz.

&mdash; [.NET arka uç](#dotnet) veya [Düğüm.js arka uç](#nodejs)ile eşleşen aşağıdaki yordamlardan birini seçin.

### <a name="net-back-end-project"></a><a name="dotnet"></a>.NET arka uç projesi

1. Visual Studio'da sunucu projesine sağ tıklayın. Ardından **NuGet Paketlerini Yönet'i**seçin. `Microsoft.Azure.NotificationHubs`Ara ve sonra **Yükle'yi**seçin. Bu işlem, bildirimleri arka uçtan göndermek için Bildirim Hub'ları kitaplığını yükler.
2. Sunucu projesinde, **açık Denetleyiciler** > **TodoItemController.cs.** Daha sonra aşağıdaki ifadeleri kullanarak ekleyin:

    ```csharp
    using System.Collections.Generic;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Azure.Mobile.Server.Config;
    ```

3. **PostTodoItem** yönteminde, **InsertAsync'e**çağrıdan sonra aşağıdaki kodu ekleyin:  

    ```csharp
    // Get the settings for the server project.
    HttpConfiguration config = this.Configuration;
    MobileAppSettingsDictionary settings =
        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

    // Get the Notification Hubs credentials for the mobile app.
    string notificationHubName = settings.NotificationHubName;
    string notificationHubConnection = settings
        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

    // Create a new Notification Hub client.
    NotificationHubClient hub = NotificationHubClient
    .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

    // Send the message so that all template registrations that contain "messageParam"
    // receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string,string> templateParams = new Dictionary<string,string>();
    templateParams["messageParam"] = item.Text + " was added to the list.";

    try
    {
        // Send the push notification and log the results.
        var result = await hub.SendTemplateNotificationAsync(templateParams);

        // Write the success result to the logs.
        config.Services.GetTraceWriter().Info(result.State.ToString());
    }
    catch (System.Exception ex)
    {
        // Write the failure result to the logs.
        config.Services.GetTraceWriter()
            .Error(ex.Message, null, "Push.SendAsync Error");
    }
    ```

    Bu işlem, öğeyi içeren bir şablon bildirimi gönderir. Yeni bir öğe eklendiğinde metin.

4. Sunucu projesini yeniden yayımlayın.

### <a name="nodejs-back-end-project"></a><a name="nodejs"></a>Düğüm.js arka uç projesi

1. Arka uç projenizi ayarlayın.
2. Todoitem.js'deki varolan kodu aşağıdaki kodla değiştirin:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs.
    logger.info('Running TodoItem.insert');

    // Define the template payload.
    var payload = '{"messageParam": "' + context.item.text + '" }';  

    // Execute the insert. The insert returns the results as a promise.
    // Do the push as a post-execute action within the promise flow.
    return context.execute()
        .then(function (results) {
            // Only do the push if configured.
            if (context.push) {
                // Send a template notification.
                context.push.send(null, payload, function (error) {
                    if (error) {
                        logger.error('Error while sending push notification: ', error);
                    } else {
                        logger.info('Push notification sent successfully!');
                    }
                });
            }
            // Don't forget to return the results from the context.execute().
            return results;
        })
        .catch(function (error) {
            logger.error('Error while running context.execute: ', error);
        });
    });

    module.exports = table;  
    ```

    Bu işlem, yeni bir öğe eklendiğinde item.text içeren bir şablon bildirimi gönderir.

3. Dosyayı yerel bilgisayarınızda düzenlemeyaptığınızda, sunucu projesini yeniden yayımlayın.
