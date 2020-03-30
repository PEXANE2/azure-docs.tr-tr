---
title: AAD ve API Yönetimi ile OAuth 2.0 kullanarak bir API'yi koruyun
titleSuffix: Azure API Management
description: Azure Etkin Dizin ve API Yönetimi ile web API arka ucunu nasıl koruyacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/21/2019
ms.author: apimpm
ms.openlocfilehash: 8b396b782c1254b3229aeeb8e51b61cc744d6318
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190374"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Azure Active Directory ve API Management ile OAuth 2.0 kullanarak API'yi koruma

Bu kılavuz, Azure Etkin Dizini (Azure AD) ile OAuth 2.0 protokolünü kullanarak bir API'yi korumak için Azure API Yönetimi örneğini nasıl yapılandırabileceğinizi gösterir. 

> [!NOTE]
> Bu özellik API **Yönetimi'nin Geliştirici,** **Standart** ve **Premium** katmanlarında kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımları izlemek için şunları yapmış olmalısınız:
* API Yönetimi örneği
* API Yönetimi örneğini kullanan bir API yayınlanıyor
* Azure AD kiracı

## <a name="overview"></a>Genel Bakış

Adımların hızlı bir özeti aşağıda verebilirsiniz:

1. API'yi temsil etmek için Azure AD'de bir uygulama (arka uç uygulaması) kaydedin.
2. API'yi çağırması gereken bir istemci uygulamasını temsil etmek için Azure AD'ye başka bir uygulama (istemci uygulaması) kaydedin.
3. Azure AD'de, istemci uygulamasının arka uç uygulamasını aramasına izin vermek için izin verin.
4. Geliştirici Konsolu'nu OAuth 2.0 kullanıcı yetkilendirmesini kullanarak API'yi arayacak şekilde yapılandırın.
5. Gelen her istek için OAuth belirteci doğrulamak için **doğrulama-jwt** ilkesini ekleyin.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>API'yi temsil etmek için bir uygulamayı Azure AD'de kaydedin

Bir API'yi Azure AD ile korumak için ilk adım, API'yi temsil eden bir uygulamayı Azure AD'ye kaydetmektir. 

1. Uygulamanızı kaydetmek için [Azure portalına](https://portal.azure.com) gidin. **APP kayıtlarını**arayın ve seçin.

1. **Yeni kayıt**seçin. 

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin: 
    - **Ad** bölümüne, uygulamanın kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin *arka uç uygulaması.* 
    - Desteklenen **hesap türleri** bölümünde, senaryonuza uygun bir seçenek seçin. 

1. Yeniden **Yönlendirme URI** bölümünü boş bırakın.

1. Uygulamayı kaydetmek için **Kaydet**'i seçin. 

1. Uygulamaya **Genel Bakış** sayfasında, **Uygulama (istemci) kimlik** değerini bulun ve daha sonraya kaydedin.

1. **ApI'yi ortaya çıkar'ı** seçin ve **Uygulama Kimliği URI'sini** varsayılan değerle ayarlayın. Bu değeri daha sonra kaydedin.

1. Kapsam sayfası **ekle'yi** görüntülemek için **kapsam ekle** düğmesini seçin. Ardından API tarafından desteklenen yeni bir kapsam oluşturun (örneğin, `Files.Read`). Son olarak, kapsamı oluşturmak için **kapsam ekle** düğmesini seçin. API'niz tarafından desteklenen tüm kapsamları eklemek için bu adımı yineleyin.

1. Kapsamlar oluşturulduğunda, sonraki bir adımda kullanılmak üzere bunları not edin. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>İstemci uygulamasını temsil etmek için Azure AD'de başka bir uygulama kaydedin

API'yi çağıran her istemci uygulamasının Azure AD'de de bir uygulama olarak kaydedilmesi gerekir. Bu örnekte, istemci uygulaması API Yönetimi geliştirici portalındaki Geliştirici Konsolu'dur. Geliştirici Konsolu'nu temsil etmek için Azure AD'de başka bir uygulamayı şu şekilde kaydedebilirsiniz.

1. Uygulamanızı kaydetmek için [Azure portalına](https://portal.azure.com) gidin. **APP kayıtlarını**arayın ve seçin.

1. **Yeni kayıt**seçin.

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin: 
    - **Ad** bölümüne, *istemci uygulaması*gibi uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin. 
    - Desteklenen **hesap türleri** bölümünde, **herhangi bir kuruluş dizinindeki (Herhangi Bir Azure REKLAM dizini - Multitenant) Hesapları**seçin. 

1. **URI'yi Yeniden Yönlendirme** `Web` bölümünde URL'yi `https://contoso5.portal.azure-api.net/signin`seçin ve girin.

1. Uygulamayı kaydetmek için **Kaydet**'i seçin. 

1. Uygulamaya **Genel Bakış** sayfasında, **Uygulama (istemci) kimlik** değerini bulun ve daha sonraya kaydedin.

Şimdi, bu uygulama nın sonraki bir adımda kullanması için bir istemci sırrı oluşturun.

1. İstemci uygulamanızın sayfaları **listesinden, Sertifikalar & sırları**seçin ve Yeni istemci **sırrını**seçin.

1. Bir **istemci gizli ekle**altında, bir **Açıklama**sağlar. Anahtarın süresinin ne zaman dolmasını belirleyin ve **Ekle'yi**seçin.

Gizli oluşturulduğunda, sonraki bir adımda kullanmak için anahtar değeri ne not edin. 

## <a name="grant-permissions-in-azure-ad"></a>Azure AD'de izin verme

API ve Geliştirici Konsolu'nu temsil etmek üzere iki uygulama kaydetmiş olduğunuza göre, istemci uygulamasının arka uç uygulamasını aramasına izin vermek için izin ler vermeniz gerekir.  

1. Müşteri başvurunuza izin vermek için [Azure portalına](https://portal.azure.com) gidin. **APP kayıtlarını**arayın ve seçin.

1. İstemci uygulamanızı seçin. Ardından uygulamanın sayfaları listesinde **API izinlerini**seçin.

1. **İzin Ekle'yi**seçin.

1. **API seçin**altında, **API'leriseçin**ve ardından arka uç uygulamanızı bulun ve seçin.

1. **Temsilci İzni altında,** arka uç uygulamanıza uygun izinleri seçin ve ardından **İzin Ekle'yi**seçin.

1. İsteğe bağlı olarak, **API izinleri** sayfasında, bu dizindeki tüm kullanıcılar adına onay vermek **>kiracı \<adınız için Grant yönetici onayı'nı** seçin. 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Geliştirici Konsolunda OAuth 2.0 kullanıcı yetkilendirmesini etkinleştirme

Bu noktada, uygulamalarınızı Azure AD'de oluşturdunuz ve istemci uygulamasının arka uç uygulamasını aramasına izin vermek için uygun izinler verdiniz. 

Bu örnekte, Geliştirici Konsolu istemci uygulamasıdır. Aşağıdaki adımlar, Geliştirici Konsolunda OAuth 2.0 kullanıcı yetkilendirmesinin nasıl etkinleştirilen açıklanmıştır. 

1. Azure portalında API Yönetimi örneğinize göz atın.

1. **OAuth 2.0** > **Ekle'yi**seçin.

1. Bir **Ekran adı** ve **Açıklama**sağlayın.

1. **İstemci kayıt sayfası**URL'si için, `http://localhost`yer tutucu değeri girin, örneğin. **İstemci kayıt sayfası URL,** kullanıcıların bunu destekleyen OAuth 2.0 sağlayıcıları için kendi hesaplarını oluşturmak ve yapılandırmak için kullanabilecekleri bir sayfayı işaret ediyor. Bu örnekte, kullanıcılar kendi hesaplarını oluşturmaz ve yapılandırmaz, bu nedenle bunun yerine bir yer tutucu kullanırsınız.

1. **Yetkilendirme hibe türleri için**Yetkilendirme **kodunu**seçin.

1. Yetkilendirme **bitiş noktası URL'sini** ve **Token uç noktası URL'sini**belirtin. Azure AD kiracınızdaki **Uç Noktalar** sayfasından bu değerleri alın. **Uygulama kayıtları** sayfasına tekrar göz atın ve **Bitiş Noktalarını**seçin.


1. **OAuth 2.0 Yetkilendirme Bitiş Noktası'nı**kopyalayın ve **Yetkilendirme bitiş noktası URL** metin kutusuna yapıştırın. Yetkilendirme isteği yöntemi altında **POST'u** seçin.

1. **OAuth 2.0 Token Endpoint'i**kopyalayın ve **Token endpoint URL** metin kutusuna yapıştırın. 

    >[!IMPORTANT]
    > **V1** veya **v2** uç noktalarını kullanabilirsiniz. Ancak, seçtiğiniz sürümbağlı olarak, aşağıdaki adım farklı olacaktır. V2 uç noktalarını kullanmanızı öneririz. 

1. **v1** uç noktaları kullanıyorsanız, **kaynak**adlı bir gövde parametresi ekleyin. Bu parametrenin değeri için arka uç uygulamasının **Uygulama Kimliği'ni** kullanın. 

1. **v2** uç noktaları kullanıyorsanız, **Varsayılan kapsam** alanında arka uç uygulaması için oluşturduğunuz kapsamı kullanın.

1. Ardından, istemci kimlik bilgilerini belirtin. Bunlar istemci uygulamasının kimlik bilgileridir.

1. **İstemci Kimliği**için, istemci uygulamasının **Uygulama Kimliğini** kullanın.

1. **İstemci gizli**için, istemci uygulaması için oluşturduğunuz anahtarı daha önce kullanın. 

1. İstemci sırrını hemen takiben, yetkilendirme kodu hibe türü için **redirect_url.** Bu URL'ye not edin.

1. **Oluştur'u**seçin.

1. İstemci uygulamanıza geri dön ve **Kimlik Doğrulama'yı**seçin.

1. **Yönlendirme URI'leri**altında, **Web**türünü seçin, **Redirect_url Redirect URI**altında yapıştırın ve sonra kaydedin. **redirect_url**

Artık bir OAuth 2.0 yetkilendirme sunucusu yapılandırdığınıza göre, Geliştirici Konsolu Azure AD'den erişim belirteçleri edinebilir. 

Bir sonraki adım, API'niz için OAuth 2.0 kullanıcı yetkilendirmesini etkinleştirmektir. Bu, Geliştirici Konsolu'nun API'nize çağrı yapmadan önce kullanıcı adına bir erişim jetonu alması gerektiğini bilmesini sağlar.

1. API Yönetimi örneğinize göz atın ve **API'lere**gidin.

2. Korumak istediğiniz API'yi seçin. Örneğin, 'yi `Echo API`kullanabilirsiniz.

3. **Ayarlar'a**gidin.

4. **Güvenlik**altında, **OAuth 2.0'ı**seçin ve daha önce yapılandırdığınız OAuth 2.0 sunucusunu seçin. 

5. **Kaydet'i**seçin.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Geliştirici portalından API'yi başarıyla arayın

> [!NOTE]
> Bu bölüm, geliştirici portalını desteklemeyen **Tüketim** katmanı için geçerli değildir.

OAuth 2.0 kullanıcı yetkilendirmesi API'nizde etkinleştirildiğinden, Geliştirici Konsolu API'yi aramadan önce kullanıcı adına bir erişim belirteci elde eder.

1. Geliştirici portalındaki API altındaki herhangi bir çalışmaya göz atın ve **Tazyi'yi**seçin. Bu sizi Geliştirici Konsolu'na getirir.

2. Az önce eklediğiniz yetkilendirme sunucusuna karşılık gelen **Yetkilendirme** bölümünde yeni bir öğe not edin.

3. Yetkilendirme açılır listesinden **Yetkilendirme kodunu** seçin ve Azure AD kiracısında oturum açmanız istenir. Hesapla zaten oturum açtıysanız, sizden istenmeyebilir.

4. Başarılı oturum açmadan `Authorization` sonra, isteğe Azure AD'den erişim belirteciyle bir üstbilgi eklenir. Aşağıda örnek belirteç (Base64 kodlanmış):

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. **Gönder'i**seçin ve API'yi başarıyla arayabilirsiniz.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>İstekleri ön yetkilendirmek için bir JWT doğrulama ilkesini yapılandırma

Bu noktada, bir kullanıcı Geliştirici Konsolu'ndan arama yapmaya çalıştığında, kullanıcıdan oturum açması istenir. Geliştirici Konsolu kullanıcı adına bir erişim belirteci alır ve API'ye yapılan istekte belirteç içerir.

Ancak, birisi API'nizi belirteç olmadan veya geçersiz bir belirteçle ararsa ne olur? Örneğin, `Authorization` üstbilgi olmadan API'yi aramaya çalışın, arama yine de geçer. Bunun nedeni, API Yönetimi'nin bu noktada erişim belirteci doğrulamamasıdır. Sadece üstbilgi `Authorization` arka uç API geçer.

Gelen her isteğin erişim belirteçlerini doğrulayarak API Yönetimi'ndeki istekleri ön yetkilendirmek için [JWT doğrula](api-management-access-restriction-policies.md#ValidateJWT) ilkesini kullanabilirsiniz. Bir istek geçerli bir belirteç yoksa, API Yönetimi bunu engeller. Örneğin, politika bölümüne `<inbound>` aşağıdaki ilkeyi `Echo API`ekleyin. Bir erişim belirtecinde hedef kitle talebini denetler ve belirteç geçerli değilse bir hata iletisi döndürür. İlkeleri yapılandırma hakkında bilgi için ilkeleri [Ayarla veya edin.](set-edit-policies.md)

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```
> [!NOTE]
> Bu `openid-config` URL v1 bitiş noktasına karşılık gelir. v2 `openid-config`bitiş noktası için. `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`

## <a name="build-an-application-to-call-the-api"></a>API'yi aramak için bir uygulama oluşturma

Bu kılavuzda, API Yönetimi'ndeki Geliştirici Konsolu'nu örnek `Echo API` istemci uygulaması olarak oAuth 2.0 tarafından korunanı aramak için kullandınız. Bir uygulamanın nasıl oluşturacağı ve OAuth 2.0'ı nasıl uygulayacağınız hakkında daha fazla bilgi edinmek için [Azure Active Directory kod örneklerine](../active-directory/develop/sample-v2-code.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory ve OAuth2.0](../active-directory/develop/authentication-scenarios.md)hakkında daha fazla bilgi edinin.
* API Yönetimi hakkında daha fazla [video](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.
* Arka uç hizmetinizi güvence altına almanın diğer yolları için [Karşılıklı Sertifika kimlik doğrulaması'na](api-management-howto-mutual-certificates.md)bakın.

* [BIR API Yönetimi hizmeti örneği oluşturun.](get-started-create-service-instance.md)

* [İlk API'nizi yönetin.](import-and-publish.md)
