---
title: Özel ilkeleri kullanmaya başlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkelere nasıl başlayacaksınız öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 856bd6c2a3546a438293e89a0b576e1392d9c6a5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81407287"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkelerle başlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Özel ilkeler,](custom-policy-overview.md) Azure Etkin Dizin B2C (Azure AD B2C) kiracınızın davranışını tanımlayan yapılandırma dosyalarıdır. Bu makalede, bir e-posta adresi ve parola kullanarak yerel hesap kaydolma veya kaydolma destekleyen özel bir ilke oluşturursunuz. Ayrıca, çevrenizi kimlik sağlayıcılar eklemek için de hazırlarsınız.

## <a name="prerequisites"></a>Ön koşullar

- Zaten bir hesabınız yoksa, Azure aboneliğinize bağlı [bir Azure AD B2C kiracıoluşturun.](tutorial-create-tenant.md)
- Azure AD B2C ile iletişim kurabilmek için [uygulamanızı oluşturduğunuz](tutorial-register-applications.md) kiracıya kaydedin.
- Bir Facebook uygulamasını yapılandırmak için [bir Facebook hesabıyla kaydolma ve kaydolma](identity-provider-facebook.md) adımlarını tamamlayın. Bir Facebook uygulaması özel ilkeler kullanmak için gerekli olmasa da, bu izinde, özel bir ilkede sosyal girişi etkinleştirme sağlamak için kullanılır.

## <a name="add-signing-and-encryption-keys"></a>İmzalama ve şifreleme anahtarları ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. Genel bakış sayfasında, **İlkeler**altında **Kimlik Deneyimi Çerçevesi'ni**seçin.

### <a name="create-the-signing-key"></a>İmza anahtarını oluşturma

1. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Generate`, seçin.
1. **Ad**, `TokenSigningKeyContainer`girin . Önek `B2C_1A_` otomatik olarak eklenebilir.
1. **Anahtar türü için** **RSA'yı**seçin.
1. **Anahtar kullanımı**için **İmza'yı**seçin.
1. **Oluştur**’u seçin.

### <a name="create-the-encryption-key"></a>Şifreleme anahtarını oluşturma

1. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Generate`, seçin.
1. **Ad**, `TokenEncryptionKeyContainer`girin . Önek `B2C_1A`_ otomatik olarak eklenebilir.
1. **Anahtar türü için** **RSA'yı**seçin.
1. **Anahtar kullanımı**için **Şifreleme'yi**seçin.
1. **Oluştur**’u seçin.

### <a name="create-the-facebook-key"></a>Facebook anahtarını oluşturma

Facebook uygulamanızın [App Secret'ını](identity-provider-facebook.md) bir politika anahtarı olarak ekleyin. Bu makalenin ön koşullarının bir parçası olarak oluşturduğunuz uygulamanın App Secret'ını kullanabilirsiniz.

1. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Manual`, seçin.
1. **Ad**için `FacebookSecret`, girin . Önek `B2C_1A_` otomatik olarak eklenebilir.
1. **Gizli**olarak, developers.facebook.com Facebook uygulamanızın *App Secret* girin. Bu değer gizlidir, uygulama kimliği değil.
1. **Anahtar kullanımı**için **İmza'yı**seçin.
1. **Oluştur**’u seçin.

## <a name="register-identity-experience-framework-applications"></a>Kayıt Kimlik Deneyimi Çerçeve uygulamaları

Azure AD B2C, yerel hesaplara sahip kullanıcılara kaydolmak ve oturum açmak için kullandığı iki uygulamayı kaydetmenizi gerektirir: *IdentityExperienceFramework*, bir web API ve *ProxyIdentityExperienceFramework*, IdentityExperienceFramework uygulamasına yetki yle yetki yle gelen yerel bir uygulama. Kullanıcılarınız, kiracıya kayıtlı uygulamalarınıza erişmek için bir e-posta adresi veya kullanıcı adı ve bir parola ile kaydolabilir ve bu da bir "yerel hesap" oluşturur. Yerel hesaplar yalnızca Azure AD B2C kiracınızda bulunur.

Bu iki uygulamayı Azure AD B2C kiracınızda yalnızca bir kez kaydetmeniz gerekir.

### <a name="register-the-identityexperienceframework-application"></a>IdentityExperienceFramework uygulamasını kaydedin

Azure AD B2C kiracınızda bir uygulama kaydettirmek için **Uygulama kayıtlarını (Eski)** deneyimimizi veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
1. Azure **Etkin Dizin** genel bakış menüsünde, **Yönet** **altında, Uygulama kayıtlarını (Eski)** seçin.
1. **Yeni uygulama kaydı**’nı seçin.
1. **Ad**için `IdentityExperienceFramework`, girin .
1. **Uygulama türü için**Web **uygulaması/API'yi**seçin.
1. **Oturum Açma URL'si için,** `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`Azure AD B2C kiracı etki alanı adınız nerede `your-tenant-name` dir. Tüm URL'ler artık [b2clogin.com](b2clogin.md)kullanmalıdır.
1. **Oluştur**’u seçin. Oluşturulduktan sonra uygulama kimliğini kopyalayın ve daha sonra kullanmak üzere kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. **Ad**için `IdentityExperienceFramework`, girin .
1. **Desteklenen hesap türleri**altında, yalnızca bu kuruluş **dizinindeki Hesapları**seçin.
1. **Uri'yi Yeniden Yönlendirme**altında **Web'i**seçin ve Azure `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`AD B2C kiracı etki alanı adınız nerede' `your-tenant-name` girin.
1. **İzinler**altında, *openid ve offline_access izinleri* onay kutusunu açmak için Grant yöneticisi onayını seçin.
1. **Kaydol**’u seçin.
1. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

Ardından, bir kapsam ekleyerek API'yi açığa çıkar:

1. **Yönet**altında, **API'yi ortaya çıkar'ı**seçin.
1. **Kapsam ekle'yi**seçin, ardından **Kaydet'i** seçin ve varsayılan uygulama kimliği URI'yi kabul etmeye devam edin.
1. Azure AD B2C kiracınızda özel ilke yürütmesine izin veren bir kapsam oluşturmak için aşağıdaki değerleri girin:
    * **Kapsam adı**:`user_impersonation`
    * **Admin onay ekran adı**:`Access IdentityExperienceFramework`
    * **Admin rıza açıklaması**:`Allow the application to access IdentityExperienceFramework on behalf of the signed-in user.`
1. **Kapsam Ekle'yi** seçin

* * *

### <a name="register-the-proxyidentityexperienceframework-application"></a>ProxyIdentityExperienceFramework uygulamasını kaydedin

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Uygulama kayıtlarında (Eski)** **Yeni uygulama kaydını**seçin.
1. **Ad**için `ProxyIdentityExperienceFramework`, girin .
1. **Uygulama türü için,** **Yerel'i**seçin.
1. **URI'yi Yönlendirmek** `myapp://auth`için girin.
1. **Oluştur**’u seçin. Oluşturulduktan sonra uygulama kimliğini kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. **Ayarlar'ı**seçin, ardından **Gerekli İzinleri**seçin ve sonra **Ekle'yi**seçin.
1. **Bir API seçin,** arama yapın ve **IdentityExperienceFramework'i**seçin ve ardından **Seç'i**tıklatın.
1. **Access IdentityExperienceFramework'ün**yanındaki onay kutusunu seçin , **Seç'i**tıklatın ve ardından **Bitti'yi**tıklatın.
1. **Hibe izinlerini**seçin ve ardından **Evet'i**seçerek onaylayın.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. **Ad**için `ProxyIdentityExperienceFramework`, girin .
1. **Desteklenen hesap türleri**altında, yalnızca bu kuruluş **dizinindeki Hesapları**seçin.
1. **Redirect URI**altında, **Açılan kullanıcı/yerel (mobil & masaüstü)** seçmek için açılır aşağı kullanın.
1. **URI'yi Yönlendirmek** `myapp://auth`için girin.
1. **İzinler**altında, *openid ve offline_access izinleri* onay kutusunu açmak için Grant yöneticisi onayını seçin.
1. **Kaydol**’u seçin.
1. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

Ardından, uygulamanın ortak istemci olarak ele alınması gerektiğini belirtin:

1. **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. **Yeni deneyimi deneyin (gösteriliyorsa)** seçin.
1. **Gelişmiş ayarlar**altında, uygulamayı ortak istemci **olarak ele'i** etkinleştirin **(Evet'i**seçin).
1. **Kaydet**’i seçin.

Şimdi, *IdentityExperienceFramework* kaydında daha önce açığa çıkardığınız API kapsamına izin ver:

1. **Yönet**altında, **API izinlerini**seçin.
1. **Yapılandırılmış izinler**altında, **İzin ekle'yi**seçin.
1. **API'lerim** sekmesini seçin ve ardından **IdentityExperienceFramework** uygulamasını seçin.
1. **İzin**altında, daha önce tanımladığınız **user_impersonation** kapsamını seçin.
1. **İzin Ekle'yi**seçin. Yönlendirildikçe, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **(kiracı adınız) için Grant yönetici onayı**seçin.
1. Şu anda oturum açmış yönetici hesabınızı seçin veya En azından *Bulut uygulama yöneticisi* rolüne atanmış Olan Azure AD B2C kiracınızda bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile'yi**seçin ve ardından "Verilenler..." her iki kapsam için **durum** altında görüntülenir. İzinlerin yayılması birkaç dakika sürebilir.

* * *

## <a name="custom-policy-starter-pack"></a>Özel ilke başlangıç paketi

Özel ilkeler, teknik profilleri ve kullanıcı yolculuklarını tanımlamak için Azure AD B2C kiracınıza yüklediğiniz XML dosyaları kümesidir. Hızlı bir şekilde devam etmek için önceden oluşturulmuş birkaç ilke ile başlangıç paketleri salıyoruz. Bu başlangıç paketlerinin her biri, açıklanan senaryoları gerçekleştirmek için gereken en az sayıda teknik profil ve kullanıcı yolculuğu içerir:

- **LocalAccounts** - Yalnızca yerel hesapların kullanımını sağlar.
- **SocialAccounts** - Yalnızca sosyal (veya federe) hesapların kullanımını sağlar.
- **SocialAndLocalAccounts** - Hem yerel hem de sosyal hesapların kullanımını sağlar.
- **SocialAndLocalAccountsWithMFA** - Sosyal, yerel ve çok faktörlü kimlik doğrulama seçeneklerini sağlar.

Her başlangıç paketi şunları içerir:

- **Temel dosya** - Temelde birkaç değişiklik gereklidir. Örnek: *TrustFrameworkBase.xml*
- **Uzantı dosyası** - Bu dosya, yapılandırma değişikliklerinin çoğunun yapıldığı dosyadır. Örnek: *TrustFrameworkExtensions.xml*
- **Parti dosyalarına güvenme** - Uygulamanız tarafından çağrılan göreve özel dosyalar. Örnekler: *SignUpOrSignin.xml*, *ProfileEdit.xml*, *PasswordReset.xml*

Bu makalede, **SocialAndLocalAccounts** başlangıç paketinde XML özel ilke dosyalarını edin. Bir XML düzenleyicisi gerekiyorsa, hafif bir çapraz platform düzenleyicisi olan [Visual Studio Code'u](https://code.visualstudio.com/download)deneyin.

### <a name="get-the-starter-pack"></a>Başlangıç paketini alın

GitHub'dan özel ilke başlangıç paketlerini alın ve Ardından SocialAndLocalAccounts başlangıç paketindeki XML dosyalarını Azure AD B2C kiracı adınız ile güncelleyin.

1. [.zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) veya depoyu klonla:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

1. **SocialAndLocalAccounts** dizinindeki tüm dosyalarda, dizeyi `yourtenant` Azure AD B2C kiracınızın adıyla değiştirin.

    Örneğin, B2C kiracınızın adı *contosotenant*ise, tüm `yourtenant.onmicrosoft.com` örnekleri `contosotenant.onmicrosoft.com`.

### <a name="add-application-ids-to-the-custom-policy"></a>Özel ilke ye uygulama adlarını ekleme

Eklentileri dosyasına uygulama disleri ekleyin *TrustFrameworkExtensions.xml*.

1. Açın `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** ve öğeyi `<TechnicalProfile Id="login-NonInteractive">`bulun.
1. Her iki örneği `IdentityExperienceFrameworkAppId` de daha önce oluşturduğunuz IdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.
1. Her iki örneği `ProxyIdentityExperienceFrameworkAppId` de daha önce oluşturduğunuz ProxyIdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.
1. Dosyayı kaydedin.

## <a name="upload-the-policies"></a>İlkeleri yükleme

1. Azure portalındaki B2C kiracınızdaki **Kimlik Deneyimi Çerçevesi** menü öğesini seçin.
1. **Özel politika yükle'yi**seçin.
1. Bu sırada, ilke dosyalarını yükleyin:
    1. *TrustFrameworkBase.xml*
    1. *TrustFrameworkExtensions.xml*
    1. *SignUpOrSignin.xml*
    1. *ProfileEdit.xml*
    1. *PasswordReset.xml*

Dosyaları yüklerken, Azure her birine `B2C_1A_` önek ekler.

> [!TIP]
> XML düzenleyiciniz doğrulamayı destekliyorsa, `TrustFrameworkPolicy_0.3.0.0.xsd` dosyaları başlangıç paketinin kök dizininde bulunan XML şemasına karşı doğrulayın. XML şema doğrulama yüklemeden önce hataları tanımlar.

## <a name="test-the-custom-policy"></a>Özel ilkeyi test edin

1. **Özel ilkeler**altında, **B2C_1A_signup_signin**seçin.
1. Özel politikanın genel bakış **sayfasındaki Uygulamayı Seç** için, daha önce kaydettiğiniz *webapp1* adlı web uygulamasını seçin.
1. **Yanıt URL'sinin** `https://jwt.ms`.
1. **Şimdi Çalıştır'ı**seçin.
1. Bir e-posta adresi kullanarak kaydolun.
1. **Şimdi Çalıştır'ı** yeniden seçin.
1. Doğru yapılandırmaya sahip olduğunuzu doğrulamak için aynı hesapla oturum açın.

## <a name="add-facebook-as-an-identity-provider"></a>Facebook'u kimlik sağlayıcısı olarak ekleme

[Önkoşullar](#prerequisites)belirtildiği gibi, Facebook özel politikalar kullanmak için gerekli *değildir,* ancak burada nasıl özel bir politika federe sosyal giriş etkinleştirebilirsiniz göstermek için kullanılır.

1. Dosyada, `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** Facebook uygulama `client_id` kimliği ile değerini değiştirin:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

1. *TrustFrameworkExtensions.xml* dosyasını kiracınıza yükleyin.
1. **Özel ilkeler**altında, **B2C_1A_signup_signin**seçin.
1. **Şimdi Çalıştır'ı** seçin ve Facebook ile oturum açıp özel politikayı test etmek için Facebook'u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, kimlik sağlayıcısı olarak Azure Active Directory'yi (Azure AD) eklemeyi deneyin. Bu başlangıç kılavuzunda kullanılan temel dosya, Azure AD gibi diğer kimlik sağlayıcılarını eklemek için gereksinim duyduğunuz içeriğin bazılarını zaten içerir.

Azure AD'yi ve kimlik sağlayıcısı olarak ayarlama hakkında bilgi için Active [Directory B2C özel ilkelerini kullanarak Bir Azure Etkin Dizin hesabıyla kaydolma ve oturum açma'ya](identity-provider-azure-ad-single-tenant-custom.md)bakın.
