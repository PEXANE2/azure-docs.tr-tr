---
title: Azure Active Directory-Azure API Management kullanarak Geliştirici hesapları yetkilendir | Microsoft Docs
description: API Management Azure Active Directory kullanarak kullanıcılara yetki verme hakkında bilgi edinin.
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
ms.openlocfilehash: 1def431bd24019c5f7d15cf7ac0e7550b85d17c4
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73176716"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Azure 'da Azure Active Directory kullanarak Geliştirici hesaplarını yetkilendirme API Management

Bu makalede, kullanıcıların Azure Active Directory (Azure AD) aracılığıyla geliştirici portalına erişimi nasıl etkinleştireceğinizi gösterilmektedir. Bu kılavuzda ayrıca, kullanıcıları içeren dış grupları ekleyerek Azure AD kullanıcıları gruplarının nasıl yönetileceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- Azure API Management örneğini içeri aktarın ve yayımlayın. Daha fazla bilgi için bkz. [Içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Azure AD 'yi kullanarak Geliştirici hesaplarını yetkilendirme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın. 
2. Seçin ![oku seçin](./media/api-management-howto-aad/arrow.png).
3. Arama kutusuna **API** yazın.
4. **API Management Hizmetleri**' ni seçin.
5. API Management hizmet örneğinizi seçin.
6. **Güvenlik**altında **kimlikler**' i seçin.
7. Üstten **+ Ekle** ' yi seçin.

    Sağ tarafta **kimlik sağlayıcı ekle** bölmesi görünür.
8. **Sağlayıcı türü**altında **Azure Active Directory**' yi seçin.

    Diğer gerekli bilgileri girmenize olanak sağlayan denetimler bölmesinde görünür. Denetimler **ISTEMCI kimliğini** ve **istemci gizli**anahtarını içerir. (Makalede daha sonra bu denetimler hakkında bilgi alırsınız.)
9. **Yeniden YÖNLENDIRME URL**'sinin içeriğini bir yere göz önünde alın.
    
   ![Azure portal bir kimlik sağlayıcısı ekleme adımları](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. Tarayıcınızda, farklı bir sekme açın. 
11. Bir uygulamayı Active Directory kaydetmek için [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) gidin.
12. **Yönet**altında **uygulama kayıtları**' yi seçin.
13. **Yeni kayıt**seçeneğini belirleyin. **Uygulama kaydetme** sayfasında, değerleri şu şekilde ayarlayın:
    
* **Adı** anlamlı bir ad olarak ayarlayın. Örneğin, *Geliştirici-Portal*
* **Yalnızca bu kuruluş dizinindeki hesaplara** **Desteklenen hesap türlerini** ayarlayın. 
* **Yeniden yönlendirme URI 'sini** adım 9 ' dan aldığınız değere ayarlayın. 
* **Kaydet**' i seçin. 

14.  Uygulama kaydedildikten sonra, **genel bakış** sayfasından **uygulama (istemci) kimliğini** kopyalayın. 
15. API Management örneğinize geri dönün. **Kimlik sağlayıcısı ekle** penceresinde, **uygulama (istemci) KIMLIĞI** değerini **istemci kimliği** kutusuna yapıştırın.
16. Azure AD yapılandırmasına dönün, **Yönet**altında **Sertifikalar & gizlilikler** ' ı seçin. **Yeni istemci parolası** düğmesini seçin. **Açıklama**değerinde bir değer girin, **süre sonu** için herhangi bir seçenek belirleyin ve **Ekle**' yi seçin. Sayfadan çıkmadan önce istemci gizli değerini kopyalayın. Bunlar sonraki adımda gerekecektir. 
17. **Yönet**altında **kimlik doğrulaması** ' nı seçin ve ardından **örtülü izin** altında **Kimlik belirteçleri** ' ni seçin
18. API Management örneğine geri dönün, gizli anahtarı **istemci gizli** kutusuna yapıştırın.

    > [!IMPORTANT]
    > Lütfen anahtarın süresi dolmadan önce **istemci parolasını** güncelleştirdiğinizden emin olun. 
    >  
    >

19. **Kimlik sağlayıcısı ekle** penceresi, **izin verilen kiracılar** metin kutusunu da içerir. Burada, API Management hizmet örneğinin API 'Lerine erişim vermek istediğiniz Azure AD örneklerinin etki alanlarını belirtin. Birden çok etki alanını newlines, boşluk veya virgül ile ayırabilirsiniz.

> [!NOTE]
> **Izin verilen kiracılar** bölümünde birden çok etki alanı belirtebilirsiniz. Herhangi bir Kullanıcı, uygulamanın kaydedildiği orijinal etki alanından farklı bir etki alanından oturum açmadan önce, farklı bir etki alanının genel yöneticisinin uygulamanın dizin verilerine erişmesi için izin vermesi gerekir. İzin vermek için genel yönetici: a. `https://<URL of your developer portal>/aadadminconsent` gidin (örneğin, https://contoso.portal.azure-api.net/aadadminconsent).
> b. Erişim vermek istedikleri Azure AD kiracının etki alanı adını yazın.
> c. **Gönder**' i seçin. 

20.  İstenen yapılandırmayı belirttikten sonra **Ekle**' yi seçin.

Değişiklikler kaydedildikten sonra, belirtilen Azure AD örneğindeki kullanıcılar, [bir Azure AD hesabı kullanarak Geliştirici Portalında oturum açma](#log_in_to_dev_portal)adımlarını izleyerek Geliştirici Portalında oturum açabilir.

## <a name="add-an-external-azure-ad-group"></a>Dış Azure AD grubu ekleme

Bir Azure AD örneğindeki kullanıcılar için erişimi etkinleştirdikten sonra API Management Azure AD grupları ekleyebilirsiniz. Daha sonra, gruptaki geliştiricilerin, istenen ürünlerle ilişkilendirmesini daha kolay bir şekilde yönetebilirsiniz.

 > [!IMPORTANT]
 > Dış bir Azure AD grubu eklemek için, önceki bölümde yer alan yordamı izleyerek önce **kimlikler** SEKMESINDE Azure AD örneğini yapılandırmanız gerekir. Ayrıca, uygulamaya `Directory.Read.All` izinle Azure AD Graph API erişimi verilmelidir. 

API Management örneğinizin **gruplar** sekmesinden dış Azure AD grupları eklersiniz.

1. **Groups (Gruplar)** sekmesini seçin.
2. **AAD grubu Ekle** düğmesini seçin.
   "AAD grubu Ekle" düğmesi ![](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Eklemek istediğiniz grubu seçin.
4. **Seç** düğmesine basın.

Dış bir Azure AD grubu ekledikten sonra özelliklerini gözden geçirebilir ve yapılandırabilirsiniz. **Gruplar** sekmesinden grubun adını seçin. Buradan, grup için **ad** ve **Açıklama** bilgilerini düzenleyebilirsiniz.
 
Yapılandırılmış Azure AD örneğindeki kullanıcılar artık Geliştirici Portalında oturum açabilir. Görünürlük sahibi oldukları grupları görüntüleyebilir ve bunlara abone olabilirler.

## <a name="a-idlog_in_to_dev_portal-developer-portal---add-azure-ad-account-authentication"></a><a id="log_in_to_dev_portal"/> geliştirici portalı-Azure AD hesabı kimlik doğrulaması ekleme

Geliştirici Portalında AAD ile oturum açmayı etkinleştirmek için, oturum açma formuna **OAuth düğmeleri** pencere öğesini eklemeniz gerekir.

![AAD düğmeleri pencere öğesi](./media/api-management-howto-aad/portal-oauth-widget.png)

Yeni bir Kullanıcı AAD ile oturum açtığında yeni bir hesap otomatik olarak oluşturulabilse de, kaydolma formuna aynı pencere öğesini eklemeyi göz önünde bulundurmanız gerekebilir.

## <a name="legacy-developer-portal---how-to-sign-in-with-azure-ad"></a>Eski geliştirici portalı-Azure AD ile oturum açma

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

Önceki bölümlerde yapılandırdığınız bir Azure AD hesabını kullanarak Geliştirici Portalında oturum açmak için:

1. Active Directory Uygulama yapılandırmasından oturum açma URL 'sini kullanarak yeni bir tarayıcı penceresi açın ve **Azure Active Directory**' ı seçin.

   ![Oturum açma sayfası][api-management-dev-portal-signin]

1. Azure AD 'deki kullanıcılardan birinin kimlik bilgilerini girin ve **oturum aç**' ı seçin.

   ![Kullanıcı adı ve parolayla oturum açma][api-management-aad-signin]

1. Herhangi bir ek bilgi gerekliyse bir kayıt formu istenebilir. Kayıt formunu doldurun ve **Kaydol**' u seçin.

   ![Kayıt formunda "kaydolun" düğmesi][api-management-complete-registration]

Kullanıcılarınız artık API Management hizmet örneğiniz için Geliştirici Portalında oturum açtı.

![Kayıt tamamlandıktan sonra geliştirici portalı][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

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
[Accessing the Graph API]: https://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
