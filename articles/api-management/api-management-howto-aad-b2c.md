---
title: Azure Active Directory B2C'yi kullanarak geliştirici hesaplarını yetkilendirme
titleSuffix: Azure API Management
description: API Yönetimi'nde Azure Active Directory B2C'yi kullanarak kullanıcıları nasıl yetkilendirmeyeceğimiz öğrenin.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: b8215cd852d54283bfc6bd47e77d7d63ee4e2582
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475502"
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Azure Active Directory B2C'yi kullanarak Azure API Management'ta geliştirici hesaplarını yetkilendirme

## <a name="overview"></a>Genel Bakış

Azure Active Directory B2C, tüketiciye yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümüdür. Geliştirici portalınıza erişimi yönetmek için kullanabilirsiniz. Bu kılavuz, Azure Active Directory B2C ile tümleştirmek için API Yönetimi hizmetinizde gereken yapılandırmayı gösterir. Klasik Azure Etkin Dizinini kullanarak geliştirici portalına erişimi etkinleştirme hakkında bilgi için Azure [Active Directory'yi kullanarak geliştirici hesaplarını nasıl yetkilendirin']e bakın.

> [!NOTE]
> Bu kılavuzdaki adımları tamamlamak için, bir uygulama oluşturmak için öncelikle bir Azure Etkin Dizin B2C kiracınız olması gerekir. Ayrıca, kayıt ve oturum açma ilkelerinin de hazır olması gerekir. Daha fazla bilgi için Azure [Etkin Dizin B2C genel bakış]bilgisine bakın.

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanarak geliştirici hesaplarını yetkilendirme

1. Başlamak için [Azure portalında](https://portal.azure.com) oturum açın ve API Yönetimi örneğini bulun.

   > [!NOTE]
   > Henüz bir API Yönetimi hizmeti örneği oluşturmadıysanız, [Azure API Yönetimi öğreticisiyle başlat'ta][Get started with Azure API Management] [bir API Yönetimi hizmeti örneği oluşturun'a][Create an API Management service instance] bakın.

1. **Kimlikler**altında. En üstte **+Ekle'yi** tıklatın.

   **Kimlik sağlayıcı ekle** bölmesi sağda görünür. **Azure Etkin Dizini B2C'yi**seçin.
    
   ![AAD B2C'yi kimlik sağlayıcısı olarak ekleme][api-management-howto-add-b2c-identity-provider]

1. Yönlendirme **URL'sini**kopyalayın.

   ![AAD B2C kimlik sağlayıcısı URL'yi yeniden yönlendirin][api-management-howto-copy-b2c-identity-provider-redirect-url]

1. Yeni bir sekmede, Azure portalındaki Azure Active Directory B2C kiracınıza erişin ve **Uygulamalar** bıçaklarını açın.

   ![Yeni bir uygulama kaydolun 1][api-management-howto-aad-b2c-portal-menu]

1. Yeni bir Azure Active Directory B2C uygulaması oluşturmak için **Ekle** düğmesini tıklatın.

   ![Yeni bir uygulama kaydolun 2][api-management-howto-aad-b2c-add-button]

1. Yeni **uygulama** bıçağına, uygulama için bir ad girin. **Web App/Web API**altında **Evet'i** seçin ve **örtülü akışa İzin ver**altında **Evet'i** seçin. Ardından, adım 3'te kopyalanan **Yeniden Yönlendirme URL'sini** **Yanıtla URL** metin kutusuna yapıştırın.

   ![Yeni bir uygulama kaydolun 3][api-management-howto-aad-b2c-app-details]

1. Yeni geliştirici portalını (eski geliştirici portalı değil) kullanıyorsanız, başvuru taleplerine **Verilen Ad,** **Soyad**ve **Kullanıcının Nesne Kimliği'ni** ekleyin.

    ![Uygulama talepleri](./media/api-management-howto-aad-b2c/api-management-application-claims.png)

1. **Oluştur** düğmesine tıklayın. Uygulama oluşturulduğunda, **Uygulamalar** bıtır. Ayrıntılarını görmek için uygulama adını tıklatın.

   ![Yeni bir uygulama kaydolun 4][api-management-howto-aad-b2c-app-created]

1. **Özellikler** bıçağından Uygulama **Kimliğini** panoya kopyalayın.

   ![Başvuru Kimliği 1][api-management-howto-aad-b2c-app-id]

1. API Management'a geri dön **Kimlik sağlayıcı** bölmesini ekleyin ve Kimliği **İstemci Kimliği** metin kutusuna yapıştırın.
    
1.  B2C uygulama kaydına geri dön, **Tuşlar** düğmesini tıklatın ve sonra **Oluştur tuşuna**tıklayın. Yapılandırmayı kaydetmek ve Uygulama **tuşunu**görüntülemek için **Kaydet'i** tıklatın. Pano tuşuna basılı tonu kopyalayın.

    ![Uygulama anahtarı 1][api-management-howto-aad-b2c-app-key]

1.  API Management Add **identity provider** bölmesine geri dön ve anahtarı **İstemci Gizli** metin kutusuna yapıştırın.
    
1.  **Oturum Aç'taki**Azure Etkin Dizin B2C kiracısının etki alanı adını belirtin.

1.  **Kurum** alanı, kullanmak üzere Azure AD B2C giriş URL'sini denetlemenize izin verebiliyor. **>.b2clogin.com your_b2c_tenant_name<** değeri ayarlayın.

1. B2C Kiracı ilkelerinden **Kaydolma İlkesi** ve **Oturum Açma İlkesi'ni** belirtin. İsteğe bağlı olarak, **Profil Düzenleme İlkesi** ve **Parola Sıfırlama İlkesi**de sağlayabilirsiniz.

1. İstediğiniz yapılandırmayı belirttikten sonra **Kaydet'i**tıklatın.

    Değişiklikler kaydedildikten sonra, geliştiriciler Azure Active Directory B2C'yi kullanarak yeni hesaplar oluşturabilir ve geliştirici portalında oturum açabilir.

## <a name="developer-portal---add-azure-ad-b2c-account-authentication"></a>Geliştirici portalı - Azure AD B2C hesap kimlik doğrulaması ekleyin

Geliştirici portalında, AAD B2C ile oturum açma düğmesi ile **oturum açabilirsiniz: OAuth** widget. Widget, varsayılan geliştirici portalı içeriğinin oturum açma sayfasında zaten yer alır.

Yeni bir kullanıcı AAD B2C ile oturum açtırdığında otomatik olarak yeni bir hesap oluşturulacak olsa da, aynı widget'ı kayıt sayfasına eklemeyi düşünebilirsiniz.

**Kayıt formu: OAuth** widget OAuth ile kaydolmak için kullanılan bir formu temsil eder.

> [!IMPORTANT]
> AAD [değişikliklerinin](api-management-howto-developer-portal-customize.md#publish) etkili olması için portalı yeniden yayımlamanız gerekir.

## <a name="legacy-developer-portal---how-to-sign-up-with-azure-ad-b2c"></a>Eski geliştirici portalı - Azure AD B2C'ye nasıl kaydolun

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

1. Azure Active Directory B2C'yi kullanarak bir geliştirici hesabına kaydolmak için yeni bir tarayıcı penceresi açın ve geliştirici portalına gidin. **Kaydol** düğmesini tıklatın.

   ![Geliştirici portalı 1][api-management-howto-aad-b2c-dev-portal]

2. **Azure Active Directory B2C**ile kaydolmayı seçin.

   ![Geliştirici portalı 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Önceki bölümde yapılandırdığınız kayıt ilkesine yönlendirilirsiniz. E-posta adresinizi veya mevcut sosyal hesaplarınızdan birini kullanarak kaydolmayı seçin.

   > [!NOTE]
   > Azure Active Directory B2C, yayımcı portalındaki **Kimlikler** sekmesinde etkinleştirilen tek seçenekse, doğrudan kayıt ilkesine yönlendirilirsiniz.

   ![Geliştirici portalı][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Kayıt tamamlandığında, geliştirici portalına geri yönlendirilirsiniz. Şimdi API Yönetimi hizmet örneğiniz için geliştirici portalında oturum açmış sınız.

    ![Kayıt tamamlandı][api-management-registration-complete]

## <a name="next-steps"></a>Sonraki adımlar

*  [Azure Active Directory B2C'ye genel bakış]
*  [Azure Active Directory B2C: Genişletilebilir ilke çerçevesi]
*  [Azure Active Directory B2C'de kimlik sağlayıcısı olarak microsoft hesabı kullanma]
*  [Azure Active Directory B2C'de bir Google hesabını kimlik sağlayıcısı olarak kullanma]
*  [Azure Active Directory B2C'de LinkedIn hesabını kimlik sağlayıcısı olarak kullanma]
*  [Azure Active Directory B2C'de bir Facebook hesabını kimlik sağlayıcısı olarak kullanma]



[api-management-howto-add-b2c-identity-provider]: ./media/api-management-howto-aad-b2c/api-management-add-b2c-identity-provider.PNG
[api-management-howto-copy-b2c-identity-provider-redirect-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-identity-provider-redirect-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Azure Active Directory B2C'ye genel bakış]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[Azure Active Directory'yi kullanarak geliştirici hesaplarını yetkilendirme]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: Genişletilebilir ilke çerçevesi]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Azure Active Directory B2C'de kimlik sağlayıcısı olarak microsoft hesabı kullanma]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[Azure Active Directory B2C'de bir Google hesabını kimlik sağlayıcısı olarak kullanma]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Azure Active Directory B2C'de bir Facebook hesabını kimlik sağlayıcısı olarak kullanma]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Azure Active Directory B2C'de LinkedIn hesabını kimlik sağlayıcısı olarak kullanma]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
