---
title: Azure AD kimlik doğrulamasını yapılandırma
description: Azure Active Directory kimlik doğrulamasını App Service uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 09/03/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 717336e0ddfe99c96afda4861f4de1239ee949bf
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77913217"
---
# <a name="configure-your-app-service-app-to-use-azure-ad-login"></a>App Service uygulamanızı Azure AD oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, bir kimlik doğrulama sağlayıcısı olarak Azure Active Directory (Azure AD) kullanmak üzere Azure App Service nasıl yapılandırılacağı gösterilmektedir.

Uygulamanızı ve kimlik doğrulamayı ayarlarken bu en iyi uygulamaları izleyin:

- Her bir App Service uygulamasına kendi izinlerini ve onayını verin.
- Her App Service uygulamasını kendi kaydıyla yapılandırın.
- Ayrı dağıtım yuvaları için ayrı uygulama kayıtları kullanarak ortamlar arasında izin paylaşımını önleyin. Yeni kodu sınarken, bu uygulama sorunların üretim uygulamasını etkilemesini önlemeye yardımcı olabilir.

## <a name="express"> </a>Hızlı ayarlarla Yapılandırma

> [!NOTE]
> **Express** seçeneği, kamu bulutları için kullanılamaz. 

1. [Azure portalındaki], **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin.
2. Sol gezinti bölmesinde, **üzerinde** **kimlik doğrulama/yetkilendirme** > seçin.
3. **Azure Active Directory** > **Express**' i seçin.

   Bunun yerine var olan bir uygulama kaydını seçmek istiyorsanız:

   1. **Var olan ad uygulamasını Seç**' i seçin ve **Azure AD uygulaması**' ye tıklayın.
   2. Mevcut bir uygulama kaydı seçin ve **Tamam**' a tıklayın.

3. App Service uygulamasını Azure Active Directory kaydettirmek için **Tamam ' ı** seçin. Yeni bir uygulama kaydı oluşturulur.
   
    ![Azure Active Directory Express ayarları](./media/configure-authentication-provider-aad/express-settings.png)
   
4. Seçim Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz. Uygulama erişimini yalnızca Azure Active Directory tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, **isteğin kimliği doğrulanmadığı zaman gerçekleştirilecek eylemi** **Azure Active Directory ile oturum**açmak için ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca, kimlik doğrulaması için Azure Active Directory tüm kimliği doğrulanmamış olarak yeniden yönlendirir.

    > [!CAUTION]
    > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için, **anonim Isteklere Izin ver (eylem yok)** , uygulama oturum açma işlemini el ile başlatarak uygulamayı tercih edebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).
5. **Kaydet**’i seçin.

## <a name="advanced"> </a>Gelişmiş ayarlarla Yapılandırma

Farklı bir Azure AD kiracısından uygulama kaydı kullanmak istiyorsanız, uygulama ayarlarını el ile yapılandırabilirsiniz. Bu özel yapılandırmayı gerçekleştirmek için:

1. Azure AD 'de bir kayıt oluşturun.
2. App Service için bazı kayıt ayrıntılarını sağlayın.

### <a name="register"> </a>App Service uygulamanız IÇIN Azure AD 'de bir uygulama kaydı oluşturma

App Service uygulamanızı yapılandırırken aşağıdaki bilgiler gerekir:

- İstemci Kimliği
- Kiracı Kimliği
- İstemci parolası (isteğe bağlı)
- Uygulama KIMLIĞI URI 'SI

Aşağıdaki adımları uygulayın:

1. [Azure portalındaki]oturum açın, **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin. Uygulamanızın **URL 'sini**aklınızda edin. Azure Active Directory Uygulama kaydınızı yapılandırmak için kullanacaksınız.
1. **Yeni kayıt** > **uygulama kayıtları** **Azure Active Directory** > seçin.
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **Web** ' i seçin ve `<app-url>/.auth/login/aad/callback`yazın. Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. **Oluştur**’u seçin.
1. Uygulama kaydı oluşturulduktan sonra, daha sonra için **uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimliğini** kopyalayın.
1. **Marka**seçin. **Giriş sayfası URL 'si**' nde App Service uygulamanızın URL 'sini girin ve **Kaydet**' i seçin.
1. **BIR apı** > **kümesini**kullanıma sunma ' yı seçin. App Service uygulamanızın URL 'sini yapıştırın ve **Kaydet**' i seçin.

   > [!NOTE]
   > Bu değer, uygulama kaydının **uygulama kimliği URI 'sidir** . Web uygulamanız buluttaki bir API 'ye erişim gerektiriyorsa, bulut App Service kaynağını yapılandırırken Web uygulamasının **uygulama kimliği URI 'sine** ihtiyacınız vardır. Bunu, örneğin, bulut hizmetinin Web uygulamasına açıkça erişim vermesini istiyorsanız kullanabilirsiniz.

1. **Kapsam ekle**’yi seçin.
   1. **Kapsam adı**alanına *user_impersonation*girin.
   1. Metin kutularına kullanıcıların onay sayfasında görmesini istediğiniz izin kapsam adını ve açıklamasını girin. Örneğin, *uygulamama erişim*' i girin. 
   1. **Kapsam Ekle**' yi seçin.
1. Seçim Bir istemci parolası oluşturmak için **sertifikalar & gizli** dizileri > **yeni istemci gizli** dizisi > **Ekle**' yi seçin. Sayfada gösterilen istemci gizli değerini kopyalayın. Yeniden gösterilmeyecektir.
1. Seçim Birden çok **yanıt URL 'si**eklemek Için **kimlik doğrulaması**' nı seçin.

### <a name="secrets"> </a>App Service uygulamanızda Azure Active Directory etkinleştirme

1. [Azure portalındaki], **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin. 
1. Sol bölmede, **Ayarlar**' ın altında, **üzerinde** **kimlik doğrulama/yetkilendirme** > ' yi seçin.
1. Seçim Varsayılan olarak, App Service kimlik doğrulaması uygulamanıza yönelik kimliği doğrulanmamış erişime izin verir. Kullanıcı kimlik doğrulamasını zorlamak için, **isteğin kimliği doğrulanmadığı zaman** **Azure Active Directory ile oturum**açmak için yapılacak eylemi ayarlayın.
1. **Kimlik doğrulama sağlayıcıları**altında **Azure Active Directory**' yi seçin.
1. **Yönetim modu**' nda **Gelişmiş** ' i seçin ve App Service kimlik doğrulamasını aşağıdaki tabloya göre yapılandırın:

    |Alan|Açıklama|
    |-|-|
    |İstemci Kimliği| Uygulama kaydının **uygulama (istemci) kimliğini** kullanın. |
    |Verenin KIMLIĞI| `https://login.microsoftonline.com/<tenant-id>`kullanın ve *\<Kiracı kimliği >* , uygulama kaydının **Dizin (kiracı) kimliğiyle** değiştirin. |
    |İstemci parolası (Isteğe bağlı)| Uygulama kaydında oluşturduğunuz istemci gizli anahtarını kullanın.|
    |İzin verilen belirteç Izleyicileri| Bu bir bulut veya sunucu uygulaması ise ve bir Web uygulamasından kimlik doğrulama belirteçlerine izin vermek istiyorsanız, Web uygulamasının **uygulama KIMLIĞI URI** 'sini buraya ekleyin. Yapılandırılan **ISTEMCI kimliği** *her zaman* örtük olarak izin verilen bir hedef kitle olarak kabul edilir. |

2. **Tamam**' ı ve ardından **Kaydet**' i seçin.

Artık App Service uygulamanızda kimlik doğrulaması için Azure Active Directory kullanmaya hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulaması yapılandırma

**Active Directory Authentication Library**gibi bir istemci kitaplığı kullanarak kimlik doğrulamaya izin vermek için yerel istemcileri kaydedebilirsiniz.

1. [Azure portalındaki] **Active Directory** > **Yeni kayıt** > **uygulama kayıtları** seçin.
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **ortak istemci (mobil & Masaüstü)** öğesini seçin ve `<app-url>/.auth/login/aad/callback`URL 'yi yazın. Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Bir Windows uygulaması için, bunun yerine URI olarak [paket SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 'sini kullanın.
1. **Oluştur**’u seçin.
1. Uygulama kaydı oluşturulduktan sonra, **uygulama (istemci) kimliği**değerini kopyalayın.
1. **API izinleri** > , API **'lerim** > **izin Ekle** ' yi seçin.
1. Daha önce App Service uygulamanız için oluşturduğunuz uygulama kaydını seçin. Uygulama kaydını görmüyorsanız, [App Service uygulamanız Için Azure AD 'de uygulama kaydı oluşturma](#register)bölümüne **user_impersonation** kapsamını eklediğinizden emin olun.
1. **User_impersonation**' yi seçin ve ardından **izin Ekle**' yi seçin.

Artık App Service uygulamanıza erişebilen bir yerel istemci uygulaması yapılandırdınız.

## <a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[Azure portalındaki]: https://portal.azure.com/
