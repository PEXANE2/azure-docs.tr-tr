---
title: iOS'a Anında Iletme Bildirimleri Ekleme
description: iOS uygulamanıza anında iletme bildirimleri göndermek için Azure Mobil Uygulamalarını nasıl kullanacağınızı öğrenin.
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: ad58ee158a2e1842c1b41db281bdd9f04b9b2ca4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461513"
---
# <a name="add-push-notifications-to-your-ios-app"></a>iOS Uygulamanıza Anında İtme Bildirimleri Ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [iOS hızlı başlatma] projesine anında iletme bildirimleri eklersiniz, böylece her kayıt ekildiğinde aygıta bir anında iletme bildirimi gönderilir.

İndirilen quickstart sunucu projesini kullanmazsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olacaktır. Daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) kılavuzuna bakın.

[iOS simülatörü anında iletme bildirimlerini desteklemiyor.](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html) Fiziksel bir iOS aygıtına ve [Apple Geliştirici Programı üyeliğine](https://developer.apple.com/programs/ios/)ihtiyacınız vardır.

## <a name="configure-notification-hub"></a><a name="configure-hub"></a>Bildirim Merkezini Yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-app-for-push-notifications"></a><a id="register"></a>Anında iletme bildirimleri için uygulama kaydedin

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure'u yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-backend-to-send-push-notifications"></a><a id="update-server"></a>Anında iletme bildirimleri göndermek için arka ucunu güncelleştirme

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="add-push-notifications-to-app"></a><a id="add-push"></a>Uygulamaya anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a name="test-push-notifications"></a><a id="test"></a>Anında iletme bildirimlerini test etme

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a name="more"></a><a id="more"></a>Daha fazla

* Şablonlar, platformlar arası itme ler ve yerelleştirilmiş itmeler gönderme esnekliği sağlar. [Azure Mobil Uygulamaları için iOS İstemci Kitaplığı nasıl kullanılır,](app-service-mobile-ios-how-to-use-client-library.md#templates) şablonları nasıl kaydedebilirsiniz gösterir.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS hızlı başlat]: app-service-mobile-ios-get-started.md
