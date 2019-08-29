---
title: Azure Active Directory kimlik doğrulamasını Yapılandırma-Azure App Service
description: Uygulama Hizmetleri uygulamanız için Azure Active Directory kimlik doğrulamasını yapılandırma hakkında bilgi edinin.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/20/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 41084c3532e29b3a52c121d48226c5a45857d5dc
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70088243"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>App Service uygulamanızı Azure Active Directory oturum açma kullanacak şekilde yapılandırma

> [!NOTE]
> Şu anda, AAD v2 (MSAL dahil) Azure uygulama hizmetleri ve Azure Işlevleri için desteklenmez. Lütfen güncelleştirmeler için yeniden denetleyin.
>

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure App Service 'in kimlik doğrulama sağlayıcısı olarak Azure Active Directory kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

## <a name="express"> </a>Hızlı ayarlarla Yapılandırma

1. [Azure Portal], App Service uygulamanıza gidin. Sol gezinti bölmesinde **kimlik doğrulama/yetkilendirme**' yi seçin.
2. **Kimlik doğrulama/yetkilendirme** etkin değilse **Açık**' ı seçin.
3. **Azure Active Directory**' yi seçin ve ardından **yönetim modu**altında **Express** ' i seçin.
4. App Service uygulamasını Azure Active Directory kaydettirmek için **Tamam ' ı** seçin. Bu, yeni bir uygulama kaydı oluşturur. Bunun yerine var olan bir uygulama kaydını seçmek istiyorsanız, **var olan bir uygulamayı Seç** ' e tıklayın ve ardından kiracınızda daha önce oluşturulmuş bir uygulama kaydının adını arayın.
   Uygulama kaydına tıklayarak seçin ve **Tamam**' a tıklayın. Ardından Azure Active Directory ayarları sayfasında **Tamam** ' a tıklayın.
   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.
5. Seçim Sitenize erişimi yalnızca Azure Active Directory tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, **isteğin kimliği doğrulanmadığı zaman gerçekleştirilecek eylemi** **Azure Active Directory ile oturum**açmak için ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Azure Active Directory yönlendirilir.

> [!CAUTION]
> Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.

6. **Kaydet**’e tıklayın.

## <a name="advanced"> </a>Gelişmiş ayarlarla Yapılandırma

Yapılandırma ayarlarını el ile de sağlayabilirsiniz. Kullanmak istediğiniz Azure Active Directory kiracı, Azure 'da oturum açarken kullandığınız kiracıdan farklıysa, bu tercih edilen çözümdür. Yapılandırmayı gerçekleştirmek için öncelikle Azure Active Directory ' de bir kayıt oluşturmanız ve ardından App Service için bazı kayıt ayrıntılarını sağlamanız gerekir.

### <a name="register"> </a>App Service uygulamanızı Azure Active Directory kaydettirin

1. [Azure Portal]oturum açın ve App Service uygulamanıza gidin. Uygulama **URL**'nizi kopyalayın. Bunu, Azure Active Directory Uygulama kaydınızı yapılandırmak için kullanacaksınız.
2. **Active Directory**gidin, ardından **uygulama kayıtları**seçin ve ardından yeni **uygulama kaydı** ' na tıklayarak yeni bir uygulama kaydı başlatın. 
3. **Oluştur** sayfasında, uygulama kaydınız Için bir **ad** girin, **Web uygulaması/API** türünü seçin, **oturum açma URL 'si** kutusunda Uygulama URL 'sini yapıştırın (1. adım). Ardından **oluşturmak**için tıklayın.
4. Birkaç saniye içinde, az önce oluşturduğunuz yeni uygulama kaydını görmeniz gerekir.
5. Uygulama kaydı eklendikten sonra, uygulama kayıt adı ' na tıklayın, üstteki **Ayarlar** ' a ve ardından **Özellikler** ' e tıklayın. 
6. Uygulama **KIMLIĞI URI** 'si kutusunda, uygulama URL 'sini (1. adımdan), **giriş sayfası URL 'sindeki** uygulama URL 'sini de (1. adım) yapıştırın ve **Kaydet** ' e tıklayın.
7. Şimdi **yanıt URL 'lerine**tıklayın, **yanıt URL**'SINI düzenleyin, uygulama URL 'sini (1. ADıMDAN) yapıştırın ve URL 'nin sonuna ekleyin (örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`). **Kaydet**’e tıklayın.

   > [!NOTE]
   > Ek **yanıt URL 'leri**ekleyerek birden çok etki alanı için aynı uygulama kaydını kullanabilirsiniz. Her bir App Service örneğini kendi kaydıyla modellediğinizden emin olun, bu nedenle kendi izinleri ve onayı vardır. Ayrıca ayrı site yuvaları için ayrı uygulama kayıtları kullanmayı göz önünde bulundurun. Bu, test etmekte olduğunuz yeni koddaki bir hata üretimi etkilemediğinden, ortamlar arasında paylaşılmasını önlemek için gereklidir.
    
8. Bu noktada, uygulamanın **uygulama kimliğini** kopyalayın. Daha sonra kullanmak üzere saklayın. App Service uygulamanızı yapılandırmak için buna ihtiyacınız olacak.
9. **Kayıtlı uygulama** sayfasını kapatın. **Uygulama kayıtları** sayfasında, üstteki **uç noktalar** düğmesine tıklayın ve ardından **WS-FEDERATION oturum açma uç noktası** URL 'sini kopyalayın, ancak URL 'den `/wsfed` sona erdirme seçeneğini kaldırın. Nihai sonuç şöyle `https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000`görünmelidir. Etki alanı adı, bir bağımsız bulutu için farklı olabilir. Bu, daha sonra veren URL 'SI olarak görev yapar.

### <a name="secrets"> </a>App Service uygulamanıza Azure Active Directory bilgileri ekleyin

1. [Azure Portal]geri dönüp App Service uygulamanıza gidin. **Kimlik doğrulama/yetkilendirme**' ye tıklayın. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse, geçişi **Açık**duruma getirin. Uygulamanızı yapılandırmak için kimlik doğrulama sağlayıcıları altında **Azure Active Directory**' ye tıklayın.

    Seçim Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz. **Azure Active Directory Ile oturum**açmak için **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylemi** ayarlayın. Bu seçenek tüm isteklerin doğrulanmasını gerektirir ve kimlik doğrulama için tüm kimliği doğrulanmamış istekler Azure Active Directory yeniden yönlendirilir.
2. Active Directory kimlik doğrulama yapılandırmasında, **yönetim modu**altında **Gelişmiş** ' e tıklayın. Uygulama KIMLIĞINI Istemci KIMLIĞI kutusuna yapıştırın (8. adım) ve URL 'yi (adım 9 ' da) veren URL değerine yapıştırın. Daha sonra, **Tamam**'a tıklayın.
3. Active Directory kimlik doğrulama yapılandırması sayfasında **Kaydet**' e tıklayın.

Artık App Service uygulamanızda kimlik doğrulaması için Azure Active Directory kullanmaya hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulaması yapılandırma
İzin eşleme üzerinde daha fazla denetim sağlayan yerel istemcileri kaydedebilirsiniz. **Active Directory Authentication Library**gibi bir istemci kitaplığı kullanarak oturum açma işlemleri gerçekleştirmek istiyorsanız buna ihtiyacınız vardır.

1. [Azure portal] **Azure Active Directory** gidin.
2. Sol gezinti bölmesinde **uygulama kayıtları**' yi seçin. En üstteki **Yeni uygulama kaydı** ' na tıklayın.
4. **Oluştur** sayfasında, uygulama kaydınız Için bir **ad** girin. **Uygulama türü**' nde **Yerel** ' i seçin.
5. **Yeniden yönlendirme URI 'si** kutusunda, https şemasını kullanarak sitenizin */.Auth/login/done* uç noktasını girin. Bu değer değerine benzer *https://contoso.azurewebsites.net/.auth/login/done* olmalıdır. Bir Windows uygulaması oluşturuyorsanız bunun yerine URI olarak [paket SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 'sini kullanın.
5. **Oluştur**'a tıklayın.
6. Uygulama kaydı eklendikten sonra açmak için seçin. **Uygulama kimliğini** bulun ve bu değeri bir yere unutmayın.
7. **Tüm ayarlar** > **gerekli izinler** > Addbir > **API Seç**' e tıklayın.
8. Daha önce aramak için kaydettiğiniz App Service uygulamasının adını yazın, sonra seçin ve **Seç**' e tıklayın.
9. **Erişim\<app_name >** seçin. Ardından **Seç**'e tıklayın. Sonra da **Bitti**’ye tıklayın.

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
