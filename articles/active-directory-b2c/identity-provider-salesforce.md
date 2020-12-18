---
title: Kayıt ayarlama ve Salesforce hesabı ile oturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanarak uygulamalarınızda Salesforce hesabı bulunan müşterilere kaydolma ve oturum açma sağlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 69c2bd96c7aa3bb3328784bb3b5027ade4902c43
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97669236"
---
# <a name="set-up-sign-up-and-sign-in-with-a-salesforce-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Salesforce hesabı ile kaydolma ve oturum açma ayarlama

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


## <a name="create-a-salesforce-application"></a>Salesforce uygulaması oluşturma

Azure Active Directory B2C (Azure AD B2C) ' de Salesforce hesabı kullanmak için Salesforce **App Manager**'da bir uygulama oluşturmanız gerekir. Daha fazla bilgi için bkz. [temel bağlı uygulama ayarlarını yapılandırma](https://help.salesforce.com/articleView?id=connected_app_create_basics.htm)ve [API Tümleştirmesi Için OAuth ayarlarını etkinleştirme](https://help.salesforce.com/articleView?id=connected_app_create_api_integration.htm)

1. [Salesforce 'Ta oturum açın](https://login.salesforce.com/).
1. Menüden **Kurulum**' u seçin.
1.  **Uygulamalar**' ı genişletin ve ardından **App Manager**' ı seçin.
1. **Yeni bağlı uygulama**' yı seçin.
1. **Temel bilgiler** altında şunu girin:
    1. **Bağlı uygulama adı** -bağlı uygulama adı App Manager 'Da ve uygulama başlatıcısı kutucuğunda görüntülenir. Ad, kuruluşunuz dahilinde benzersiz olmalıdır. 
    1. **API adı** 
    1. **Iletişim e-postası** -Salesforce için iletişim e-postası
1. **API (OAuth ayarlarını etkinleştir)** altında **OAuth ayarlarını etkinleştir** ' i seçin.
1. **Geri arama URL 'si**' nde, girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` . `your-tenant-name`Kiracınızın adıyla değiştirin. Kiracı, Azure AD B2C büyük harfle tanımlansa bile kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
1. **Seçili OAuth kapsamlarında**, **temel bilgilere (kimlik, profil, e-posta, adres, telefon) erişin** ve **benzersiz tanımlayıcısına (OpenID) erişim izni verin**.
1. **Web sunucusu akışı Için gizli iste**' yi seçin.
1. **Kimlik belirtecini Yapılandır**' ı seçin ve **Standart talepler dahil et**' i seçin.
1. **Kaydet**’e tıklayın.
1. **Tüketici anahtarı** ve **Tüketici parolası** değerlerini kopyalayın. Salesforce 'ı kiracınızda bir kimlik sağlayıcısı olarak yapılandırmak için her ikisine de ihtiyacınız olacak. **İstemci parolası** önemli bir güvenlik kimlik bilgileridir.

::: zone pivot="b2c-user-flow"

## <a name="configure-a-salesforce-account-as-an-identity-provider"></a>Bir Salesforce hesabını kimlik sağlayıcısı olarak yapılandırma

1. Azure AD B2C kiracı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Yeni OpenID Connect sağlayıcısı**' nı seçin.
1. Bir **ad** girin. Örneğin *Salesforce* girin.
1. **Meta veri URL 'si** için Salesforce kuruluşunuzla aşağıdaki URL 'yi girin `{org}` :

    ```
    https://{org}.my.salesforce.com/.well-known/openid-configuration
    ```

1. **ISTEMCI kimliği** için, daha önce KAYDETTIĞINIZ uygulama kimliğini girin.
1. **İstemci parolası** için, daha önce kaydettiğiniz istemci gizli anahtarını girin.
1. **Kapsam** için, girin `openid id profile email` .
1. **Yanıt türü** ve **Yanıt modu** için varsayılan değerleri bırakın.
1. Seçim **Etki alanı ipucu** için girin `contoso.com` . Daha fazla bilgi için bkz. [Azure Active Directory B2C kullanarak doğrudan oturum açma ayarlama](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. **Kimlik sağlayıcısı talep eşlemesi** altında aşağıdaki talepleri seçin:

    - **Kullanıcı kimliği**: *Sub*
    - **Görünen ad**: *ad*
    - **Verilen ad**: *given_name*
    - **Soyadı**: *family_name*
    - **E-posta**: *preferred_username*

1. **Kaydet**’i seçin.
::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Daha önce Azure AD B2C kiracınızda kaydettiğiniz istemci gizli anahtarını depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. Genel Bakış sayfasında **kimlik deneyimi çerçevesi**' ni seçin.
5. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
6. **Seçenekler** için öğesini seçin `Manual` .
7. İlke anahtarı için bir **ad** girin. Örneğin, `SalesforceSecret`. Ön ek, `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. **Gizli**, daha önce kaydettiğiniz istemci gizli anahtarını girin.
9. **Anahtar kullanımı** için öğesini seçin `Signature` .
10. **Oluştur**’a tıklayın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcısı ekleme

Kullanıcıların bir Salesforce hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C bir uç nokta aracılığıyla iletişim kurabildiği bir talep sağlayıcı olarak tanımlamanız gerekir. Uç noktası, belirli bir kullanıcının kimliği doğrulandığını doğrulamak için Azure AD B2C tarafından kullanılan bir talep kümesi sağlar.

Bir Salesforce hesabını, ilkenizin uzantı dosyasındaki **Claimsproviders** öğesine ekleyerek bir talep sağlayıcı olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml* açın.
2. **Claimsproviders** öğesini bulun. Yoksa, kök öğenin altına ekleyin.
3. Yeni bir **ClaimsProvider** 'ı aşağıdaki şekilde ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>salesforce.com</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Salesforce-OIDC">
          <DisplayName>Salesforce</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <!-- Update the {org} below to your Salesforce organization -->
            <Item Key="METADATA">https://{org}.my.salesforce.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid id profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">Your Salesforce application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_SalesforceSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="salesforce.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. **Meta veri** URI 'sini `{org}` Salesforce kuruluşunuzla ayarlayın.
5. Uygulama kaydından uygulama KIMLIĞINE **client_id** ayarlayın.
6. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Uzantı dosyasını doğrulama için karşıya yükle

Şimdi, ilkenizi Azure AD B2C Salesforce hesabınızla nasıl iletişim kuracağını öğrenmek için yapılandırdığınıza ulaştınız. Şu ana kadar herhangi bir sorun olmadığını doğrulamak için, ilkenizin uzantı dosyasını karşıya yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **özel ilkeler** sayfasında, **ilkeyi karşıya yükle**' yi seçin.
2. Varsa **Ilkenin üzerine yazmayı** etkinleştirin ve sonra *TrustFrameworkExtensions.xml* dosyasına gidip seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısını Kaydet

Bu noktada, kimlik sağlayıcısı ayarlanmıştır, ancak kaydolma/oturum açma ekranlarından hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, var olan bir şablon Kullanıcı yolculuğunun bir yinelemesini oluşturun ve ardından bunu, Salesforce kimlik sağlayıcısına da sahip olacak şekilde değiştirin.

1. *TrustFrameworkBase.xml* dosyasını başlangıç paketinden açın.
2. Dahil olan **Userelde ney** öğesinin tüm içeriğini bulup kopyalayın `Id="SignUpOrSignIn"` .
3. *TrustFrameworkExtensions.xml* açın ve **User, neys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **User, neys** öğesinin bir alt öğesi olarak kopyaladığınız **User, ney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun KIMLIĞINI yeniden adlandırın. Örneğin, `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**Claimsproviderselection** öğesi, kaydolma/oturum açma ekranındaki bir kimlik sağlayıcısı düğmesine benzer. Bir Salesforce hesabı için bir **Claimsproviderselection** öğesi eklerseniz, bir Kullanıcı sayfada yer alıyorsa yeni bir düğme görüntülenir.

1. Oluşturduğunuz Kullanıcı yolculuğuna dahil olan **Orchestrationstep** öğesini bulun `Order="1"` .
2. **Claimsproviderseçilir** altında aşağıdaki öğeyi ekleyin. **Targetclaimsexchangeıd** değerini uygun bir değere ayarlayın, örneğin `SalesforceExchange` :

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi bir eyleme bağlama

Artık bir düğmeye sahip olduğunuza göre, bunu bir eyleme bağlamanız gerekir. Bu durumda, bir belirteç almak için Salesforce hesabıyla iletişim kurmak Azure AD B2C.

1. Kullanıcı yolculuğu ' nda yer alan **Orchestrationstep** ' i bulun `Order="2"` .
2. **Targetclaimsexchangeıd** IÇIN kullandığınız ID için aynı değeri kullandığınızdan emin olmak Için aşağıdaki **claimsexchange** öğesini ekleyin:

    ```xml
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="Salesforce-OIDC" />
    ```

    **TechnicalProfileReferenceId** değerini daha önce oluşturduğunuz teknık profilin kimliğiyle güncelleştirin. Örneğin, `Salesforce-OIDC`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-salesforce-identity-provider-to-a-user-flow"></a>Bir Kullanıcı akışına Salesforce kimlik sağlayıcısı ekleme 

1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
1. Salesforce kimlik sağlayıcısına istediğiniz kullanıcı akışına tıklayın.
1. **Sosyal kimlik sağlayıcıları** altında **Salesforce**' ı seçin.
1. **Kaydet**’i seçin.
1. İlkenizi test etmek için **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama** için, daha önce kaydettiğiniz *testapp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır** 'a tıklayın

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Bağlı olan taraf dosyasını güncelleştirme ve test etme

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml* bir kopyasını oluşturun ve yeniden adlandırın. Örneğin, *SignUpSignInSalesforce.xml* olarak yeniden adlandırın.
1. Yeni dosyayı açın ve **TrustFrameworkPolicy** Için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInSalesforce`.
1. **Publicpolicyuri** DEĞERINI ilke URI 'siyle güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_Salesforce`
1. **Defaultuseryolculuney** Içindeki **referenceıd** özniteliğinin değerini, oluşturduğunuz yeni Kullanıcı yolculuğu (signupsignsalesforce) kimliğiyle eşleşecek şekilde güncelleştirin.
1. Değişikliklerinizi kaydedin, dosyayı karşıya yükleyin.
1. **Özel ilkeler** altında **B2C_1A_signup_signin**' yi seçin.
1. **Uygulama Seç** için, daha önce kaydettiğiniz *testapp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Şimdi Çalıştır** ' ı seçin ve Salesforce ile oturum açmak için Salesforce ' ı seçin ve özel ilkeyi test edin.

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

[Salesforce belirtecini uygulamanıza nasıl geçirebileceğinizi](idp-pass-through-user-flow.md)öğrenin.
