---
title: Azure AD kimlik doğrulamasını yapılandırma
description: Azure Active Directory kimlik doğrulamasını App Service veya Azure Işlevleri uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 04/14/2020
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: c3892cfe3f8bd6966f5bd00c0747590eef3bc50d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83860537"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-azure-ad-login"></a>App Service veya Azure Işlevleri uygulamanızı Azure AD oturum açma bilgilerini kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, bir kimlik doğrulama sağlayıcısı olarak Azure Active Directory (Azure AD) kullanmak üzere Azure App Service veya Azure Işlevlerinin nasıl yapılandırılacağı gösterilmektedir.

> [!NOTE]
> Hızlı ayarlar akışı bir AAD v1 uygulama kaydı ayarlar. [Azure Active Directory v 2.0](../active-directory/develop/v2-overview.md) ( [msal](../active-directory/develop/msal-overview.md)dahil) öğesini kullanmak istiyorsanız lütfen [Gelişmiş yapılandırma yönergelerini](#advanced)izleyin.

Uygulamanızı ve kimlik doğrulamayı ayarlarken bu en iyi uygulamaları izleyin:

- Her bir App Service uygulamasına kendi izinlerini ve onayını verin.
- Her App Service uygulamasını kendi kaydıyla yapılandırın.
- Ayrı dağıtım yuvaları için ayrı uygulama kayıtları kullanarak ortamlar arasında izin paylaşımını önleyin. Yeni kodu sınarken, bu uygulama sorunların üretim uygulamasını etkilemesini önlemeye yardımcı olabilir.

> [!NOTE]
> Bu özellik şu anda Azure Işlevleri için Linux tüketim planında kullanılamıyor

## <a name="configure-with-express-settings"></a><a name="express"> </a>Hızlı ayarlarla Yapılandırma

> [!NOTE]
> **Express** seçeneği, kamu bulutları için kullanılamaz.

1. [Azure Portal], **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin.
2. Sol gezinmeden üzerinde **kimlik doğrulama/yetkilendirme**' yi seçin  >  **On**.
3. **Azure Active Directory**  >  **Express**' i seçin.

   Bunun yerine var olan bir uygulama kaydını seçmek istiyorsanız:

   1. **Var olan ad uygulamasını Seç**' i seçin ve **Azure AD uygulaması**' ye tıklayın.
   2. Mevcut bir uygulama kaydı seçin ve **Tamam**' a tıklayın.

3. App Service uygulamasını Azure Active Directory kaydettirmek için **Tamam ' ı** seçin. Yeni bir uygulama kaydı oluşturulur.

    ![Azure Active Directory Express ayarları](./media/configure-authentication-provider-aad/express-settings.png)

4. Seçim Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz. Uygulama erişimini yalnızca Azure Active Directory tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, **isteğin kimliği doğrulanmadığı zaman gerçekleştirilecek eylemi** **Azure Active Directory ile oturum**açmak için ayarlayın. Bu işlevi ayarladığınızda, uygulamanız tüm isteklerin doğrulanmasını gerektirir. Ayrıca, kimlik doğrulaması için Azure Active Directory tüm kimliği doğrulanmamış olarak yeniden yönlendirir.

    > [!CAUTION]
    > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi, genel kullanıma açık bir giriş sayfasına sahip olan uygulamalar için istenmeyebilir. Bu tür uygulamalar için, **anonim Isteklere Izin ver (eylem yok)** , uygulama oturum açma işlemini el ile başlatarak uygulamayı tercih edebilir. Daha fazla bilgi için bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow).
5. **Kaydet**'i seçin.

## <a name="configure-with-advanced-settings"></a><a name="advanced"> </a>Gelişmiş ayarlarla Yapılandırma

Farklı bir Azure AD kiracısından uygulama kaydı kullanmak istiyorsanız, uygulama ayarlarını el ile yapılandırabilirsiniz. Bu özel yapılandırmayı gerçekleştirmek için:

1. Azure AD 'de bir kayıt oluşturun.
2. App Service için bazı kayıt ayrıntılarını sağlayın.

### <a name="create-an-app-registration-in-azure-ad-for-your-app-service-app"></a><a name="register"> </a>App Service uygulamanız IÇIN Azure AD 'de bir uygulama kaydı oluşturma

App Service uygulamanızı yapılandırırken aşağıdaki bilgiler gerekir:

- İstemci Kimliği
- Kiracı Kimliği
- İstemci parolası (isteğe bağlı)
- Uygulama KIMLIĞI URI 'SI

Aşağıdaki adımları uygulayın:

1. [Azure Portal]oturum açın, **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin. Uygulamanızın **URL 'sini**aklınızda edin. Azure Active Directory Uygulama kaydınızı yapılandırmak için kullanacaksınız.
1. **Azure Active Directory**  >  **App registrations**  >  **Yeni kayıt**uygulama kayıtları Azure Active Directory seçin.
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **Web** ' i seçin ve yazın `<app-url>/.auth/login/aad/callback` . Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.
1. **Oluştur**'u seçin.
1. Uygulama kaydı oluşturulduktan sonra, daha sonra için **uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimliğini** kopyalayın.
1. **Kimlik doğrulaması**' nı seçin. **Örtük izin**' ın altında, App Service **kimlik belirteçlerini** , OpenID kullanıcı oturum açma işlemlerinin izin verecek şekilde etkinleştirin.
1. Seçim **Marka**seçin. **Giriş sayfası URL 'si**' nde App Service uygulamanızın URL 'sini girin ve **Kaydet**' i seçin.
1. **Bir API kümesini kullanıma**sunma ' yı seçin  >  **Set**. Tek kiracılı uygulama için App Service uygulamanızın URL 'sini yapıştırın ve **Kaydet** ' i seçin ve çok kiracılı uygulama için, kiracı tarafından doğrulanan etki alanlarından birini temel alan URL 'yi yapıştırın ve ardından **Kaydet**' i seçin.

   > [!NOTE]
   > Bu değer, uygulama kaydının **uygulama kimliği URI 'sidir** . Web uygulamanız buluttaki bir API 'ye erişim gerektiriyorsa, bulut App Service kaynağını yapılandırırken Web uygulamasının **uygulama kimliği URI 'sine** ihtiyacınız vardır. Bunu, örneğin, bulut hizmetinin Web uygulamasına açıkça erişim vermesini istiyorsanız kullanabilirsiniz.

1. **Kapsam ekle**’yi seçin.
   1. **Kapsam adı**alanına *user_impersonation*girin.
   1. Metin kutularına kullanıcıların onay sayfasında görmesini istediğiniz izin kapsam adını ve açıklamasını girin. Örneğin, *uygulamama erişim*' i girin.
   1. **Kapsam Ekle**' yi seçin.
1. Seçim Bir istemci parolası oluşturmak için **Sertifikalar & gizli**  >  **anahtar Ekle yeni istemci parolası**' nı seçin  >  **Add**. Sayfada gösterilen istemci gizli değerini kopyalayın. Yeniden gösterilmeyecektir.
1. Seçim Birden çok **yanıt URL 'si**eklemek Için **kimlik doğrulaması**' nı seçin.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>App Service uygulamanızda Azure Active Directory etkinleştirme

1. [Azure Portal], **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin.
1. Sol bölmedeki **Ayarlar**altında **kimlik doğrulama/yetkilendirme**' yi seçin  >  **On**.
1. Seçim Varsayılan olarak, App Service kimlik doğrulaması uygulamanıza yönelik kimliği doğrulanmamış erişime izin verir. Kullanıcı kimlik doğrulamasını zorlamak için, **isteğin kimliği doğrulanmadığı zaman** **Azure Active Directory ile oturum**açmak için yapılacak eylemi ayarlayın.
1. **Kimlik doğrulama sağlayıcıları**altında **Azure Active Directory**' yi seçin.
1. **Yönetim modu**' nda **Gelişmiş** ' i seçin ve App Service kimlik doğrulamasını aşağıdaki tabloya göre yapılandırın:

    |Alan|Açıklama|
    |-|-|
    |İstemci Kimliği| Uygulama kaydının **uygulama (istemci) kimliğini** kullanın. |
    |Veren URL 'Si| `<authentication-endpoint>/<tenant-id>/v2.0`' İ kullanın ve, *\<authentication-endpoint>* [bulut ortamınız için kimlik doğrulama uç noktası](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints) ile değiştirin (örneğin, " https://login.microsoft.com Genel Azure için"), Ayrıca, *\<tenant-id>* uygulama KAYDıNıN oluşturulduğu **Dizin (kiracı) kimliğiyle** değiştirin. Bu değer, kullanıcıları doğru Azure AD kiracısına yönlendirmek için ve ilgili meta verileri, uygun belirteç imzalama anahtarları ve belirteç verenin talep değerini öğrenmek için indirmek için kullanılır. `/v2.0`Bölüm AAD v1 kullanan uygulamalar için atlanabilir. |
    |İstemci parolası (Isteğe bağlı)| Uygulama kaydında oluşturduğunuz istemci gizli anahtarını kullanın.|
    |İzin verilen belirteç Izleyicileri| Bu bir bulut veya sunucu uygulaması ise ve bir Web uygulamasından kimlik doğrulama belirteçlerine izin vermek istiyorsanız, Web uygulamasının **uygulama KIMLIĞI URI** 'sini buraya ekleyin. Yapılandırılan **ISTEMCI kimliği** *her zaman* örtük olarak izin verilen bir hedef kitle olarak kabul edilir. |

2. **Tamam**' ı ve ardından **Kaydet**' i seçin.

Artık App Service uygulamanızda kimlik doğrulaması için Azure Active Directory kullanmaya hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulaması yapılandırma

**Active Directory Authentication Library**gibi bir istemci kitaplığı kullanarak uygulamanızda BARıNDıRıLAN Web API 'sine kimlik doğrulamasına izin vermek için yerel istemcileri kaydedebilirsiniz.

1. [Azure Portal] **Active Directory**  >  **App registrations**  >  **Yeni kayıt**uygulama kayıtları Active Directory seçin.
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **ortak istemci (mobil & Masaüstü)** öğesini seçin ve URL 'yi yazın `<app-url>/.auth/login/aad/callback` . Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Microsoft Store bir uygulama için, bunun yerine URI olarak [paket SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 'sini kullanın.
1. **Oluştur**'u seçin.
1. Uygulama kaydı oluşturulduktan sonra, **uygulama (istemci) kimliği**değerini kopyalayın.
1. **API izinleri**Seç  >  **Add a permission**  >  **API 'lerim**izin Ekle.
1. Daha önce App Service uygulamanız için oluşturduğunuz uygulama kaydını seçin. Uygulama kaydını görmüyorsanız, [App Service uygulamanız Için Azure AD 'de uygulama kaydı oluşturma](#register)bölümüne **user_impersonation** kapsamını eklediğinizden emin olun.
1. **Temsilci izinleri**altında **user_impersonation**' yi seçin ve ardından **izin Ekle**' yi seçin.

Artık App Service uygulamanıza bir kullanıcı adına erişebilen bir yerel istemci uygulaması yapılandırdınız.

## <a name="configure-a-daemon-client-application-for-service-to-service-calls"></a>Hizmetten hizmete çağrılar için bir Daemon istemci uygulaması yapılandırma

Uygulamanız, App Service veya Işlev uygulamanızda barındırılan bir Web API 'sini kendi adına (Kullanıcı adına değil) çağırmak için bir belirteç alabilir. Bu senaryo, oturum açmış bir kullanıcı olmadan görevleri gerçekleştiren Etkileşimli olmayan Daemon uygulamaları için yararlıdır. Standart OAuth 2,0 [istemci kimlik bilgileri](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md) izni ' nı kullanır.

1. [Azure Portal] **Active Directory**  >  **App registrations**  >  **Yeni kayıt**uygulama kayıtları Active Directory seçin.
1. **Uygulama kaydetme** sayfasında, Daemon uygulama kaydınız Için bir **ad** girin.
1. Bir Daemon uygulaması için bir yeniden yönlendirme URI 'SI gerekmez, bu da boş bırakabilirsiniz.
1. **Oluştur**'u seçin.
1. Uygulama kaydı oluşturulduktan sonra, **uygulama (istemci) kimliği**değerini kopyalayın.
1. **Sertifikalar & gizli**  >  **anahtar Ekle yeni istemci parolası**' nı seçin  >  **Add**. Sayfada gösterilen istemci gizli değerini kopyalayın. Yeniden gösterilmeyecektir.

Artık, parametreyi hedef uygulamanın uygulama KIMLIĞI URI 'sine ayarlayarak [ISTEMCI kimliğini ve istemci gizli anahtarını kullanarak bir erişim belirteci isteyebilirsiniz](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret) `resource` . **Application ID URI** Elde edilen erişim belirteci daha sonra, standart [OAuth 2,0 yetkilendirme üst bilgisi](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#use-the-access-token-to-access-the-secured-resource)kullanılarak hedef uygulamaya sunulabilir ve App Service kimlik doğrulaması/yetkilendirme, Şimdi çağıranın (Bu durumda bir kullanıcı değil) kimliğinin doğrulandığını göstermek için belirteci doğrular ve kullanır.

Bu durumda, Azure AD kiracınızdaki _Tüm_ istemci uygulamalarının erişim belirteci istemesi ve hedef uygulamada kimlik doğrulaması yapmasına izin verir. Ayrıca, yalnızca belirli istemci uygulamalarına izin vermek için _Yetkilendirmeyi_ zorlamak istiyorsanız, bazı ek yapılandırmalar gerçekleştirmeniz gerekir.

1. Korumak istediğiniz App Service veya Işlev uygulamasını temsil eden uygulama kaydının bildiriminde [bir uygulama rolü tanımlayın](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) .
1. Yetkilendirilmiş olması gereken istemciyi temsil eden uygulama kaydında, **API izinleri**  >  **izin Ekle**  >  **API 'leri**seçin.
1. Daha önce oluşturduğunuz uygulama kaydını seçin. Uygulama kaydını görmüyorsanız, [bir uygulama rolü](../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md)eklediğinizden emin olun.
1. **Uygulama izinleri**altında, daha önce oluşturduğunuz uygulama rolünü seçin ve ardından **izin Ekle**' yi seçin.
1. İstemci uygulamasını izin istemek üzere yetkilendirmek için **yönetici Izni ver** ' e tıkladığınızdan emin olun.
1. Önceki senaryoya benzer şekilde (herhangi bir rol eklenmeden önce), artık aynı hedef için [bir erişim belirteci isteyebilirsiniz](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md#first-case-access-token-request-with-a-shared-secret) `resource` ve erişim belirteci, `roles` Istemci uygulaması için yetkilendirilmiş uygulama rollerini içeren bir talep içerir.
1. Hedef App Service veya Işlev uygulama kodu içinde, artık beklenen rollerin belirteçte bulunduğunu doğrulayabilirsiniz (Bu, App Service kimlik doğrulaması/yetkilendirme tarafından gerçekleştirilmemektedir). Daha fazla bilgi için bkz. [Kullanıcı taleplerine erişme](app-service-authentication-how-to.md#access-user-claims).

Artık kendi kimliğini kullanarak App Service uygulamanıza erişebilen bir Daemon istemci uygulaması yapılandırdınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]
* [Öğretici: Azure App Service'te kullanıcıların kimliğini doğrulama ve kullanıcıları uçtan uca yetkilendirme](app-service-web-tutorial-auth-aad.md)
<!-- URLs. -->

[Azure portalındaki]: https://portal.azure.com/
