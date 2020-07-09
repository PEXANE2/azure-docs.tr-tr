---
title: Arka uç hizmeti aracılığıyla Azure Notification Hubs kullanarak flutter uygulamalarına anında iletme bildirimleri gönderin | Microsoft Docs
description: Arka uç hizmeti aracılığıyla Azure Notification Hubs kullanan flutter uygulamalarına anında iletme bildirimleri gönderme hakkında bilgi edinin.
author: mikeparker104
ms.service: notification-hubs
ms.topic: tutorial
ms.date: 07/07/2020
ms.author: miparker
ms.openlocfilehash: 5fa753a6b8b1284c4f8fcd046f74fabcbae3f8fb
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171015"
---
# <a name="tutorial-send-push-notifications-to-flutter-apps-using-azure-notification-hubs-via-a-backend-service"></a>Öğretici: arka uç hizmeti aracılığıyla Azure Notification Hubs kullanarak flutter uygulamalarına anında iletme bildirimleri gönderme  

[![Örnek indirin örnek ](./media/notification-hubs-backend-service-flutter/download.png) indirin](https://github.com/xamcat/mobcat-samples/tree/master/notification_hub_backend_service)  

> [!div class="op_single_selector"]
>
> * [Xamarin.Forms](notification-hubs-backend-service-xamarin-forms.md)
> * [Flutter](notification-hubs-backend-service-flutter.md)
> * [React Native](notification-hubs-backend-service-react-native.md)

Bu öğreticide, **Android** ve **iOS**'u hedefleyen bir [flutter](https://flutter.dev) uygulamasına anında iletme bildirimleri göndermek için [Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-overview) kullanırsınız.  

[!INCLUDE [Notification Hubs Backend Service Introduction](../../includes/notification-hubs-backend-service-introduction.md)]

Bu öğreticide aşağıdaki adımları izleyebilirsiniz:

> [!div class="checklist"]
>
> * [Anında Iletme Bildirim Hizmetleri ve Azure Notification Hubs ayarlayın.](#set-up-push-notification-services-and-azure-notification-hub)
> * [ASP.NET Core Web API arka uç uygulaması oluşturun.](#create-an-aspnet-core-web-api-backend-application)
> * [Platformlar arası flutter uygulaması oluşturun.](#create-a-cross-platform-flutter-application)
> * [Yerel Android projesini anında iletme bildirimleri için yapılandırın.](#configure-the-native-android-project-for-push-notifications)
> * [Yerel iOS projesini anında iletme bildirimleri için yapılandırın.](#configure-the-native-ios-project-for-push-notifications)
> * [Çözümü test edin.](#test-the-solution)

## <a name="prerequisites"></a>Ön koşullar

Bu arada izlemek için şunları yapmanız gerekir:

* Kaynakları oluşturabileceğiniz ve yönetebileceğiniz bir [Azure aboneliği](https://portal.azure.com) .
* [Flutter](https://flutter.dev/docs/get-started/install) araç seti (önkoşulları ile birlikte).
* [Flutter ve dart eklentileri](https://flutter.dev/docs/get-started/editor?tab=vscode) yüklü [Visual Studio Code](https://code.visualstudio.com) .
* Uygulamayı **Android** (fiziksel veya öykünücü cihazlar) ya da **iOS** (yalnızca fiziksel cihazlar) üzerinde çalıştırma özelliği.

Android için şunları yapmanız gerekir:

* Bir geliştirici fiziksel cihaz ya da öykünücü *(API 26 ve üzeri Google Play Hizmetleri yüklü) olarak*açıldı.

İOS için, şunları yapmanız gerekir:

* Etkin bir [Apple geliştirici hesabı](https://developer.apple.com).
* [Geliştirici hesabınıza kayıtlı](https://help.apple.com/developer-account/#/dev40df0d9fa) olan bir fiziksel iOS cihazı *(iOS 13,0 ve üzeri çalıştıran)*.
* **Anahtarınıza** yüklenmiş bir **. p12** [geliştirme sertifikası](https://help.apple.com/developer-account/#/dev04fd06d56) , bir [uygulamayı fiziksel bir cihazda çalıştırmanızı](https://help.apple.com/xcode/mac/current/#/dev5a825a1ca)sağlar.
* [Cocoapods](https://guides.cocoapods.org/using/getting-started.html#installation) , kitaplık bağımlılıklarını yönetmek için yüklendi.

> [!NOTE]
> İOS simülatörü uzak bildirimleri desteklemez ve bu nedenle iOS üzerinde bu örnek araştırılırken fiziksel bir cihaz gereklidir. Ancak, bu öğreticiyi tamamlayabilmeniz için uygulamayı hem **Android** hem de **iOS** üzerinde çalıştırmanız gerekmez.

Önceki deneyimler olmadan bu ilk ilkeler örneğinde bulunan adımları izleyebilirsiniz. Ancak, aşağıdaki yönlere sahip olmanın avantajlarından yararlanabilirsiniz.

* [Apple geliştirici portalı](https://developer.apple.com)
* [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1)
* [Azure Bildirim Merkezleri](notification-hubs-push-notification-overview.md)
* [Google Firebase konsolu](https://console.firebase.google.com/u/0/)
* Platformlar arası geliştirme için [flutter](https://flutter.dev) ve [Dart](https://dart.dev)
* Android ve iOS yerel geliştirme için [Kotlin](https://kotlinlang.org) ve [Swift](https://developer.apple.com/swift)

Belirtilen adımlar [MacOS](https://developer.apple.com/macos)'a özgüdür. **İOS** yönlerini atlayarak [Windows](https://www.microsoft.com/windows) üzerinde takip etmek mümkündür.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Anında Iletme Bildirim Hizmetleri ve Azure Notification Hub 'ı ayarlama

Bu bölümde, **[Firebase Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging)** ve **[Apple Push bildirim hizmetleri (APNs)](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)** ayarlarsınız. Daha sonra bu hizmetlerle çalışmak için bir Bildirim Hub 'ı oluşturup yapılandırırsınız.

[!INCLUDE [Create a Firebase project and enable Firebase Cloud Messaging](../../includes/notification-hubs-common-enable-firebase-cloud-messaging.md)]

[!INCLUDE [Enable Apple Push Notifications](../../includes/notification-hubs-common-enable-apple-push-notifications.md)]

[!INCLUDE [Create a notification hub](../../includes/notification-hubs-common-create-notification-hub.md)]

[!INCLUDE [Configure your notification hub with APNs information](../../includes/notification-hubs-common-configure-with-apns-information.md)]

[!INCLUDE [Configure your notification hub with FCM information](../../includes/notification-hubs-common-configure-with-fcm-information.md)]

## <a name="create-an-aspnet-core-web-api-backend-application"></a>ASP.NET Core Web API arka uç uygulaması oluşturma

Bu bölümde, [cihaz kaydını](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) Işleyecek ve flutter mobil uygulamasına bildirimlerin gönderilmesi Için [ASP.NET Core Web API](https://dotnet.microsoft.com/apps/aspnet/apis) arka ucu oluşturacaksınız.

[!INCLUDE [Create an ASP.NET Core Web API backend application](../../includes/notification-hubs-backend-service-web-api.md)]

## <a name="create-a-cross-platform-flutter-application"></a>Platformlar arası flutter uygulaması oluşturma

Bu bölümde, bir platformlar arası şekilde anında iletme bildirimleri uygulayan bir [flutter](https://flutter.dev) mobil uygulaması oluşturacaksınız.

[!INCLUDE [Sample application generic overview](../../includes/notification-hubs-backend-service-sample-app-overview.md)]

[!INCLUDE [Create Flutter application](../../includes/notification-hubs-backend-service-sample-app-flutter.md)]

## <a name="configure-the-native-android-project-for-push-notifications"></a>Yerel Android projesini anında iletme bildirimleri için yapılandırma

[!INCLUDE [Configure the native Android project](../../includes/notification-hubs-backend-service-configure-flutter-android.md)]

## <a name="configure-the-native-ios-project-for-push-notifications"></a>Yerel iOS projesini anında iletme bildirimleri için yapılandırma

[!INCLUDE [Configure the native iOS project](../../includes/notification-hubs-backend-service-configure-flutter-ios.md)]

## <a name="test-the-solution"></a>Çözümü test etme

Artık arka uç hizmeti üzerinden bildirimleri göndermeye test edebilirsiniz.

[!INCLUDE [Testing the solution](../../includes/notification-hubs-backend-service-testing.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık arka uç hizmeti aracılığıyla bir Bildirim Hub 'ına bağlı temel bir flutter uygulamasına sahip olmanız ve bildirim gönderebilmeniz ve alabilmesi gerekir.

[!INCLUDE [Next steps](../../includes/notification-hubs-backend-service-next-steps.md)]

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [Troubleshooting](../../includes/notification-hubs-backend-service-troubleshooting.md)]

## <a name="related-links"></a>İlgili bağlantılar

* [Azure Notification Hubs genel bakış](notification-hubs-push-notification-overview.md)
* [MacOS üzerinde flutter yükleme](https://flutter.dev/docs/get-started/install/macos)
* [Windows üzerinde flutter yükleme](https://flutter.dev/docs/get-started/install/windows)
* [Arka uç işlemleri için SDK Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
* [GitHub 'da SDK 'Yı Notification Hubs](https://github.com/Azure/azure-notificationhubs)
* [Uygulama arka ucuna kaydetme](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
* [Kayıt yönetimi](notification-hubs-push-notification-registration-management.md)
* [Etiketlerle çalışma](notification-hubs-tags-segment-push-message.md)
* [Özel şablonlarla çalışma](notification-hubs-templates-cross-platform-push-messages.md)
