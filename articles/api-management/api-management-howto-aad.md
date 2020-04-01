---
title: Azure Etkin Dizini'ni kullanarak geliştirici hesaplarını yetkilendirme
titleSuffix: Azure API Management
description: API Yönetimi'nde Azure Active Directory'yi kullanarak kullanıcıları nasıl yetkilendirmeyeceğimiz öğrenin.
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
ms.openlocfilehash: 6102b1e1d6ddbac01033b9cecfeba96a7eb33777
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473549"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Azure API Management'ta geliştirici hesaplarını yetkilendirmek için Azure Active Directory kullanın

Bu makalede, Azure Etkin Dizini (Azure AD) kullanıcıları için geliştirici portalına erişimi nasıl etkinleştirdiğinizgösterilmektedir. Bu kılavuz, kullanıcıları içeren dış gruplar ekleyerek Azure AD kullanıcı gruplarını nasıl yöneteceklerini de gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- Azure API Yönetimi örneğini içe aktarın ve yayımlayın. Daha fazla bilgi için [Içe Aktar ve yayımla'](import-and-publish.md)ya bakın.

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Azure AD'yi kullanarak geliştirici hesaplarını yetkilendirme

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
3. Arama **kutusuna api** yazın.
4. **API Yönetimi hizmetlerini**seçin.
5. API Management hizmet örneğinizi seçin.
6. **Güvenlik**altında, **Kimlikler'i**seçin.
7. Üstten **+Ekle'yi** seçin.

    **Kimlik sağlayıcı ekle** bölmesi sağda görünür.
8. **Sağlayıcı türü altında,** **Azure Etkin Dizini'ni**seçin.

    Diğer gerekli bilgileri girmenizi sağlayan denetimler bölmede görünür. **Denetimler, İstemci Kimliği** ve **İstemci sırrını**içerir. (Bu denetimler hakkında daha sonra makalede bilgi almak.)
9. Yönlendirme URL'sinin içeriğine dikkat **edin.**
    
   ![Azure portalına kimlik sağlayıcı ekleme adımları](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. Tarayıcınızda farklı bir sekme açın. 
11. Bir uygulamayı Active Directory'ye kaydetmek için [Azure portalı - Uygulama kayıtlarına](https://go.microsoft.com/fwlink/?linkid=2083908) gidin.
12. **Yönet**'in altında **Uygulama kayıtları**nı seçin.
13. **Yeni kayıt**seçin. Bir uygulama sayfası **kaydol,değerleri** aşağıdaki gibi ayarlayın:
    
    * **Adı** anlamlı bir ada ayarlayın. örneğin, geliştirici *portalı*
    * **Desteklenen hesap türlerini** **yalnızca bu kuruluş dizininde Hesaplara**ayarlayın. 
    * **Uri'yi** adım 9'dan aldığınız değere yönlendirmeyi ayarlayın. 
    * **Kaydol'u**seçin. 

14.  Uygulama kaydedildikten sonra, **Genel Bakış** sayfasından **Uygulama (istemci) kimliğini** kopyalayın. 
15. API Yönetimi örneğinize geri dön. Kimlik **sağlayıcı ekle** penceresinde, **Uygulama (istemci) kimlik** değerini **İstemci Kimliği** kutusuna yapıştırın.
16. Azure AD yapılandırmasına geri dön, **Yönet**altında **Sertifika & sırları** seçin. Yeni **istemci gizli** düğmesini seçin. **Açıklama'ya**bir değer girin, **Sona Erer** için herhangi bir seçeneği seçin ve **Ekle'yi**seçin. Sayfadan çıkmadan önce istemcinin gizli değerini kopyalayın. Bu sonraki adımda gerekecektir. 
17. **Yönet**altında, **Kimlik Doğrulama'yı** seçin ve ardından **Örtülü Hibe** altında **kimlik belirteçlerini** seçin
18. API Yönetimi örneğinize geri dön, sırrı **Istemci gizli** kutusuna yapıştır.

    > [!IMPORTANT]
    > Lütfen anahtarın süresi dolmadan önce **İstemci sırrını** güncelleştirdiğinden emin olun. 
    >  
    >

19. **Kimlik ekle sağlayıcısı** penceresi, İzin Verilen **Kiracılar** metin kutusunu da içerir. Burada, API Yönetimi hizmet örneğinin API'lerine erişim izni vermek istediğiniz Azure AD örneklerinin etki alanlarını belirtin. Birden çok etki alanını yeni satırlarla, boşluklarla veya virgüllerle ayırabilirsiniz.

    > [!NOTE]
    > **İzin Verilen Kiracılar** bölümünde birden çok etki alanı belirtebilirsiniz. Herhangi bir kullanıcı, uygulamanın kaydedildiği orijinal etki alanından farklı bir etki alanında oturum açabilmesi için, farklı etki alanının genel yöneticisinin uygulamanın dizin verilerine erişmesi için izin vermesi gerekir. İzin vermek için, genel yönetici şunları yapmalı: a. Gidin `https://<URL of your developer portal>/aadadminconsent` (örneğin, https://contoso.portal.azure-api.net/aadadminconsent).
    > b. Erişmek istedikleri Azure AD kiracısının etki alanı adını yazın.
    > c. **Gönder**’i seçin. 

20.  İstenilen yapılandırmayı belirttikten sonra **Ekle'yi**seçin.

Değişiklikler kaydedildikten sonra, belirtilen Azure AD örneğindeki [kullanıcılar, bir Azure AD hesabı kullanarak geliştirici portalında Oturum Aç'taki](#log_in_to_dev_portal)adımları izleyerek geliştirici portalında oturum açabilir.

## <a name="add-an-external-azure-ad-group"></a>Harici Bir Azure REKLAM grubu ekleme

Azure AD kiracısındaki kullanıcılara erişimi etkinleştirdikten sonra, API Yönetimi'ne Azure REKLAM grupları ekleyebilirsiniz. Sonuç olarak, Azure REKLAM gruplarını kullanarak ürün görünürlüğünü denetleyebilirsiniz.

APIM'e harici bir Azure REKLAM grubu eklemek için önce önceki bölümü tamamlamanız gerekir. Ayrıca, kaydettiğiniz uygulamaya aşağıdaki adımları izleyerek izin alınarak `Directory.Read.All` Microsoft Graph API'ye erişim izni verilmelidir: 

1. Önceki bölümde oluşturulan Uygulama Kaydınıza geri dön.
2. **API İzinleri'ni**seçin ve ardından **+İzin Ekle'yi**tıklatın. 
3. İstek **API İzinleri** bölmesinde, **Microsoft API'leri** sekmesini seçin ve ardından **Microsoft Graph** döşemesini seçin. **Uygulama izinlerini**seçin, **Dizin'i**arayın ve ardından **Dizin.Read.All** iznini seçin. 
4. Bölmenin altındaki **İzinler Ekle'yi** tıklatın ve sonra bu dizindeki tüm kullanıcılara erişim izni vermek **için {kiracı adı} için Yönetici Onayı Ver'i** tıklatın. 

Artık API Yönetimi örneğinizin **Gruplar** sekmesinden harici Azure REKLAM grupları ekleyebilirsiniz.

1. **Gruplar** sekmesini seçin.
2. **AAD grup ekle** düğmesini seçin.
    !["AAD grubu ekle" düğmesi](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Eklemek istediğiniz grubu seçin.
4. **Seç** düğmesine basın.

Harici bir Azure REKLAM grubu ekledikten sonra, özelliklerini gözden geçirebilir ve yapılandırabilirsiniz. **Gruplar** sekmesinden grubun adını seçin. Buradan, grubun **Ad** ve **Açıklama** bilgilerini atabilirsiniz.
 
Yapılandırılan Azure AD örneğindeki kullanıcılar artık geliştirici portalında oturum açabilir. Görünürlük leri olan grupları görüntüleyebilir ve bunlara abone olabilirler.

## <a name="developer-portal---add-azure-ad-account-authentication"></a><a id="log_in_to_dev_portal"/>Geliştirici portalı - Azure AD hesap kimlik doğrulaması ekleme

Geliştirici portalında, AAD ile oturum açma düğmesi ile **oturum açabilirsiniz: OAuth** widget. Widget, varsayılan geliştirici portalı içeriğinin oturum açma sayfasında zaten yer alır.

Yeni bir kullanıcı AAD ile oturum açtırdığında otomatik olarak yeni bir hesap oluşturulacak olsa da, aynı widget'ı kayıt sayfasına eklemeyi düşünebilirsiniz.

**Kayıt formu: OAuth** widget OAuth ile kaydolmak için kullanılan bir formu temsil eder.

> [!IMPORTANT]
> AAD [değişikliklerinin](api-management-howto-developer-portal-customize.md#publish) etkili olması için portalı yeniden yayımlamanız gerekir.

## <a name="legacy-developer-portal---how-to-sign-in-with-azure-ad"></a>Eski geliştirici portalı - Azure AD ile nasıl oturum açar

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

Önceki bölümlerde yapılandırdığınız bir Azure REKLAM hesabı kullanarak geliştirici portalında oturum açabilirsiniz:

1. Active Directory uygulama yapılandırmasındaki oturum açma URL'sini kullanarak yeni bir tarayıcı penceresi açın ve **Azure Etkin Dizini'ni**seçin.

   ![Oturum açma sayfası][api-management-dev-portal-signin]

1. Azure AD'deki kullanıcılardan birinin kimlik bilgilerini girin ve **Oturum Aç'ı**seçin.

   ![Kullanıcı adı ve parola ile oturum açma][api-management-aad-signin]

1. Herhangi bir ek bilgi gerekirse bir kayıt formu ile isteniziistilen olabilir. Kayıt formunu doldurun ve **Kaydolun'u**seçin.

   ![Kayıt formunda "Kaydol" düğmesi][api-management-complete-registration]

Kullanıcınız artık API Yönetimi hizmet örneğiniz için geliştirici portalında oturum açmış durumda.

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

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
