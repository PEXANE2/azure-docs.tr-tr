---
title: Azure Notification Hubs ile kullanıcılara platformlar arası bildirimler gönderme (ASP.NET)
description: Tüm platformları hedefleyen platformdan bağımsız bir bildirim olan tek bir istekte göndermek için Notification Hubs şablonlarını nasıl kullanacağınızı öğrenin.
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: cea0d63c20af781fcfc6ba5d7c06061b12992702
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212032"
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Notification Hubs ile kullanıcılara platformlar arası bildirimler gönderme

Önceki bir öğreticide, [Kullanıcılara Notification Hubs bildirme], belirli bir kimliği doğrulanmış bir kullanıcıya kayıtlı olan tüm cihazlara anında iletme bildirimi göndermeyi öğrendiniz. Bu öğreticide, desteklenen her istemci platformuna bildirim göndermek için birden çok istek gerekiyordu. Azure Notification Hubs, belirli bir cihazın nasıl bildirim almak istediğini belirtebileceğiniz şablonları destekler. Bu yöntem platformlar arası bildirimlerin gönderilmesini basitleştirir.

Bu makalede, tüm platformları hedefleyen platformdan bağımsız bir bildirim olan tek bir istekte göndermek için şablonlardan nasıl yararlanulacağı gösterilmektedir. Şablonlar hakkında daha ayrıntılı bilgi için bkz. [Azure Notification Hubs genel bakış][Templates].

> [!IMPORTANT]
> Windows Phone projeler 8,1 ve önceki sürümleri Visual Studio 2017 ' de desteklenmez. Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Notification Hubs, bir cihaz aynı etiketle birden çok şablonu kaydedebilir. Bu durumda, etiketi hedefleyen bir gelen ileti, her şablon için bir tane olmak üzere cihaza sunulan birden çok bildirim ile sonuçlanır. Bu işlem, bir Windows Mağazası uygulamasında hem rozet hem de bildirim olarak aynı iletiyi birden çok görsel bildirimde görüntülemenizi sağlar.

## <a name="send-cross-platform-notifications-using-templates"></a>Şablonları kullanarak platformlar arası bildirimler gönderme

Şablonları kullanarak platformlar arası bildirimler göndermek için aşağıdakileri yapın:

1. Visual Studio 'daki Çözüm Gezgini, **denetleyiciler** klasörünü genişletin ve ardından RegisterController.cs dosyasını açın.

2. Yeni bir kayıt oluşturan `Put` yöntemdeki kod bloğunu bulun ve ardından `switch` içeriği şu kodla değiştirin:

    ```csharp
    switch (deviceUpdate.Platform)
    {
        case "mpns":
            var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>$(message)</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
            registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "wns":
            toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
            registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "apns":
            var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
            registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
            break;
        case "fcm":
            var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
            registration = new FcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }
    ```

    Bu kod, yerel kayıt yerine bir şablon kaydı oluşturmak için platforma özgü yöntemi çağırır. Şablon kayıtları yerel kayıtlardan türetiğinden, var olan kayıtları değiştirmenize gerek yoktur.

3. `Notifications` Denetleyicisinde `sendNotification` yöntemini aşağıdaki kodla değiştirin:

    ```csharp
    public async Task<HttpResponseMessage> Post()
    {
        var user = HttpContext.Current.User.Identity.Name;
        var userTag = "username:" + user;

        var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
        await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

    Bu kod, yerel yük belirtmek zorunda kalmadan tüm platformlara aynı anda bir bildirim gönderir. Notification Hubs, kayıtlı şablonlarda belirtildiği gibi, belirtilen *etiket* değeri ile her cihaza doğru yükü oluşturur ve gönderir.

4. WebApi arka uç projenizi yeniden yayımlayın.

5. İstemci uygulamasını yeniden çalıştırın ve kaydın başarılı olduğunu doğrulayın.

6. Seçim İstemci uygulamasını ikinci bir cihaza dağıtın ve uygulamayı çalıştırın.
    Her cihazda bir bildirim görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticiyi tamamladığınıza göre, bu konularda Notification Hubs ve şablonlar hakkında daha fazla bilgi edinebilirsiniz:

* [Use Notification Hubs to send breaking news]: Demonstrates another scenario for using templates.
* [Azure Notification Hubs genel bakış][Templates]: Şablonlar hakkında daha ayrıntılı bilgi içerir.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Push to users ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Push to users Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Visual Studio 2012 Express for Windows 8]: https://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: https://go.microsoft.com/fwlink/p/?LinkId=314257
[Kullanıcılara Notification Hubs bildirme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: https://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: https://msdn.microsoft.com/library/windowsazure/jj927172.aspx
