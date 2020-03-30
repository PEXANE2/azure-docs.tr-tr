---
title: Android uygulamanıza anında iletme bildirimleri ekleme
description: Android uygulamanıza anında iletme bildirimleri göndermek için Mobil Uygulamaları nasıl kullanacağınızı öğrenin.
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 6fec85c028e992c15fb9503ffb599023e668c58f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459965"
---
# <a name="add-push-notifications-to-your-android-app"></a>Android uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [Android hızlı başlatma] projesine anında iletme bildirimleri eklersiniz, böylece her kayıt ekildiğinde cihaza bir anında iletme bildirimi gönderilir.

İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olur. Daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdakileri yapmanız gerekir:

* Projenizin arka ucuna bağlı olarak bir IDE:

  * [Android Studio](https://developer.android.com/sdk/index.html) bu uygulama bir Düğüm.js arka uç varsa.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) veya daha sonra bu uygulamamicrosoft .NET arka uç varsa.
* Android 2.3 veya sonrası, Google Depo revizyonu 27 veya daha sonra ve Google Play Hizmetleri 9.0.2 veya daha sonra Firebase Cloud Messaging için.
* Android [hızlı başlangıç]tamamlayın.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Firebase Cloud Messaging'i destekleyen bir proje oluşturma

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Bildirim hub’ını yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure'u yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Sunucu projesi için anında iletme bildirimlerini etkinleştirme

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme

Bu bölümde, anında iletme bildirimlerini işlemek için istemci Android uygulamanızı güncellersiniz.

### <a name="verify-android-sdk-version"></a>Android SDK sürümünü doğrula

[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Bir sonraki adımGoogle Play hizmetlerini yüklemektir. Firebase Bulut Mesajlaşma geliştirme ve test için bazı minimum API düzeyi gereksinimleri vardır, hangi manifestoda **minSdkVersion** özelliği uyması gerekir.

Eski bir cihazla test ediyorsanız, bu değeri ne kadar düşük ayarlayabileceğinizi belirlemek ve uygun şekilde ayarlamak [için Android Projenize Firebase Ekle'ye] başvurun.

### <a name="add-firebase-cloud-messaging-to-the-project"></a>Projeye Firebase Bulut Mesajlaşması ekleme

[!INCLUDE [Add Firebase Cloud Messaging](../../includes/app-service-mobile-add-firebase-cloud-messaging.md)]

### <a name="add-code"></a>Kod ekleme

[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Uygulamayı yayınlanan mobil hizmetle karşı test edin

Uygulamayı doğrudan bir ANDROID telefona USB kablosu yla takarak veya emülatöre sanal bir aygıt kullanarak test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi bu öğretici tamamladı, aşağıdaki öğreticiler birine devam düşünün:

* [Android uygulamanıza kimlik doğrulama ekleyin.](app-service-mobile-android-get-started-users.md)
  Desteklenen bir kimlik sağlayıcısını kullanarak Android'deki todolist hızlı başlatma projesine kimlik doğrulamayı nasıl ekleyeceğinizi öğrenin.
* [Android uygulamanız için çevrimdışı eşitleme'yi etkinleştirin.](app-service-mobile-android-get-started-offline-data.md)
  Mobil Uygulamalar arka uçunu kullanarak uygulamanıza çevrimdışı desteği nasıl ekleyeceğinizi öğrenin. Çevrimdışı eşitleme yle, kullanıcılar ağ&mdash;bağlantısı olmasa bile bir&mdash;mobil uygulamayı görüntüleme, ekleme veya değiştirme ile etkileşimkurabilir.

<!-- URLs -->
[Android hızlı başlangıç]: app-service-mobile-android-get-started.md
[Android Projenize Firebase Ekle]:https://firebase.google.com/docs/android/setup
