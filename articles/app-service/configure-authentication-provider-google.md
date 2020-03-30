---
title: Google kimlik doğrulamasını yapılandırma
description: Uygulama Hizmeti uygulamanız için bir kimlik sağlayıcısı olarak Google kimlik doğrulamasını nasıl yapılandıracaksınız öğrenin.
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.topic: article
ms.date: 09/02/2019
ms.custom: seodec18
ms.openlocfilehash: 81ce3e393d308323c8d5a3d688c16c9b45e7be9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74670820"
---
# <a name="configure-your-app-service-app-to-use-google-login"></a>Google oturum açma işlemlerini kullanacak şekilde Uygulama Hizmeti uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu, Azure Uygulama Hizmeti'ni Google'ı kimlik doğrulama sağlayıcısı olarak kullanacak şekilde nasıl yapılandırdığınızı gösterir.

Bu konudaki yordamı tamamlamak için, doğrulanmış bir e-posta adresine sahip bir Google hesabınız olması gerekir. Yeni bir Google hesabı oluşturmak için [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302) adresine gidin.

## <a name="register-your-application-with-google"></a><a name="register"> </a>Uygulamanızı Google'a kaydedin

1. Bir istemci kimliği ve istemci sırrı oluşturmak [için sunucu tarafındaki uygulamalar için Google Oturum Aç'taki Google](https://developers.google.com/identity/sign-in/web/server-side-flow) belgelerini izleyin. Herhangi bir kod değişikliği yapmanıza gerek yoktur. Sadece aşağıdaki bilgileri kullanın:
    - **Yetkili JavaScript Origins**için, `https://<app-name>.azurewebsites.net` * \<uygulama adı>* uygulamanızın adı ile kullanın.
    - **Yetkili Yeniden Yönlendirme URI** `https://<app-name>.azurewebsites.net/.auth/login/google/callback`için , kullanın.
1. Uygulama Kimliğini ve Uygulama gizli değerlerini kopyalayın.

    > [!IMPORTANT]
    > App sırrı önemli bir güvenlik kimlik bilgisidir. Bu sırrı kimseyle paylaşmayın veya istemci uygulaması içinde dağıtmayın.

## <a name="add-google-information-to-your-application"></a><a name="secrets"> </a>Google bilgilerini uygulamanıza ekleyin

1. Azure [portalında,]Uygulama Hizmeti uygulamanıza gidin.
1. **Ayarlar** > **Kimlik Doğrulaması / Yetkilendirme'yi**seçin ve Uygulama Hizmeti Kimlik **Doğrulamasının** **Üzerinde**olduğundan emin olun.
1. **Google'ı**seçin, ardından daha önce elde ettiğiniz App Id ve App Secret değerlerini yapıştırın. Uygulamanızın gerektirdiği tüm kapsamları etkinleştirin.
1. **Tamam'ı**seçin.

   Uygulama Hizmeti kimlik doğrulaması sağlar, ancak sitenizin içeriğine ve API'lerine yetkili erişimi kısıtlamaz. Daha fazla bilgi için [bkz.](app-service-authentication-how-to.md#authorize-or-deny-users)

1. (İsteğe bağlı) Site erişimini yalnızca Google tarafından kimlik doğrulaması yapılan kullanıcılarla sınırlamak için, istek **Google'da** **kimlik doğrulaması olmadığında Eylem'i harekete** geçirin. Bu işlevselliği ayarladığınızda, uygulamanız tüm isteklerin doğrulamasını gerektirir. Ayrıca, kimlik doğrulama için tüm kimlik doğrulamalı istekleri Google'a yönlendirir.

    > [!CAUTION]
    > Bu şekilde erişimi kısıtlamak, uygulamanıza yapılan tüm aramalar için geçerlidir ve bu da birçok tek sayfalı uygulamada olduğu gibi herkese açık bir ana sayfaya sahip uygulamalar için isden edilmeyebilir. Bu tür uygulamalar için, uygulamanın kimlik doğrulamayı el ile başlatması için **anonim isteklere izin ver (eylem yok)** tercih edilebilir. Daha fazla bilgi için [kimlik doğrulama akışına](overview-authentication-authorization.md#authentication-flow)bakın.

1. **Kaydet'i**seçin.

Artık uygulamanızda kimlik doğrulama için Google'ı kullanmaya hazırsınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portalında]: https://portal.azure.com/

