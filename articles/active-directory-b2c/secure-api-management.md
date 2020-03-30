---
title: Azure Active Directory B2C'yi kullanarak Azure API Yönetimi API'nizi güvenli hale
description: Azure API Yönetimi API bitiş noktasını güvence altına almak için Azure Active Directory B2C tarafından verilen erişim belirteçlerini nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 00938d831e70289b24acb599b81016aa6e564d78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186939"
---
# <a name="secure-an-azure-api-management-api-with-azure-ad-b2c"></a>Azure AD B2C ile Azure API Yönetimi API'nizi güvenli hale

Azure API Yönetimi (APIM) API'nize erişimi Azure Active Directory B2C (Azure AD B2C) ile kimlik doğrulaması yapılan istemcilerle nasıl kısıtladığınızı öğrenin. APIM'de yalnızca geçerli bir Azure AD B2C tarafından verilmiş erişim jetonunu içeren bu isteklere erişimi kısıtlayan bir gelen ilkesi oluşturmak ve sınamak için bu makaledeki adımları izleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımlara devam etmeden önce aşağıdaki kaynaklara ihtiyacınız var:

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* [Kiracınızda kayıtlı başvuru](tutorial-register-applications.md)
* Kiracınızda [oluşturulan kullanıcı akışları](tutorial-create-user-flows.md)
* Azure API Yönetimi'nde [yayınlanan API](../api-management/import-and-publish.md)
* Güvenli erişimi test etmek için [postacı](https://www.getpostman.com/) (isteğe bağlı)

## <a name="get-azure-ad-b2c-application-id"></a>Azure AD B2C uygulama kimliğini alın

Azure AD B2C ile Azure API Yönetimi'nde bir API'yi güvenli hale aldığınızda, APIM'de oluşturduğunuz [gelen ilke](../api-management/api-management-howto-policies.md) için birkaç değere gereksinim duyarsınız. İlk olarak, daha önce oluşturduğunuz bir uygulamanın uygulama kimliğini Azure AD B2C kiracınıza kaydedin. Ön koşullarda oluşturduğunuz uygulamayı kullanıyorsanız, *webbapp1*için uygulama kimliğini kullanın.

Uygulama kimliğini almak için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Yönet**altında, **Uygulamaları**seçin.
1. *Webapp1* veya daha önce oluşturduğunuz başka bir uygulama için **APPLICATION ID** sütunundaki değeri kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtları (Önizleme) seçeneğini**belirleyin, ardından Sahip **olunan uygulamalar** sekmesini seçin.
1. *Webapp1* veya daha önce oluşturduğunuz başka bir uygulama için **Uygulama (istemci) kimliği** sütununa değeri kaydedin.

* * *

## <a name="get-token-issuer-endpoint"></a>Belirteç veren bitiş noktasını alın

Ardından, Azure AD B2C kullanıcı akışlarından birinin tanınmış config URL'sini alın. Azure API Yönetimi'nde desteklemek istediğiniz belirteç veren uç noktası URI'ye de ihtiyacınız var.

1. [Azure portalında](https://portal.azure.com)Azure AD B2C kiracınıza göz atın.
1. **İlkeler** **altında, Kullanıcı akışlarını (ilkeler)** seçin.
1. Varolan bir ilke seçin, örneğin *B2C_1_signupsignin1,* sonra **kullanıcı akışını çalıştır'ı**seçin.
1. URL'yi sayfanın üst kısmındaki **Kullanıcı akışı** başlığı altında görüntülenen köprüye kaydedin. Bu URL, kullanıcı akışı için OpenID Connect iyi bilinen keşif bitiş noktasıdır ve Azure API Yönetimi'nde gelen ilkeyi yapılandırDığınızda bir sonraki bölümde kullanırsınız.

    ![Azure portalının Çalıştır şimdi sayfasında tanınmış URI köprü](media/secure-apim-with-b2c-token/portal-01-policy-link.png)

1. OpenID Connect tanınmış yapılandırma sayfasına göz atmak için köprüseçin.
1. Tarayıcınızda açılan sayfada, örneğin değeri `issuer` kaydedin:

    `https://your-b2c-tenant.b2clogin.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/v2.0/`

    API'nizi Azure API Yönetimi'nde yapılandırırken bu değeri bir sonraki bölümde kullanırsınız.

Şimdi bir sonraki bölümde kullanılmak üzere iki URL'niz olmalıdır: OpenID Connect iyi bilinen yapılandırma uç noktası URL'si ve veren URI. Örnek:

```
https://yourb2ctenant.b2clogin.com/yourb2ctenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1_signupsignin1
https://yourb2ctenant.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/
```

## <a name="configure-inbound-policy-in-azure-api-management"></a>Azure API Yönetimi'nde gelen ilkeyi yapılandırma

Artık Azure API Yönetimi'nde API çağrılarını doğrulayan gelen ilkeyi eklemeye hazırsınız. Bir erişim belirtecinde hedef kitleyi ve vereni doğrulayan bir [JWT doğrulama](../api-management/api-management-access-restriction-policies.md#ValidateJWT) ilkesi ekleyerek, yalnızca geçerli bir belirteç içeren API çağrılarının kabul edilmesini sağlayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)Azure API Yönetimi örneğinize göz atın.
1. **API’ler** seçeneğini belirleyin.
1. Azure AD B2C ile güvenli hale getirmek istediğiniz API'yi seçin.
1. **Tasarım** sekmesini seçin.
1. **Gelen işleme**altında, ** \< / ** ilke kodu düzenleyicisini açmak için seçin.
1. Aşağıdaki `<validate-jwt>` etiketi ilkenin `<inbound>` içine yerleştirin.

    1. İlkinizin `url` tanınmış `<openid-config>` yapılandırma URL'si ile öğedeki değeri güncelleştirin.
    1. B2C kiracınızda daha önce oluşturduğunuz uygulamanın Uygulama Kimliği ile `<audience>` öğeyi güncelleştirin (örneğin, *webapp1).*
    1. Öğeyi `<issuer>` daha önce kaydettiğiniz belirteç veren uç noktasıyla güncelleştirin.

    ```xml
    <policies>
        <inbound>
            <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
                <openid-config url="https://yourb2ctenant.b2clogin.com/yourb2ctenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1_signupsignin1" />
                <audiences>
                    <audience>44444444-0000-0000-0000-444444444444</audience>
                </audiences>
                <issuers>
                    <issuer>https://yourb2ctenant.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
                </issuers>
            </validate-jwt>
            <base />
        </inbound>
        <backend> <base /> </backend>
        <outbound> <base /> </outbound>
        <on-error> <base /> </on-error>
    </policies>
    ```

## <a name="validate-secure-api-access"></a>Güvenli API erişimini doğrulama

Yalnızca kimlik doğrulaması yapılan arayanların API'nize erişebilmesini sağlamak için, [Postacı](https://www.getpostman.com/)ile API'yi arayarak Azure API Yönetimi yapılandırmanızı doğrulayabilirsiniz.

API'yi aramak için hem Azure AD B2C tarafından verilen bir erişim belirteci hem de Bir APIM abonelik anahtarı gerekir.

### <a name="get-an-access-token"></a>Bir erişim belirteci alma

Postacı'daki `Authorization` üstbilgide kullanmak için öncelikle Azure AD B2C tarafından verilen bir belirteç gerekir. Ön koşullardan biri olarak oluşturmanız gereken kaydolma/kaydolma kullanıcı akışınızın **Şimdi Çalıştır** özelliğini kullanarak bir tane alabilirsiniz.

1. [Azure portalında](https://portal.azure.com)Azure AD B2C kiracınıza göz atın.
1. **İlkeler** **altında, Kullanıcı akışlarını (ilkeler)** seçin.
1. Varolan bir kaydolma/kaydolma kullanıcı akışını seçin( örneğin, örneğin *B2C_1_signupsignin1.*
1. **Uygulama**için, *webapp1*seçin.
1. **Yanıtla URL** `https://jwt.ms`için , seçin.
1. **Kullanıcı akışını çalıştır'ı**seçin.

    ![Azure portalında kaydolma için kullanıcı akışı sayfasını çalıştırın](media/secure-apim-with-b2c-token/portal-03-user-flow.png)

1. Oturum açma işlemini tamamlayın. 'ye `https://jwt.ms`yönlendirilmelisiniz.
1. Tarayıcınızda görüntülenen kodlanmış belirteç değerini kaydedin. Bu belirteç değerini Postacı'daki Yetkilendirme üstbilgisi için kullanırsınız.

    ![jwt.ms'da görüntülenen kodlanmış belirteç değeri](media/secure-apim-with-b2c-token/jwt-ms-01-token.png)

### <a name="get-api-subscription-key"></a>API abonelik anahtarını alın

Yayımlanmış bir API çağıran bir istemci uygulaması (bu durumda, Postacı), API'deki HTTP isteklerine geçerli bir API Yönetimi abonelik anahtarı içermelidir. Postacı HTTP isteğinize eklemek için bir abonelik anahtarı almak için:

1. [Azure portalındaki](https://portal.azure.com)Azure API Yönetimi hizmet örneğinize göz atın.
1. **Abonelikleri**seçin.
1. Ürün için elipsleri **seçin: Sınırsız**, ardından **Göster/Gizle tuşlarını**seçin.
1. Ürünün **BIRINCIL ANAHTARINI** kaydedin. Postman'daki HTTP `Ocp-Apim-Subscription-Key` isteğinizdeki üstbilgi için bu anahtarı kullanırsınız.

![Azure portalında seçilen Göster/gizle anahtarları içeren abonelik anahtar sayfası](media/secure-apim-with-b2c-token/portal-04-api-subscription-key.png)

### <a name="test-a-secure-api-call"></a>Güvenli bir API çağrısını test edin

Erişim jetonu ve APIM abonelik anahtarı kaydedilirken, API'ye güvenli erişimi doğru şekilde yapılandırıp yapılandırmadığınızı test etmeye hazırsınız.

1. `GET` [Postacı'da](https://www.getpostman.com/)yeni bir istek oluşturun. İstek URL'si için, yayınladığınız API'nin konuşmacılistesini ön koşullardan biri olarak belirtin. Örnek:

    `https://contosoapim.azure-api.net/conference/speakers`

1. Ardından, aşağıdaki üstbilgi ekleyin:

    | Anahtar | Değer |
    | --- | ----- |
    | `Authorization` | Önceden kaydettiğiniz, önceden önceden belirlenmiş kodlanmış `Bearer ` belirteç değeri ("Taşıyıcı"dan sonraki alanı içerir) |
    | `Ocp-Apim-Subscription-Key` | Daha önce kaydettiğiniz APIM abonelik anahtarı |

    **GET** istek URL'niz ve **Üstbilginiz** benzer görünmelidir:

    ![GET istek URL'sini ve üstbilgilerini gösteren Postacı Kullanıcı UI](media/secure-apim-with-b2c-token/postman-01-headers.png)

1. İsteği yürütmek için Postacı'da **Gönder** düğmesini seçin. Her şeyi doğru şekilde yapılandırmışsanız, konferans konuşmacılarından oluşan bir koleksiyoniçeren bir JSON yanıtı ile birlikte sunulmalıdır (burada kesilmiş olarak gösterilmiştir):

    ```JSON
    {
      "collection": {
        "version": "1.0",
        "href": "https://conferenceapi.azurewebsites.net:443/speakers",
        "links": [],
        "items": [
          {
            "href": "https://conferenceapi.azurewebsites.net/speaker/1",
            "data": [
              {
                "name": "Name",
                "value": "Scott Guthrie"
              }
            ],
            "links": [
              {
                "rel": "http://tavis.net/rels/sessions",
                "href": "https://conferenceapi.azurewebsites.net/speaker/1/sessions"
              }
            ]
          },
    [...]
    ```

### <a name="test-an-insecure-api-call"></a>Güvenli olmayan bir API çağrısını test edin

Başarılı bir istekte bulunduğunuza göre, *geçersiz* bir belirteçle API'nize yapılan çağrıların beklendiği gibi reddedildiğinden emin olmak için hata durumunu test edin. Testi gerçekleştirmenin bir yolu, belirteç değerindebirkaç karakter eklemek veya değiştirmek, `GET` ardından öncekiyle aynı isteği gerçekleştirmektir.

1. Geçersiz bir belirteci simüle etmek için belirteç değerine birkaç karakter ekleyin. Örneğin, belirteç değerine "GEÇERSIZ" ekleyin:

    ![Postacı UI'nin GEÇERSIZ gösteren başlıkları bölümü belirteci eklendi](media/secure-apim-with-b2c-token/postman-02-invalid-token.png)

1. İsteği yürütmek için **Gönder** düğmesini seçin. Geçersiz bir belirteç ile beklenen `401` sonuç yetkisiz bir durum kodudur:

    ```JSON
    {
        "statusCode": 401,
        "message": "Unauthorized. Access token is missing or invalid."
    }
    ```

`401` Durum kodunu görürseniz, yalnızca Azure AD B2C tarafından verilen geçerli bir erişim jetonu olan arayanların Azure API Yönetimi API'nize başarılı isteklerde bulunabileceğini doğruladınız.

## <a name="support-multiple-applications-and-issuers"></a>Birden çok uygulamayı ve vereni destekleme

Çeşitli uygulamalar genellikle tek bir REST API ile etkileşim. API'nizin birden çok uygulama için tasarlanmış belirteçleri kabul etmesini `<audiences>` sağlamak için, uygulama kimliklerini APIM gelen ilkesindeki öğeye ekleyin.

```XML
<!-- Accept tokens intended for these recipient applications -->
<audiences>
    <audience>44444444-0000-0000-0000-444444444444</audience>
    <audience>66666666-0000-0000-0000-666666666666</audience>
</audiences>
```

Benzer şekilde, birden çok belirteç vereni desteklemek için, apim gelen ilkesindeki `<issuers>` öğeye uç nokta URI'lerini ekleyin.

```XML
<!-- Accept tokens from multiple issuers -->
<issuers>
    <issuer>https://yourb2ctenant.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
    <issuer>https://login.microsoftonline.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
</issuers>
```

## <a name="migrate-to-b2clogincom"></a>b2clogin.com'a geçirin

Eski `login.microsoftonline.com` bitiş noktası tarafından verilen belirteçleri doğrulayan bir APIM API'niz varsa, API'yi ve onu [b2clogin.com](b2clogin.md)tarafından verilen belirteçleri kullanmak için arayan uygulamaları geçirmeniz gerekir.

Aşamalı bir geçiş gerçekleştirmek için bu genel işlemi izleyebilirsiniz:

1. Hem b2clogin.com hem de login.microsoftonline.com tarafından verilen belirteçler için APIM gelen ilkesine destek ekleyin.
1. b2clogin.com uç noktasından belirteçler elde etmek için uygulamalarınızı teker teker güncelleyin.
1. Tüm uygulamalarınız b2clogin.com belirteçleri doğru bir şekilde aldıktan sonra, API'den login.microsoftonline.com verilen belirteçler için desteği kaldırın.

Aşağıdaki örnek APIM gelen ilkesi, hem b2clogin.com hem de login.microsoftonline.com tarafından verilen belirteçlerin nasıl kabul edilebildiğini göstermektedir. Ayrıca, iki uygulamadan gelen API isteklerini destekler.

```XML
<policies>
    <inbound>
        <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
            <openid-config url="https://yourb2ctenant.b2clogin.com/yourb2ctenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1_signupsignin1" />
            <audiences>
                <audience>44444444-0000-0000-0000-444444444444</audience>
                <audience>66666666-0000-0000-0000-666666666666</audience>
            </audiences>
            <issuers>
                <issuer>https://login.microsoftonline.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
                <issuer>https://yourb2ctenant.b2clogin.com/99999999-0000-0000-0000-999999999999/v2.0/</issuer>
            </issuers>
        </validate-jwt>
        <base />
    </inbound>
    <backend> <base /> </backend>
    <outbound> <base /> </outbound>
    <on-error> <base /> </on-error>
</policies>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure API Yönetimi ilkeleri yle ilgili ek ayrıntılar için [APIM ilke başvuru dizinine](../api-management/api-management-policies.md)bakın.

OWIN tabanlı web API'leri ve bunların uygulamaları hakkında [owin tabanlı bir web API'sini b2clogin.com](multiple-token-endpoints.md)geçir'e b2clogin.com için bilgi bulabilirsiniz.
