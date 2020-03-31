---
title: Azure Bildirim Hub'larına (ASP.NET) sahip kullanıcılara platform ötesi bildirimler gönderme
description: Bildirim Hub'ları şablonlarını kullanarak, tek bir istekte, tüm platformları hedefleyen bir platform-agnostik bildirim göndermeyi öğrenin.
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
ms.date: 09/30/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: fc3cec348517244c8a7f54d2d3d17298c58e4262
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127041"
---
# <a name="send-cross-platform-notifications-with-azure-notification-hubs"></a>Azure Bildirim Hub'ları ile platform ötesi bildirimler gönderme

Bu öğretici, azure bildirim [hub'larını kullanarak belirli kullanıcılara bildirim gönderme,]önceki öğreticiye dayanmaktadır. Bu öğretici, belirli bir kimlik doğrulaması kullanıcıya kayıtlı tüm aygıtlara bildirimlerin nasıl iletilen açıklanır. Bu yaklaşım, desteklenen her istemci platformuna bildirim göndermek için birden çok istek gerekti. Azure Bildirim Hub'ları, belirli bir aygıtın bildirimleri nasıl almak istediğini belirtebileceğiniz şablonları destekler. Bu yöntem, platformlar arası bildirimler göndermeyi kolaylaştırır.

Bu makalede, tüm platformları hedefleyen bir bildirim göndermek için şablonlardan nasıl yararlanılanın caizdir. Bu makalede, bir platform nötr bildirim göndermek için tek bir istek kullanır. Şablonlar hakkında daha ayrıntılı bilgi için [Bildirim Hub'larına Genel Bakış'a][Templates]bakın.

> [!IMPORTANT]
> Windows Phone 8.1 ve önceki projeleri Visual Studio 2019'da desteklenmez. Daha fazla bilgi için [Visual Studio 2019 Platform Hedefleme ve Uyumluluk](/visualstudio/releases/2019/compatibility)bilgisine bakın.

> [!NOTE]
> Bildirim Hub'ları ile aygıt aynı etiketi kullanarak birden çok şablon kaydedebilir. Bu durumda, etiketi hedefleyen gelen bir ileti, aygıta her şablon için bir tane olmak üzere birden çok bildirimin gönderilmesiyle sonuçlanır. Bu işlem, aynı iletiyi hem rozet hem de bir Windows Mağazası uygulamasında tost bildirimi gibi birden çok görsel bildirimde görüntülemenize olanak tanır.

## <a name="send-cross-platform-notifications-using-templates"></a>Şablonları kullanarak platform ötesi bildirimler gönderme

Bu bölümde, [Azure Bildirim Hub'ları] öğreticisini kullanarak belirli kullanıcılara bildirim gönder'de oluşturduğun örnek kod kullanır. Örneği [GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/NotifyUsers)'dan indirebilirsiniz.

Şablonları kullanarak platformlar arası bildirimler göndermek için aşağıdaki adımları yapın:

1. **Solution Explorer'daki**Visual Studio'da **Denetleyiciler** klasörünü genişletin ve *RegisterController.cs* dosyasını açın.

1. Yeni bir kayıt oluşturan `Put` yöntemde kod bloğunu bulun ve `switch` ardından içeriği aşağıdaki kodla değiştirin:

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

    Bu kod, yerel kayıt yerine şablon kaydı oluşturmak için platforma özgü yöntemi çağırır. Şablon kayıtları yerel kayıtlardan türediği için, varolan kayıtları değiştirmeniz gerekmez.

1. **Çözüm Gezgini'nde,** **Denetleyiciler** klasöründe *NotificationsController.cs* dosyasını açın. `Post` yöntemini aşağıdaki kod ile değiştirin:

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

    Bu kod, tüm platformlara aynı anda bildirim gönderir. Yerel bir yük belirtmedin. Bildirim Hub'ları, kayıtlı şablonlarda belirtildiği gibi, sağlanan etiket değerine sahip her cihaza doğru yükü oluşturur ve teslim eder.

1. Web API projenizi yeniden yayımlayın.

1. Kaydın başarılı olduğunu doğrulamak için istemci uygulamasını yeniden çalıştırın.

1. İsteğe bağlı olarak, istemci uygulamasını ikinci bir cihaza dağıtın ve ardından uygulamayı çalıştırın. Her aygıtta bir bildirim görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticiyi tamamladığınızda, bildirim hub'ları ve şablonları hakkında daha fazla bilgi edinin:

* Şablonları kullanmak için farklı bir senaryo için, Evrensel Windows Platformu uygulamaları öğretici [çalıştıran belirli Windows aygıtlarına Anında Bildirimleri'ne][Use Notification Hubs to send breaking news] bakın.
* Şablonlar hakkında daha ayrıntılı bilgi için [Bildirim Hub'larına Genel Bakış'a][Templates]bakın.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Push to users ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Push to users Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Visual Studio 2012 Express for Windows 8]: https://visualstudio.microsoft.com/downloads/

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: https://go.microsoft.com/fwlink/p/?LinkId=314257
[Azure Bildirim Hub'larını kullanarak belirli kullanıcılara bildirim gönderme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: https://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: https://msdn.microsoft.com/library/windowsazure/jj927172.aspx
