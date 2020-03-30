---
title: 'Öğretici: Uygulamalarınıza kimlik sağlayıcılar ekleme'
titleSuffix: Azure AD B2C
description: Azure portalını kullanarak Azure Active Directory B2C'deki uygulamalarınıza kimlik sağlayıcılar eklemeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 07/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1f49061210ca8e3c106b0569f77a67d1f10757a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183525"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'de uygulamalarınız için kimlik sağlayıcılar ekleme

Uygulamalarınızda, kullanıcıların farklı kimlik sağlayıcılarla oturum açmalarını etkinleştirmek isteyebilirsiniz. Bir *kimlik sağlayıcısı,* uygulamalara kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini oluşturur, korur ve yönetir. Azure portalını kullanarak [kullanıcı akışlarınıza](user-flow-overview.md) Azure Active Directory B2C (Azure AD B2C) tarafından desteklenen kimlik sağlayıcıları ekleyebilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kimlik sağlayıcı uygulamaları oluşturma
> * Kimlik sağlayıcılarını kiracınıza ekleme
> * Kimlik sağlayıcılarını kullanıcı akışınıza ekleme

Uygulamalarınızda genellikle yalnızca bir kimlik sağlayıcısı kullanırsınız, ancak daha fazlasını ekleme seçeneğiniz vardır. Bu öğretici, uygulamanıza bir Azure AD kimlik sağlayıcısı ve Facebook kimlik sağlayıcısını nasıl ekleyeceğinizgösterilmektedir. Bu kimlik sağlayıcılarının her ikisini de uygulamanıza eklemek isteğe bağlıdır. Ayrıca [Amazon,](identity-provider-amazon.md) [GitHub,](identity-provider-github.md) [Google,](identity-provider-google.md) [LinkedIn,](identity-provider-linkedin.md) [Microsoft](identity-provider-microsoft-account.md)veya [Twitter](identity-provider-twitter.md)gibi diğer kimlik sağlayıcıları ekleyebilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Kullanıcıların uygulamanıza kaydolmasını ve oturum açmalarını sağlamak için [bir kullanıcı akışı oluşturun.](tutorial-create-user-flows.md)

## <a name="create-applications"></a>Uygulama oluşturma

Kimlik sağlayıcı uygulamaları, Azure AD B2C kiracınızla iletişimi etkinleştirmek için tanımlayıcıyı ve anahtarı sağlar. Öğreticinin bu bölümünde, bir Azure AD uygulaması ve kiracınıza kimlik sağlayıcılarını eklemek için tanımlayıcılar ve anahtarlar aldığınız bir Facebook uygulaması oluşturursunuz. Kimlik sağlayıcılardan yalnızca birini ekliyorsanız, yalnızca bu sağlayıcı için uygulama oluşturmanız gerekir.

### <a name="create-an-azure-active-directory-application"></a>Azure Etkin Dizin uygulaması oluşturma

Azure AD'deki kullanıcılar için oturum açmayı etkinleştirmek için, bir uygulamayı Azure AD kiracısına kaydetmeniz gerekir. Azure AD kiracı, Azure AD B2C kiracınızla aynı değildir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Yeni kayıt**seçin.
1. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
1. Bu kuruluş **dizinindeki Hesapların** seçimini yalnızca bu uygulama için kabul edin.
1. Yeniden **Yönlendirme**URI'si için **Web** değerini kabul edin ve Azure `your-B2C-tenant-name` AD B2C kiracınızın adını değiştirerek tüm küçük harflerle aşağıdaki URL'yi girin.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Örneğin, `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

    Tüm URL'ler artık [b2clogin.com](b2clogin.md)kullanmalıdır.

1. Daha sonra kullandığınız **Uygulama (istemci) kimliğini** kaydedin, **ardından Kaydol'u**seçin.
1. Uygulama menüsünde **Yönet** **altında, Sertifikalar & sırları**seçin, ardından Yeni istemci **gizli**seçin.
1. İstemci sırrı için bir **Açıklama** girin. Örneğin, `Azure AD B2C App Secret`.
1. Son kullanma süresini seçin. Bu uygulama için, **1 yıl içinde**seçim kabul edin.
1. **Ekle'yi**seçin, ardından daha sonraki bir adımda kullandığınız yeni istemci sırrının değerini kaydedin.

### <a name="create-a-facebook-application"></a>Facebook uygulaması oluşturma

Azure AD B2C'de bir Facebook hesabını kimlik sağlayıcısı olarak kullanmak için Facebook'ta bir uygulama oluşturmanız gerekir. Zaten bir Facebook hesabınız yoksa, 'den [https://www.facebook.com/](https://www.facebook.com/)alabilirsiniz.

1. Facebook hesap kimlik bilgilerinize sahip [geliştiriciler için Facebook'ta](https://developers.facebook.com/) oturum açın.
1. Bunu zaten yapmadıysanız, bir Facebook geliştiricisi olarak kaydolmanız gerekir. Bunu yapmak için sayfanın sağ üst köşesinde **Başlat'ı** seçin, Facebook'un politikalarını kabul edin ve kayıt adımlarını tamamlayın.
1. **Uygulamalarımı** Seçin ve ardından **Uygulama Oluştur'u oluşturun.**
1. Bir **Görüntü Adı** ve geçerli bir **Kişi E-postası**girin.
1. **Uygulama Kimliği Oluştur'u**tıklatın. Bu, Facebook platform politikalarını kabul etmenizi ve çevrimiçi güvenlik denetimini tamamlamanızı gerektirebilir.
1. **Ayarlar** > **Temel'i**seçin.
1. Bir **Kategori**seçin `Business and Pages`, örneğin . Bu değer Facebook tarafından gereklidir, ancak Azure AD B2C tarafından kullanılmaz.
1. Sayfanın alt kısmında **Platform Ekle'yi**seçin ve ardından **Web Sitesi'ni**seçin.
1. Site URL'sinde, `your-tenant-name` kiracınızın adıyla **Site URL** `https://your-tenant-name.b2clogin.com/` değiştirin.
1. `http://www.contoso.com/`Örneğin, Gizlilik **Politikası URL'si**için bir URL girin. Gizlilik ilkesi URL'si, uygulamanız için gizlilik bilgileri sağlamak için koruduğunuz bir sayfadır.
1. **Değişiklikleri Kaydet'i**seçin.
1. Sayfanın üst kısmında, **App ID**değerini kaydedin.
1. App **Secret'ın**yanında **Göster'i** seçin ve değerini kaydedin. Facebook'u kiracınızda kimlik sağlayıcısı olarak yapılandırmak için hem App Id'yi hem de App Secret'ı kullanırsınız. **App Secret,** güvenli bir şekilde saklamanız gereken önemli bir güvenlik kimlik bilgisidir.
1. **ÜRÜNLERİn**yanındaki artı işaretini seçin, ardından **Facebook Giriş'in**altında , **Ayarla'yı**seçin.
1. Sol menüdeki **Facebook Giriş'in** altında **Ayarlar'ı**seçin.
1. **Geçerli OAuth yönlendirme URI,** girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Kiracınızın adıyla değiştirin. `your-tenant-name` Sayfanın altındaki **Değişiklikleri Kaydet'i** seçin.
1. Facebook uygulamanızı Azure AD B2C'de kullanılabilir hale getirmek için sayfanın sağ üst kısmındaki **Durum** seçicisini tıklatın ve Uygulamayı herkese açık hale getirmek için **açın** ve sonra **Onayla'yı**tıklatın. Bu noktada, Durum **Geliştirme'den** **Canlı'ya**değişmeli.

## <a name="add-the-identity-providers"></a>Kimlik sağlayıcılarını ekleme

Eklemek istediğiniz kimlik sağlayıcısı için uygulama oluşturduktan sonra, kimlik sağlayıcınızı kiracınıza eklersiniz.

### <a name="add-the-azure-active-directory-identity-provider"></a>Azure Etkin Dizin kimlik sağlayıcısıekleme

1. Azure AD B2C kiracıiçeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **Kimlik sağlayıcılarını**seçin ve ardından **Yeni OpenID Bağlantı sağlayıcısını**seçin.
1. Bir **Ad**girin. Örneğin, *Contoso Azure AD'yi*girin.
1. **Metadata url'si için,** `your-AD-tenant-domain` Azure AD kiracınızın alan adı ile değiştirerek aşağıdaki URL'yi girin:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Örneğin, `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

1. **İstemci Kimliği**için, daha önce kaydettiğiniz başvuru kimliğini girin.
1. **İstemci sırrı**için, daha önce kaydettiğiniz istemci sırrını girin.
1. **Kapsam,** **Yanıt türü**ve Yanıt **modu**için varsayılan değerleri bırakın.
1. (İsteğe bağlı) **Domain_hint**için bir değer girin. Örneğin, *ContosoAD*. [Etki alanı ipuçları,](../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) bir uygulamadan gelen kimlik doğrulama isteğine dahil edilen yönergelerdir. Kullanıcıyı federe IdP oturum açma sayfasına hızlandırmak için kullanılabilirler. Veya çok kiracılı bir uygulama tarafından, kullanıcıyı doğrudan kiracıları için markalı Azure AD oturum açma sayfasına hızlandırmak için kullanılabilirler.
1. **Kimlik sağlayıcı talepleri eşleme**altında, aşağıdaki iddiaları eşleme değerleri girin:

    * **Kullanıcı Kimliği**: *oid*
    * **Görüntü adı**: *ad*
    * **Ad soyad**: *given_name*
    * **Ad soyad**: *family_name*
    * **E-posta**: *unique_name*

1. **Kaydet'i**seçin.

### <a name="add-the-facebook-identity-provider"></a>Facebook kimlik sağlayıcısıekleme

1. **Kimlik sağlayıcılarını**seçin, ardından **Facebook'u**seçin.
1. Bir **Ad**girin. Örneğin, *Facebook*.
1. **İstemci Kimliği**için, daha önce oluşturduğunuz Facebook uygulamasının Uygulama Kimliğini girin.
1. Müşteri **sırrı**için, kaydettiğiniz App Secret'ı girin.
1. **Kaydet'i**seçin.

## <a name="update-the-user-flow"></a>Kullanıcı akışını güncelleştirme

Ön koşulların bir parçası olarak tamamladığınız öğreticide, *B2C_1_signupsignin1*adlı kaydolma ve kaydolmak için bir kullanıcı akışı oluşturdunuz. Bu bölümde, kimlik sağlayıcılarını *B2C_1_signupsignin1* kullanıcı akışına eklersiniz.

1. **Kullanıcı akışlarını (ilkeler)** seçin ve ardından *B2C_1_signupsignin1* kullanıcı akışını seçin.
2. **Kimlik sağlayıcılarını**seçin, eklediğiniz **Facebook** ve **Contoso Azure AD** kimlik sağlayıcılarını seçin.
3. **Kaydet'i**seçin.

## <a name="test-the-user-flow"></a>Kullanıcı akışını test edin

1. Oluşturduğunuz kullanıcı akışının Genel Bakış **sayfasında, Kullanıcı akışını çalıştır'ı**seçin.
1. **Uygulama**için, daha önce kaydolduğunuz *webapp1* adlı web uygulamasını seçin. **Yanıt URL'si** gösterilmelidir. `https://jwt.ms`
1. **Kullanıcı akışını çalıştır'ı**seçin ve ardından daha önce eklediğiniz bir kimlik sağlayıcısıyla oturum açın.
1. Eklediğiniz diğer kimlik sağlayıcıları için 1'den 3'e kadar olan adımları yineleyin.

Oturum açma işlemi başarılı olursa, aşağıdakilere benzer `https://jwt.ms` şekilde Kodlanmış Belirteci'ni görüntüleyenlere yönlendirilirsiniz:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "<key-ID>"
}.{
  "exp": 1562346892,
  "nbf": 1562343292,
  "ver": "1.0",
  "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
  "sub": "20000000-0000-0000-0000-000000000000",
  "aud": "30000000-0000-0000-0000-000000000000",
  "nonce": "defaultNonce",
  "iat": 1562343292,
  "auth_time": 1562343292,
  "name": "User Name",
  "idp": "facebook.com",
  "postalCode": "12345",
  "tfp": "B2C_1_signupsignin1"
}.[Signature]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl öğrendim:

> [!div class="checklist"]
> * Kimlik sağlayıcı uygulamaları oluşturma
> * Kimlik sağlayıcılarını kiracınıza ekleme
> * Kimlik sağlayıcılarını kullanıcı akışınıza ekleme

Ardından, uygulamalarınızda kimlik deneyimlerinin bir parçası olarak kullanıcılara gösterilen sayfaların Kullanıcı Birliğini nasıl özelleştireceğimize öğrenin:

> [!div class="nextstepaction"]
> [Azure Active Directory B2C'de uygulamalarınızın kullanıcı arabirimini özelleştirin](tutorial-customize-ui.md)
