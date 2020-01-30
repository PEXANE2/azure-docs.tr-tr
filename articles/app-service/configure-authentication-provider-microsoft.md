---
title: Microsoft kimlik doğrulamasını yapılandırma
description: Microsoft hesabı kimlik doğrulamasını App Service uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.topic: article
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: 95c603d4a10eb0e4d0817e20755c0f9b36baa96f
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842342"
---
# <a name="configure-your-app-service-app-to-use-microsoft-account-login"></a>App Service uygulamanızı Microsoft hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu başlığı altında, kişisel Microsoft hesabı oturumlarını desteklemek için AAD 'yi kullanmak üzere Azure App Service nasıl yapılandırılacağı gösterilmektedir.

> [!NOTE]
> Hem kişisel Microsoft hesapları hem de kuruluş hesapları AAD kimlik sağlayıcısını kullanır. Şu anda, bu kimlik sağlayıcısını her iki oturum açma türünü de destekleyecek şekilde yapılandırmak mümkün değildir.

## <a name="register-microsoft-account"> </a>Uygulamanızı Microsoft hesabı ile kaydedin

1. Azure portal [**uygulama kayıtları**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) gidin. Gerekirse Microsoft hesabı oturum açın.
1. **Yeni kayıt**' ı seçin ve ardından bir uygulama adı girin.
1. **Desteklenen hesap türleri**altında **herhangi bir kuruluş dizininde (HERHANGI bir Azure ad dizini-çoklu kiracı) ve kişisel Microsoft hesaplarından (örn. Skype, Xbox) hesaplar** ' ı seçin
1. **Yeniden yönlendirme URI 'lerinde**, **Web**' i seçin ve ardından `https://<app-domain-name>/.auth/login/aad/callback`girin. *\<App-domain-name >* değerini uygulamanızın etki alanı adıyla değiştirin.  Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. URL 'de HTTPS şemasını kullandığınızdan emin olun.

1. **Kaydol**’u seçin.
1. **Uygulama (istemci) kimliğini**kopyalayın. Buna daha sonra ihtiyacınız olacak.
1. Sol bölmeden **sertifikalar & gizli** dizileri > **yeni istemci parolası**' nı seçin. Bir açıklama girin, geçerlilik süresini seçin ve **Ekle**' yi seçin.
1. **Sertifikalar & gizlilikler** sayfasında görüntülenen değeri kopyalayın. Sayfadan ayrıldıktan sonra yeniden görüntülenmezler.

    > [!IMPORTANT]
    > İstemci gizli değeri (parola) önemli bir güvenlik kimlik bilgileridir. Parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.

## <a name="secrets"> </a>App Service uygulamanıza Microsoft hesabı bilgilerini ekleme

1. [Azure Portal]uygulamanıza gidin.
1. **Kimlik doğrulama ve yetkilendirme** > **ayarları** seçin ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
1. **Kimlik doğrulama sağlayıcıları**altında **Azure Active Directory**' yi seçin. **Yönetim modu**altında **Gelişmiş** ' i seçin. Daha önce edindiğiniz uygulama (istemci) KIMLIĞINI ve istemci gizli anahtarını yapıştırın. **Verenin URL 'si** alanı için **https://login.microsoftonline.com/9188040d-6c67-4c5b-b112-36a304b66dad/v2.0** kullanın.
1. **Tamam**’ı seçin.

   App Service kimlik doğrulaması sağlar, ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.

1. Seçim Microsoft hesabı kullanıcılarına erişimi kısıtlamak için, **isteğin kimliği doğrulanmadığı zaman gerçekleştirilecek eylemi** **Azure Active Directory ile oturum**açmak için ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca kimlik doğrulaması için AAD kullanmak üzere tüm kimliği doğrulanmamış istekleri yeniden yönlendirir. **Sertifikayı veren URL** 'Nizi Microsoft hesabı kiracısını kullanacak şekilde yapılandırdığınıza göre, yalnızca kişisel birikmiş sayımların kimlik doğrulaması başarılı olur.

   > [!CAUTION]
   > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için **anonim Isteklere Izin ver (eylem yok)** , uygulamanın kimlik doğrulamanın kendisini el ile başlattığı şekilde tercih edilebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).

1. **Kaydet**’i seçin.

Artık uygulamanızda kimlik doğrulaması için Microsoft hesabı 'nı kullanmaya hazırsınız.

## <a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure Portal]: https://portal.azure.com/
