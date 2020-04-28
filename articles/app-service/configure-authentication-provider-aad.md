---
title: Azure AD kimlik doğrulamasını yapılandırma
description: Azure Active Directory kimlik doğrulamasını App Service veya Azure Işlevleri uygulamanız için bir kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.topic: article
ms.date: 04/14/2020
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: 913aac7755e6c4f9a4b42d45933728fcc8840bfb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82190019"
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
2. Sol gezinmeden**üzerinde** **kimlik doğrulama/yetkilendirme** > ' yi seçin.
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
1. **Yeni kayıt****uygulama kayıtları** >  **Azure Active Directory** > seçin.
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **Web** ' i `<app-url>/.auth/login/aad/callback`seçin ve yazın. Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. **Oluştur**’u seçin.
1. Uygulama kaydı oluşturulduktan sonra, daha sonra için **uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimliğini** kopyalayın.
1. **Kimlik Doğrulaması**'nı seçin. **Örtük izin**' ın altında, App Service **kimlik belirteçlerini** , OpenID kullanıcı oturum açma işlemlerinin izin verecek şekilde etkinleştirin.
1. Seçim **Marka**seçin. **Giriş sayfası URL 'si**' nde App Service uygulamanızın URL 'sini girin ve **Kaydet**' i seçin.
1. **Bir API** > **kümesini**kullanıma sunma ' yı seçin. Tek kiracılı uygulama için App Service uygulamanızın URL 'sini yapıştırın ve **Kaydet** ' i seçin ve çok kiracılı uygulama için, kiracı tarafından doğrulanan etki alanlarından birini temel alan URL 'yi yapıştırın ve ardından **Kaydet**' i seçin.

   > [!NOTE]
   > Bu değer, uygulama kaydının **uygulama kimliği URI 'sidir** . Web uygulamanız buluttaki bir API 'ye erişim gerektiriyorsa, bulut App Service kaynağını yapılandırırken Web uygulamasının **uygulama kimliği URI 'sine** ihtiyacınız vardır. Bunu, örneğin, bulut hizmetinin Web uygulamasına açıkça erişim vermesini istiyorsanız kullanabilirsiniz.

1. **Kapsam ekle**’yi seçin.
   1. **Kapsam adı**alanına *user_impersonation*girin.
   1. Metin kutularına kullanıcıların onay sayfasında görmesini istediğiniz izin kapsam adını ve açıklamasını girin. Örneğin, *uygulamama erişim*' i girin. 
   1. **Kapsam Ekle**' yi seçin.
1. Seçim Bir istemci parolası oluşturmak için **Sertifikalar & gizli** > **anahtar Ekle yeni istemci parolası** > **Add**' nı seçin. Sayfada gösterilen istemci gizli değerini kopyalayın. Yeniden gösterilmeyecektir.
1. Seçim Birden çok **yanıt URL 'si**eklemek Için **kimlik doğrulaması**' nı seçin.

### <a name="enable-azure-active-directory-in-your-app-service-app"></a><a name="secrets"> </a>App Service uygulamanızda Azure Active Directory etkinleştirme

1. [Azure Portal], **uygulama hizmetleri**' ni arayıp seçin ve ardından uygulamanızı seçin. 
1. Sol bölmedeki **Ayarlar**altında **kimlik doğrulama/yetkilendirme** > **' yi seçin.**
1. Seçim Varsayılan olarak, App Service kimlik doğrulaması uygulamanıza yönelik kimliği doğrulanmamış erişime izin verir. Kullanıcı kimlik doğrulamasını zorlamak için, **isteğin kimliği doğrulanmadığı zaman** **Azure Active Directory ile oturum**açmak için yapılacak eylemi ayarlayın.
1. **Kimlik doğrulama sağlayıcıları**altında **Azure Active Directory**' yi seçin.
1. **Yönetim modu**' nda **Gelişmiş** ' i seçin ve App Service kimlik doğrulamasını aşağıdaki tabloya göre yapılandırın:

    |Alan|Açıklama|
    |-|-|
    |İstemci Kimliği| Uygulama kaydının **uygulama (istemci) kimliğini** kullanın. |
    |Veren URL 'Si| ' `<authentication-endpoint>/<tenant-id>/v2.0`İ kullanın ve * \<kimlik doğrulama uç noktası>* , [bulut ortamınız için kimlik doğrulama uç noktası](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints) ile değiştirin (örneğinhttps://login.microsoft.com, "Genel Azure için"), ayrıca * \<Kiracı kimliği>* , uygulama kaydının oluşturulduğu **Dizin (kiracı) kimliğiyle** değiştirin. Bu değer, kullanıcıları doğru Azure AD kiracısına yönlendirmek için ve ilgili meta verileri, uygun belirteç imzalama anahtarları ve belirteç verenin talep değerini öğrenmek için indirmek için kullanılır. `/v2.0` Bölüm AAD v1 kullanan uygulamalar için atlanabilir. |
    |İstemci parolası (Isteğe bağlı)| Uygulama kaydında oluşturduğunuz istemci gizli anahtarını kullanın.|
    |İzin verilen belirteç Izleyicileri| Bu bir bulut veya sunucu uygulaması ise ve bir Web uygulamasından kimlik doğrulama belirteçlerine izin vermek istiyorsanız, Web uygulamasının **uygulama KIMLIĞI URI** 'sini buraya ekleyin. Yapılandırılan **ISTEMCI kimliği** *her zaman* örtük olarak izin verilen bir hedef kitle olarak kabul edilir. |

2. **Tamam**' ı ve ardından **Kaydet**' i seçin.

Artık App Service uygulamanızda kimlik doğrulaması için Azure Active Directory kullanmaya hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulaması yapılandırma

**Active Directory Authentication Library**gibi bir istemci kitaplığı kullanarak uygulamanızda BARıNDıRıLAN Web API 'sine kimlik doğrulamasına izin vermek için yerel istemcileri kaydedebilirsiniz.

1. [Azure Portal]**Yeni kayıt****uygulama kayıtları** >  **Active Directory** > seçin.
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **ortak istemci (mobil & Masaüstü)** öğesini seçin ve URL `<app-url>/.auth/login/aad/callback`'yi yazın. Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.

    > [!NOTE]
    > Microsoft Store bir uygulama için, bunun yerine URI olarak [paket SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 'sini kullanın.
1. **Oluştur**’u seçin.
1. Uygulama kaydı oluşturulduktan sonra, **uygulama (istemci) kimliği**değerini kopyalayın.
1. **API izinleri** > Seç > **API 'lerim****izin Ekle**.
1. Daha önce App Service uygulamanız için oluşturduğunuz uygulama kaydını seçin. Uygulama kaydını görmüyorsanız, [App Service uygulamanız Için Azure AD 'de uygulama kaydı oluşturma](#register)bölümüne **user_impersonation** kapsamını eklediğinizden emin olun.
1. **User_impersonation**' yi seçin ve ardından **izin Ekle**' yi seçin.

Artık App Service uygulamanıza bir kullanıcı adına erişebilen bir yerel istemci uygulaması yapılandırdınız.

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
