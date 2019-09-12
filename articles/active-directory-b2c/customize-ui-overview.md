---
title: Azure Active Directory B2C 'da Kullanıcı arabirimi özelleştirmesi hakkında | Microsoft Docs
description: Azure Active Directory B2C kullanan uygulamalarınız için Kullanıcı arabirimini nasıl özelleştireceğinizi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5ae12742c2ad50d5bf1caaf14ae2f6d34bd6d3a2
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70880793"
---
# <a name="about-user-interface-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'da Kullanıcı arabirimi özelleştirmesi hakkında

Azure Active Directory B2C (Azure AD B2C) uygulamalarına hizmet veren kullanıcı arabirimini (UI) markamanızı ve özelleştirmenizi sağlamak, müşterinize sorunsuz bir deneyim sunmak için önemlidir. Bu deneyimlere kaydolma, oturum açma, profil düzenlemesi ve parola sıfırlama dahildir. Bu makalede, uygulamalarınızın Kullanıcı arabirimini özelleştirmenize yardımcı olacak bilgiler sağlanmaktadır.

Bu deneyimlere geldiğinde gereksinimlerinize bağlı olarak, uygulamanızın kullanıcı arabirimini farklı yollarla özelleştirirsiniz. Örneğin:

- Uygulamanızda kaydolma veya oturum açma, parola sıfırlama veya profil düzenlemesi deneyimleri sağlamak için [Kullanıcı akışları](active-directory-b2c-reference-policies.md) kullanıyorsanız, Kullanıcı [arabirimini özelleştirmek için Azure Portal](tutorial-customize-ui.md)kullanın.
- V2 Kullanıcı akışı kullanıyorsanız, daha fazla özelleştirme yapmadan Kullanıcı akış sayfalarınızın görünümünü değiştirmek için bir [sayfa düzeni şablonu](#page-layout-templates) kullanabilirsiniz. Örneğin, Kullanıcı akışındaki tüm sayfalara bir okyanus mavi veya kurşun grisi teması uygulayabilirsiniz.
- Yalnızca oturum açma, buna eşlik eden parola sıfırlama sayfası ve doğrulama e-postaları sağladıysanız, bir [Azure AD oturum açma sayfasında](../active-directory/fundamentals/customize-branding.md)kullanılan aynı özelleştirme adımlarını kullanırsınız.
- Müşteriler oturum açmadan önce profilini düzenlemeye çalıştıklarında, Azure AD oturum açma sayfasını özelleştirmek için kullanılan adımların aynısını kullanarak özelleştirdiğiniz bir sayfaya yönlendirilir.
- Uygulamanızda kaydolma veya oturum açma, parola sıfırlama veya profil düzenlemesi sağlamak için [özel ilkeler](active-directory-b2c-overview-custom.md) kullanıyorsanız, [Kullanıcı arabirimini özelleştirmek için ilke dosyalarını](active-directory-b2c-ui-customization-custom.md)kullanırsınız.
- Bir müşterinin kararına bağlı olarak dinamik içerik sağlamanız gerekiyorsa, bir sorgu dizesinde gönderilen parametreye bağlı olarak [sayfa içeriğini değiştirebiliriz özel ilkeler](active-directory-b2c-ui-customization-custom-dynamic.md) kullanırsınız. Örneğin, Azure AD B2C kaydolma veya oturum açma sayfasındaki arka plan görüntüsü, Web veya mobil uygulamanızdan geçirdiğiniz parametreye göre değişir.
- Azure AD B2C [Kullanıcı akışlarınızda](user-flow-javascript-overview.md) JavaScript istemci tarafı kodunu etkinleştirebilir veya [özel ilkeleriniz](page-layout.md)olabilir.

Azure AD B2C, müşterinizin tarayıcısında kodu çalıştırır ve [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/)adlı modern bir yaklaşım kullanır. Çalışma zamanında, içerik, Kullanıcı akışında veya ilkesinde belirttiğiniz bir URL 'den yüklenir. Farklı sayfalar için farklı URL 'Ler belirtirsiniz. URL 'nizden içerik yüklendikten sonra, Azure AD B2C bir HTML parçası ile birleştirilir ve daha sonra müşterinize görüntülenir.

Kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyalarını kullanırken, başlamadan önce aşağıdaki kılavuzu gözden geçirin:

- Azure AD B2C HTML içeriğini sayfalarınıza birleştirir. Azure AD B2C sağladığı varsayılan içeriği kopyalamayın ve değiştirmeyi denemeyin. HTML içeriğinizi sıfırdan oluşturmak ve varsayılan içeriği başvuru olarak kullanmak en iyisidir.
- JavaScript artık özel içeriğinize dahil edilebilir.
- Desteklenen tarayıcı sürümleri şunlardır:
    - Internet Explorer 11, 10 ve Microsoft Edge
    - Internet Explorer 9 ve 8 için sınırlı destek
    - Google Chrome 42,0 ve üzeri
    - Mozilla Firefox 38,0 ve üzeri
- Azure AD B2C eklenen HTML tarafından oluşturulan POST işlemlerini kesintiye uğratan, HTML 'nize form etiketleri eklemeyin emin olun.

## <a name="page-layout-templates"></a>Sayfa düzeni şablonları

V2 Kullanıcı akışları için, varsayılan sayfalarınıza daha iyi bir bakış sağlayan ve kendi Özelleştirmenizde iyi bir şekilde hizmet veren önceden tasarlanmış bir şablon seçebilirsiniz.

Sol taraftaki menüde, **Özelleştir**altında **sayfa düzenleri**' ni seçin. Ardından **şablon (Önizleme)** öğesini seçin.

![Azure portal Kullanıcı akışı sayfasında şablon seçme açılan listesi](media/customize-ui-overview/template.png)

Listeden bir şablon seçin. Örneğin, **okyanus mavi** şablonu, Kullanıcı akış sayfalarınıza aşağıdaki yerleşimi uygular:

![Kaydolma oturum açma sayfasında işlenen okyanus mavi şablonu örneği](media/customize-ui-overview/ocean-blue.png)

Bir şablon seçtiğinizde, seçilen düzen Kullanıcı akışındaki tüm sayfalara uygulanır ve her sayfanın URI 'SI **özel sayfa URI 'si** alanında görünür.

## <a name="where-do-i-store-ui-content"></a>UI içeriğini nerede depolayabilirim?

Kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanırken, Kullanıcı arabirimi içeriğinizi [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md), Web sunucuları, CDNS, AWS S3 veya dosya paylaşım sistemleri gibi her yerde barındırabilirsiniz. Önemli nokta, içeriği CORS 'yi etkin olan genel kullanıma açık bir HTTPS uç noktası üzerinde barındırmanıza bağlıdır. İçeriğinizi belirttiğinizde, mutlak bir URL kullanmanız gerekir.

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?

Kullanıcı arabirimini özelleştirmek için aşağıdakileri yapın:

- İçinde herhangi bir yerde bulunan boş `<div id="api"></div>` bir öğeyle düzgün biçimlendirilmiş HTML içeriği oluşturun. `<body>` Bu öğe Azure AD B2C içeriğin nereye ekleneceğini işaretler. Aşağıdaki örnek, en az bir sayfa göstermektedir:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>!Add your title here!</title>
        <link rel="stylesheet" href="https://mystore1.blob.core.windows.net/b2c/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
      </body>
    </html>
    ```

- İçeriğinizi bir HTTPS uç noktasında (CORS ile izin verilir) barındırın. CORS yapılandırılırken hem GET hem de OPTIONS istek yöntemlerinin etkinleştirilmesi gerekir.
- Azure AD B2C, sayfanıza eklediği Kullanıcı Arabirimi öğelerine stil eklemek için CSS kullanın. Aşağıdaki örnek, kayıt eklenmiş HTML öğelerinin ayarlarını da içeren basit bir CSS dosyasını göstermektedir:

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center;
    }
    .entry {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center;
    }
    .create {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

- Oluşturduğunuz içeriği kullanmak için bir ilke oluşturun veya düzenleyin.

Aşağıdaki tabloda, Azure AD B2C içeriklerde bulunan `<div id="api"></div>` öğe ile birleştirmekte olan HTML parçaları listelenmektedir.

| Sayfa ekleniyor | HTML açıklaması |
| ------------- | ------------------- |
| Kimlik sağlayıcı seçimi | Müşterinin kaydolma veya oturum açma sırasında aralarından seçim yapabileceğiniz kimlik sağlayıcılarının düğmelerinin bir listesini içerir. Bu düğmeler Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını (e-posta adresine veya Kullanıcı adına göre) içerir. |
| Yerel hesap kaydı | Bir e-posta adresine veya Kullanıcı adına göre yerel hesap kaydolma için bir form içerir. Form, metin girişi kutusu, parola giriş kutusu, radyo düğmesi, tek seçim açılan kutuları ve çoklu seçim onay kutuları gibi farklı giriş denetimleri içerebilir. |
| Sosyal hesap kaydolma | Facebook veya Google gibi bir sosyal kimlik sağlayıcısından mevcut bir hesap kullanılarak kaydolurken görünebilir. Bir kaydolma formu kullanılarak müşteriden ek bilgiler toplandığında kullanılır. |
| Birleşik kaydolma veya oturum açma | Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını kullanabilecek müşterilerin hem kaydolma hem de oturum açma sürümlerini işler. |
| Multi-factor authentication | Müşteriler, kaydolma veya oturum açma sırasında telefon numaralarını (metin veya ses kullanarak) doğrulayabilirler. |
| Hata | Müşteriye hata bilgilerini sağlar. |


## <a name="how-do-i-localize-content"></a>Nasıl yaparım? içeriği yerelleştiresin mi?

Azure AD B2C kiracınızda [dil özelleştirmesini](active-directory-b2c-reference-language-customization.md) etkinleştirerek HTML içeriğinizi yerelleştirebilirsiniz. Bu özelliği etkinleştirmek Azure AD B2C, OpenID Connect parametresini `ui-locales` uç noktanıza iletmesine izin verir. İçerik sunucunuz, dile özgü HTML sayfaları sağlamak için bu parametreyi kullanabilir.

İçerik, kullanılan yerel ayara bağlı olarak farklı yerlerden çekeklenebilir. CORS etkin uç noktanıza, belirli diller için içerik barındırmak üzere bir klasör yapısı ayarlarsınız. {Culture: RFC5646} joker karakter değerini kullanırsanız, doğru olanı çağıracaksınız. Örneğin, özel sayfa URI 'niz gibi `https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html`görünebilir. Buradan içerik çekerek sayfayı Fransızca olarak yükleyebilirsiniz`https://contoso.blob.core.windows.net/fr/myHTML/unified.html`

## <a name="examples"></a>Örnekler

Özelleştirme örnekleri için bu [örnek şablon dosyalarını](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip)indirip gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- **Kullanıcı akışları**kullanıyorsanız, Kullanıcı arabiriminizi öğreticiyle özelleştirmeye başlayabilirsiniz:

    [Azure Active Directory B2C ' de uygulamalarınızın Kullanıcı arabirimini özelleştirin](tutorial-customize-ui.md).
- **Özel ilkeler**kullanıyorsanız, şu makaleyle Kullanıcı arabirimini özelleştirmeye başlayabilirsiniz:

    [Azure Active Directory B2C özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirin](active-directory-b2c-ui-customization-custom.md).
