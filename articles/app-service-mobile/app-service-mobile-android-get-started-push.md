---
title: Android uygulamanıza anında iletme bildirimleri ekleme
description: Android uygulamanıza anında iletme bildirimleri göndermek için Mobile Apps kullanmayı öğrenin.
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 6fec85c028e992c15fb9503ffb599023e668c58f
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77459965"
---
# <a name="add-push-notifications-to-your-android-app"></a>Android uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir kayıt her eklendiğinde cihaza anında iletme bildirimi gönderilmesi için [Android hızlı başlangıç] projesine anında iletme bildirimleri eklersiniz.

İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, anında iletme bildirimi uzantı paketine ihtiyacınız vardır. Daha fazla bilgi için bkz. [Azure için .net arka uç sunucu SDK 'sı Mobile Apps çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Şunlar gerekir:

* Projenizin arka ucuna bağlı olarak bir IDE:

  * Bu uygulamanın bir Node. js arka ucu varsa [Android Studio](https://developer.android.com/sdk/index.html) .
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) veya sonraki bir sürümü, bu uygulamanın bir Microsoft .net arka ucu varsa.
* Android 2,3 veya üzeri, Google Repository düzeltmesi 27 veya üzeri ve Firebase Cloud Messaging için Google Play Hizmetleri 9.0.2 veya üzeri.
* [Android hızlı başlangıç]doldurun.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Firebase Cloud Messaging'i destekleyen bir proje oluşturma

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Bildirim hub’ını yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure 'ı anında iletme bildirimleri gönderecek şekilde yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Sunucu projesi için anında iletme bildirimlerini etkinleştir

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme

Bu bölümde, anında iletme bildirimlerini işleyecek istemci Android uygulamanızı güncelleştirmeniz gerekir.

### <a name="verify-android-sdk-version"></a>Android SDK sürümünü doğrula

[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Sonraki adımınız Google Play hizmetleri yüklemektir. Firebase Cloud Messaging, geliştirme ve test için en düşük API düzeyi gereksinimlerine sahiptir ve bu, bildirimdeki **Minsdkversion** özelliği ile uyumlu olmalıdır.

Daha eski bir cihazla test ediyorsanız, bu değeri nasıl ayarlayameyeceğinizi ve uygun şekilde ayarlamayı öğrenmek için [Android projenize Firebase ekleyin] ' e başvurun.

### <a name="add-firebase-cloud-messaging-to-the-project"></a>Projeye Firebase bulut Iletileri ekleyin

[!INCLUDE [Add Firebase Cloud Messaging](../../includes/app-service-mobile-add-firebase-cloud-messaging.md)]

### <a name="add-code"></a>Kod Ekle

[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Uygulamayı yayımlanan mobil hizmete karşı test etme

Uygulamayı, USB kablosuyla doğrudan bir Android telefonu ekleyerek veya öykünücüsünde bir sanal cihaz kullanarak test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticiyi tamamladığınıza göre, aşağıdaki öğreticilerden birine devam etmeyi göz önünde bulundurun:

* [Android uygulamanıza kimlik doğrulaması ekleyin](app-service-mobile-android-get-started-users.md).
  Desteklenen bir kimlik sağlayıcısı kullanarak Android 'de ToDoList hızlı başlangıç projesine nasıl kimlik doğrulaması ekleneceğini öğrenin.
* [Android uygulamanız için çevrimdışı eşitlemeyi etkinleştirin](app-service-mobile-android-get-started-offline-data.md).
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme sayesinde, kullanıcılar bir mobil uygulamayla etkileşime geçerek, ağ bağlantısı olmasa bile veri&mdash;görüntüleme, ekleme veya değiştirme&mdash;.

<!-- URLs -->
[Android hızlı başlangıç]: app-service-mobile-android-get-started.md
[Android projenize Firebase ekleyin]: https://firebase.google.com/docs/android/setup
