---
title: API Yönetimi'nde OAuth 2.0 kullanarak geliştirici hesaplarını yetkilendirme
titleSuffix: Azure API Management
description: API Yönetimi'nde OAuth 2.0'ı kullanan kullanıcıları nasıl yetkilendirmeyeceğimiz hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 90c890925378c30ce5688d2713990b4b2cdd20c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430685"
---
# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Azure API Yönetimi'nde OAuth 2.0 kullanarak geliştirici hesaplarını yetkilendirme

Birçok API, API'yi güvence altına almak ve yalnızca geçerli kullanıcıların erişebilmesini ve yalnızca hak kazandıkları kaynaklara erişebilmelerini sağlamak için [OAuth 2.0'ı](https://oauth.net/2/) destekler. Azure API Yönetimi'nin etkileşimli Geliştirici Konsolunu bu TÜR API'lerle kullanmak için, hizmet örneğinizi OAuth 2.0 etkin API'nizle çalışacak şekilde yapılandırmanıza olanak tanır.

> [!IMPORTANT]
> OAuth 2.0 yetkilendirmesi henüz yeni geliştirici portalının etkileşimli konsolunda mevcut değildir.

## <a name="prerequisites"></a><a name="prerequisites"> </a>Ön koşullar

Bu kılavuz, geliştirici hesapları için OAuth 2.0 yetkilendirmesini kullanacak API Yönetimi hizmet örneğini nasıl yapılandırdığınızı gösterir, ancak bir OAuth 2.0 sağlayıcısını nasıl yapılandırabileceğinizi göstermez. Adımlar benzer olmasına rağmen her OAuth 2.0 sağlayıcısı için yapılandırma farklıdır ve API Yönetimi hizmet örneğinde OAuth 2.0'ı yapılandırmada kullanılan gerekli bilgi parçaları aynıdır. Bu konu, OAuth 2.0 sağlayıcısı olarak Azure Active Directory kullanarak örnekleri gösterir.

> [!NOTE]
> Azure Active Directory kullanarak OAuth 2.0'ı yapılandırma hakkında daha fazla bilgi için [WebApp-GraphAPI-DotNet][WebApp-GraphAPI-DotNet] örneğine bakın.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="configure-an-oauth-20-authorization-server-in-api-management"></a><a name="step1"> </a>API Yönetimi'nde OAuth 2.0 yetkilendirme sunucusu yapılandırma

> [!NOTE]
> Henüz bir API Yönetimi hizmeti örneği oluşturmadıysanız, [bkz.][Create an API Management service instance]

1. Soldaki menüdeki OAuth 2.0 sekmesine tıklayın ve **+Ekle'ye**tıklayın.

    ![OAuth 2.0 menüsü](./media/api-management-howto-oauth2/oauth-01.png)

2. **Ad** ve **Açıklama** alanlarına bir ad ve isteğe bağlı açıklama girin.

    > [!NOTE]
    > Bu alanlar, geçerli API Yönetimi hizmeti örneği içindeki OAuth 2.0 yetkilendirme sunucusunu tanımlamak için kullanılır ve değerleri OAuth 2.0 sunucusundan gelmez.

3. **İstemci kayıt sayfası URL'sini**girin. Bu sayfa, kullanıcıların hesaplarını oluşturup yönetebilecekleri ve kullanılan OAuth 2.0 sağlayıcısına bağlı olarak değişebildiği sayfadır. **İstemci kayıt sayfası URL,** kullanıcıların hesapların kullanıcı yönetimini destekleyen OAuth 2.0 sağlayıcıları için kendi hesaplarını oluşturmak ve yapılandırmak için kullanabilecekleri sayfayı işaret ediyor. Bazı kuruluşlar, OAuth 2.0 sağlayıcısı desteklese bile bu işlevselliği yapılandırmaz veya kullanmaz. OAuth 2.0 sağlayıcınızda yapılandırılmış hesapların kullanıcı yönetimi yoksa, buraya şirketinizin URL'si gibi bir yer `https://placeholder.contoso.com`tutucu URL veya .

    ![OAuth 2.0 yeni sunucu](./media/api-management-howto-oauth2/oauth-02.png)

4. Formun sonraki bölümünde **Yetkilendirme hibe türleri,** **Yetkilendirme bitiş noktası URL'si**ve Yetkilendirme isteği **yöntemi** ayarları yer almaktadır.

    İstenilen türleri denetleyerek **Yetkilendirme hibe türlerini** belirtin. **Yetkilendirme kodu** varsayılan olarak belirtilir.

    Yetkilendirme **bitiş noktası URL'sini**girin. Azure Etkin Dizini için bu URL, Azure `<tenant_id>` AD kiracınızın kimliğiyle değiştirildiği aşağıdaki URL'ye benzer.

    `https://login.microsoftonline.com/<tenant_id>/oauth2/authorize`

    **Yetkilendirme isteği yöntemi,** yetkilendirme isteğinin OAuth 2.0 sunucusuna nasıl gönderildiğini belirtir. Varsayılan olarak **GET** seçilir.

5. Daha sonra, **Belirteç uç noktası URL,** **İstemci kimlik doğrulama yöntemleri,** **Erişim belirteç gönderme yöntemi** ve Varsayılan **kapsamı** belirtilmesi gerekir.

    ![OAuth 2.0 yeni sunucu](./media/api-management-howto-oauth2/oauth-03.png)

    Bir Azure Active Directory OAuth 2.0 sunucusu **için, Token uç** `<TenantID>` noktası URL'si aşağıdaki biçime sahip olacak ve burada ' nın `yourapp.onmicrosoft.com`biçimi .

    `https://login.microsoftonline.com/<TenantID>/oauth2/token`

    **İstemci kimlik doğrulama yöntemleri** için varsayılan ayar **Temel**ve **Access belirteç gönderme yöntemi** Yetkilendirme **üstbilgisidir.** Bu değerler, **varsayılan kapsamla**birlikte formun bu bölümünde yapılandırılır.

6. **İstemci kimlik bilgileri** bölümü, OAuth 2.0 sunucunuzun oluşturma ve yapılandırma işlemi sırasında elde edilen **İstemci Kimliği** ve **İstemci sırrını**içerir. **İstemci kimliği** ve **İstemci sırrı** belirtildikten sonra, **yetkilendirme kodu** için **redirect_uri** oluşturulur. Bu URI, OAuth 2.0 sunucu yapılandırmanızdaki yanıt URL'sini yapılandırmak için kullanılır.

    ![OAuth 2.0 yeni sunucu](./media/api-management-howto-oauth2/oauth-04.png)

    **Yetkilendirme hibe türleri** Kaynak sahibi **parolasına**ayarlanmışsa, **Kaynak sahibi nin parola kimlik bilgileri** bölümü bu kimlik bilgilerini belirtmek için kullanılır; aksi takdirde boş bırakabilirsiniz.

    Form tamamlandıktan sonra, API Management OAuth 2.0 yetkilendirme sunucusu yapılandırmasını kaydetmek için **Oluştur'u** tıklatın. Sunucu yapılandırması kaydedildikten sonra, bir sonraki bölümde gösterildiği gibi API'leri bu yapılandırmayı kullanacak şekilde yapılandırabilirsiniz.

## <a name="configure-an-api-to-use-oauth-20-user-authorization"></a><a name="step2"> </a>OAuth 2.0 kullanıcı yetkilendirmesini kullanacak bir API'yi yapılandırma

1. Soldaki **API Yönetimi** menüsünden **API'leri** tıklatın.

    ![OAuth 2.0 API'ler](./media/api-management-howto-oauth2/oauth-05.png)

2. İstenilen API'nin adını tıklatın ve **Ayarlar'ı**tıklatın. **Güvenlik** bölümüne gidin ve sonra **OAuth 2.0**için kutuyu işaretleyin.

    ![OAuth 2.0 ayarları](./media/api-management-howto-oauth2/oauth-06.png)

3. Açılan listeden istenen **Yetkilendirme sunucusunu** seçin ve **Kaydet'i**tıklatın.

    ![OAuth 2.0 ayarları](./media/api-management-howto-oauth2/oauth-07.png)

## <a name="legacy-developer-portal---test-the-oauth-20-user-authorization"></a><a name="step3"> </a>Eski geliştirici portalı - OAuth 2.0 kullanıcı yetkilendirmesini test edin

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

OAuth 2.0 yetkilendirme sunucunuzu yapılandırdıktan ve API'nizi bu sunucuyu kullanacak şekilde yapılandırdıktan sonra, Geliştirici Portalı'na giderek ve bir API çağırarak bunu test edebilirsiniz. Azure API Yönetimi örneği **Genel Bakış** sayfanızdan üst menüdeki **Geliştirici portalına (eski)** tıklayın.

Üst menüdeki **API'leri** tıklatın ve **Yankı API'sini**seçin.

![Echo API’si][api-management-apis-echo-api]

> [!NOTE]
> Yapılandırılmış ya da hesabınıza görünen yalnızca bir API’niz varsa, API’lere tıklamak sizi doğrudan bu API’nin işlemlerine götürür.

Kaynak **Al** işlemini seçin, **Konsolu Aç'ı**tıklatın ve ardından açılan dan **Yetkilendirme kodunu** seçin.

![Konsolu açma][api-management-open-console]

**Yetkilendirme kodu** seçildiğinde, OAuth 2.0 sağlayıcısının oturum açma formuyla bir açılır pencere görüntülenir. Bu örnekte oturum açma formu Azure Active Directory tarafından sağlanmaktadır.

> [!NOTE]
> Açılır pencereler devre dışı bırakılırsa, bunları tarayıcı tarafından etkinleştirmeniz istenir. Etkinleştirdikten sonra Yetkilendirme **kodunu** yeniden seçin ve oturum açma formu görüntülenir.

![Oturum aç][api-management-oauth2-signin]

Oturum açtıktan sonra, **İstek üstbilgisi** `Authorization : Bearer` isteği onaylayan bir üstbilgiyle doldurulur.

![Üstbilgi belirteci isteği][api-management-request-header-token]

Bu noktada kalan parametreler için istenen değerleri yapılandırabilir ve isteği gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2.0 ve API Management'ı kullanma hakkında daha fazla bilgi için aşağıdaki videove beraberindeki [makaleye](api-management-howto-protect-backend-with-aad.md)bakın.

[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png

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

