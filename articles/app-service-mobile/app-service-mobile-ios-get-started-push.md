---
title: Azure Mobile Apps iOS uygulamasına anında Iletme bildirimleri ekleme
description: Azure Mobile Apps kullanarak iOS uygulamanıza anında iletme bildirimleri gönderme hakkında bilgi edinin.
services: app-service\mobile
documentationcenter: ios
manager: crdun
editor: ''
author: elamalani
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 7a037cfb411eb3c15f60bb6a8763374d9102bc4e
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72027311"
---
# <a name="add-push-notifications-to-your-ios-app"></a>İOS uygulamanıza anında Iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirmeye kadar uçtan uca ve tümleşik hizmetler merkezi 'ni destekler. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir.
> Mobil uygulamanızda bulut hizmetlerini tümleştirmek istiyorsanız bugün App Center [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) kaydolun.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir kayıt her eklendiğinde cihaza anında iletme bildirimi gönderilmesi için [iOS hızlı başlangıç] projesine anında iletme bildirimleri eklersiniz.

İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, anında iletme bildirimi uzantı paketine ihtiyacınız olacaktır. Daha fazla bilgi için bkz. [Azure için .net arka uç sunucu SDK Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Kılavuzu.

[İOS simülatörü anında iletme bildirimlerini desteklemez](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Fiziksel bir iOS cihazına ve bir [Apple Geliştirici Programı üyeliğine](https://developer.apple.com/programs/ios/)ihtiyacınız vardır.

## <a name="configure-hub"></a>Bildirim Hub 'ını yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Uygulamayı anında iletme bildirimleri için Kaydet

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure 'ı anında iletme bildirimleri gönderecek şekilde yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Anında iletme bildirimleri göndermek için arka ucu güncelleştirme

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Uygulamaya anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Anında iletme bildirimlerini sına

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Daha fazla

* Şablonlar, platformlar arası gönderimleri ve yerelleştirilmiş gönderimleri gönderme esnekliği sunar. [Azure Için IOS Istemci kitaplığı 'Nı kullanma Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) şablonları nasıl kaydedeceğinizi gösterir.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS hızlı başlangıç]: app-service-mobile-ios-get-started.md
