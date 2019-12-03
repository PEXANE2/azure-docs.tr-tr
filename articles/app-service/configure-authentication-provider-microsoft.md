---
title: Microsoft kimlik doğrulamasını yapılandırma
description: Microsoft hesabı kimlik doğrulamasını App Service uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.topic: article
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: f9158a4094b7d2ec148c2cae85decb3ad959b7c3
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671932"
---
# <a name="configure-your-app-service-app-to-use-microsoft-account-login"></a>App Service uygulamanızı Microsoft hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Microsoft hesabı kullanmak üzere nasıl yapılandırılacağı gösterilmektedir. 

## <a name="register-microsoft-account"> </a>Uygulamanızı Microsoft hesabı ile kaydedin

1. Azure portal [**uygulama kayıtları**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) gidin. Gerekirse Microsoft hesabı oturum açın.
1. **Yeni kayıt**' ı seçin ve ardından bir uygulama adı girin.
1. **Yeniden yönlendirme URI 'lerinde**, **Web**' i seçin ve ardından `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`girin. *\<App-domain-name >* değerini uygulamanızın etki alanı adıyla değiştirin.  Örneğin, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. URL 'de HTTPS şemasını kullandığınızdan emin olun.

1. **Kaydol**’u seçin.
1. **Uygulama (istemci) kimliğini**kopyalayın. Buna daha sonra ihtiyacınız olacak.
1. Sol bölmeden **sertifikalar & gizli** dizileri > **yeni istemci parolası**' nı seçin. Bir açıklama girin, geçerlilik süresini seçin ve **Ekle**' yi seçin.
1. **Sertifikalar & gizlilikler** sayfasında görüntülenen değeri kopyalayın. Sayfadan ayrıldıktan sonra yeniden görüntülenmezler.

    > [!IMPORTANT]
    > Parola, önemli bir güvenlik kimlik bilgileridir. Parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.

## <a name="secrets"> </a>App Service uygulamanıza Microsoft hesabı bilgilerini ekleme

1. [Azure portalda]uygulamanıza gidin.
1. **Kimlik doğrulama ve yetkilendirme** > **ayarları** seçin ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
1. **Kimlik doğrulama sağlayıcıları**altında **Microsoft hesabı**' nı seçin. Daha önce edindiğiniz uygulama (istemci) KIMLIĞINI ve istemci gizli anahtarını yapıştırın. Uygulamanız için gereken tüm kapsamları etkinleştirin.
1. **Tamam**’ı seçin.

   App Service kimlik doğrulaması sağlar, ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.

1. Seçim Microsoft hesabı kullanıcılara erişimi kısıtlamak için, **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylemi** **Microsoft hesabıyla oturum**açmak üzere ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca, kimlik doğrulaması için tüm kimliği doğrulanmamış istekleri Microsoft hesabı yönlendirir.

   > [!CAUTION]
   > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için **anonim Isteklere Izin ver (eylem yok)** , uygulamanın kimlik doğrulamanın kendisini el ile başlattığı şekilde tercih edilebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).

1. **Kaydet**’i seçin.

Artık uygulamanızda kimlik doğrulaması için Microsoft hesabı 'nı kullanmaya hazırsınız.

## <a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portalda]: https://portal.azure.com/
