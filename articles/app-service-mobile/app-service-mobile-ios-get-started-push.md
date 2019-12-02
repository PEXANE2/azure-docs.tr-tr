---
title: İOS 'a anında Iletme bildirimleri ekleme
description: Azure Mobile Apps kullanarak iOS uygulamanıza anında iletme bildirimleri gönderme hakkında bilgi edinin.
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 2864dfca72c91acd10fd6a4ee8038040ba5defc8
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668810"
---
# <a name="add-push-notifications-to-your-ios-app"></a>İOS uygulamanıza anında Iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center mobil uygulama dağıtımında merkezi konumdaki uçtan uca ve tümleşik hizmetleri destekler. Geliştiriciler Sürekli Tümleştirme ve Teslim işlem hattını ayarlamak için **Oluşturma**, **Test** ve **Dağıtım** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **Analiz** ve **Tanılama** hizmetlerini kullanarak uygulamanın durumunu ve kullanımını izleyebilir, **Gönderme** hizmetini kullanarak kullanıcılarla etkileşim kurabilir. Geliştiriciler ayrıca kullanıcıların kimliğini doğrulamak için **Kimlik Doğrulaması**'ndan ve uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için **Veri** hizmetinden yararlanabilir.
>
> Bulut hizmetlerini mobil uygulamanızla tümleştirmek istiyorsanız [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc)'a hemen kaydolun.

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
