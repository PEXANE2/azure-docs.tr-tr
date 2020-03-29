---
title: Azure Bildirim Hub'ları ve Node.js ile anında iletme bildirimleri gönderme
description: Bir Düğüm.js uygulamasından anında iletme bildirimleri göndermek için Bildirim Hub'larını nasıl kullanacağınızı öğrenin.
keywords: push notification,push bildirimleri,düğüm.js push,ios push
services: notification-hubs
documentationcenter: nodejs
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 6e109c5a7f4911893c81c88ae84322fb962fff6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71213199"
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Azure Bildirim Hub'ları ve Node.js ile anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa, [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs)Sürümü aracılığıyla birkaç dakika içinde ücretsiz bir deneme hesabı oluşturun.

Bu kılavuz, doğrudan bir [Düğüm.js](https://nodejs.org) uygulamasından Azure Bildirim Hub'ları yardımıyla anında iletme bildirimlerinin nasıl gönderileceğini gösterir.

Kapsanan senaryolar arasında aşağıdaki platformlardaki uygulamalara anında iletme bildirimleri gönderilmesi yer almaktadır:

- Android
- iOS
- Evrensel Windows Platformu
- Windows Phone

## <a name="notification-hubs"></a>Notification Hubs

Azure Bildirim Hub'ları, mobil cihazlara anında iletme bildirimleri göndermek için kullanımı kolay, çok platformlu, ölçeklenebilir bir altyapı sağlar. Hizmet altyapısıyla ilgili ayrıntılar için [Azure Bildirim Hub'ları](https://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sayfasına bakın.

## <a name="create-a-nodejs-application"></a>Düğüm Oluşturma.js Uygulaması

Bu öğreticinin ilk adımı yeni bir boş Düğüm.js uygulaması oluşturmaktır. Bir [Node.js Cloud Service][Node.js Cloud Service] Düğüm.js uygulaması oluşturma yla [Create and deploy a Node.js application to Azure Web Site][nodejswebsite]ilgili talimatlar için bkz. [Web Site with WebMatrix][webmatrix]

## <a name="configure-your-application-to-use-notification-hubs"></a>Uygulamanızı Bildirim Hub'larını Kullanacak Şekilde Yapılandırın

Azure Bildirim Hub'larını kullanmak için, anında iletme bildirimi REST hizmetleriyle iletişim kuran yerleşik bir yardımcı kitaplıklar kümesi içeren Düğüm.js [azure paketini](https://www.npmjs.com/package/azure)indirmeniz ve kullanmanız gerekir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi almak için Düğüm Paket Yöneticisi'ni (NPM) kullanın

1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Linux) gibi bir komut satırı arabirimi kullanın ve boş uygulamanızı oluşturduğunuz klasöre gidin.
2. Komut `npm install azure-sb` penceresinde yürütün.
3. Bir `node_modules` klasörün oluşturulduğunu `dir` doğrulamak için komutu `ls` el ile çalıştırabilirsiniz.
4. Bu klasörün içinde, Bildirim Hub'ına erişmek için gereken kitaplıkları içeren **azure** paketini bulun.

> [!NOTE]
> Resmi [NPM blogunda NPM](https://blog.npmjs.org/post/85484771375/how-to-install-npm)yükleme hakkında daha fazla bilgi edinebilirsiniz.

### <a name="import-the-module"></a>Modülü alma
Metin düzenleyicisi kullanarak, uygulama `server.js` dosyasının üst bölümüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure-sb');
```

### <a name="set-up-an-azure-notification-hub-connection"></a>Azure Bildirim Hub bağlantısı ayarlama

Nesne, `NotificationHubService` bildirim hub'larıyla çalışmanızı sağlar. Aşağıdaki kod, bildirim `NotificationHubService` hub'ı için `hubname`bir nesne oluşturur. Azure modülünün içe `server.js` aktarılması için deyimden sonra dosyanın üst kısmına ekleyin:

```javascript
var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');
```

Aşağıdaki adımları `connectionstring` gerçekleştirerek [Azure portalından] bağlantı değerini edinin:

1. Sol daki gezinti bölmesinde **Gözat'ı**tıklatın.
2. **Bildirim Hub'larını**seçin ve ardından örnek için kullanmak istediğiniz hub'ı bulun. Yeni bir Bildirim Hub'ı oluşturma konusunda yardıma ihtiyacınız [varsa, Windows Mağazası'na Başlarken öğreticisine](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) başvurabilirsiniz.
3. **Ayarlar'ı**seçin.
4. Erişim **İlkeleri'ni**tıklatın. Hem paylaşılan hem de tam erişim bağlantı dizelerini görürsünüz.

![Azure portalı - Bildirim Hub'ları](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Ayrıca, [Azure PowerShell](/powershell/azureps-cmdlets-docs) tarafından sağlanan **Get-AzureSbNamespace** cmdlet'ini veya Azure Komut Satırı [Arabirimi (Azure CLI)](../cli-install-nodejs.md)ile azure **sb ad alanı göster** komutunu kullanarak bağlantı dizesini de alabilirsiniz.

## <a name="general-architecture"></a>Genel mimari

Nesne, `NotificationHubService` belirli aygıtlara ve uygulamalara anında iletme bildirimleri göndermek için aşağıdaki nesne örneklerini ortaya çıkarır:

- **Android** - `GcmService` mevcut nesne, kullanın`notificationHubService.gcm`
- **iOS** - `ApnsService` erişilebilen nesneyi kullanın`notificationHubService.apns`
- **Windows Phone** - `MpnsService` kullanılabilir nesne, kullanın`notificationHubService.mpns`
- **Evrensel Windows Platformu** `WnsService` - kullanılabilir nesne, kullanın`notificationHubService.wns`

### <a name="how-to-send-push-notifications-to-android-applications"></a>Nasıl yapilir: Android uygulamalarına anında iletme bildirimleri gönderme

Nesne, `GcmService` Android `send` uygulamalarına anında iletme bildirimleri göndermek için kullanılabilecek bir yöntem sağlar. Yöntem `send` aşağıdaki parametreleri kabul eder:

- **Etiketler** - etiket tanımlayıcısı. Etiket sağlanmazsa, bildirim tüm istemcilere gönderilir.
- **Yük** - iletinin JSON veya ham dize yükü.
- **Geri arama** - geri arama işlevi.

Taşıma yükü biçimi hakkında daha fazla bilgi için [Taşıma Yükü belgelerine](https://distriqt.github.io/ANE-PushNotifications/m.FCM-GCM%20Payload)bakın.

Aşağıdaki kod, `GcmService` tüm kayıtlı `NotificationHubService` istemcilere anında iletme bildirimi göndermek için ortaya çıkan örneği kullanır.

```javascript
var payload = {
  data: {
    message: 'Hello!'
  }
};
notificationHubService.gcm.send(null, payload, function(error){
  if(!error){
    //notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Nasıl yapilir: iOS uygulamalarına anında iletme bildirimleri gönderme

Yukarıda açıklanan Android uygulamalarında olduğu `ApnsService` gibi, `send` nesne de iOS uygulamalarına anında iletme bildirimleri göndermek için kullanılabilecek bir yöntem sağlar. Yöntem `send` aşağıdaki parametreleri kabul eder:

- **Etiketler** - etiket tanımlayıcısı. Etiket sağlanmazsa, bildirim tüm istemcilere gönderilir.
- **Yük** - iletinin JSON veya dize yükü.
- **Geri arama** - geri arama işlevi.

Taşıma biçimi hakkında daha fazla bilgi için Yerel [ve Anında Nakış Bildirim Programlama Kılavuzu](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) belgesinin Bildirim **Yükü** bölümüne bakın.

Aşağıdaki kod, `ApnsService` tüm istemcilere `NotificationHubService` bir uyarı iletisi göndermek için ortaya çıkan örneği kullanır:

```javascript
var payload={
    alert: 'Hello!'
  };
notificationHubService.apns.send(null, payload, function(error){
  if(!error){
      // notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Nasıl yapilir: Windows Phone uygulamalarına anında iletme bildirimleri gönderme

Nesne, `MpnsService` Windows `send` Phone uygulamalarına anında iletme bildirimleri göndermek için kullanılabilecek bir yöntem sağlar. Yöntem `send` aşağıdaki parametreleri kabul eder:

- **Etiketler** - etiket tanımlayıcısı. Etiket sağlanmazsa, bildirim tüm istemcilere gönderilir.
- **Yük** - iletinin XML yükü.
- **TargetName** -  `toast` Tost bildirimleri için TargetName. `token`döşeme bildirimleri için.
- **BildirimSınıfı** - Bildirimin önceliği. Geçerli değerler için [bir sunucu belgesinden Push bildirimleri](https://msdn.microsoft.com/library/hh221551.aspx) **HTTP Üstbilgi Öğeleri** bölümüne bakın.
- **Seçenekler** - isteğe bağlı istek üstbilgi.
- **Geri arama** - geri arama işlevi.

Geçerli `TargetName` `NotificationClass` ve üstbilgi seçeneklerinin listesi için, [sunucu sayfasından Push bildirimlerine](https://msdn.microsoft.com/library/hh221551.aspx) göz atın.

Aşağıdaki örnek kod, `MpnsService` tost `NotificationHubService` push bildirimi göndermek için maruz kalan örneği kullanır:

```javascript
var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
  if(!error){
    //notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Nasıl yapılır: Evrensel Windows Platformu (UWP) uygulamalarına anında iletme bildirimleri gönderme

Nesne, `WnsService` Evrensel `send` Windows Platformu uygulamalarına anında iletme bildirimleri göndermek için kullanılabilecek bir yöntem sağlar.  Yöntem `send` aşağıdaki parametreleri kabul eder:

- **Etiketler** - etiket tanımlayıcısı. Etiket sağlanmazsa, bildirim tüm kayıtlı istemcilere gönderilir.
- **Yük** - XML ileti yükü.
- **Türü** - bildirim türü.
- **Seçenekler** - isteğe bağlı istek üstbilgi.
- **Geri arama** - geri arama işlevi.

Geçerli türlerin ve istek üstbilgilerinin listesi [için, Anında Bildirim hizmeti isteği ve yanıt üstbilgisini](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx)görün.

Aşağıdaki kod, `WnsService` bir UWP `NotificationHubService` uygulamasına tost push bildirimi göndermek için maruz kalan örneği kullanır:

```javascript
var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
  if(!error){
      // notification sent
  }
});
```

## <a name="next-steps"></a>Sonraki Adımlar

Yukarıdaki örnek parçacıklar, çok çeşitli aygıtlara anında iletme bildirimleri sunmak için kolayca hizmet altyapısı oluşturmanıza olanak tanır. Node.js ile Bildirim Hub'larını kullanmanın temellerini öğrendiğinize göre, bu yetenekleri nasıl daha da genişletebileceğiniz hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

- [Azure Bildirim Hub'ları](https://msdn.microsoft.com/library/azure/jj927170.aspx)için MSDN Başvurusu'na bakın.
- Daha fazla örnek ve uygulama ayrıntıları için GitHub'daki Düğüm deposu [için Azure SDK'yı] ziyaret edin.

[Node için Azure SDK]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: https://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: https://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs
[webmatrix]: https://docs.microsoft.com/aspnet/web-pages/videos/introduction/create-a-website-using-webmatrix
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portalı]: https://portal.azure.com
