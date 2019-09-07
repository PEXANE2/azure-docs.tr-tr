---
title: Azure Active Directory kimlik doğrulamasını Yapılandırma-Azure App Service
description: App Service uygulamanız için Azure Active Directory kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
author: cephalin
services: app-service
documentationcenter: ''
manager: gwallace
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service
ms.workload: web,mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/03/2019
ms.author: cephalin
ms.custom: fasttrack-edit
ms.openlocfilehash: 8de464a00867dd397f28de1dc35cf264244f6905
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70743251"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>App Service uygulamanızı Azure Active Directory oturum açma kullanacak şekilde yapılandırma

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

> [!NOTE]
> Şu anda AAD v2 (MSAL dahil) Azure App Service ve Azure Işlevleri için desteklenmez.
>

Bu makalede, bir kimlik doğrulama sağlayıcısı olarak Azure Active Directory kullanmak üzere Azure App Service nasıl yapılandırılacağı gösterilmektedir.

Her bir App Service uygulamasını kendi kaydıyla yapılandırmanız önerilir, bu nedenle kendi izinleri ve onayı vardır. Ayrıca ayrı dağıtım yuvaları için ayrı uygulama kayıtları kullanmayı göz önünde bulundurun. Bu, ortamlar arasındaki izin paylaşımını önler, böylece test ettiğiniz yeni koddaki bir sorun üretimi etkilemez.

## <a name="express"> </a>Hızlı ayarlarla Yapılandırma

1. [Azure Portal], App Service uygulamanıza gidin. Sol gezinti bölmesinde **kimlik doğrulama/yetkilendirme**' yi seçin.
2. **Kimlik doğrulama/yetkilendirme** etkin değilse **Açık**' ı seçin.
3. **Azure Active Directory**' yi seçin ve ardından **yönetim modu**altında **Express** ' i seçin.
4. App Service uygulamasını Azure Active Directory kaydettirmek için **Tamam ' ı** seçin. Bu, yeni bir uygulama kaydı oluşturur. Bunun yerine var olan bir uygulama kaydını seçmek istiyorsanız, **var olan bir uygulamayı Seç** ' e tıklayın ve ardından kiracınızda daha önce oluşturulmuş bir uygulama kaydının adını arayın. Uygulama kaydına tıklayarak seçin ve **Tamam**' a tıklayın. Ardından Azure Active Directory ayarları sayfasında **Tamam** ' a tıklayın.
Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.
5. Seçim Uygulamanıza erişimi yalnızca Azure Active Directory tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, **isteğin kimliği doğrulanmadığı zaman gerçekleştirilecek eylemi** **Azure Active Directory ile oturum**açmak için ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Azure Active Directory yönlendirilir.

    > [!NOTE]
    > Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.
6. **Kaydet**’e tıklayın.

## <a name="advanced"> </a>Gelişmiş ayarlarla Yapılandırma

Ayrıca, kullanmak istediğiniz Azure Active Directory kiracı, Azure 'da oturum açmak istediğiniz kiracıdan farklıysa, yapılandırma ayarlarını el ile de sağlayabilirsiniz. Yapılandırmayı gerçekleştirmek için öncelikle Azure Active Directory ' de bir kayıt oluşturmanız ve ardından App Service için bazı kayıt ayrıntılarını sağlamanız gerekir.

### <a name="register"> </a>App Service uygulamanız IÇIN Azure AD 'de bir uygulama kaydı oluşturma

El ile uygulama kaydı oluştururken, App Service uygulamanızı yapılandırırken daha sonra ihtiyacınız olacak üç bilgi parçasına göz önünde bulabilirsiniz: istemci KIMLIĞI, kiracı KIMLIĞI ve isteğe bağlı olarak, istemci gizli anahtarı ve uygulama KIMLIĞI URI 'SI.

1. [Azure Portal], App Service uygulamanıza gidin ve uygulamanızın **URL 'sini**aklınızda edin. Bunu, Azure Active Directory Uygulama kaydınızı yapılandırmak için kullanacaksınız.
1. [Azure Portal], sol menüden**Yeni kayıt** **uygulama kayıtları** >  **Active Directory** > ' i seçin. 
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **Web** ' i seçin ve App Service uygulamanızın URL 'sini yazın ve yolu `/.auth/login/aad/callback`ekleyin. Örneğin: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Ardından **Oluştur**’u seçin.
1. Uygulama kaydı oluşturulduktan sonra, daha sonra için **uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimliğini** kopyalayın.
1. **Marka**seçin. **Giriş sayfası URL 'si**' nde App Service uygulamanızın URL 'sini yazın ve **Kaydet**' i seçin.
1. **Bir API** > **kümesini**kullanıma sunma ' yı seçin. App Service uygulamanızın URL 'sini yapıştırın ve **Kaydet**' i seçin.

    > [!NOTE]
    > Bu değer, uygulama kaydının **uygulama kimliği URI 'sidir** . Örneğin, bir arka uç API 'sine erişmek için bir ön uç Web uygulamasına sahip olmak istiyorsanız ve arka ucun ön uca açıkça erişim vermesini istiyorsanız, < c2 'nin App Service uygulama kaynağını yapılandırırken *ön ucun* **uygulama kimliği URI 'sine** ihtiyacınız vardır. > arka ucu.
1. **Kapsam ekle**’yi seçin. **Kapsam adı**alanına *user_impersonation*yazın. Metin kutularında, kullanıcıların izin sayfasında görmesini istediğiniz izin kapsam adını ve açıklamasını yazın; örneğin *uygulamama erişin*. İşiniz bittiğinde **Kapsam Ekle**' ye tıklayın.
1. Seçim Bir istemci parolası oluşturmak için **Sertifikalar & gizli** > anahtar**Ekle** **yeni istemci parolası** > ' nı seçin. Sayfada gösterilen istemci gizli değerini kopyalayın. Ayrıldığınızda, tekrar gösterilmeyecektir.
1. Seçim Birden çok **yanıt URL 'si**eklemek için, menüden **kimlik doğrulaması** ' nı seçin.

### <a name="secrets"> </a>App Service uygulamanıza Azure Active Directory bilgileri ekleyin

1. [Azure Portal], App Service uygulamanıza gidin. Sol menüden **kimlik doğrulama/yetkilendirme**' yi seçin. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse **Açık**' ı seçin. 
1. Seçim Varsayılan olarak, App Service kimlik doğrulaması uygulamanıza yönelik kimliği doğrulanmamış erişime izin verir. Kullanıcı kimlik doğrulamasını zorlamak için, **isteğin kimliği doğrulanmadığı zaman** **Azure Active Directory ile oturum**açmak için yapılacak eylemi ayarlayın.
1. Kimlik doğrulama sağlayıcıları altında **Azure Active Directory**' yi seçin.
1. **Yönetim modu**' nda **Gelişmiş** ' i seçin ve App Service kimlik doğrulamasını aşağıdaki tabloya göre yapılandırın:

    |Alan|Açıklama|
    |-|-|
    |İstemci Kimliği| Uygulama kaydının **uygulama (istemci) kimliğini** kullanın. |
    |Verenin KIMLIĞI| Kullanın `https://login.microsoftonline.com/<tenant-id>`ve  *\<Kiracı kimliği >* , uygulama kaydının **Dizin (kiracı) kimliğiyle** değiştirin. |
    |İstemci parolası (Isteğe bağlı)| Uygulama kaydında oluşturduğunuz istemci gizli anahtarını kullanın.|
    |İzin Verilen Belirteç Hedef Kitleleri| Bu bir *arka uç* uygulaması ise ve ön uç uygulamadan kimlik doğrulama belirteçlerine izin vermek istiyorsanız, *ön ucun* **uygulama kimliği URI** 'sini buraya ekleyin. |

    > [!NOTE]
    > **İzin verilen belirteç kitlelerini**nasıl yapılandırdığınıza bakmaksızın, YAPıLANDıRıLAN **istemci kimliği** *her zaman* , izin verilen bir hedef kitle olarak kabul edilir.
1. **Tamam**' ı ve ardından **Kaydet**' i seçin.

Artık App Service uygulamanızda kimlik doğrulaması için Azure Active Directory kullanmaya hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulaması yapılandırma
**Active Directory Authentication Library**gibi bir istemci kitaplığı kullanarak oturum açma işlemleri gerçekleştirmek istiyorsanız yerel istemcileri kaydedebilirsiniz.

1. [Azure Portal], sol menüden**Yeni kayıt** **uygulama kayıtları** >  **Active Directory** > ' i seçin. 
1. **Uygulama kaydetme** sayfasında, uygulama kaydınız Için bir **ad** girin.
1. **Yeniden yönlendirme URI 'si**içinde **ortak istemci (mobil & Masaüstü)** öğesini seçin ve App Service uygulamanızın URL 'sini yazın ve yolu `/.auth/login/aad/callback`ekleyin. Örneğin: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Ardından **Oluştur**’u seçin.

    > [!NOTE]
    > Bir Windows uygulaması için, bunun yerine URI olarak [paket SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 'sini kullanın.
1. Uygulama kaydı oluşturulduktan sonra, **uygulama (istemci) kimliği**değerini kopyalayın.
1. Sol taraftaki menüden **API izinleri** > **izin** > Ekle**API 'lerim**' i seçin.
1. Daha önce App Service uygulamanız için oluşturduğunuz uygulama kaydını seçin. Uygulama kaydını görmüyorsanız, [App Service uygulamanız Için Azure AD 'de uygulama kaydı oluşturma](#register)bölümüne **user_impersonation** kapsamını eklemişseniz emin olun.
1. **User_impersonation** öğesini seçin ve **izin Ekle**' ye tıklayın.

Artık App Service uygulamanıza erişebilen bir yerel istemci uygulaması yapılandırdınız.

## <a name="related-content"> </a>İlgili içerik

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[alternative method]:#advanced
