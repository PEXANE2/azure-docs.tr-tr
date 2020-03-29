---
title: Android'de kimlik doğrulama ekleme
description: Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla Android uygulamanızın kullanıcılarını doğrulamak için Azure Uygulama Hizmetini nasıl kullanacağınızı öğrenin.
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 705ebb5809840155e6bbf3f8eef091eb95f63e63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461649"
---
# <a name="add-authentication-to-your-android-app"></a>Android uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğreticide, desteklenen bir kimlik sağlayıcısı kullanarak Android'deki todolist quickstart projesine kimlik doğrulama eklersiniz. Bu öğretici, önce tamamlamanız gereken [Mobil Uygulamalar öğreticisini] başlatın'a dayanmaktadır.

## <a name="register-your-app-for-authentication-and-configure-azure-app-service"></a><a name="register"></a>Uygulamanızı kimlik doğrulama için kaydedin ve Azure Uygulama Hizmetini yapılandırın
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><a name="redirecturl"></a>Uygulamanızı İzin Verilen Harici Yönlendirme URL'lerine ekleyin

Güvenli kimlik doğrulaması, uygulamanız için yeni bir URL düzeni tanımlamanızı gerektirir. Bu, kimlik doğrulama işlemi tamamlandıktan sonra kimlik doğrulama sisteminin uygulamanıza geri yönlendirmesine olanak tanır. Bu öğreticide, URL şeması _ekadını_ sonuna kadar kullanırız. Ancak, seçtiğiniz herhangi bir URL düzenini kullanabilirsiniz. Mobil uygulamanıza özgü olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. Azure [portalında]Uygulama Hizmetinizi seçin.

2. Kimlik **Doğrulama / Yetkilendirme** menüsü seçeneğini tıklatın.

3. İzin **Verilen Harici Yönlendirme URL'lerine**girin. `appname://easyauth.callback`  Bu dizedeki _ad,_ mobil uygulamanızın URL Düzeni'dir.  Bir protokol için normal URL belirtimini izlemeli (yalnızca harfleri ve sayıları kullanın ve harfle başlamalıdır).  Mobil uygulama kodunuzu URL Düzeni ile birkaç yerde ayarlamanız gerektiğinden, seçtiğiniz dizeye not alabilirsiniz.

4. **Tamam**'a tıklayın.

5. **Kaydet**'e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a><a name="permissions"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Android Studio'da, [mobil uygulamalarla başlayın]öğretici ile tamamladığınız projeyi açın. **Çalıştır** menüsünden **Çalıştır uygulamasını**tıklatın ve uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durum ortaya çıkar.

     Bu özel durum, uygulamanın kimliği doğrulanmamış bir kullanıcı olarak arka uca erişmeye çalıştığından, ancak *TodoItem* tablosu artık kimlik doğrulaması gerektirdiğinden olur.

Ardından, Uygulamayı Mobile Apps'tan kaynak istemeden önce kullanıcıların kimliğini doğrulamak için güncellersiniz.

## <a name="add-authentication-to-the-app"></a>Uygulamaya kimlik doğrulama ekleme
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-authentication-tokens-on-the-client"></a><a name="cache-tokens"></a>İstemci üzerinde önbellek kimlik doğrulama belirteçleri
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Sonraki adımlar
Şimdi bu temel kimlik doğrulama öğretici tamamladı, aşağıdaki öğreticiler birine devam düşünün:

* [Android uygulamanıza anında iletme bildirimleri ekleyin.](app-service-mobile-android-get-started-push.md)
  Anında iletme bildirimleri göndermek için Azure bildirim hub'larını kullanmak için Mobil Uygulamalarınızı arka uçta nasıl yapılandıracaklarınızı öğrenin.
* [Android uygulamanız için çevrimdışı eşitleme'yi etkinleştirin.](app-service-mobile-android-get-started-offline-data.md)
  Mobil Uygulamalar arka uçunu kullanarak uygulamanıza çevrimdışı desteği nasıl ekleyeceğinizi öğrenin. Çevrimdışı eşitleme yle, kullanıcılar ağ&mdash;bağlantısı olmasa bile bir&mdash;mobil uygulamayı görüntüleme, ekleme veya değiştirme ile etkileşimkurabilir.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Mobil Uygulamalarla başlayın]: app-service-mobile-android-get-started.md
[Azure portalında]: https://portal.azure.com/
