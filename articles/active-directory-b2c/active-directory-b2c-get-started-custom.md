---
title: Özel ilkeleri kullanmaya başlama-Azure Active Directory B2C
description: Azure Active Directory B2C özel ilkeleri kullanmaya nasıl başlaleyeceğinizi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8e858869d742120138e7997ce21d9e4cca93ed9b
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71264374"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanmaya başlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Özel ilkeler](active-directory-b2c-overview-custom.md) , Azure Active Directory B2C (Azure AD B2C) kiracınızın davranışını tanımlayan yapılandırma dosyalarıdır. Bu makalede, bir e-posta adresi ve parola kullanarak yerel hesap kaydolma veya oturum açma 'yı destekleyen özel bir ilke oluşturacaksınız. Ayrıca, ortamınızı kimlik sağlayıcıları eklemek için hazırlarsınız.

## <a name="prerequisites"></a>Önkoşullar

- Henüz bir tane yoksa, Azure aboneliğinize bağlı [bir Azure AD B2C kiracı oluşturun](tutorial-create-tenant.md) .
- [Uygulamanızı](tutorial-register-applications.md) , Azure AD B2C ile iletişim kurabilmesi için oluşturduğunuz kiracıya kaydedin.
- Facebook uygulamasını yapılandırmak için [kayıt ayarlama ve Facebook hesabıyla oturum açma](active-directory-b2c-setup-fb-app.md) adımlarını izleyin.

## <a name="add-signing-and-encryption-keys"></a>İmzalama ve şifreleme anahtarları Ekle

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Azure AD B2C kiracınızı içeren dizini seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. Genel Bakış sayfasında **kimlik deneyimi çerçevesi**' ni seçin.

### <a name="create-the-signing-key"></a>İmzalama anahtarını oluşturma

1. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Generate`.
1. **Ad**alanına girin `TokenSigningKeyContainer`. Ön ek `B2C_1A_` otomatik olarak eklenebilir.
1. **Anahtar türü**için **RSA**' yı seçin.
1. **Anahtar kullanımı**için **imza**' yı seçin.
1. **Oluştur**’u seçin.

### <a name="create-the-encryption-key"></a>Şifreleme anahtarı oluşturma

1. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Generate`.
1. **Ad**alanına girin `TokenEncryptionKeyContainer`. _ Ön `B2C_1A`eki otomatik olarak eklenebilir.
1. **Anahtar türü**için **RSA**' yı seçin.
1. **Anahtar kullanımı**için **şifreleme**' yi seçin.
1. **Oluştur**’u seçin.

### <a name="create-the-facebook-key"></a>Facebook anahtarını oluşturma

Facebook uygulamanızın [uygulama gizli](active-directory-b2c-setup-fb-app.md) anahtarını ilke anahtarı olarak ekleyin. Oluşturduğunuz uygulamanın uygulama gizli anahtarını, bu makalenin önkoşullarının bir parçası olarak kullanabilirsiniz.

1. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Manual`.
1. **Ad**için girin `FacebookSecret`. Ön ek `B2C_1A_` otomatik olarak eklenebilir.
1. **Gizli**dizi bölümünde Facebook uygulamanızın *uygulama gizli* anahtarını Developers.facebook.com adresinden girin. Bu değer, uygulama KIMLIĞI değil gizli dizi.
1. **Anahtar kullanımı**için **imza**' yı seçin.
1. **Oluştur**’u seçin.

## <a name="register-identity-experience-framework-applications"></a>Kimlik deneyimi çerçevesi uygulamalarını kaydetme

Azure AD B2C, kullanıcılara kaydolmak ve oturum açmak için kullanılan iki uygulamayı kaydetmenizi gerektirir: IdentityExperienceFramework uygulamasından temsilci izni olan IdentityExperienceFramework (bir Web uygulaması) ve ProxyIdentityExperienceFramework (yerel bir uygulama). Yerel hesaplar yalnızca kiracınızda bulunur. Kullanıcılarınız, kiracıya kayıtlı uygulamalarınıza erişmek için benzersiz bir e-posta adresi/parola birleşimine kaydoldu.

### <a name="register-the-identityexperienceframework-application"></a>IdentityExperienceFramework uygulamasını kaydetme

1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin.
1. Arama kutusuna `Azure Active Directory` yazın.
1. Arama sonuçlarında **Azure Active Directory** ' yi seçin.
1. Sol taraftaki menüde **Yönet** ' in altında **uygulama kayıtları (eski)** seçeneğini belirleyin.
1. **Yeni uygulama kaydı**’nı seçin.
1. **Ad**için girin `IdentityExperienceFramework`.
1. **Uygulama türü**için **Web uygulaması/API**' yi seçin.
1. **Oturum açma URL 'si**için girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`, burada `your-tenant-name` Azure AD B2C kiracı etki alanı adıdır. Tüm URL 'Ler artık [b2clogin.com](b2clogin.md)kullanıyor olmalıdır.
1. **Oluştur**’u seçin. Oluşturulduktan sonra uygulama KIMLIĞINI kopyalayın ve daha sonra kullanmak üzere kaydedin.

### <a name="register-the-proxyidentityexperienceframework-application"></a>ProxyIdentityExperienceFramework uygulamasını kaydetme

1. **Uygulama kayıtları (eski)** bölümünde **Yeni uygulama kaydı**' nı seçin.
1. **Ad**için girin `ProxyIdentityExperienceFramework`.
1. **Uygulama türü**için **Yerel**' i seçin.
1. **Yeniden yönlendirme URI 'si**için `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`, Azure AD B2C `your-tenant-name` kiracınız yazın.
1. **Oluştur**’u seçin. Oluşturulduktan sonra uygulama KIMLIĞINI kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. **Ayarlar**' ı ve ardından **gerekli izinler**' i seçin ve ardından **Ekle**' yi seçin.
1. **BIR API seçin**' i seçin, **IdentityExperienceFramework**' yi arayıp seçin ve ardından **Seç**' e tıklayın.
1. **Erişim IdentityExperienceFramework**' nın yanındaki onay kutusunu Işaretleyin, **Seç**' e ve ardından **bitti**' ye tıklayın.
1. **Izin ver**' i seçin ve ardından **Evet**' i seçerek onaylayın.

## <a name="custom-policy-starter-pack"></a>Özel ilke başlangıç paketi

Özel ilkeler, teknik profiller ve Kullanıcı ilerliklerini tanımlamak için Azure AD B2C kiracınıza yüklediğiniz XML dosyaları kümesidir. Hızlı bir şekilde çalışmaya başlamanızı sağlamak için önceden oluşturulmuş birkaç ilke ile başlangıç paketleri sunuyoruz. Bu başlangıç paketlerinin her biri, açıklanan senaryolara ulaşmak için gereken en az teknik profil ve Kullanıcı bağlantısı sayısını içerir:

- **LocalAccounts** -yalnızca yerel hesapların kullanılmasına izin verir.
- **Socialaccounts** -yalnızca sosyal (veya federal) hesapların kullanılmasını mümkün.
- **SocialAndLocalAccounts** -hem yerel hem de sosyal hesapların kullanımını mümkün.
- **SocialAndLocalAccountsWithMFA** -sosyal, yerel ve Multi-Factor Authentication seçeneklerini sunar.

Her bir başlatıcı paketi şunları içerir:

- **Temel dosya** -temel için birkaç değişiklik yapılması gerekir. Örnek: *TrustFrameworkBase. xml*
- **Uzantı dosyası** -bu dosya, çoğu yapılandırma değişikliğini yapılmıştır. Örnek: *TrustFrameworkExtensions. xml*
- **Bağlı olan taraf dosyaları** -uygulamanız tarafından çağrılan göreve özel dosyalar. Örnekler: *Signuporsignın. xml*, *profileedit. xml*, *passwordreset. xml*

Bu makalede, **SocialAndLocalAccounts** Starter paketindeki XML özel ilke dosyalarını düzenlersiniz. Bir XML düzenleyiciye ihtiyacınız varsa, hafif bir platformlar arası düzenleyici [Visual Studio Code](https://code.visualstudio.com/download)deneyin.

### <a name="get-the-starter-pack"></a>Başlangıç paketini al

GitHub 'dan özel ilke başlangıç paketlerini alın, sonra SocialAndLocalAccounts Starter paketindeki XML dosyalarını Azure AD B2C kiracı adınızla güncelleştirin.

1. [. Zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) veya depoyu kopyalayın:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

1. **SocialAndLocalAccounts** dizinindeki tüm dosyalar içinde, dizeyi `yourtenant` Azure AD B2C kiracınızın adıyla değiştirin.

    Örneğin, B2C kiracınızın adı *contosotenant*ise, tüm örnekleri `yourtenant.onmicrosoft.com` olur. `contosotenant.onmicrosoft.com`

### <a name="add-application-ids-to-the-custom-policy"></a>Özel ilkeye uygulama kimlikleri ekleme

Uygulama kimliklerini *TrustFrameworkExtensions. xml*uzantı dosyasına ekleyin.

1. Açın `SocialAndLocalAccounts/` **ve`TrustFrameworkExtensions.xml`** öğesini`<TechnicalProfile Id="login-NonInteractive">`bulun.
1. Her iki örneğini `IdentityExperienceFrameworkAppId` , daha önce oluşturduğunuz IdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.
1. Her iki örneğini `ProxyIdentityExperienceFrameworkAppId` , daha önce oluşturduğunuz ProxyIdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.
1. Dosyayı kaydedin.

## <a name="upload-the-policies"></a>İlkeleri karşıya yükle

1. Azure portal B2C kiracınızda **kimlik deneyimi çerçevesi** menü öğesini seçin.
1. **Özel Ilkeyi karşıya yükle**' yi seçin.
1. Bu sırada, ilke dosyalarını karşıya yükleyin:
    1. *TrustFrameworkBase. xml*
    1. *TrustFrameworkExtensions. xml*
    1. *Signuporsignın. xml*
    1. *ProfileEdit. xml*
    1. *PasswordReset. xml*

Dosyaları karşıya yüklerken, Azure her birine ön ek `B2C_1A_` ekler.

> [!TIP]
> XML düzenleyiciniz doğrulamayı destekliyorsa, dosyaları başlangıç paketinin kök dizininde bulunan `TrustFrameworkPolicy_0.3.0.0.xsd` XML şemasına göre doğrulayın. XML şeması doğrulaması karşıya yüklemeden önce hataları tanımlar.

## <a name="test-the-custom-policy"></a>Özel ilkeyi test etme

1. **Özel ilkeler**altında **B2C_1A_signup_signin**öğesini seçin.
1. Özel ilkenin genel bakış sayfasında **Uygulama Seç** için, daha önce kaydettiğiniz *WebApp1* adlı Web uygulamasını seçin.
1. **Yanıt URL 'sinin** `https://jwt.ms`olduğundan emin olun.
1. **Şimdi Çalıştır**' ı seçin.
1. Bir e-posta adresi kullanarak kaydolun.
1. **Şimdi Çalıştır** ' ı seçin.
1. Doğru yapılandırmaya sahip olmadığınızı onaylamak için aynı hesapla oturum açın.

## <a name="add-facebook-as-an-identity-provider"></a>Bir kimlik sağlayıcısı olarak Facebook ekleme

1. Dosyasında,değeriniFacebookuygulamakimliğiyledeğiştirin:`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** `client_id`

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

1. *TrustFrameworkExtensions. xml* dosyasını kiracınıza yükleyin.
1. **Özel ilkeler**altında **B2C_1A_signup_signin**öğesini seçin.
1. **Şimdi Çalıştır** ' ı seçin ve Facebook ile oturum açmak ve özel ilkeyi test etmek için Facebook ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) eklemeyi deneyin. Bu başlangıç kılavuzunda kullanılan temel dosya, Azure AD gibi diğer kimlik sağlayıcılarını eklemek için gereken bazı içerikleri zaten içeriyor.

Azure AD 'yi ve kimlik sağlayıcısını ayarlama hakkında daha fazla bilgi için bkz. [Active Directory B2C özel ilkeler kullanarak kayıt ayarlama ve Azure Active Directory hesabıyla oturum açma](active-directory-b2c-setup-aad-custom.md).
