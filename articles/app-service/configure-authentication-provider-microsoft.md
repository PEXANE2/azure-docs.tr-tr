---
title: Microsoft kimlik doğrulamasını yapılandırma
description: Uygulama Hizmeti uygulamanız için kimlik sağlayıcısı olarak Microsoft Hesabı kimlik doğrulamasını nasıl yapılandıracaksınız öğrenin.
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.topic: article
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: 95c603d4a10eb0e4d0817e20755c0f9b36baa96f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842342"
---
# <a name="configure-your-app-service-app-to-use-microsoft-account-login"></a>Microsoft Hesabı girişini kullanacak şekilde Uygulama Hizmeti uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu, Azure Uygulama Hizmeti'ni kişisel Microsoft hesap oturumlarını desteklemek için AAD'yi kullanacak şekilde nasıl yapılandırabileceğinizi gösterir.

> [!NOTE]
> Hem kişisel Microsoft hesapları hem de kuruluş hesapları AAD kimlik sağlayıcısını kullanır. Şu anda, bu kimlik sağlayıcısını her iki oturum açma türünü destekleyecek şekilde yapılandırmak mümkün değildir.

## <a name="register-your-app-with-microsoft-account"></a><a name="register-microsoft-account"> </a>Uygulamanızı Microsoft Hesabı'na kaydedin

1. Azure portalındaki [**Uygulama kayıtlarına**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) gidin. Gerekirse Microsoft hesabınızla oturum açın.
1. **Yeni kayıt**seçin, ardından bir uygulama adı girin.
1. **Desteklenen hesap türleri**altında, herhangi bir kuruluş **dizininde (Azure REKLAM dizini - Çoklu Kiracı) ve kişisel Microsoft hesaplarında (örneğin Skype, Xbox) Hesapları** seçin
1. **Url'leri Yönlendirme'de** **Web'i** `https://<app-domain-name>/.auth/login/aad/callback`seçin ve sonra girin. * \<Uygulama alan adı>* uygulamanızın etki alanı adı ile değiştirin.  Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. URL'deki HTTPS düzenini kullandığınızdan emin olun.

1. **Kaydol**’u seçin.
1. Uygulama **(İstemci) Kimliğini**kopyalayın. Buna daha sonra ihtiyacınız olacak.
1. Sol bölmeden **Sertifikalar & sırları** > **yeni istemci gizli**seçin. Açıklama girin, geçerlilik süresini seçin ve **Ekle'yi**seçin.
1. **Sertifikalar & sırlar** sayfasında görünen değeri kopyalayın. Sayfadan çıktıktan sonra, sayfa bir daha görüntülenmez.

    > [!IMPORTANT]
    > İstemci gizli değeri (parola) önemli bir güvenlik kimlik bilgisidir. Parolayı kimseyle paylaşmayın veya istemci uygulaması içinde dağıtmayın.

## <a name="add-microsoft-account-information-to-your-app-service-application"></a><a name="secrets"> </a>Uygulama Hizmeti uygulamanıza Microsoft Hesabı bilgilerini ekleme

1. [Azure portalındaki]uygulamanıza gidin.
1. **Ayarlar** > **Kimlik Doğrulaması / Yetkilendirme'yi**seçin ve Uygulama Hizmeti Kimlik **Doğrulamasının** **Üzerinde**olduğundan emin olun.
1. **Kimlik Doğrulama Sağlayıcıları**altında Azure Etkin **Dizini'ni**seçin. Yönetim altında **Gelişmiş** **modu'nu**seçin. Daha önce aldığınız Uygulama (istemci) kimliğine ve istemci sırrına yapıştırın. Veren **https://login.microsoftonline.com/9188040d-6c67-4c5b-b112-36a304b66dad/v2.0** **Url** alanı için kullanın.
1. **Tamam'ı**seçin.

   Uygulama Hizmeti kimlik doğrulaması sağlar, ancak sitenizin içeriğine ve API'lerine yetkili erişimi kısıtlamaz. Uygulama kodunuzdaki kullanıcıları yetkilendirmeniz gerekir.

1. (İsteğe bağlı) Microsoft hesap kullanıcılarına erişimi kısıtlamak için, istek Azure **Active Directory ile oturum**açma kimliği **doğrulandığında Yapılacak Eylem'i** ayarlayın. Bu işlevselliği ayarladığınızda, uygulamanız tüm isteklerin kimliğinin doğrulanmasını gerektirir. Ayrıca, kimlik doğrulaması için AAD'yi kullanmak için tüm kimlik doğrulamalı istekleri yeniden yönlendirir. **İhraçÇı Url'nizi** Microsoft Hesabı kiracısını kullanacak şekilde yapılandırdığınız için, yalnızca kişisel acccounts'un başarılı bir şekilde doğrulanmış olacağını unutmayın.

   > [!CAUTION]
   > Bu şekilde erişimi kısıtlamak, uygulamanıza yapılan tüm aramalar için geçerlidir ve bu da birçok tek sayfalı uygulamada olduğu gibi herkese açık bir ana sayfaya sahip uygulamalar için isden edilmeyebilir. Bu tür uygulamalar için, uygulamanın kimlik doğrulamayı el ile başlatması için **anonim isteklere izin ver (eylem yok)** tercih edilebilir. Daha fazla bilgi için [kimlik doğrulama akışına](overview-authentication-authorization.md#authentication-flow)bakın.

1. **Kaydet'i**seçin.

Artık uygulamanızda kimlik doğrulama için Microsoft Hesabı'nı kullanmaya hazırsınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portalında]: https://portal.azure.com/
