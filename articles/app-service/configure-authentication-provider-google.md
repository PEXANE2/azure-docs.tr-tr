---
title: Google kimlik doğrulamasını Yapılandırma-Azure App Service
description: App Service uygulamanız için Google kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/02/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: fcbb284a0807ef88c5f40a7c8b65398d45bf73d7
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232140"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>App Service uygulamanızı Google oturumu kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service Google 'ı bir kimlik doğrulama sağlayıcısı olarak kullanmak üzere nasıl yapılandırabileceğiniz gösterilmektedir.

Bu konudaki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresine sahip bir Google hesabınızın olması gerekir. Yeni bir Google hesabı oluşturmak için [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302) adresine gidin.

## <a name="register"> </a>Uygulamanızı Google ile kaydedin
1. İstemci KIMLIĞI ve istemci gizli anahtarı oluşturmak için, aşağıdaki bilgilerle (herhangi bir kod değişikliği yapmanız gerekmez), [Google oturum açma bölümündeki](https://developers.google.com/identity/sign-in/web/server-side-flow) Google belgelerini takip edin:
    - **Yetkili JavaScript kaynakları**için `https://<app-name>.azurewebsites.net`  *\<app-name >* içinde uygulamanızın adıyla birlikte kullanın.
    - **Yetkili yeniden yönlendirme URI 'si**için `https://<app-name>.azurewebsites.net/.auth/login/google/callback`kullanın.
1. İstemci KIMLIĞI ve istemci gizli dizileri oluşturulduktan sonra değerlerini kopyalayın.

    > [!IMPORTANT]
    > İstemci parolası önemli bir güvenlik kimlik bilgileridir. Bu parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.


## <a name="secrets"> </a>Uygulamanıza Google bilgilerini ekleyin
1. [Azure Portal], App Service uygulamanıza gidin. Sol menüden **kimlik doğrulama/yetkilendirme**' yi seçin.
2. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse, geçişi **Açık**duruma getirin.
3. **Google**'a tıklayın. Daha önce edindiğiniz uygulama KIMLIĞI ve uygulama gizli anahtarı değerlerini yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği tüm kapsamları etkinleştirin. Daha sonra, **Tamam**'a tıklayın.

   App Service kimlik doğrulaması sağlar, ancak site içeriğinize ve API 'Lerine yönelik yetkili erişimi kısıtlamaz. Daha fazla bilgi için bkz. [Yetkilendirmeyi veya reddetme kullanıcıları](app-service-authentication-how-to.md#authorize-or-deny-users).
4. Seçim Sitenize erişimi yalnızca Google tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Google**'da **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Google 'a yönlendirilir.

    > [!NOTE]
    > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.
    
5. **Kaydet**’e tıklayın.

Artık uygulamanızda kimlik doğrulaması için Google 'ı kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

