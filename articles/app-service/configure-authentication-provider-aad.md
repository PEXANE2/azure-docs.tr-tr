---
title: Azure AD kimlik doğrulamasını yapılandırma
description: Uygulama Hizmetiniz veya Azure İşlevler uygulamanız için kimlik sağlayıcısı olarak Azure Active Directory kimlik doğrulamasını nasıl yapılandırabilirsiniz öğrenin.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 09/03/2019
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: dbbe58df4f1cfe93555b494e525fad18f5b02664
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632579"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-azure-ad-login"></a>Azure AD oturum unu kullanmak için Uygulama Hizmetinizi veya Azure İşlevleri uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure Uygulama Hizmeti veya Azure İşlevlerini, kimlik doğrulama sağlayıcısı olarak Azure Etkin Dizini (Azure AD) kullanacak şekilde nasıl yapılandırabileceğiniz gösterilmektedir.

> [!NOTE]
> Şu anda Azure [Active Directory v2.0](../active-directory/develop/v2-overview.md) [(MSAL](../active-directory/develop/msal-overview.md)dahil) Azure Uygulama Hizmeti ve Azure İşlevleri için desteklenmez. Güncellemeler için lütfen tekrar kontrol edin.
>

Uygulamanızı ve kimlik doğrulamanızı ayarlarken aşağıdaki en iyi uygulamaları izleyin:

- Her Uygulama Hizmeti uygulamasına kendi izinlerini ve onayını verin.
- Her App Service uygulamasını kendi kaydıyla yapılandırın.
- Ayrı dağıtım yuvaları için ayrı uygulama kayıtları kullanarak ortamlar arasında izin paylaşımından kaçının. Yeni kodu sınarken, bu uygulama sorunların üretim uygulamasını etkilemesini önlemeye yardımcı olabilir.

## <a name="configure-with-express-settings"></a><a name="express"> </a>Ekspres ayarlarla yapılandırma

> [!NOTE]
> **Express** seçeneği devlet bulutları için kullanılamaz. 

1. Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin.
2. Sol daki gezintiden **Kimlik Doğrulama / Yetkilendirme** > **On'u**seçin.
3. **Azure Active Directory Express'i** > **Express**seçin.

   Bunun yerine varolan bir uygulama kaydı seçmek istiyorsanız:

   1. **Varolan AD uygulamasını seçin**ve **ardından Azure AD Uygulamasını**tıklatın.
   2. Varolan bir uygulama kaydı seçin ve **Tamam'ı**tıklatın.

3. Uygulama Hizmeti uygulamasını Azure Active Directory'ye kaydetmek için **Tamam'ı** seçin. Yeni bir uygulama kaydı oluşturulur.
   
    ![Azure Active Directory'de ekspres ayarlar](./media/configure-authentication-provider-aad/express-settings.png)
   
4. (İsteğe bağlı) Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak sitenizin içeriğine ve API'lerine yetkili erişimi kısıtlamaz. Uygulama kodunuzdaki kullanıcıları yetkilendirmeniz gerekir. Uygulama erişimini yalnızca Azure Active Directory tarafından kimlik doğrulaması yapılan kullanıcılarla sınırlamak için, istek Azure **Active Directory ile oturum**açma kimliği ne zaman gerçekleşse de Eylem'i **ayarlayın.** Bu işlevselliği ayarladığınızda, uygulamanız tüm isteklerin kimliğinin doğrulanmasını gerektirir. Ayrıca, kimlik doğrulaması için kimlik doğrulaması olmayan tüm bunları Azure Etkin Dizin'e yönlendirir.

    > [!CAUTION]
    > Bu şekilde erişimi kısıtlamak, uygulamanıza yapılan tüm aramalar için geçerlidir ve bu da birçok tek sayfalı uygulamada olduğu gibi herkese açık bir ana sayfaya sahip uygulamalar için isden edilmeyebilir. Bu tür uygulamalar için, uygulamanın oturum açmayı el ile başlatmasıyla anonim isteklere izin ver **(eylem yok)** tercih edilebilir. Daha fazla bilgi için [kimlik doğrulama akışına](overview-authentication-authorization.md#authentication-flow)bakın.
5. **Kaydet'i**seçin.

## <a name="configure-with-advanced-settings"></a><a name="advanced"> </a>Gelişmiş ayarlarla yapılandırma

Farklı bir Azure AD kiracısından uygulama kaydı kullanmak istiyorsanız uygulama ayarlarını el ile yapılandırabilirsiniz. Bu özel yapılandırmayı tamamlamak için:

1. Azure AD'de bir kayıt oluşturun.
2. Kayıt bilgilerinin bir kısmını Uygulama Hizmeti'ne sağlayın.

### <a name="create-an-app-registration-in-azure-ad-for-your-app-service-app"></a><a name="register"> </a>Uygulama Hizmeti uygulamanız için Azure AD'de bir uygulama kaydı oluşturma

Uygulama Hizmeti uygulamanızı yapılandırırken aşağıdaki bilgilere ihtiyacınız olacak:

- İstemci Kimliği
- Kiracı Kimliği
- İstemci sırrı (isteğe bağlı)
- Uygulama Kimliği URI

Aşağıdaki adımları uygulayın:

1. [Azure portalında]oturum açın, **Uygulama Hizmetlerini**arayın ve seçin ve ardından uygulamanızı seçin. Uygulamanızın URL'sine dikkat edin. **URL** Azure Active Directory uygulama kaydınızı yapılandırmak için kullanırsınız.
1. **Azure Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.
1. Bir uygulama sayfası **kaydol,** uygulama kaydı için bir **Ad** girin.
1. **URI'yi Yeniden**Yönlendirme'de `<app-url>/.auth/login/aad/callback` **Web'i** seçin ve yazın. Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. **Oluştur'u**seçin.
1. Uygulama kaydı oluşturulduktan **sonra, Uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimliğini** daha sonra kopyalayın.
1. **Kimlik Doğrulaması**'nı seçin. **Örtülü hibe**kapsamında, Uygulama Hizmeti'nden OpenID Connect kullanıcı oturum açmalarına izin vermek için **kimlik belirteçlerini** etkinleştirin.
1. (İsteğe bağlı) **Markaseçin.** **Giriş sayfası URL'sinde,** Uygulama Hizmeti uygulamanızın URL'sini girin ve **Kaydet'i**seçin.
1.  >  **API Kümesini Ortaya Çıkar'ı**seçin.**Set** Tek kiracılı uygulama için, App Service uygulamanızın URL'sini yapıştırın ve **Kaydet'i** seçin ve çok kiracılı uygulamayı seçin, kiracı onaylı etki alanlarından birini temel alan URL'ye yapıştırın ve ardından **Kaydet'i**seçin.

   > [!NOTE]
   > Bu değer, uygulama kaydının Uygulama Kimliği URI'sidir. **Application ID URI** Web uygulamanız buluttaki bir API'ye erişim gerektiriyorsa, bulut Uygulama Hizmeti kaynağını yapılandırırken web uygulamasının **Uygulama Kimliği URI'sine** ihtiyacınız vardır. Örneğin, bulut hizmetinin web uygulamasına açıkça erişim izni vermesini istiyorsanız bunu kullanabilirsiniz.

1. **Kapsam ekle**’yi seçin.
   1. **Kapsam adına**, *user_impersonation*girin.
   1. Metin kutularına, kullanıcıların onay sayfasında görmesini istediğiniz onay kapsamı adını ve açıklamasını girin. Örneğin, *uygulamama Eriş'i*girin. 
   1. **Kapsam Ekle'yi**seçin.
1. (İsteğe bağlı) İstemci sırrı oluşturmak için **Sertifikalar & sırları** > **yeni istemci gizli** > **Ekle**seçin. Sayfada gösterilen istemci gizli değerini kopyalayın. Bir daha gösterilmeyecek.
1. (İsteğe bağlı) Birden çok **Yanıt URL'si**eklemek için **Kimlik Doğrulama'yı**seçin.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>Uygulama Hizmeti uygulamanızda Azure Active Directory'yi etkinleştirin

1. Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin. 
1. Sol bölmede, **Ayarlar**altında, **Kimlik Doğrulama / Yetkilendirme** > **Açık'ı**seçin.
1. (İsteğe bağlı) Varsayılan olarak, App Service kimlik doğrulaması uygulamanıza kimlik doğrulamasız erişim sağlar. Kullanıcı kimlik doğrulamasını zorlamak için, istek Azure Etkin **Dizini ile oturum**açmak için **kimlik doğrulaması olmadığında Eylem'i ayarla.**
1. **Kimlik Doğrulama Sağlayıcıları**altında Azure Etkin **Dizini'ni**seçin.
1. **Yönetim modunda,** **Gelişmiş** ve aşağıdaki tabloya göre Uygulama Hizmeti kimlik doğrulamasını yapılandırın:

    |Alan|Açıklama|
    |-|-|
    |İstemci Kimliği| Uygulama kaydının **Uygulama (istemci) kimliğini** kullanın. |
    |İhraççı Url| Kiracı `https://login.microsoftonline.com/<tenant-id>`kimliği * \<>* uygulama kaydının **Dizin (kiracı) kimliğiyle** kullanın ve değiştirin. Bu değer, kullanıcıları doğru Azure AD kiracısına yönlendirmek ve örneğin uygun belirteç imzalama anahtarlarını ve belirteç veren talep değerini belirlemek için uygun meta verileri indirmek için kullanılır. |
    |İstemci Sırrı (İsteğe Bağlı)| Uygulama kaydında oluşturduğunuz istemci sırrını kullanın.|
    |İzin Verilen Belirteç Kitleleri| Bu bir bulut veya sunucu uygulamasıysa ve bir web uygulamasından kimlik doğrulama belirteçlerine izin vermek istiyorsanız, web uygulamasının **Uygulama Kimliği URI'sini** buraya ekleyin. Yapılandırılan **İstemci Kimliği** *her zaman* dolaylı olarak izin verilen bir hedef kitle olarak kabul edilir. |

2. **Tamam'ı**seçin ve ardından **Kaydet'i**seçin.

Artık Uygulama Hizmeti uygulamanızda kimlik doğrulama için Azure Active Directory'yi kullanmaya hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulamasını yapılandırma

**Active Directory Authentication Library**gibi bir istemci kitaplığını kullanarak uygulamanızda barındırılan Web API'sına kimlik doğrulamasına izin vermek için yerel istemcileri kaydedebilirsiniz.

1. Azure [portalında] **Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.
1. Bir uygulama sayfası **kaydol,** uygulama kaydı için bir **Ad** girin.
1. **URI'yi Yeniden**Yönlendirme'de, **Ortak istemciyi (mobil & masaüstü)** seçin ve URL'yi `<app-url>/.auth/login/aad/callback`yazın. Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Bir Microsoft Mağazası uygulaması için, bunun yerine URI olarak [SID paketini](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) kullanın.
1. **Oluştur'u**seçin.
1. Uygulama kaydı oluşturulduktan **sonra, Uygulama (istemci) kimliğinin**değerini kopyalayın.
1. API **izinlerini** > seçin**İzin** > **apilerim**ekleyin.
1. Uygulama Hizmeti uygulamanız için daha önce oluşturduğunuz uygulama kaydını seçin. Uygulama kaydını görmüyorsanız, [Uygulama Hizmeti uygulamanız için Azure AD'da uygulama kaydı oluştur'da](#register) **user_impersonation** kapsamını eklediğinizden emin olun.
1. **user_impersonation'yi**seçin ve ardından **İzin Ekle'yi**seçin.

Artık bir kullanıcı adına Uygulama Hizmeti uygulamanıza erişebilecek yerel bir istemci uygulaması yapılandırıldınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
