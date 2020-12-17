---
title: Azure Active Directory B2C kullanarak bir QQ hesabı ile kaydolma ve oturum açma ayarlama
description: Azure Active Directory B2C kullanarak uygulamalarınızda QQ hesaplarına kaydolma ve oturum açma özelliği sağlayın.
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
ms.openlocfilehash: b497176deff896e785387f4b64a8e66ff4d6d58e
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654328"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir QQ hesabı ile kaydolma ve oturum açma ayarlama

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-qq-application"></a>QQ uygulaması oluşturma

Bir QQ hesabını Azure Active Directory B2C (Azure AD B2C) ' de bir kimlik sağlayıcısı olarak kullanmak için kiracınızda onu temsil eden bir uygulama oluşturmanız gerekir. Zaten bir QQ hesabınız yoksa, ' de kaydolabilirsiniz [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033) .

### <a name="register-for-the-qq-developer-program"></a>QQ geliştirici programına kaydolma

1. QQ hesabı kimlik bilgilerinizle [QQ Geliştirici Portalında](http://open.qq.com) oturum açın.
1. Oturum açtıktan sonra, [https://open.qq.com/reg](https://open.qq.com/reg) kendinizi bir geliştirici olarak kaydetmek için bölümüne gidin.
1. **个人**(bireysel Geliştirici) seçeneğini belirleyin.
1. Gerekli bilgileri girin ve **下一步** (sonraki adım) seçeneğini belirleyin.
1. E-posta doğrulama sürecini doldurun. Bir geliştirici olarak kaydedildikten sonra birkaç günün onaylanmasını beklemeniz gerekecektir.

### <a name="register-a-qq-application"></a>Bir QQ uygulamasını kaydetme

1. Adresine gidin [https://connect.qq.com/index.html](https://connect.qq.com/index.html) .
1. **应用管理**(uygulama yönetimi) öğesini seçin.
1. **创建应用**(uygulama oluştur) seçeneğini belirleyin ve gerekli bilgileri girin.
1. `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp`**授权回调域**(geri arama URL) girin. Örneğin, `tenant_name` contoso ise, URL 'yi olarak ayarlayın `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp` .
1. **创建应用**(uygulama oluştur) seçeneğini belirleyin.
1. Onay sayfasında, uygulama yönetimi sayfasına dönmek için **应用管理** (uygulama yönetimi) öğesini seçin.
1. Oluşturduğunuz uygulamanın yanındaki **查看** (görünüm) seçeneğini belirleyin.
1. **修改**(Düzenle) öğesini seçin.
1. **Uygulama kimliği** ve **uygulama anahtarı**' nı kopyalayın. Kimlik sağlayıcısını kiracınıza eklemek için bu değerlerin her ikisine de ihtiyacınız vardır.

::: zone pivot="b2c-user-flow"

## <a name="configure-qq-as-an-identity-provider"></a>QQ 'ı kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve **QQ (Önizleme)** öğesini seçin.
1. Bir **ad** girin. Örneğin, *QQ*.
1. **ISTEMCI kimliği** için, daha önce oluşturduğunuz QQ UYGULAMASıNıN uygulama kimliğini girin.
1. **İstemci parolası** için, KAYDETTIĞINIZ uygulama anahtarını girin.
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
7. İlke anahtarı için bir **ad** girin. Örneğin, `QQSecret`. Ön ek, `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
8. **Gizli**, daha önce kaydettiğiniz istemci gizli anahtarını girin.
9. **Anahtar kullanımı** için öğesini seçin `Signature` .
10. **Oluştur**’a tıklayın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcısı ekleme

Kullanıcıların bir QQ hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C bir uç nokta aracılığıyla iletişim kurabildiği bir talep sağlayıcı olarak tanımlamanız gerekir. Uç noktası, belirli bir kullanıcının kimliği doğrulandığını doğrulamak için Azure AD B2C tarafından kullanılan bir talep kümesi sağlar.

Bir QQ hesabını, ilkenizin uzantısı dosyasındaki **Claimsproviders** öğesine ekleyerek bir talep sağlayıcı olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions.xml* açın.
2. **Claimsproviders** öğesini bulun. Yoksa, kök öğenin altına ekleyin.
3. Yeni bir **ClaimsProvider** 'ı aşağıdaki şekilde ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>qq.com</Domain>
      <DisplayName>QQ (Preview)</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="QQ-OAUTH">
          <DisplayName>QQ</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">qq</Item>
            <Item Key="authorization_endpoint">https://graph.qq.com/oauth2.0/authorize</Item>
            <Item Key="AccessTokenEndpoint">https://graph.qq.com/oauth2.0/token</Item>
            <Item Key="ClaimsEndpoint">https://graph.qq.com/oauth2.0/me</Item>
            <Item Key="scope">get_user_info</Item>
            <Item Key="HttpBinding">GET</Item>
            <Item Key="ClaimsResponseFormat">JsonP</Item>
            <Item Key="ResponseErrorCodeParamName">error</Item>
            <Item Key="external_user_identity_claim_id">openid</Item>
            <Item Key="client_id">Your QQ application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_QQSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="UserId" PartnerClaimType="openid" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="qq.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Uygulama kaydından uygulama KIMLIĞINE **client_id** ayarlayın.
5. Dosyayı kaydedin.

### <a name="upload-the-extension-file-for-verification"></a>Uzantı dosyasını doğrulama için karşıya yükle

Şimdi, ilkenizi, Azure AD B2C QQ hesabınızla nasıl iletişim kuracağını bilmesini sağlayacak şekilde yapılandırdınız. Şu ana kadar herhangi bir sorun olmadığını doğrulamak için, ilkenizin uzantı dosyasını karşıya yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **özel ilkeler** sayfasında, **ilkeyi karşıya yükle**' yi seçin.
2. Varsa **Ilkenin üzerine yazmayı** etkinleştirin ve sonra *TrustFrameworkExtensions.xml* dosyasına gidip seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısını Kaydet

Bu noktada, kimlik sağlayıcısı ayarlanmıştır, ancak kaydolma/oturum açma ekranlarından hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, var olan bir şablon Kullanıcı yolculuğunun bir yinelemesini oluşturun ve ardından QQ kimlik sağlayıcısı 'nı da içerecek şekilde değiştirin.

1. *TrustFrameworkBase.xml* dosyasını başlangıç paketinden açın.
2. Dahil olan **Userelde ney** öğesinin tüm içeriğini bulup kopyalayın `Id="SignUpOrSignIn"` .
3. *TrustFrameworkExtensions.xml* açın ve **User, neys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **User, neys** öğesinin bir alt öğesi olarak kopyaladığınız **User, ney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun KIMLIĞINI yeniden adlandırın. Örneğin, `SignUpSignInQQ`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**Claimsproviderselection** öğesi, kaydolma/oturum açma ekranındaki bir kimlik sağlayıcısı düğmesine benzer. Bir QQ hesabı için bir **Claimsproviderselection** öğesi eklerseniz, bir Kullanıcı sayfada yer alıyorsa yeni bir düğme görüntülenir.

1. Oluşturduğunuz Kullanıcı yolculuğuna dahil olan **Orchestrationstep** öğesini bulun `Order="1"` .
2. **Claimsproviderseçilir** altında aşağıdaki öğeyi ekleyin. **Targetclaimsexchangeıd** değerini uygun bir değere ayarlayın, örneğin `QQExchange` :

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="QQExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi bir eyleme bağlama

Artık bir düğmeye sahip olduğunuza göre, bunu bir eyleme bağlamanız gerekir. Bu durumda Azure AD B2C, belirteç almak üzere bir QQ hesabıyla iletişim kurmak için kullanılır.

1. Kullanıcı yolculuğu ' nda yer alan **Orchestrationstep** ' i bulun `Order="2"` .
2. **Targetclaimsexchangeıd** IÇIN kullandığınız ID için aynı değeri kullandığınızdan emin olmak Için aşağıdaki **claimsexchange** öğesini ekleyin:

    ```xml
    <ClaimsExchange Id="QQExchange" TechnicalProfileReferenceId="QQ-OAuth" />
    ```

    **TechnicalProfileReferenceId** değerini daha önce oluşturduğunuz teknık profilin kimliğiyle güncelleştirin. Örneğin, `QQ-OAuth`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-qq-identity-provider-to-a-user-flow"></a>Kullanıcı akışına QQ kimlik sağlayıcısı ekleme 

1. Azure AD B2C kiracınızda **Kullanıcı akışları**' nı seçin.
1. QQ kimlik sağlayıcısına istediğiniz kullanıcı akışına tıklayın.
1. **Sosyal kimlik sağlayıcıları** altında **QQ** öğesini seçin.
1. **Kaydet**’i seçin.
1. İlkenizi test etmek için **Kullanıcı akışını Çalıştır**' ı seçin.
1. **Uygulama** için, daha önce kaydettiğiniz *testapp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Kullanıcı akışını Çalıştır** 'a tıklayın

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Bağlı olan taraf dosyasını güncelleştirme ve test etme

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml* bir kopyasını oluşturun ve yeniden adlandırın. Örneğin, *SignUpSignInQQ.xml* olarak yeniden adlandırın.
1. Yeni dosyayı açın ve **TrustFrameworkPolicy** Için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInQQ`.
1. **Publicpolicyuri** DEĞERINI ilke URI 'siyle güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_QQ`
1. **Defaultuseryolculuney** Içindeki **referenceıd** özniteliğinin değerini, oluşturduğunuz yenı Kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde güncelleştirin (SignUpSignQQ).
1. Değişikliklerinizi kaydedin, dosyayı karşıya yükleyin.
1. **Özel ilkeler** altında **B2C_1A_signup_signin**' yi seçin.
1. **Uygulama Seç** için, daha önce kaydettiğiniz *testapp1* adlı Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Şimdi Çalıştır** ' ı seçip QQ ' ı seçerek QQ ile oturum açın ve özel ilkeyi test edin.

::: zone-end
