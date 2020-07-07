---
title: Google kimlik doğrulamasını yapılandırma
description: Google kimlik doğrulamasını App Service veya Azure Işlevleri uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.topic: article
ms.date: 09/02/2019
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: e8a9fbe6072f3628d755ad3ad5aa5a623fc3ab23
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80519938"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-google-login"></a>App Service veya Azure Işlevleri uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu, bir kimlik doğrulama sağlayıcısı olarak Google 'ı kullanmak üzere Azure App Service veya Azure Işlevlerinin nasıl yapılandırılacağını gösterir.

Bu konudaki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresine sahip bir Google hesabınızın olması gerekir. Yeni bir Google hesabı oluşturmak için [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302) adresine gidin.

## <a name="register-your-application-with-google"></a><a name="register"> </a>Uygulamanızı Google ile kaydedin

1. İstemci KIMLIĞI ve istemci parolası oluşturmak için, [Google oturum açma ve sunucu tarafı uygulamaları için](https://developers.google.com/identity/sign-in/web/server-side-flow) Google belgelerini izleyin. Herhangi bir kod değişikliği yapmanız gerekmez. Yalnızca aşağıdaki bilgileri kullanın:
    - **Yetkili JavaScript kaynakları**için ' `https://<app-name>.azurewebsites.net` de uygulamanızın adıyla birlikte kullanın *\<app-name>* .
    - **Yetkili yeniden yönlendirme URI 'si**için kullanın `https://<app-name>.azurewebsites.net/.auth/login/google/callback` .
1. Uygulama KIMLIĞI ve uygulama gizli anahtarı değerlerini kopyalayın.

    > [!IMPORTANT]
    > Uygulama gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.

## <a name="add-google-information-to-your-application"></a><a name="secrets"> </a>Uygulamanıza Google bilgilerini ekleyin

1. [Azure portal]App Service uygulamanıza gidin.
1. **Ayarlar**  >  **kimlik doğrulaması/yetkilendirme**' yi seçin ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
1. **Google**' ı seçin ve daha önce EDINDIĞINIZ uygulama kimliği ve uygulama gizli değerlerini yapıştırın. Uygulamanız için gereken tüm kapsamları etkinleştirin.
1. **Tamam**’ı seçin.

   App Service kimlik doğrulaması sağlar, ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Daha fazla bilgi için bkz. [Yetkilendirmeyi veya reddetme kullanıcıları](app-service-authentication-how-to.md#authorize-or-deny-users).

1. Seçim Site erişimini yalnızca Google tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Google**'da **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca kimlik doğrulaması için tüm kimliği doğrulanmamış istekleri Google 'a yönlendirir.

    > [!CAUTION]
    > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için **anonim Isteklere Izin ver (eylem yok)** , uygulamanın kimlik doğrulamanın kendisini el ile başlattığı şekilde tercih edilebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).

1. **Kaydet**’i seçin.

Artık uygulamanızda kimlik doğrulaması için Google 'ı kullanmaya hazırsınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portalındaki]: https://portal.azure.com/

