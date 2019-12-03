---
title: Twitter kimlik doğrulamasını yapılandırma
description: Twitter kimlik doğrulamasını App Service uygulamanız için bir kimlik sağlayıcısı olarak nasıl yapılandıracağınızı öğrenin.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 04/19/2018
ms.custom: seodec18
ms.openlocfilehash: b5ec17c18cec8053f0732366c0cc5d0c5003e4de
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74670808"
---
# <a name="configure-your-app-service-app-to-use-twitter-login"></a>App Service uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Twitter kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

Bu makaledeki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresi ve telefon numarası olan bir Twitter hesabına ihtiyacınız vardır. Yeni bir Twitter hesabı oluşturmak için [Twitter.com]adresine gidin.

## <a name="register"> </a>Uygulamanızı Twitter 'a kaydetme

1. [Azure portalda] oturum açın ve uygulamanıza gidin. **URL**'nizi kopyalayın. Twitter uygulamanızı yapılandırmak için kullanacaksınız.
1. [Twitter geliştiricileri] Web sitesine gidin, Twitter hesabı kimlik bilgilerinizle oturum açın ve **Yeni uygulama oluştur**' u seçin.
1. Yeni uygulamanız için bir **ad** ve **Açıklama** girin. Uygulamanızın **URL 'Sini** **Web sitesi** alanına yapıştırın. **Geri arama URL 'si** alanında App Service uygulamanızın URL 'sini girin ve yolu `/.auth/login/aad/callback`ekleyin. Örneğin, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. HTTPS şemasını kullandığınızdan emin olun.
1. Sayfanın alt kısmındaki koşulları okuyun ve kabul edin. **Twitter uygulamanızı oluştur ' u**seçin. Uygulama ayrıntıları görüntülenir.
1. **Ayarlar** sekmesini seçin, **Bu uygulamanın Twitter 'da oturum açmak Için kullanılmasına izin ver**' i Işaretleyin ve ardından **Ayarları Güncelleştir**' i seçin.
1. **Anahtarlar ve erişim belirteçleri** sekmesini seçin.

   Şu değerleri bir yere göz önünde oluşturun:
   - Tüketici anahtarı (API anahtarı)
   - Tüketici gizli dizisi (API parolası)

   > [!NOTE]
   > Tüketici gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı herkesle paylaşmayın veya uygulamanızla dağıtın.

## <a name="secrets"> </a>Uygulamanıza Twitter bilgilerini ekleyin

1. [Azure portalda]uygulamanıza gidin.
1. **Kimlik doğrulama ve yetkilendirme** > **ayarları** seçin ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
1. **Twitter**' ı seçin.
1. Daha önce edindiğiniz `API Key` ve `API Secret` değerlerini yapıştırın.
1. **Tamam**’ı seçin.

   ![Mobil uygulama Twitter ayarlarının ekran görüntüsü][1]

   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.

1. Seçim Sitenize erişimi yalnızca Twitter tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Twitter**'da **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca kimlik doğrulaması için tüm kimliği doğrulanmamış istekleri Twitter 'a yönlendirir.

   > [!CAUTION]
   > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için **anonim Isteklere Izin ver (eylem yok)** , uygulamanın kimlik doğrulamanın kendisini el ile başlattığı şekilde tercih edilebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).

1. **Kaydet**’i seçin.

Artık uygulamanızda kimlik doğrulaması için Twitter kullanmaya hazırsınız.

## <a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure portalda]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
