---
title: Azure Notification Hubs 'de APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması | Microsoft Docs
description: Bu konuda, APNS için yeni belirteç kimlik doğrulamasının nasıl kullanılacağı açıklanmaktadır
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 02/13/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/13/2019
ms.openlocfilehash: a7fdaae33e28bd543b44c54868324339d1269bc2
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213128"
---
# <a name="token-based-http2-authentication-for-apns"></a>APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması

## <a name="overview"></a>Genel Bakış

Bu makalede, yeni APNS HTTP/2 protokolünü belirteç tabanlı kimlik doğrulama ile kullanma hakkında bilgi yer aldığı açıklanır.

Yeni Protokolü kullanmanın başlıca avantajları şunlardır:

* Belirteç oluşturma oldukça sorunsuz (sertifikalarla karşılaştırıldığında)
* Daha fazla sona erme tarihi yok: kimlik doğrulama belirteçlerinizin ve bunların iptallerinin denetimi
* Yük artık en fazla 4 KB olabilir
* Zaman uyumlu geri bildirim
* Apple 'ın en son protokolde kullanıyorsunuz: sertifikalar hala kullanımdan kaldırma için işaretlenen ikili protokolü kullanır

Bu yeni mekanizmanın kullanılması, birkaç dakika içinde iki adımda yapılabilir:

1. Apple Geliştirici Hesap portalından gerekli bilgileri edinin
2. Bildirim Hub 'ınızı yeni bilgilerle yapılandırın

Notification Hubs artık, APNS ile yeni kimlik doğrulama sistemini kullanmak üzere ayarlanmıştır.

APNS için sertifika kimlik bilgilerini kullanarak geçiş yaptıysanız:

* belirteç özellikleri, bizim sistemimizde sertifikanızın üzerine yazılır,
* ancak uygulamanız bildirimleri sorunsuz bir şekilde almaya devam eder.

## <a name="obtaining-authentication-information-from-apple"></a>Apple 'dan kimlik doğrulama bilgileri alma

Belirteç tabanlı kimlik doğrulamasını etkinleştirmek için Apple geliştirici hesabınızdan aşağıdaki özellikler gereklidir:

### <a name="key-identifier"></a>Anahtar Tanımlayıcısı

Anahtar tanımlayıcısı, Apple geliştirici hesabınızdaki "anahtarlar" sayfasından elde edilebilir

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Uygulama tanımlayıcısı & uygulama adı

Uygulama adı, geliştirici hesabındaki uygulama kimlikleri sayfası aracılığıyla kullanılabilir.

![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

Uygulama tanımlayıcısı, geliştirici hesabındaki üyelik ayrıntıları sayfası aracılığıyla kullanılabilir.

![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)

### <a name="authentication-token"></a>Kimlik doğrulama belirteci

Kimlik doğrulama belirteci, uygulamanız için bir belirteç oluşturduktan sonra indirilebilir. Bu belirteci oluşturma hakkında ayrıntılı bilgi için [Apple 'ın geliştirici belgelerine](https://help.apple.com/xcode/mac/current/#/devdfd3d04a1)bakın.

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>Bildirim Hub 'ınızı belirteç tabanlı kimlik doğrulaması kullanacak şekilde yapılandırma

### <a name="configure-via-the-azure-portal"></a>Azure portal aracılığıyla yapılandırma

Portalda belirteç tabanlı kimlik doğrulamasını etkinleştirmek için Azure portal oturum açın ve Bildirim Hizmetleri > APNS paneline > Notification Hub 'ınıza gidin.

Yeni bir özellik – *kimlik doğrulama modu*vardır. Belirteç seçilmesi, merkezinizi ilgili tüm belirteç özellikleriyle güncelleştirmenize olanak tanır.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

* Apple geliştirici hesabınızdan aldığınız özellikleri girin
* Uygulama modlarınızı seçin (üretim veya korumalı alan)
* APNS kimlik bilgilerinizi güncelleştirmek için **Kaydet** düğmesine tıklayın

### <a name="configure-via-management-api-rest"></a>Yönetim API 'SI aracılığıyla yapılandırma (REST)

Bildirim Hub 'ınızı belirteç tabanlı kimlik doğrulaması kullanacak şekilde güncelleştirmek için [Yönetim API](https://msdn.microsoft.com/library/azure/dn495827.aspx) 'lerimizi kullanabilirsiniz.
Yapılandırmakta olduğunuz uygulamanın bir korumalı alan veya üretim uygulaması (Apple Developer hesabınızda belirtilen) olmasına bağlı olarak, ilgili uç noktalardan birini kullanın:

* Korumalı alan uç noktası:[https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)
* Üretim uç noktası:[https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Belirteç tabanlı kimlik doğrulaması için bir API sürümü gerekir: **2017-04 veya üzeri**.

Bir hub 'ı belirteç tabanlı kimlik doğrulama ile güncelleştirmek için bir PUT isteği örneği aşağıda verilmiştir:

    ```text
    PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
      "Properties": {
        "ApnsCredential": {
          "Properties": {
            "KeyId": "<Your Key Id>",
            "Token": "<Your Authentication Token>",
            "AppName": "<Your Application Name>",
            "AppId": "<Your Application Id>",
            "Endpoint":"<Sandbox/Production Endpoint>"
          }
        }
      }
    ```

### <a name="configure-via-the-net-sdk"></a>.NET SDK aracılığıyla yapılandırma

Hub 'ınızı, [en son istemci SDK 'sını](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8)kullanarak belirteç tabanlı kimlik doğrulaması kullanacak şekilde yapılandırabilirsiniz.

Doğru kullanımı gösteren bir kod örneği aşağıda verilmiştir:

```csharp
NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
string token = "YOUR TOKEN HERE";
string keyId = "YOUR KEY ID HERE";
string appName = "YOUR APP NAME HERE";
string appId = "YOUR APP ID HERE";
NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
nm.UpdateNotificationHubAsync(desc);
```

## <a name="reverting-to-using-certificate-based-authentication"></a>Sertifika tabanlı kimlik doğrulaması kullanmaya geri dönme

Herhangi bir önceki yöntemi kullanarak ve belirteç özellikleri yerine sertifikayı geçirerek sertifika tabanlı kimlik doğrulaması kullanmak için dilediğiniz zaman döndürebilirsiniz. Bu eylem, önceden depolanmış kimlik bilgilerinin üzerine yazar.
