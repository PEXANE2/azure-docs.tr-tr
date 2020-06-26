---
title: Özel ilkeleri kullanmaya başlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C özel ilkeleri kullanmaya nasıl başlaleyeceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/28/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 892e94ba1943b667ffeba63a80f4409b35ea5ec3
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85389301"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanmaya başlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Özel ilkeler](custom-policy-overview.md) , Azure Active Directory B2C (Azure AD B2C) kiracınızın davranışını tanımlayan yapılandırma dosyalarıdır. Bu makalede, bir e-posta adresi ve parola kullanarak yerel hesap kaydolma veya oturum açma 'yı destekleyen özel bir ilke oluşturacaksınız. Ayrıca, ortamınızı kimlik sağlayıcıları eklemek için hazırlarsınız.

## <a name="prerequisites"></a>Ön koşullar

- Henüz bir tane yoksa, Azure aboneliğinize bağlı [bir Azure AD B2C kiracı oluşturun](tutorial-create-tenant.md) .
- [Uygulamanızı](tutorial-register-applications.md) , Azure AD B2C ile iletişim kurabilmesi için oluşturduğunuz kiracıya kaydedin.
- Facebook uygulamasını yapılandırmak için [kayıt ayarlama ve Facebook hesabıyla oturum açma](identity-provider-facebook.md) adımlarını izleyin. Özel ilkeleri kullanmak için Facebook uygulaması gerekli olmasa da, özel bir ilkede sosyal oturum açmayı etkinleştirmeyi göstermek için bu izlenecek yolda kullanılır.

## <a name="add-signing-and-encryption-keys"></a>İmzalama ve şifreleme anahtarları Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. Genel Bakış sayfasında, **ilkeler**altında **kimlik deneyimi çerçevesi**' ni seçin.

### <a name="create-the-signing-key"></a>İmzalama anahtarını oluşturma

1. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Generate` .
1. **Ad**alanına girin `TokenSigningKeyContainer` . Ön ek `B2C_1A_` otomatik olarak eklenebilir.
1. **Anahtar türü**için **RSA**' yı seçin.
1. **Anahtar kullanımı**için **imza**' yı seçin.
1. **Oluştur**'u seçin.

### <a name="create-the-encryption-key"></a>Şifreleme anahtarı oluşturma

1. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Generate` .
1. **Ad**alanına girin `TokenEncryptionKeyContainer` . _ Ön eki `B2C_1A` otomatik olarak eklenebilir.
1. **Anahtar türü**için **RSA**' yı seçin.
1. **Anahtar kullanımı**için **şifreleme**' yi seçin.
1. **Oluştur**'u seçin.

### <a name="create-the-facebook-key"></a>Facebook anahtarını oluşturma

Facebook uygulamanızın [uygulama gizli](identity-provider-facebook.md) anahtarını ilke anahtarı olarak ekleyin. Oluşturduğunuz uygulamanın uygulama gizli anahtarını, bu makalenin önkoşullarının bir parçası olarak kullanabilirsiniz.

1. **Ilke anahtarlarını** seçin ve ardından **Ekle**' yi seçin.
1. **Seçenekler**için öğesini seçin `Manual` .
1. **Ad**için girin `FacebookSecret` . Ön ek `B2C_1A_` otomatik olarak eklenebilir.
1. **Gizli**dizi bölümünde Facebook uygulamanızın *uygulama gizli* anahtarını Developers.facebook.com adresinden girin. Bu değer, uygulama KIMLIĞI değil gizli dizi.
1. **Anahtar kullanımı**için **imza**' yı seçin.
1. **Oluştur**'u seçin.

## <a name="register-identity-experience-framework-applications"></a>Kimlik deneyimi çerçevesi uygulamalarını kaydetme

Azure AD B2C, yerel hesaplarla kullanıcılara kaydolmak ve oturum açmak için kullandığı iki uygulamayı kaydetmenizi gerektirir: *IdentityExperienceFramework*, BIR Web API 'Si ve *ProxyIdentityExperienceFramework*, IdentityExperienceFramework uygulaması için temsilci izni olan yerel bir uygulama. Kullanıcılarınız bir e-posta adresi veya Kullanıcı adı ile kaydolabilir ve bir "yerel hesap" oluşturan kiracınızın kayıtlı uygulamalarına erişmek için bir parolayla kaydolabilirsiniz. Yerel hesaplar yalnızca Azure AD B2C kiracınızda bulunur.

Bu iki uygulamayı Azure AD B2C kiracınızda yalnızca bir kez kaydetmeniz gerekir.

### <a name="register-the-identityexperienceframework-application"></a>IdentityExperienceFramework uygulamasını kaydetme

Bir uygulamayı Azure AD B2C kiracınıza kaydetmek için **uygulama kayıtları** deneyimini kullanabilirsiniz.

1. **Uygulama kayıtları**öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. **Ad**için girin `IdentityExperienceFramework` .
1. **Desteklenen hesap türleri**altında **yalnızca bu kuruluş dizinindeki hesaplar**' ı seçin.
1. **Yeniden yönlendirme URI 'si**altında **Web**' i seçin ve ardından `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` , `your-tenant-name` Azure AD B2C kiracı etki alanı adınız yazın.
1. **İzinler**altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu seçin.
1. **Kaydol**’u seçin.
1. Daha sonraki bir adımda kullanmak üzere **uygulama (istemci) kimliğini** kaydedin.

Ardından, bir kapsam ekleyerek API 'YI kullanıma sunun:

1. **Yönet**altında **bir API 'yi kullanıma**sunma ' yı seçin.
1. **Kapsam Ekle**' yi seçin, ardından **Kaydet** ' i SEÇIN ve varsayılan uygulama kimliği URI 'sini kabul edin.
1. Azure AD B2C kiracınızda özel ilke yürütmeye izin veren bir kapsam oluşturmak için aşağıdaki değerleri girin:
    * **Kapsam adı**:`user_impersonation`
    * **Yönetici onayı görünen adı**:`Access IdentityExperienceFramework`
    * **Yönetici onay açıklaması**:`Allow the application to access IdentityExperienceFramework on behalf of the signed-in user.`
1. **Kapsam Ekle** ' yi seçin

* * *

### <a name="register-the-proxyidentityexperienceframework-application"></a>ProxyIdentityExperienceFramework uygulamasını kaydetme

1. **Uygulama kayıtları**öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. **Ad**için girin `ProxyIdentityExperienceFramework` .
1. **Desteklenen hesap türleri**altında **yalnızca bu kuruluş dizinindeki hesaplar**' ı seçin.
1. **Yeniden yönlendirme URI 'si**altında, **genel istemci/yerel ' i (mobil & Masaüstü)** seçmek için açılan eklentiyi kullanın.
1. **Yeniden yönlendirme URI 'si**için girin `myapp://auth` .
1. **İzinler**altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu seçin.
1. **Kaydol**’u seçin.
1. Daha sonraki bir adımda kullanmak üzere **uygulama (istemci) kimliğini** kaydedin.

Sonra, uygulamanın ortak istemci olarak değerlendirilip değerlendirilmeyeceğini belirtin:

1. **Yönet**altında **kimlik doğrulaması**' nı seçin.
1. **Gelişmiş ayarlar**altında, **uygulamayı ortak istemci olarak değerlendir** ' i etkinleştirin ( **Evet**' i seçin). Uygulama bildiriminde **"Allowpublicclient": true** değerinin ayarlandığından emin olun. 
1. **Kaydet**’i seçin.

Şimdi *IdentityExperienceFramework* kaydında daha önce sunulan API kapsamına izin verin:

1. **Yönet**altında **API izinleri**' ni seçin.
1. **Yapılandırılan izinler**altında **izin Ekle**' yi seçin.
1. **API 'Lerim** sekmesini seçin, sonra **IdentityExperienceFramework** uygulamasını seçin.
1. **İzin**altında, daha önce tanımladığınız **user_impersonation** kapsamını seçin.
1. **Izin Ekle**' yi seçin. Yönlendirildiğinden, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **Yönetici onayı ver ' i (kiracı adınız)** seçin.
1. Şu anda oturum açmış olan yönetici hesabınızı seçin veya Azure AD B2C kiracınızda, en azından *bulut uygulama Yöneticisi* rolüne atanan bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile**' yi seçin ve ardından "verilen..." öğesini doğrulayın offline_access, OpenID ve user_impersonation kapsamların **durumu** altında görüntülenir. İzinlerin yayılması birkaç dakika sürebilir.

* * *

## <a name="custom-policy-starter-pack"></a>Özel ilke başlangıç paketi

Özel ilkeler, teknik profiller ve Kullanıcı ilerliklerini tanımlamak için Azure AD B2C kiracınıza yüklediğiniz XML dosyaları kümesidir. Hızlı bir şekilde çalışmaya başlamanızı sağlamak için önceden oluşturulmuş birkaç ilke ile başlangıç paketleri sunuyoruz. Bu başlangıç paketlerinin her biri, açıklanan senaryolara ulaşmak için gereken en az teknik profil ve Kullanıcı bağlantısı sayısını içerir:

- **LocalAccounts** -yalnızca yerel hesapların kullanılmasına izin verir.
- **Socialaccounts** -yalnızca sosyal (veya federal) hesapların kullanılmasını mümkün.
- **SocialAndLocalAccounts** -hem yerel hem de sosyal hesapların kullanımını mümkün.
- **SocialAndLocalAccountsWithMFA** -sosyal, yerel ve Multi-Factor Authentication seçeneklerini sunar.

Her bir başlatıcı paketi şunları içerir:

- **Temel dosya** -temel için birkaç değişiklik yapılması gerekir. Örnek: *TrustFrameworkBase.xml*
- **Uzantı dosyası** -bu dosya, çoğu yapılandırma değişikliğini yapılmıştır. Örnek: *TrustFrameworkExtensions.xml*
- **Bağlı olan taraf dosyaları** -uygulamanız tarafından çağrılan göreve özel dosyalar. Örnekler: *SignUpOrSignin.xml*, *ProfileEdit.xml*, *PasswordReset.xml*

Bu makalede, **SocialAndLocalAccounts** Starter paketindeki XML özel ilke dosyalarını düzenlersiniz. Bir XML düzenleyiciye ihtiyacınız varsa, hafif bir platformlar arası düzenleyici [Visual Studio Code](https://code.visualstudio.com/download)deneyin.

### <a name="get-the-starter-pack"></a>Başlangıç paketini al

GitHub 'dan özel ilke başlangıç paketlerini alın, sonra SocialAndLocalAccounts Starter paketindeki XML dosyalarını Azure AD B2C kiracı adınızla güncelleştirin.

1. [. Zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) veya depoyu kopyalayın:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

1. **SocialAndLocalAccounts** dizinindeki tüm dosyalar içinde, dizeyi `yourtenant` Azure AD B2C kiracınızın adıyla değiştirin.

    Örneğin, B2C kiracınızın adı *contosotenant*ise, tüm örnekleri `yourtenant.onmicrosoft.com` olur `contosotenant.onmicrosoft.com` .

### <a name="add-application-ids-to-the-custom-policy"></a>Özel ilkeye uygulama kimlikleri ekleme

Uygulama kimliklerini uzantı dosyasına ekleyin *TrustFrameworkExtensions.xml*.

1. Açın `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** ve öğesini bulun `<TechnicalProfile Id="login-NonInteractive">` .
1. Her iki örneğini, `IdentityExperienceFrameworkAppId` daha önce oluşturduğunuz IdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.
1. Her iki örneğini, `ProxyIdentityExperienceFrameworkAppId` daha önce oluşturduğunuz ProxyIdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.
1. Dosyayı kaydedin.

## <a name="upload-the-policies"></a>İlkeleri karşıya yükle

1. Azure portal B2C kiracınızda **kimlik deneyimi çerçevesi** menü öğesini seçin.
1. **Özel Ilkeyi karşıya yükle**' yi seçin.
1. Bu sırada, ilke dosyalarını karşıya yükleyin:
    1. *TrustFrameworkBase.xml*
    1. *TrustFrameworkExtensions.xml*
    1. *SignUpOrSignin.xml*
    1. *ProfileEdit.xml*
    1. *PasswordReset.xml*

Dosyaları karşıya yüklerken, Azure her birine ön ek ekler `B2C_1A_` .

> [!TIP]
> XML düzenleyiciniz doğrulamayı destekliyorsa, dosyaları `TrustFrameworkPolicy_0.3.0.0.xsd` Başlangıç paketinin kök dizininde bulunan XML şemasına göre doğrulayın. XML şeması doğrulaması karşıya yüklemeden önce hataları tanımlar.

## <a name="test-the-custom-policy"></a>Özel ilkeyi test etme

1. **Özel ilkeler**altında **B2C_1A_signup_signin**' yi seçin.
1. Özel ilkenin genel bakış sayfasında **Uygulama Seç** için, daha önce kaydettiğiniz *WebApp1* adlı Web uygulamasını seçin.
1. **Yanıt URL 'sinin** olduğundan emin olun `https://jwt.ms` .
1. **Şimdi Çalıştır**' ı seçin.
1. Bir e-posta adresi kullanarak kaydolun.
1. **Şimdi Çalıştır** ' ı seçin.
1. Doğru yapılandırmaya sahip olmadığınızı onaylamak için aynı hesapla oturum açın.

## <a name="add-facebook-as-an-identity-provider"></a>Bir kimlik sağlayıcısı olarak Facebook ekleme

[Önkoşullar](#prerequisites)bölümünde belirtildiği gibi, Facebook özel ilkeleri kullanmak için gerekli *değildir* , ancak burada, Federasyon sosyal oturum açmayı özel bir ilkede nasıl etkinleştirebileceğinizi göstermek için kullanılır.

1. `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** Dosyasında, değerini `client_id` Facebook uygulama kimliğiyle değiştirin:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

1. *TrustFrameworkExtensions.xml* dosyasını kiracınıza yükleyin.
1. **Özel ilkeler**altında **B2C_1A_signup_signin**' yi seçin.
1. **Şimdi Çalıştır** ' ı seçin ve Facebook ile oturum açmak ve özel ilkeyi test etmek için Facebook ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) eklemeyi deneyin. Bu başlangıç kılavuzunda kullanılan temel dosya, Azure AD gibi diğer kimlik sağlayıcılarını eklemek için gereken bazı içerikleri zaten içeriyor.

Azure AD 'yi ve kimlik sağlayıcısını ayarlama hakkında daha fazla bilgi için bkz. [Active Directory B2C özel ilkeler kullanarak kayıt ayarlama ve Azure Active Directory hesabıyla oturum açma](identity-provider-azure-ad-single-tenant-custom.md).
