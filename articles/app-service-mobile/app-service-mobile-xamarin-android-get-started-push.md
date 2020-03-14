---
title: Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme
description: Azure App Service ve Azure Notification Hubs kullanarak Xamarin. Android uygulamanıza anında iletme bildirimleri gönderme hakkında bilgi edinin.
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 5657be0dbaeb46f8f899a9b4a2f8ba9b4fe9ebaa
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79249314"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir kayıt her eklendiğinde cihaza anında iletme bildirimi gönderilmesi için [Xamarin. Android hızlı başlangıç](app-service-mobile-windows-store-dotnet-get-started.md) projesine anında iletme bildirimleri eklersiniz.

İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, anında iletme bildirimi uzantı paketine ihtiyacınız olacaktır. Daha fazla bilgi için bkz. [Azure için .net arka uç sunucu SDK Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Kılavuzu.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için kurulum gereklidir:

* Etkin bir Google hesabı. [Accounts.Google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302)adresinde bir Google hesabı için kaydolabilirsiniz.
* [Google Cloud Messaging Istemci bileşeni](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Bildirim Hub 'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Firebase bulut Iletilerini etkinleştir

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Azure 'ı anında iletme istekleri gönderecek şekilde yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Sunucu projesini anında iletme bildirimleri gönderecek şekilde Güncelleştir

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>İstemci projesini anında iletme bildirimleri için yapılandırma

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Uygulamanıza anında iletme bildirimleri kodu ekleyin

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Uygulamanızda anında iletme bildirimlerini test etme

Öykünücüsünde bir sanal cihaz kullanarak uygulamayı test edebilirsiniz. Öykünücü üzerinde çalışırken ek yapılandırma adımları gereklidir.

1. Sanal cihazın Android sanal cihaz (AVD) Yöneticisi 'nde hedef olarak ayarlanmış Google API 'Leri olmalıdır.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. **Uygulamalar** > **Ayarlar** > **Hesap Ekle**' ye tıklayarak Android cihazına bir Google hesabı ekleyin ve ardından istemleri izleyin.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. ToDoList uygulamasını daha önce olduğu gibi çalıştırın ve yeni bir Todo öğesi ekleyin. Bu kez, bildirim alanında bir bildirim simgesi görüntülenir. Bildirimin tam metnini görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
