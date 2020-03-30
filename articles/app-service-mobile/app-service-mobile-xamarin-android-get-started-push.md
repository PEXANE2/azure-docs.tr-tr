---
title: Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme
description: Xamarin.Android uygulamanıza anında iletme bildirimleri göndermek için Azure Uygulama Hizmeti ve Azure Bildirim Hub'larını nasıl kullanacağınızı öğrenin.
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 5657be0dbaeb46f8f899a9b4a2f8ba9b4fe9ebaa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249314"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [Xamarin.Android quickstart](app-service-mobile-windows-store-dotnet-get-started.md) projesine anında iletme bildirimleri eklersiniz, böylece her kayıt ekildiğinde cihaza bir anında iletme bildirimi gönderilir.

İndirilen quickstart sunucu projesini kullanmazsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olacaktır. Daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile Çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) kılavuzuna bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici kurulum gerektirir:

* Etkin bir Google hesabı. [accounts.google.com'da](https://go.microsoft.com/fwlink/p/?LinkId=268302)bir Google hesabına kaydolabilirsiniz.
* [Google Cloud Mesajlaşma İstemci Bileşeni](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-a-notification-hub"></a><a name="configure-hub"></a>Bildirim Hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="enable-firebase-cloud-messaging"></a><a id="register"></a>Firebase Cloud Messaging'i etkinleştirme

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Anından etme istekleri göndermek için Azure'u yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a><a id="update-server"></a>Anında iletme bildirimleri göndermek için sunucu projesini güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-the-client-project-for-push-notifications"></a><a id="configure-app"></a>Anında iletme bildirimleri için istemci projesini yapılandırma

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a name="add-push-notifications-code-to-your-app"></a><a id="add-push"></a>Uygulamanıza anında iletme bildirimleri kodu ekleme

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test-push-notifications-in-your-app"></a><a name="test"></a>Uygulamanızdaki anında iletme bildirimlerini test edin

Uygulamayı emülatörde sanal bir cihaz kullanarak test edebilirsiniz. Emülatör üzerinde çalışırken gereken ek yapılandırma adımları vardır.

1. Sanal cihaz, Android Sanal Cihaz (AVD) yöneticisinde hedef olarak Google API'leri ayarlamış olmalıdır.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. **Uygulamalar** > **Ayarlar** > **Hesabı Ekle'yi**tıklatarak Android cihazına bir Google hesabı ekleyin, ardından istemleri izleyin.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Todolist uygulamasını eskisi gibi çalıştırın ve yeni bir todo öğesi ekleyin. Bu kez, bildirim alanında bir bildirim simgesi görüntülenir. Bildirimin tam metnini görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
