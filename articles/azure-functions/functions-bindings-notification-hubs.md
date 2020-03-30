---
title: Azure İşlevleri için Bildirim Hub'ları bağlamaları
description: Azure İşlevlerinde Azure Bildirim Hub bağlamasını nasıl kullanacağız öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 211f8c8a203b81a4df6a8e9515b403f99cec572a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277290"
---
# <a name="notification-hubs-output-binding-for-azure-functions"></a>Azure İşlevleri için Bildirim Hub'ları çıktısı bağlama

Bu makalede, Azure İşlevlerinde [Azure Bildirim Hub'ları](../notification-hubs/notification-hubs-push-notification-overview.md) bağlamalarını kullanarak anında iletme bildirimlerinin nasıl gönderilen açıklanmaktadır. Azure İşlevleri, Bildirim Hub'ları için çıktı bağlamalarını destekler.

Azure Bildirim Hub'ları, kullanmak istediğiniz Platform Bildirimleri Hizmeti (PNS) için yapılandırılmalıdır. Bildirim Hub'larından müşteri uygulamanızda anında iletme bildirimleri almayı öğrenmek için Bildirim [Hub'ları ile başlarken](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) bakın ve sayfanın üst kısmındaki açılır listeden hedef istemci platformunuzu seçin.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!IMPORTANT]
> Google [Firebase Bulut Mesajlaşma (FCM) lehine Google Cloud Mesajlaşma (GCM) deprecated](https://developers.google.com/cloud-messaging/faq)vardır. Bu çıktı bağlama FCM desteklemez. FCM kullanarak bildirim göndermek için [Firebase API'sini](https://firebase.google.com/docs/cloud-messaging/server#choosing-a-server-option) doğrudan işlevinizde kullanın veya [şablon bildirimlerini](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)kullanın.

## <a name="packages---functions-1x"></a>Paketler - Fonksiyonlar 1.x

Bildirim Hub'ları ciltlemeleri [Microsoft.Azure.WebJobs.Extensions.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs) NuGet paketi, sürüm 1.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.NotificationHubs) GitHub deposundadır.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler - Fonksiyonlar 2.x ve üzeri

Bu bağlama 2.x ve üzeri işlevlerde kullanılamaz.

## <a name="example---template"></a>Örnek - şablon

Gönderdiğiniz bildirimler yerel bildirimler veya [şablon bildirimleri](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)olabilir. Yerel bildirimler, çıktı bağlama `platform` özelliğinde yapılandırılan belirli bir istemci platformını hedeflemektedir. Şablon bildirimi birden çok platformu hedeflemek için kullanılabilir.   

Dile özel örneğe bakın:

* [C# komut dosyası - dışarı parametre](#c-script-template-example---out-parameter)
* [C# komut dosyası - asynchronous](#c-script-template-example---asynchronous)
* [C# komut dosyası - JSON](#c-script-template-example---json)
* [C# komut dosyası - kitaplık türleri](#c-script-template-example---library-types)
* [F#](#f-template-example)
* [Javascript](#javascript-template-example)

### <a name="c-script-template-example---out-parameter"></a>C# komut dosyası şablonu örneği - parametre çıkış

Bu örnek, şablonda yer tutucu `message` içeren bir şablon [kaydı](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) için bildirim gönderir.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

### <a name="c-script-template-example---asynchronous"></a>C# komut dosyası şablonu örneği - asynchronous

Eşzamanlı kod kullanıyorsanız, dışarı parametrelere izin verilmez. Bu durumda `IAsyncCollector` şablon bildiriminizi döndürmek için kullanın. Aşağıdaki kod yukarıdaki kodun eşzamanlı bir örneğidir. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

### <a name="c-script-template-example---json"></a>C# komut dosyası şablonu örneği - JSON

Bu örnek, geçerli [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) bir JSON `message` dizesi kullanarak şablonda yer tutucu içeren bir şablon kaydı için bildirim gönderir.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

### <a name="c-script-template-example---library-types"></a>C# komut dosyası şablonu örneği - kitaplık türleri

Bu örnek, [Microsoft Azure Bildirim Hub'ları Kitaplığı'nda](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)tanımlanan türlerin nasıl kullanılacağını gösterir. 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

### <a name="f-template-example"></a>F# şablon örneği

Bu örnek, içeren `location` bir [şablon kaydı](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) için bir bildirim gönderir ve `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

### <a name="javascript-template-example"></a>JavaScript şablon örneği

Bu örnek, içeren `location` bir [şablon kaydı](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) için bir bildirim gönderir ve `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if (myTimer.IsPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="example---apns-native"></a>Örnek - APNS yerel

Bu C# komut dosyası örneği, yerel bir APNS bildiriminin nasıl gönderilebildiğini gösterir. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.LogInformation($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.LogInformation($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="example---wns-native"></a>Örnek - WNS yerli

Bu C# komut dosyası örneği, yerel bir WNS tost bildirimi göndermek için [Microsoft Azure Bildirim Hub'ları Kitaplığı'nda](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) tanımlanan türlerin nasıl kullanılacağını gösterir. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="attributes"></a>Öznitelikler

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [NotificationHub](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucu parametreleri ve özellikleri [yapılandırma](#configuration) bölümünde açıklanmıştır.

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `NotificationHub` yapılandırma özellikleri ve öznitelik açıklanmaktadır:

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** |yok| Ayarlanmış `notificationHub`olmalı. |
|**Yön** |yok| Ayarlanmış `out`olmalı. | 
|**Adı** |yok| Bildirim hub iletisi için işlev kodunda kullanılan değişken adı. |
|**tagExpression** |**TagExpression** | Etiket ifadeleri, bildirimlerin etiket ifadesiyle eşleşen bildirimleri almak için kaydolmuş aygıtlar kümesine teslim edilmesini belirtmenize olanak tanır.  Daha fazla bilgi için [Yönlendirme ve etiket ifadeleri'ne](../notification-hubs/notification-hubs-tags-segment-push-message.md)bakın. |
|**hubName** | **HubName** | Azure portalındaki bildirim merkezi kaynağının adı. |
|**bağlantı** | **ConnectionStringSetting** | Bildirim Hub'ları bağlantı dizesi içeren bir uygulama ayarının adı.  Bağlantı dizesi, bildirim hub'ınız için *Varsayılan FullSharedAccessSignature* değerine ayarlanmalıdır. Bu makalenin ilerleyen saatlerinde [Bağlantı dizesi kurulumuna](#connection-string-setup) bakın.|
|**Platform** | **Platform** | Platform özelliği, bildirim hedeflerinizin istemci platformunu gösterir. Varsayılan olarak, platform özelliği çıktı bağlamadan atlanırsa, şablon bildirimleri Azure Bildirim Hub'ında yapılandırılan herhangi bir platformu hedeflemek için kullanılabilir. Azure Bildirim Hub'ı ile platform lar arası bildirimler göndermek için genel olarak şablon kullanma hakkında daha fazla bilgi için [Şablonlar'a](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)bakın. Ayarlandığında, **platform** aşağıdaki değerlerden biri olmalıdır: <ul><li><code>apns</code>&mdash;Apple Anında Iletme Bildirim Hizmeti. APNS bildirim hub'ını yapılandırma ve bildirimin bir istemci uygulamasında alınması hakkında daha fazla bilgi için azure [bildirim hub'ları ile iOS'a anında iletme bildirimleri gönderme](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)'ye bakın.</li><li><code>adm</code>&mdash;[Amazon Cihaz Mesajlaşma](https://developer.amazon.com/device-messaging). ADM bildirim hub'ını yapılandırma ve bildirimi kindle uygulamasında alma hakkında daha fazla bilgi için [Kindle uygulamaları için Bildirim Hub'larıyla Başlarken'e](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)bakın.</li><li><code>wns</code>&mdash;Windows platformlarını hedefleyen [Windows Anında Bildirim Hizmetleri.](/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview) Windows Phone 8.1 ve daha sonra da WNS tarafından desteklenir. Daha fazla bilgi için Windows [Evrensel Platform Uygulamaları için Bildirim Hub'ları ile başlarken](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)bakın.</li><li><code>mpns</code>&mdash;[Microsoft Push Bildirim Hizmeti](/previous-versions/windows/apps/ff402558(v=vs.105)). Bu platform Windows Phone 8 ve önceki Windows Phone platformlarını destekler. Daha fazla bilgi için windows [phone'da Azure Bildirim Hub'ları ile anında iletme bildirimleri gönderme'ye](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)bakın.</li></ul> |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="functionjson-file-example"></a>function.json dosya örneği

Burada, *bir function.json* dosyasında bağlayıcı Bildirim Hub'larına bir örnek verilmiştir.

```json
{
  "bindings": [
    {
      "type": "notificationHub",
      "direction": "out",
      "name": "notification",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "apns"
    }
  ],
  "disabled": false
}
```

### <a name="connection-string-setup"></a>Bağlantı dizesi kurulumu

Bildirim merkezi çıktıbağlamasını kullanmak için hub için bağlantı dizesini yapılandırmanız gerekir. Varolan bir bildirim hub'ını seçebilir veya Azure portalındaki *Tümleştir* sekmesinden yeni bir bildirim hub'ı oluşturabilirsiniz. Bağlantı dizesini el ile de yapılandırabilirsiniz. 

Bağlantı dizesini varolan bir bildirim hub'ına yapılandırmak için:

1. [Azure portalındaki](https://portal.azure.com)bildirim merkezinize gidin, **Access ilkelerini**seçin ve **Varsayılan FullSharedAccessSignature** ilkesinin yanındaki kopyalama düğmesini seçin. Bu işlem, *Varsayılan FullSharedAccessSignature* ilkesinin bağlantı dizesini bildirim hub'ınıza kopyalar. Bu bağlantı dizesi, işlevinizin hub'a bildirim iletileri göndermesine olanak tanır.
    ![Bildirim merkezi bağlantı dizesini kopyalama](./media/functions-bindings-notification-hubs/get-notification-hub-connection.png)
1. Azure portalında işlev uygulamanıza gidin, **Uygulama ayarlarını**seçin, **MyHubConnectionString**gibi bir anahtar ekleyin, bildirim hub'ınız için kopyalanan *VarsayılanFullSharedAccessSignature'ı* değer olarak yapıştırın ve ardından **Kaydet'i**tıklatın.

Bu uygulama ayarı *adı, function.json* veya .NET özniteliğindeki çıktı bağlama bağlantı ayarına gider. Bu makalenin önceki [Yapılandırma bölümüne](#configuration) bakın.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama | Başvuru |
|---|---|
| Bildirim Merkezi | [Operasyon Rehberi](https://docs.microsoft.com/rest/api/notificationhubs/) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

