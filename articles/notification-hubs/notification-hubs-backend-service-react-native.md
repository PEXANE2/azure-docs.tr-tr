---
title: Arka uç hizmeti aracılığıyla Azure Notification Hubs kullanarak yerel uygulamalara tepki vermek için anında iletme bildirimleri gönderin | Microsoft Docs
description: Arka uç hizmeti aracılığıyla Azure Notification Hubs kullanan yerel uygulamalara tepki vermek için bildirimleri anında iletme hakkında bilgi edinin.
author: alexeystrakh
ms.service: notification-hubs
ms.topic: tutorial
ms.date: 06/11/2020
ms.author: alstrakh
ms.openlocfilehash: 007be386b7b64fd3461fa508d35a4ef9be377c1f
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170933"
---
# <a name="tutorial-send-push-notifications-to-react-native-apps-using-azure-notification-hubs-via-a-backend-service"></a>Öğretici: arka uç hizmeti aracılığıyla Azure Notification Hubs kullanarak yerel uygulamalara tepki vermek için anında iletme bildirimleri gönderin  

[![Örnek indirin örnek ](./media/notification-hubs-backend-service-react-native/download.png) indirin](https://github.com/xamcat/mobcat-samples/tree/master/notification_hub_backend_service)  

> [!div class="op_single_selector"]
>
> * [Xamarin.Forms](notification-hubs-backend-service-xamarin-forms.md)
> * [Flutter](notification-hubs-backend-service-flutter.md)
> * [React Native](notification-hubs-backend-service-react-native.md)

Bu öğreticide, **Android** ve **iOS**'u hedefleyen, tepki temelli bir [Yerel](https://reactnative.dev/) uygulamaya bildirim göndermek için [Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-overview) 'yi kullanırsınız.  

[!INCLUDE [Notification Hubs Backend Service Introduction](../../includes/notification-hubs-backend-service-introduction.md)]

Bu öğreticide aşağıdaki adımları izleyebilirsiniz:

> [!div class="checklist"]
>
> * [Anında Iletme Bildirim Hizmetleri ve Azure Notification Hubs ayarlayın.](#set-up-push-notification-services-and-azure-notification-hub)
> * [ASP.NET Core Web API arka uç uygulaması oluşturun.](#create-an-aspnet-core-web-api-backend-application)
> * [Platformlar arası tepki verme yerel uygulaması oluşturun.](#create-a-cross-platform-react-native-application)
> * [Yerel Android projesini anında iletme bildirimleri için yapılandırın.](#configure-the-native-android-project-for-push-notifications)
> * [Yerel iOS projesini anında iletme bildirimleri için yapılandırın.](#configure-the-native-ios-project-for-push-notifications)
> * [Çözümü test edin.](#test-the-solution)

## <a name="prerequisites"></a>Ön koşullar

Bu arada izlemek için şunları yapmanız gerekir:

* Kaynakları oluşturabileceğiniz ve yönetebileceğiniz bir [Azure aboneliği](https://portal.azure.com) .
* [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/) yüklü bir Mac (veya [Visual Studio 2019](https://visualstudio.microsoft.com/vs) çalıştıran bir bilgisayar, .net Iş yüküne **sahip mobil geliştirmeyle** birlikte).
* Uygulamayı **Android** (fiziksel veya öykünücü cihazlar) ya da **iOS** (yalnızca fiziksel cihazlar) üzerinde çalıştırma özelliği.

Android için şunları yapmanız gerekir:

* Bir geliştirici fiziksel cihaz ya da öykünücü *(API 26 ve üzeri Google Play Hizmetleri yüklü) olarak*açıldı.

İOS için, şunları yapmanız gerekir:

* Etkin bir [Apple geliştirici hesabı](https://developer.apple.com).
* [Geliştirici hesabınıza kayıtlı](https://help.apple.com/developer-account/#/dev40df0d9fa) olan bir fiziksel iOS cihazı *(iOS 13,0 ve üzeri çalıştıran)*.
* **Anahtarınıza** yüklenmiş bir **. p12** [geliştirme sertifikası](https://help.apple.com/developer-account/#/dev04fd06d56) , bir [uygulamayı fiziksel bir cihazda çalıştırmanızı](https://help.apple.com/xcode/mac/current/#/dev5a825a1ca)sağlar.

> [!NOTE]
> İOS simülatörü uzak bildirimleri desteklemez ve bu nedenle iOS üzerinde bu örnek araştırılırken fiziksel bir cihaz gereklidir. Ancak, bu öğreticiyi tamamlayabilmeniz için uygulamayı hem **Android** hem de **iOS** üzerinde çalıştırmanız gerekmez.

Önceki deneyimler olmadan bu ilk ilkeler örneğinde bulunan adımları izleyebilirsiniz. Ancak, aşağıdaki yönlere sahip olmanın avantajlarından yararlanabilirsiniz.

* [Apple geliştirici portalı](https://developer.apple.com)
* [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1)
* [Azure Bildirim Merkezleri](notification-hubs-push-notification-overview.md)
* [Google Firebase konsolu](https://console.firebase.google.com/u/0/)
* [React Native](https://reactnative.dev/docs/getting-started)

[Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/) ve [Visual Studio Code](https://code.visualstudio.com/download) için sunulan adımlar, ancak [Visual Studio 2019](https://visualstudio.microsoft.com/vs)' i kullanarak takip etmek mümkündür.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Anında Iletme Bildirim Hizmetleri ve Azure Notification Hub 'ı ayarlama

Bu bölümde, **[Firebase Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging)** ve **[Apple Push bildirim hizmetleri (APNs)](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)** ayarlarsınız. Daha sonra bu hizmetlerle çalışmak için bir Bildirim Hub 'ı oluşturup yapılandırırsınız.

[!INCLUDE [Create a Firebase project and enable Firebase Cloud Messaging](../../includes/notification-hubs-common-enable-firebase-cloud-messaging.md)]

[!INCLUDE [Enable Apple Push Notifications](../../includes/notification-hubs-common-enable-apple-push-notifications.md)]

[!INCLUDE [Create a notification hub](../../includes/notification-hubs-common-create-notification-hub.md)]

[!INCLUDE [Configure your notification hub with APNs information](../../includes/notification-hubs-common-configure-with-apns-information.md)]

[!INCLUDE [Configure your notification hub with FCM information](../../includes/notification-hubs-common-configure-with-fcm-information.md)]

## <a name="create-an-aspnet-core-web-api-backend-application"></a>ASP.NET Core Web API arka uç uygulaması oluşturma

Bu bölümde, [cihaz kaydını](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration) işleyecek ve bilgilendirmeler yerel mobil uygulamasına bildirimlerin gönderilmesi Için [ASP.NET Core Web API](https://dotnet.microsoft.com/apps/aspnet/apis) arka ucu oluşturacaksınız.

[!INCLUDE [Create an ASP.NET Core Web API backend application](../../includes/notification-hubs-backend-service-web-api.md)]

## <a name="create-a-cross-platform-react-native-application"></a>Platformlar arası tepki verme yerel uygulaması oluşturma

Bu bölümde, platformlar arası bir şekilde anında iletme bildirimleri uygulayan bir [tepki temelli yerel](https://reactnative.dev/) mobil uygulama oluşturacaksınız.

[!INCLUDE [Sample application generic overview](../../includes/notification-hubs-backend-service-sample-app-overview.md)]

[!INCLUDE [Create React Native application](../../includes/notification-hubs-backend-service-sample-app-reactnative.md)]

## <a name="configure-the-native-android-project-for-push-notifications"></a>Yerel Android projesini anında iletme bildirimleri için yapılandırma

[!INCLUDE [Configure the native Android project](../../includes/notification-hubs-backend-service-configure-reactnative-android.md)]

## <a name="configure-the-native-ios-project-for-push-notifications"></a>Yerel iOS projesini anında iletme bildirimleri için yapılandırma

[!INCLUDE [Configure the native iOS project](../../includes/notification-hubs-backend-service-configure-reactnative-ios.md)]

## <a name="test-the-solution"></a>Çözümü test etme

Artık arka uç hizmeti üzerinden bildirimleri göndermeye test edebilirsiniz.

[!INCLUDE [Testing the solution](../../includes/notification-hubs-backend-service-testing.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık arka uç hizmeti aracılığıyla bir Bildirim Hub 'ına bağlı, temel bir yanıt verme yerel uygulamasına sahip olmanız ve bildirim gönderebilmeniz ve alabilmesi gerekir.

[!INCLUDE [Next steps](../../includes/notification-hubs-backend-service-next-steps.md)]

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [Troubleshooting](../../includes/notification-hubs-backend-service-troubleshooting.md)]

## <a name="related-links"></a>İlgili bağlantılar

* [Azure Notification Hubs genel bakış](notification-hubs-push-notification-overview.md)
* [Mac için Visual Studio’yu Yükleme](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* [Visual Studio Code yükleniyor](https://code.visualstudio.com/download)
* [Tepki verme yerel geliştirme ortamını ayarlama](https://reactnative.dev/docs/environment-setup)
* [Arka uç işlemleri için SDK Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
* [GitHub 'da SDK 'Yı Notification Hubs](https://github.com/Azure/azure-notificationhubs)
* [Uygulama arka ucuna kaydetme](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
* [Kayıt yönetimi](notification-hubs-push-notification-registration-management.md)
* [Etiketlerle çalışma](notification-hubs-tags-segment-push-message.md)
* [Özel şablonlarla çalışma](notification-hubs-templates-cross-platform-push-messages.md)
