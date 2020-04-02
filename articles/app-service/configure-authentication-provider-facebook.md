---
title: Facebook kimlik doğrulamasını yapılandırma
description: App Service veya Azure İşlevleri uygulamanız için kimlik sağlayıcısı olarak Facebook kimlik doğrulamasını nasıl yapılandıracaksınız öğrenin.
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.topic: article
ms.date: 06/06/2019
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: b6aad323c0d6fa8f59c9fad203640c477b162503
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519953"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-facebook-login"></a>Facebook oturumunu kullanmak için Uygulama Hizmetinizi veya Azure İşlevleri uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Facebook'u kimlik doğrulama sağlayıcısı olarak kullanmak üzere Azure Uygulama Hizmeti veya Azure İşlevleri nasıl yapılandırılabildiğiniz gösterilmektedir.

Bu makaledeki yordamı tamamlamak için, doğrulanmış bir e-posta adresi ve cep telefonu numarası olan bir Facebook hesabınız gerekir. Yeni bir Facebook hesabı oluşturmak için [facebook.com]gidin.

## <a name="register-your-application-with-facebook"></a><a name="register"> </a>Başvurunuzu Facebook'a kaydedin

1. [Facebook Geliştiricileri] web sitesine gidin ve Facebook hesap kimlik bilgilerinizle oturum açın.

   Geliştiriciler için Facebook hesabınız yoksa, **Başlat'ı** seçin ve kayıt adımlarını izleyin.
1. **Uygulamalarımı** > Seçin**Yeni Uygulama Ekle**.
1. **Display Name** alanında:
   1. Uygulamanız için benzersiz bir ad yazın.
   1. İletişim **E-postanızı**sağlayın.
   1. **Uygulama Kimliği Oluştur'u**seçin.
   1. Güvenlik kontrolünü tamamla.

   Yeni Facebook uygulamanızın geliştirici panosu açılır.
1. **Pano** > **Facebook Giriş** > **Web'i** > **Web**seçin.
1. **Facebook Giriş**altında sol navigasyon , **Ayarlar**seçin.
1. Geçerli **OAuth yönlendirme URI** alanına `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback`girin. Azure Uygulama `<app-name>` Hizmeti uygulamanızın adıyla değiştirmeyi unutmayın.
1. **Değişiklikleri Kaydet'i**seçin.
1. Sol bölmede **Ayarlar** > **Temel'i**seçin. 
1. App **Secret** alanında **Göster'i**seçin. **App ID** ve **App Secret**değerlerini kopyalayın. Bunları daha sonra Azure'da Uygulama Hizmeti uygulamanızı yapılandırmak için kullanırsınız.

   > [!IMPORTANT]
   > Uygulama sırrı önemli bir güvenlik kimlik bilgisidir. Bu sırrı kimseyle paylaşmayın veya istemci uygulaması içinde dağıtmayın.
   >

1. Uygulamayı kaydetmek için kullandığınız Facebook hesabı uygulamanın yöneticisidir. Bu noktada, bu uygulamada yalnızca yöneticiler oturum açabilir.

   Diğer Facebook hesaplarının kimliğini doğrulamak için **App Review'u** seçin ve genel halkın Facebook kimlik doğrulamasını kullanarak uygulamaya erişmesini sağlamak için **app adınızı herkese açık> \<** etkinleştirin.

## <a name="add-facebook-information-to-your-application"></a><a name="secrets"> </a>Uygulamanıza Facebook bilgileri ekleme

1. [Azure portalında] oturum açın ve Uygulama Hizmeti uygulamanıza gidin.
1. **Ayarlar** > **Kimlik Doğrulaması / Yetkilendirme'yi**seçin ve Uygulama Hizmeti Kimlik **Doğrulamasının** **Üzerinde**olduğundan emin olun.
1. **Facebook'u**seçin ve ardından daha önce elde ettiğiniz App Id ve App Secret değerlerini yapıştırın. Uygulamanızın gerektirdiği tüm kapsamları etkinleştirin.
1. **Tamam'ı**seçin.

   ![Mobil Uygulama Facebook Ayarları Ekran Görüntüsü][0]

    Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak sitenizin içeriğine ve API'lerine yetkili erişimi kısıtlamaz. Uygulama kodunuzdaki kullanıcıları yetkilendirmeniz gerekir.
1. (İsteğe bağlı) Yalnızca Facebook tarafından kimliği doğrulanan kullanıcılara erişimi kısıtlamak için, istek **Facebook'ta** **kimlik doğrulaması olmadığında Eylem'i harekete** geçirin. Bu işlevselliği ayarladığınızda, uygulamanız tüm isteklerin kimliğinin doğrulanmasını gerektirir. Ayrıca, kimlik doğrulama için tüm kimlik doğrulama lı istekleri Facebook'a yönlendirir.

   > [!CAUTION]
   > Bu şekilde erişimi kısıtlamak, uygulamanıza yapılan tüm aramalar için geçerlidir ve bu da birçok tek sayfalı uygulamada olduğu gibi herkese açık bir ana sayfaya sahip uygulamalar için isden edilmeyebilir. Bu tür uygulamalar için, uygulamanın kimlik doğrulamayı el ile başlatması için **anonim isteklere izin ver (eylem yok)** tercih edilebilir. Daha fazla bilgi için [kimlik doğrulama akışına](overview-authentication-authorization.md#authentication-flow)bakın.

1. **Kaydet'i**seçin.

Artık uygulamanızda kimlik doğrulama için Facebook'u kullanmaya hazırsınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook Geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portalı]: https://portal.azure.com/
