---
title: Twitter kimlik doğrulamasını yapılandırma
description: Twitter kimlik doğrulamasını App Service veya Azure Işlevleri uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.topic: article
ms.date: 02/28/2020
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: 11c913b12b4dcb7d2a5ffa532064b347b82904ef
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519904"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-twitter-login"></a>App Service veya Azure Işlevleri uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, bir kimlik doğrulama sağlayıcısı olarak Twitter kullanmak üzere Azure App Service veya Azure Işlevlerinin nasıl yapılandırılacağı gösterilmektedir.

Bu makaledeki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresi ve telefon numarası olan bir Twitter hesabına ihtiyacınız vardır. Yeni bir Twitter hesabı oluşturmak için [Twitter.com]adresine gidin.

## <a name="register-your-application-with-twitter"></a><a name="register"> </a>Uygulamanızı Twitter 'a kaydetme

1. [Azure Portal] oturum açın ve uygulamanıza gidin. **URL**'nizi kopyalayın. Twitter uygulamanızı yapılandırmak için kullanacaksınız.
1. [Twitter geliştiricileri] Web sitesine gidin, Twitter hesabı kimlik bilgilerinizle oturum açın ve **uygulama oluştur**' u seçin.
1. Yeni uygulamanız için **uygulama adı** ve **uygulama açıklaması** girin. Uygulamanızın **URL** 'Sini **Web sitesi URL 'si** alanına yapıştırın. **Geri arama URL 'leri** bölümünde app SERVICE uygulamanızın https URL 'sini girin ve yolu `/.auth/login/twitter/callback`ekleyin. Örneğin, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.
1. Sayfanın alt kısmındaki en az 100 karakter yazarak **Bu uygulamanın nasıl kullanılacağını bize söyleyin**, sonra **Oluştur**' u seçin. Açılır pencerede yeniden **Oluştur** ' a tıklayın. Uygulama ayrıntıları görüntülenir.
1. **Anahtarlar ve erişim belirteçleri** sekmesini seçin.

   Şu değerleri bir yere göz önünde oluşturun:
   - API anahtarı
   - API gizli anahtarı

   > [!NOTE]
   > API gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı herkesle paylaşmayın veya uygulamanızla dağıtın.

## <a name="add-twitter-information-to-your-application"></a><a name="secrets"> </a>Uygulamanıza Twitter bilgilerini ekleyin

1. [Azure Portal]uygulamanıza gidin.
1. **Ayarlar** > **kimlik doğrulaması/yetkilendirme**' yi seçin ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
1. **Twitter**' ı seçin.
1. Daha önce edindiğiniz `API key` ve `API secret key` değerlerini yapıştırın.
1. **Tamam**’ı seçin.

   ![Mobil uygulama Twitter ayarlarının ekran görüntüsü][1]

   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.

1. Seçim Sitenize erişimi yalnızca Twitter tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Twitter**'da **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca kimlik doğrulaması için tüm kimliği doğrulanmamış istekleri Twitter 'a yönlendirir.

   > [!CAUTION]
   > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için **anonim Isteklere Izin ver (eylem yok)** , uygulamanın kimlik doğrulamanın kendisini el ile başlattığı şekilde tercih edilebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).

1. **Kaydet**’i seçin.

Artık uygulamanızda kimlik doğrulaması için Twitter kullanmaya hazırsınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
