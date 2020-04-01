---
title: Twitter kimlik doğrulamasını yapılandırma
description: App Service veya Azure İşlevleri uygulamanız için bir kimlik sağlayıcısı olarak Twitter kimlik doğrulamasını nasıl yapılandıracaksınız öğrenin.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 02/28/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 3f85f30e0a64b6e39b905fc05503a445aa5876ba
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437992"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-twitter-login"></a>Twitter oturumunu kullanmak için Uygulama Hizmetinizi veya Azure İşlevleri uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Twitter'ı kimlik doğrulama sağlayıcısı olarak kullanmak üzere Azure Uygulama Hizmeti veya Azure İşlevleri nasıl yapılandırılabildiğiniz gösterilmektedir.

Bu makaledeki yordamı tamamlamak için, doğrulanmış bir e-posta adresi ve telefon numarası olan bir Twitter hesabına ihtiyacınız vardır. Yeni bir Twitter hesabı oluşturmak için [twitter.com]gidin.

## <a name="register-your-application-with-twitter"></a><a name="register"> </a>Başvurunuzu Twitter'a kaydedin

1. [Azure portalında] oturum açın ve uygulamanıza gidin. **URL'nizi**kopyalayın. Twitter uygulamanızı yapılandırmak için kullanırsınız.
1. [Twitter Geliştiricileri] web sitesine gidin, Twitter hesap kimlik bilgilerinizle oturum açın ve **uygulama oluştur'u**seçin.
1. Yeni uygulamanız için **Uygulama adını** ve **Uygulama açıklamasını** girin. Uygulamanızın **URL'sini** Web **Sitesi URL** alanına yapıştırın. Geri **Arama URL'leri** bölümünde, Uygulama Hizmeti uygulamanızın HTTPS URL'sini `/.auth/login/twitter/callback`girin ve yolu tamamla. Örneğin, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.
1. Sayfanın alt kısmında, **bu uygulamanın nasıl kullanılacağını bize bildirin**en az 100 karakter yazın, sonra **Oluştur'u**seçin. Açılır pencerede yeniden **Oluştur'u** tıklatın. Uygulama ayrıntıları görüntülenir.
1. **Anahtarlar ve Erişim Jetonları** sekmesini seçin.

   Bu değerlere dikkat edin:
   - API anahtarı
   - API gizli anahtar

   > [!NOTE]
   > API gizli anahtarı önemli bir güvenlik kimlik bilgisidir. Bu sırrı kimseyle paylaşmayın veya uygulamanızla dağıtmayın.

## <a name="add-twitter-information-to-your-application"></a><a name="secrets"> </a>Uygulamanıza Twitter bilgileri ekleyin

1. [Azure portalındaki]uygulamanıza gidin.
1. **Ayarlar** > **Kimlik Doğrulaması / Yetkilendirme'yi**seçin ve Uygulama Hizmeti Kimlik **Doğrulamasının** **Üzerinde**olduğundan emin olun.
1. **Twitter'ı**seçin.
1. Daha önce `API key` elde `API secret key` ettiğiniz değerleri ve değerleri yapıştırın.
1. **Tamam'ı**seçin.

   ![Mobil Uygulama Twitter ayarlarıekran görüntüsü][1]

   Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak sitenizin içeriğine ve API'lerine yetkili erişimi kısıtlamaz. Uygulama kodunuzdaki kullanıcıları yetkilendirmeniz gerekir.

1. (İsteğe bağlı) Sitenize erişimi yalnızca Twitter tarafından kimlik doğrulaması yapılan kullanıcılarla sınırlamak için, istek **Twitter'da** **kimlik doğrulaması olmadığında Eylem'i harekete** geçirin. Bu işlevselliği ayarladığınızda, uygulamanız tüm isteklerin kimliğinin doğrulanmasını gerektirir. Ayrıca kimlik doğrulama için twitter'a tüm kimlik doğrulama isteklerini yönlendirir.

   > [!CAUTION]
   > Bu şekilde erişimi kısıtlamak, uygulamanıza yapılan tüm aramalar için geçerlidir ve bu da birçok tek sayfalı uygulamada olduğu gibi herkese açık bir ana sayfaya sahip uygulamalar için isden edilmeyebilir. Bu tür uygulamalar için, uygulamanın kimlik doğrulamayı el ile başlatması için **anonim isteklere izin ver (eylem yok)** tercih edilebilir. Daha fazla bilgi için [kimlik doğrulama akışına](overview-authentication-authorization.md#authentication-flow)bakın.

1. **Kaydet'i**seçin.

Artık uygulamanızda kimlik doğrulama için Twitter'ı kullanmaya hazırsınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter Geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
