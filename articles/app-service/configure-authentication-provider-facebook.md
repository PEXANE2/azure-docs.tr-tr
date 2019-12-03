---
title: Facebook kimlik doğrulamasını yapılandırma
description: Facebook kimlik doğrulamasını App Service uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.topic: article
ms.date: 06/06/2019
ms.custom: seodec18
ms.openlocfilehash: cd9c8a1bab3616b9b4eb1fe97ee3a9b2307ba77b
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671949"
---
# <a name="configure-your-app-service-app-to-use-facebook-login"></a>App Service uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Facebook kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

Bu makaledeki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresine ve cep telefonu numarasına sahip bir Facebook hesabınızın olması gerekir. Yeni bir Facebook hesabı oluşturmak için [Facebook.com]adresine gidin.

## <a name="register"> </a>Uygulamanızı Facebook ile kaydetme

1. [Facebook geliştiricileri] Web sitesine gidin ve Facebook hesabı kimlik bilgilerinizle oturum açın.

   Geliştiriciler için Facebook hesabınız yoksa, **kullanmaya** başlayın ' ı seçin ve kayıt adımlarını izleyin.
1. **Yeni uygulama eklemek** > **uygulamalarım** ' ı seçin.
1. **Görünen ad** alanında:
   1. Uygulamanız için benzersiz bir ad yazın.
   1. **Iletişim e-postanızı**girin.
   1. **Uygulama kimliği oluştur**' u seçin.
   1. Güvenlik denetimini doldurun.

   Yeni Facebook uygulamanız için geliştirici panosu açılır.
1.  >  > **Web**'i **kurmak** > **Facebook oturum açma** **panosunu** seçin.
1. **Facebook oturum açma**altındaki sol gezinti bölmesinde **Ayarlar**' ı seçin.
1. **Geçerli OAuth yeniden yönlendirme URI 'leri** alanında `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback`girin. `<app-name>` Azure App Service uygulamanızın adıyla değiştirmeyi unutmayın.
1. **Değişiklikleri Kaydet**' i seçin.
1. Sol bölmede **ayarlar** > **temel**' yı seçin. 
1. **Uygulama gizli** alanından **göster**' i seçin. **Uygulama kimliği** ve **uygulama gizli anahtarı**değerlerini kopyalayın. Azure 'da App Service uygulamanızı yapılandırmak için bunları daha sonra kullanırsınız.

   > [!IMPORTANT]
   > Uygulama gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.
   >

1. Uygulamayı kaydetmek için kullandığınız Facebook hesabı, uygulamanın bir yöneticisidir. Bu noktada, yalnızca Yöneticiler bu uygulamada oturum açabilir.

   Diğer Facebook hesaplarının kimliğini doğrulamak için, **uygulama gözden geçirmesi** ' nı seçin ve genel genel ' i Facebook kimlik doğrulaması kullanarak uygulamaya erişmesine olanak tanımak için uygulama **adı > Genel \<** etkinleştirin.

## <a name="secrets"> </a>Uygulamanıza Facebook bilgilerini ekleyin

1. [Azure portalda] oturum açın ve App Service uygulamanıza gidin.
1. **Kimlik doğrulama ve yetkilendirme** > **ayarları** seçin ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
1. **Facebook**' u seçin ve ardından daha önce EDINDIĞINIZ uygulama kimliği ve uygulama gizli anahtarı değerlerini yapıştırın. Uygulamanız için gereken tüm kapsamları etkinleştirin.
1. **Tamam**’ı seçin.

   ![Mobile App Facebook ayarlarının ekran görüntüsü][0]

    Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içeriğinize ve API 'Lerine yönelik yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmeniz gerekir.
1. Seçim Yalnızca Facebook tarafından kimliği doğrulanan kullanıcılarla erişimi kısıtlamak için, isteğin **Facebook 'ta** **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca kimlik doğrulaması için tüm kimliği doğrulanmamış istekleri Facebook 'a yönlendirir.

   > [!CAUTION]
   > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için **anonim Isteklere Izin ver (eylem yok)** , uygulamanın kimlik doğrulamanın kendisini el ile başlattığı şekilde tercih edilebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).

1. **Kaydet**’i seçin.

Artık uygulamanızda kimlik doğrulaması için Facebook kullanmaya hazırsınız.

## <a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portalda]: https://portal.azure.com/
