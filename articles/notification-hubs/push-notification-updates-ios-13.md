---
title: Azure Notification Hubs iOS 13 güncelleştirmeleri | Microsoft Docs
description: Azure Notification Hubs iOS 13 ile ilgili son değişiklikler hakkında bilgi edinin
author: sethmanheim
ms.author: sethm
ms.date: 10/16/2019
ms.topic: article
ms.service: notification-hubs
ms.reviewer: jowargo
ms.lastreviewed: 10/16/2019
ms.openlocfilehash: 697e8ba9c9f27e8d5644e3a78950ff006290efe7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74228136"
---
# <a name="azure-notification-hubs-updates-for-ios-13"></a>İOS 13 için Azure Notification Hubs güncelleştirmeleri

Apple yakın zamanda genel gönderim hizmetinde bazı değişiklikler yaptı; değişiklikler genellikle iOS 13 ve Xcode sürümleriyle hizalanır. Bu makalede, bu değişikliklerin Azure Notification Hubs üzerindeki etkileri açıklanmaktadır.

## <a name="apns-push-payload-changes"></a>APNS itme yükü değişiklikleri

### <a name="apns-push-type"></a>APNS gönderim türü

Apple artık geliştiricilerin, APNS API 'sindeki yeni `apns-push-type` üst bilgi aracılığıyla bildirimleri bir uyarı veya arka plan bildirimleri olarak belirlemesini gerektirir. [Apple belgelerine](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns)göre: "bu üstbilginin değeri, bildirimin yükünün içeriğini doğru şekilde yansıtmalıdır. Bir uyuşmazlık varsa veya gerekli sistemlerde üst bilgi eksikse, APNs bir hata döndürebilir, bildirimin teslimini erteleyebilir veya onu tamamen bırakabilir. "

Geliştiricilerin artık bu üstbilgiyi Azure Notification Hubs aracılığıyla bildirim gönderen uygulamalarda ayarlaması gerekir. Teknik bir sınırlama nedeniyle, müşteriler bu özniteliği içeren isteklerle APNS kimlik bilgileri için belirteç tabanlı kimlik doğrulaması kullanmalıdır. APNS kimlik bilgileriniz için sertifika tabanlı kimlik doğrulaması kullanıyorsanız, belirteç tabanlı kimlik doğrulaması ile geçiş yapmanız gerekir.

Aşağıdaki kod örnekleri, Azure Notification Hubs aracılığıyla gönderilen bildirim isteklerinde bu üstbilgi özniteliğinin nasıl ayarlanacağını göstermektedir.

#### <a name="template-notifications---net-sdk"></a>Şablon bildirimleri-.NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var tempprop = new Dictionary<string, string> {{"message", "value"}};
var notification = new TemplateNotification(tempprop);
notification.Headers = headers;
await hub.SendNotificationAsync(notification);
```

#### <a name="native-notifications---net-sdk"></a>Yerel bildirimler-.NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var notification = new AppleNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

#### <a name="direct-rest-calls"></a>Doğrudan REST çağrıları

```csharp
var request = new HttpRequestMessage(method, $"<resourceUri>?api-version=2017-04");
request.Headers.Add("Authorization", createToken(resourceUri, KEY_NAME, KEY_VALUE));
request.Headers.Add("ServiceBusNotification-Format", "apple");
request.Headers.Add("apns-push-type", "alert");
```

Bu geçiş sırasında size yardımcı olmak için, Azure Notification Hubs `apns-push-type` küme olmayan bir bildirim algıladığında, hizmet bildirim isteğinden gönderim türünü belirler ve değeri otomatik olarak ayarlar. Gerekli üstbilgiyi ayarlamak için Azure Notification Hubs belirteç tabanlı kimlik doğrulaması kullanacak şekilde yapılandırmanız gerektiğini unutmayın; daha fazla bilgi için bkz. [APNs Için belirteç tabanlı (http/2) kimlik doğrulaması](notification-hubs-push-notification-http2-token-authentification.md).

## <a name="apns-priority"></a>APNS önceliği

Başka bir küçük değişiklik, ancak bildirim gönderen arka uç uygulamasında bir değişiklik gerektiren bir arka plan bildirimleri için `apns-priority` üst bilginin 5 olarak ayarlanması gerekir. Birçok uygulama, `apns-priority` üst bilgiyi 10 ' a (hemen teslim olduğunu gösterir) ya da ayarlamazsanız ve varsayılan değeri (aynı zamanda 10) alır.

Arka plan bildirimleri için bu değerin 10 olarak ayarlanmasına artık izin verilmez ve her istek için değeri ayarlamanız gerekir. Bu değer eksikse Apple, arka plan bildirimleri teslim etmez. Örneğin:

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "background"}, { "apns-priority", "5" }};
var notification = new AppleNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

## <a name="sdk-changes"></a>SDK değişiklikleri

İOS geliştiricileri, bir arka uç uygulamasının `description` cihaza bildirim göndermek `deviceToken` için kullandığı anında iletme belirtecini ayıklamak için gönderme belirteci temsilcisine gönderilen verilerin özniteliğini kullanıyordu. Xcode 11 ile, bu `description` öznitelik farklı bir biçimde değiştirilmiştir. Geliştiricilerin bu öznitelik için kullandığı mevcut kod artık kopuk. Azure Notification Hubs SDK 'sını bu değişikliğe uyum sağlayacak şekilde güncelleştirdik, bu nedenle lütfen uygulamalarınız tarafından kullanılan SDK 'Yı [azure Notification Hubs IOS SDK 'sının](https://github.com/Azure/azure-notificationhubs-ios)2.0.4 veya daha yeni bir sürümüne güncelleştirin.
