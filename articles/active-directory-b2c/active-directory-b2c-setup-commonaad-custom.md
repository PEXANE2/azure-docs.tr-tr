---
title: Azure Active Directory B2C | içinde özel ilkeler kullanarak çok kiracılı Azure AD kimlik sağlayıcısı için oturum açma ayarlayın | Microsoft Docs
description: Özel ilkeler kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısı ekleyin-Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8524eb8f9a8d220964e5dd1f6f8dc6d1aaf94a6d
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980714"
---
# <a name="set-up-sign-in-for-multi-tenant-azure-active-directory-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içindeki özel ilkeleri kullanarak çok kiracılı Azure Active Directory için oturum açma ayarlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure AD B2C içindeki [özel ilkeleri](active-directory-b2c-overview-custom.md) kullanarak Azure Active Directory (Azure AD) için çok kiracılı uç noktasını kullanan kullanıcılar için oturum açma özelliğini nasıl etkinleştireceğinizi gösterilmektedir. Bu, birden çok Azure AD kiracısından kullanıcıların her kiracı için bir teknik sağlayıcı yapılandırmadan Azure AD B2C oturum açmasına olanak tanır. Ancak, bu kiracılardan herhangi birinde bulunan Konuk üyeleri oturum açamaz. Bu şekilde, [her bir kiracıyı tek tek yapılandırmanız](active-directory-b2c-setup-aad-custom.md)gerekir.

>[!NOTE]
>`Contoso.com`, kuruluş Azure AD kiracısı için kullanılır ve `fabrikamb2c.onmicrosoft.com` aşağıdaki yönergelerde Azure AD B2C kiracı olarak kullanılır.

## <a name="prerequisites"></a>Önkoşullar

[Azure Active Directory B2C özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md)bölümündeki adımları uygulayın.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Kullanıcıların belirli bir Azure AD kuruluştan oturum açmasını etkinleştirmek için, uygulamayı kurumsal Azure AD kiracısında kaydetmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin ve abonelik filtresi** ' ne tıklayarak ve kiracınızı içeren dizini seçerek kuruluş Azure AD kiracısı (contoso.com) içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **uygulama kayıtları**' i arayıp seçin.
4. **Yeni uygulama kaydı**’nı seçin.
5. Uygulamanız için bir ad girin. Örneğin: `Azure AD B2C App`.
6. **Uygulama türü**için öğesini seçin `Web app / API`.
7. **Oturum açma URL 'si**için, aşağıdaki URL 'yi tüm küçük harflerle girin; burada `your-tenant` , Azure AD B2C kiracınızın adı ile değiştirilmiştir (fabrikamb2c.onmicrosoft.com):

    ```
    https://yourtenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```

8. **Oluştur**'a tıklayın. Daha sonra kullanılacak **uygulama kimliğini** kopyalayın.
9. Uygulamayı seçin ve ardından **Ayarlar**' ı seçin.
10. **Anahtarlar**' ı seçin, anahtar açıklamasını girin, bir süre seçin ve **Kaydet**' e tıklayın. Daha sonra kullanılmak üzere görüntülenen anahtarın değerini kopyalayın.
11. **Ayarlar**altında **Özellikler**' i seçin, **çok** kiracılı olarak ayarlayın ve `Yes`ardından **Kaydet** ' e tıklayın.

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda oluşturduğunuz uygulama anahtarını depolamanız gerekir.

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
2. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
3. Genel Bakış sayfasında **kimlik deneyimi çerçevesi**' ni seçin.
4. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
5. **Seçenekler**için öğesini seçin `Manual`.
6. İlke anahtarı için bir **ad** girin. Örneğin: `ContosoAppSecret`.  Ön ek `B2C_1A_` , anahtarınızın adına otomatik olarak eklenir.
7. **Gizli**, daha önce kaydettiğiniz uygulama anahtarınızı girin.
8. **Anahtar kullanımı**için öğesini seçin `Signature`.
9. **Oluştur**'a tıklayın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcısı ekleme

Kullanıcıların Azure AD 'yi kullanarak oturum açmasını istiyorsanız, Azure AD 'yi Azure AD B2C bir uç nokta aracılığıyla iletişim kurabildiği bir talep sağlayıcı olarak tanımlamanız gerekir. Uç noktası, belirli bir kullanıcının kimliği doğrulandığını doğrulamak için Azure AD B2C tarafından kullanılan bir talep kümesi sağlar.

Azure AD 'yi, ilkenizin uzantısı dosyasındaki **ClaimsProvider** öğesine ekleyerek bir talep sağlayıcı olarak tanımlayabilirsiniz.

1. *TrustFrameworkExtensions. xml*' i açın.
2. **Claimsproviders** öğesini bulun. Yoksa, kök öğenin altına ekleyin.
3. Yeni bir **ClaimsProvider** 'ı aşağıdaki şekilde ekleyin:

    ```XML
    <ClaimsProvider>
      <Domain>commonaad</Domain>
      <DisplayName>Common AAD</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Common-AAD">
          <DisplayName>Multi-Tenant AAD</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="METADATA">https://login.microsoftonline.com/common/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="DiscoverMetadataByTokenIssuer">true</Item>

            <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
            <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item>

            <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
            <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item> -->
          </Metadata>
          <CryptographicKeys>
            <!-- Make sure to update the reference ID of the client secret below you just created (B2C_1A_AADAppSecret) -->
            <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="unique_name" />
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

4. **ClaimsProvider** öğesi altında, **etki alanı** değerini diğer kimlik sağlayıcılarından ayırt etmek için kullanılabilecek benzersiz bir değer olarak güncelleştirin.
5. **Teknisyen** öğesinin altında **DisplayName**değerini güncelleştirin. Bu değer, oturum açma ekranınızdaki oturum açma düğmesinde görüntülenir.
6. **Client_id** 'ı Azure AD mulity-kiracı uygulama KAYDıNDAN uygulama kimliği olarak ayarlayın.

### <a name="restrict-access"></a>Erişimi kısıtla

> [!NOTE]
> `https://sts.windows.net` **Validtokenıssueröneklerini** değer olarak kullanmak tüm Azure AD kullanıcılarının uygulamanızda oturum açmasını sağlar.

Geçerli belirteç verenler listesini güncelleştirmeniz ve erişimi, oturum açabilen Azure AD kiracı kullanıcıları 'nın belirli bir listesiyle kısıtlamanız gerekir. Değerleri almak için, kullanıcıların oturum açmasını istediğiniz her bir Azure AD kiracısından oluşan meta verilere bakmanız gerekir. Verilerin biçimi şu şekilde görünür: `https://login.windows.net/your-tenant/.well-known/openid-configuration`, burada `your-tenant` Azure AD kiracı adınız (contoso.com veya diğer Azure AD kiracısıdır).

1. Tarayıcınızı açın, **meta veri** URL 'sine gidin ve **veren** nesnesini bulun ve değerini kopyalayın. Aşağıdaki gibi görünmelidir: `https://sts.windows.net/tenant-id/`.
2. **Validtokenıssueröneklerini** anahtarı için değeri kopyalayıp yapıştırın. Bir virgül kullanarak ayırarak birden çok ekleyebilirsiniz. Bunun bir örneği yukarıdaki örnek XML 'de yorumbir örnektir.

### <a name="upload-the-extension-file-for-verification"></a>Uzantı dosyasını doğrulama için karşıya yükle

Şimdi, ilkenizi Azure AD dizininizle nasıl iletişim kuracağını öğrenmek için Azure AD B2C ilkenizi yapılandırdınız. Şu ana kadar herhangi bir sorun olmadığını doğrulamak için, ilkenizin uzantı dosyasını karşıya yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **özel ilkeler** sayfasında, **ilkeyi karşıya yükle**' yi seçin.
2. Varsa **Ilkenin üzerine yazmayı**etkinleştirin ve ardından *TrustFrameworkExtensions. xml* dosyasına gidip seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcısını Kaydet

Bu noktada, kimlik sağlayıcısı ayarlanmıştır, ancak kaydolma/oturum açma ekranlarından hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, var olan bir şablon Kullanıcı yolculuğunun bir yinelemesini oluşturun ve ardından Azure AD kimlik sağlayıcısı 'nı da içerecek şekilde değiştirin.

1. *TrustFrameworkBase. xml* dosyasını başlangıç paketinden açın.
2. Dahil`Id="SignUpOrSignIn"`olan **userelde ney** öğesinin tüm içeriğini bulup kopyalayın.
3. *TrustFrameworkExtensions. xml* ' i açın ve **User, neys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **User, neys** öğesinin bir alt öğesi olarak kopyaladığınız **User, ney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun KIMLIĞINI yeniden adlandırın. Örneğin: `SignUpSignInContoso`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**Claimsproviderselection** öğesi, kaydolma/oturum açma ekranındaki bir kimlik sağlayıcısı düğmesine benzer. Azure AD için bir **Claimsproviderselection** öğesi eklerseniz, bir Kullanıcı sayfada yer alıyorsa yeni bir düğme görüntülenir.

1. Oluşturduğunuz Kullanıcı yolculuğuna dahil `Order="1"` olan **orchestrationstep** öğesini bulun.
2. **Claimsproviderseçilir**altında aşağıdaki öğeyi ekleyin. **Targetclaimsexchangeıd** değerini uygun bir değere ayarlayın, örneğin `AzureADExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi bir eyleme bağlama

Artık bir düğmeye sahip olduğunuza göre, bunu bir eyleme bağlamanız gerekir. Bu durumda Azure AD B2C eylem, Azure AD ile iletişim kurmak için bir belirteç almak üzere kullanılır. Azure AD talep sağlayıcınızla ilgili teknik profili bağlayarak düğmeyi bir eyleme bağlayın.

1. Kullanıcı yolculuğu `Order="2"` ' nda yer alan **orchestrationstep** ' i bulun.
2. **Targetclaimsexchangeıd**Için kullandığınız **ID** için aynı değeri kullandığınızdan emin olmak Için aşağıdaki **claimsexchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

    **TechnicalProfileReferenceId** değerini daha önce oluşturduğunuz teknik profilin **kimliğiyle** güncelleştirin. Örneğin: `Common-AAD`.

3. *TrustFrameworkExtensions. xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, kiracınızda oluşturduğunuz bir uygulama aracılığıyla oluşur. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabildiğiniz isteğe bağlı adımlar listelenmektedir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin, örneğin *testapp1*.
6. **Web uygulaması/Web API 'si**için, `Yes`öğesini seçin ve ardından `https://jwt.ms` **yanıt URL 'si**için yazın.
7. **Oluştur**'a tıklayın.

## <a name="update-and-test-the-relying-party-file"></a>Bağlı olan taraf dosyasını güncelleştirme ve test etme

Oluşturduğunuz Kullanıcı yolculuğunu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *Signuporsignın. xml* ' in bir kopyasını oluşturun ve yeniden adlandırın. Örneğin, bunu *Signupsigncontoso. xml*olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** Için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin: `SignUpSignInContoso`.
3. **Publicpolicyuri** DEĞERINI ilke URI 'siyle güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_contoso`
4. **Defaultuseryolculuney** Içindeki **referenceıd** özniteliğinin değerini, oluşturduğunuz yenı Kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde güncelleştirin (signupsigncontoso).
5. Değişikliklerinizi kaydedin, dosyayı karşıya yükleyin ve ardından listeden yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının **Uygulama Seç** alanında seçildiğinden emin olun ve **Şimdi Çalıştır**' a tıklayarak test edin.
