---
title: Azure Bildirim Hub'ları iOS 13 güncellemeleri | Microsoft Dokümanlar
description: Azure Bildirim Hub'larında iOS 13 son dakika değişiklikleri hakkında bilgi edinin
author: sethmanheim
ms.author: sethm
ms.date: 10/16/2019
ms.topic: article
ms.service: notification-hubs
ms.reviewer: jowargo
ms.lastreviewed: 10/16/2019
ms.openlocfilehash: 697e8ba9c9f27e8d5644e3a78950ff006290efe7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74228136"
---
# <a name="azure-notification-hubs-updates-for-ios-13"></a>iOS 13 için Azure Bildirim Hub güncellemeleri

Apple son zamanlarda kendi kamu itme hizmeti bazı değişiklikler yaptı; değişiklikler çoğunlukla iOS 13 ve Xcode sürümleri ile uyumlu. Bu makalede, bu değişikliklerin Azure Bildirim Hub'ları üzerindeki etkisi açıklanmaktadır.

## <a name="apns-push-payload-changes"></a>APNS itme yükü değişiklikleri

### <a name="apns-push-type"></a>APNS push türü

Apple artık geliştiricilerin APNS API'deki yeni `apns-push-type` üstbilgi aracılığıyla bildirimleri bir uyarı veya arka plan bildirimleri olarak tanımlamasını gerektirir. [Apple'ın belgelerine](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns)göre : "Bu üstbilginin değeri, bildiriminizin yükünün içeriğini doğru bir şekilde yansıtmalıdır. Bir uyuşmazlık varsa veya üstbilgi gerekli sistemlerde eksikse, APN'ler bir hata döndürebilir, bildirimin teslimini geciktirebilir veya tamamen bırakabilir."

Geliştiricilerin artık bu üstbilgiyi Azure Bildirim Hub'ları üzerinden bildirim gönderen uygulamalarda ayarlamaları gerekir. Teknik bir sınırlama nedeniyle, müşterilerin APNS kimlik bilgileri için bu özniteliği içeren isteklerle belirteç tabanlı kimlik doğrulaması kullanması gerekir. APNS kimlik bilgileriniz için sertifika tabanlı kimlik doğrulaması kullanıyorsanız, belirteç tabanlı kimlik doğrulamasını kullanmaya geçmeniz gerekir.

Aşağıdaki kod örnekleri, azure bildirim hub'ları aracılığıyla gönderilen bildirim isteklerinde bu üstbilgi özniteliğinin nasıl ayarlanır olduğunu gösterir.

#### <a name="template-notifications---net-sdk"></a>Şablon bildirimleri - .NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var tempprop = new Dictionary<string, string> {{"message", "value"}};
var notification = new TemplateNotification(tempprop);
notification.Headers = headers;
await hub.SendNotificationAsync(notification);
```

#### <a name="native-notifications---net-sdk"></a>Yerel bildirimler - .NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var notification = new AppleNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

#### <a name="direct-rest-calls"></a>Doğrudan REST aramaları

```csharp
var request = new HttpRequestMessage(method, $"<resourceUri>?api-version=2017-04");
request.Headers.Add("Authorization", createToken(resourceUri, KEY_NAME, KEY_VALUE));
request.Headers.Add("ServiceBusNotification-Format", "apple");
request.Headers.Add("apns-push-type", "alert");
```

Bu geçiş sırasında size yardımcı olmak için, Azure Bildirim Hub'ları `apns-push-type` kümeye sahip olmayan bir bildirim algıladığında, hizmet anında iletme türünü bildirim isteğinden çıkarve değeri otomatik olarak ayarlar. Gerekli üstbilgiayarlamak için belirteç tabanlı kimlik doğrulaması kullanacak Şekilde Azure Bildirim Hub'larını yapılandırmanız gerektiğini unutmayın; daha fazla bilgi için, [APNS için Belirteç tabanlı (HTTP/2) Kimlik Doğrulaması'na](notification-hubs-push-notification-http2-token-authentification.md)bakın.

## <a name="apns-priority"></a>APNS önceliği

Başka bir küçük değişiklik, ancak bildirimleri gönderen arka uç uygulamasında bir değişiklik gerektiren bir `apns-priority` üstbilgi şimdi 5 olarak ayarlanmalıdır arka plan bildirimleri için gereksinimidir. Birçok uygulama üstbilgi10 `apns-priority` (hemen teslim belirten) ayarlayın veya ayarlamak ve varsayılan değeri (aynı zamanda 10) almak değil.

Bu değeri 10 olarak ayarlamak artık arka plan bildirimleri için izin verilmez ve her istek için değer ayarlamanız gerekir. Bu değer eksikse Apple arka plan bildirimleri sağlamaz. Örnek:

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "background"}, { "apns-priority", "5" }};
var notification = new AppleNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

## <a name="sdk-changes"></a>SDK değişiklikleri

IOS geliştiricileri, bir `description` arka uç `deviceToken` uygulamasının aygıta bildirim göndermek için kullandığı itme belirtecisini ayıklamak için itme belirteci temsilcisine gönderilen verilerin özniteliğini yıllarca kullandı. Xcode 11 ile `description` bu öznitelik farklı bir biçime değiştirildi. Geliştiricilerin bu öznitelik için kullandığı varolan kod artık bozuk. Bu değişikliği karşılamak için Azure Bildirim Hub'ları SDK'yı güncelledik, bu nedenle lütfen uygulamalarınız tarafından kullanılan SDK'yı 2.0.4 sürümüne veya [Azure Bildirim Hub'ları iOS SDK'nın](https://github.com/Azure/azure-notificationhubs-ios)daha yeni sürümüne güncelleyin.
