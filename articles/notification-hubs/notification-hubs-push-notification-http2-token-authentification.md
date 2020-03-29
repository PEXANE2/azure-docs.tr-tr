---
title: Azure Bildirim Hub'larında APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması | Microsoft Dokümanlar
description: APNS için yeni belirteç kimlik doğrulamasını nasıl kullanacağınızı öğrenin.
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
ms.date: 11/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/13/2019
ms.openlocfilehash: 448b5c38371024c2eae900f4f87b343ee0a3b36a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263821"
---
# <a name="token-based-http2-authentication-for-apns"></a>APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması

## <a name="overview"></a>Genel Bakış

Bu makalede, belirteç tabanlı kimlik doğrulama ile yeni APNS HTTP/2 protokolü nasıl kullanılacağı açıklanmaktadır.

Yeni protokolü kullanmanın temel yararları şunlardır:

* Belirteç üretimi nispeten basittir (sertifikalarla karşılaştırıldığında)
* Artık son kullanma tarihi yok – kimlik doğrulama belirteçlerinizi ve bunların iptalini siz kontrol edeyimsiniz
* Yükler artık 4 KB'ye kadar olabilir
* Eşzamanlı geri bildirim
* Apple'ın en son protokolündesiniz – sertifikalar hala amortisman için işaretlenmiş ikili protokolü kullanmaya devam ediyor

Bu yeni mekanizma kullanılarak iki adımda gerçekleştirilebilir:

* Apple Developer hesap portalından gerekli bilgileri alın.
* Bildirim merkezinizi yeni bilgilerle yapılandırın.

Bildirim Hub'ları artık APNS ile yeni kimlik doğrulama sistemini kullanacak şekilde ayarlanmıştır.

APNS için sertifika kimlik bilgilerini kullanarak geçiş yaptığınızda, belirteç özelliklerinin sistemimizde sertifikanızın üzerine yazdığını, ancak uygulamanızın bildirimleri sorunsuz bir şekilde almaya devam ettiğini unutmayın.

## <a name="obtaining-authentication-information-from-apple"></a>Apple'dan kimlik doğrulama bilgileri alma

Belirteç tabanlı kimlik doğrulamasını etkinleştirmek için Apple Developer hesabınızdan aşağıdaki özelliklere ihtiyacınız vardır:

### <a name="key-identifier"></a>Anahtar tanımlayıcısı

Anahtar tanımlayıcı, Apple Developer hesabınızda **Sertifikalar, Tanımlayıcılar & Profiller**altında **Keys** sayfasından elde edilebilir:

![](./media/notification-hubs-push-notification-http2-token-authentification/keys.png)

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier-and-application-name"></a>Uygulama tanımlayıcısı ve başvuru adı

Uygulama adı ve tanımlayıcısı, geliştirici hesabındaki **Sertifikalar, Tanımlayıcılar & Profiller** sayfasında da mevcuttur:

![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

### <a name="configure-via-the-net-sdk-or-the-azure-portal"></a>.NET SDK veya Azure portalı üzerinden yapılandırma

Hub'ınızı [en son istemcimiz SDK'yı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs)kullanarak veya Azure portalında belirteç tabanlı kimlik doğrulaması kullanacak şekilde yapılandırabilirsiniz. Portalda belirteç tabanlı kimlik doğrulamasını etkinleştirmek için Azure portalında oturum açın ve bildirim merkezinizin **Apple (APNS)** paneli > ayarlarına gidin. Hub'ınızı ilgili tüm belirteç özellikleriyle güncellemek için **Kimlik Doğrulama Modu** özelliğinden **Belirteç'i** seçin.

![Belirteci yapılandırma](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

* Apple Developer hesabınızdan aldığınız mülkleri girin.
* Uygulama modunu seçin **(Üretim** veya **Sandbox).**
* APNS kimlik bilgilerinizi güncellemek için **Kaydet** düğmesini tıklatın.

Belirteç tabanlı kimlik bilgileri aşağıdaki alanlardan oluşur:

* **Anahtar Kimliği**: Apple Developer portalında oluşturulan özel anahtarın tanımlayıcısı; örneğin, `2USFGKSKLT`.
* **Takım Kimliği**: "Önek" veya "Uygulama Öneki" olarak da adlandırılır. Bu, Apple Developer portalındaki kuruluşun tanımlayıcısıdır; örneğin, `S4V3D7CHJR`.
* **Paket Kimliği**: "Uygulama Kimliği" olarak da adlandırılır. Bu uygulama için paket tanımlayıcısI; örneğin, `com.microsoft.nhubsample2019`. Birçok uygulama için tek bir tuş kullanabileceğinizi unutmayın. Bu değer, `apns-topic` bildirim gönderirken HTTP üstbilgisine eşlenir ve belirli uygulamayı hedeflemek için kullanılır.
* **Belirteç**: "Anahtar" veya "Özel Anahtar" olarak da adlandırılır. Bu, Apple Developer portalında oluşturulan .p8 dosyasından elde edilir. Anahtar APNS etkin olmalıdır (anahtar oluştururken Apple Developer portalında seçilir). NH Portal/API'ye tedarik ettiğinizde değer, PEM üstbilgisini/altbilgisini ondan çıkarmalıdır.
* **Uç Nokta**: Bildirim Hub'ları portal bıçaklarında ve API'deki bir dize alanında bir geçiştir. Geçerli değerler `https://api.push.apple.com` `https://api.sandbox.push.apple.com`veya . Bildirim Hub'ları bu değeri üretim veya kum havuzu ortamı için bildirimler göndermek için kullanır. Bu, uygulamadaki `aps-environment` yetkilendirmeyle eşleşmelidir, aksi takdirde oluşturulan APNS aygıt belirteçleri ortamla eşleşmiyor ve bildirimler gönderilemez.

Doğru kullanımı gösteren bir kod örneği aşağıda vereyim:

```csharp
NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
string token = "YOUR PRIVATE KEY HERE";
string keyId = "YOUR KEY ID HERE";
string appName = "YOUR APP NAME HERE";
string appId = "YOUR APP ID HERE";
NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
nm.UpdateNotificationHubAsync(desc);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalında Bir Azure bildirim merkezi oluşturma](create-notification-hub-portal.md)
* [Azure portalında bir bildirim hub'ı yapılandırma](create-notification-hub-portal.md)
