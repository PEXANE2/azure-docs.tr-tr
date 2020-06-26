---
title: Kullanıcı arabirimini özelleştirme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanan uygulamalarınız için Kullanıcı arabirimini özelleştirmeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/04/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ebf83807629cc56aa381c97a9ce36d90c94d61f2
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85388910"
---
# <a name="customize-the-user-interface-in-azure-active-directory-b2c"></a>Azure Active Directory B2C Kullanıcı arabirimini özelleştirme

Azure Active Directory B2C (Azure AD B2C) müşterileriniz tarafından görüntülenen kullanıcı arabirimini markalamayı ve özelleştirmeyi, uygulamanızda sorunsuz bir kullanıcı deneyimi sağlamaya yardımcı olur. Bu deneyimlere kaydolma, oturum açma, profil düzenlemesi ve parola sıfırlama dahildir. Bu makalede hem Kullanıcı akışları hem de özel ilkeler için Kullanıcı arabirimi (UI) özelleştirmesi yöntemleri tanıtılmaktadır.

## <a name="ui-customization-in-different-scenarios"></a>Farklı senaryolarda UI özelleştirmesi

Uygulamanın kullanıcı ARABIRIMINI özelleştirmek için kullanabileceğiniz çeşitli yollar vardır. her biri farklı senaryolar için uygundur.

### <a name="user-flows"></a>Kullanıcı akışları

[Kullanıcı akışları](user-flow-overview.md)kullanıyorsanız, yerleşik *sayfa düzeni şablonlarını*kullanarak veya kendi HTML ve CSS 'nizi kullanarak Kullanıcı akış sayfalarınızın görünümünü değiştirebilirsiniz. Her iki yöntem de bu makalenin ilerleyen kısımlarında ele alınmıştır.

Kullanıcı akışları için UI özelleştirmesini yapılandırmak üzere [Azure Portal](tutorial-customize-ui.md) kullanırsınız.

> [!TIP]
> Kullanıcı akış sayfalarınızın yalnızca başlık logosunu, arka plan resmini ve arka plan rengini değiştirmek istiyorsanız, bu makalenin ilerleyen kısımlarında açıklanan [Şirket markası (Önizleme)](#company-branding-preview) özelliğini deneyebilirsiniz.

### <a name="custom-policies"></a>Özel ilkeler

Uygulamanızda kaydolma veya oturum açma, parola sıfırlama veya profil düzenlemesi sağlamak için [özel ilkeler](custom-policy-overview.md) kullanıyorsanız, [Kullanıcı arabirimini özelleştirmek için ilke dosyalarını](custom-policy-ui-customization.md)kullanın.

Bir müşterinin kararına göre dinamik içerik sağlamanız gerekiyorsa, bir sorgu dizesinde gönderilen parametreye bağlı olarak [sayfa içeriğini dinamik olarak değiştirecek](custom-policy-ui-customization.md#configure-dynamic-custom-page-content-uri) özel ilkeler kullanın. Örneğin, Azure AD B2C kaydolma veya oturum açma sayfasındaki arka plan görüntüsünü, Web veya mobil uygulamanızdan geçirdiğiniz bir parametreye göre değiştirebilirsiniz.

### <a name="javascript"></a>JavaScript

İstemci tarafı JavaScript kodunu, hem [Kullanıcı akışlarında](user-flow-javascript-overview.md) hem de [özel ilkelerde](page-layout.md)etkinleştirebilirsiniz.

### <a name="sign-in-only-ui-customization"></a>Yalnızca oturum açma kullanıcı arabirimi özelleştirmesi

Kendisine yönelik parola sıfırlama sayfası ve doğrulama e-postalarıyla birlikte yalnızca oturum açma sağlıyorsanız, bir [Azure AD oturum açma sayfasında](../active-directory/fundamentals/customize-branding.md)kullanılan özelleştirme adımlarını kullanın.

Müşteriler, oturum açmadan önce profilini düzenlemeye çalışırlarsa, Azure AD oturum açma sayfasını özelleştirmek için kullanılan adımların aynısını kullanarak özelleştirdiğiniz bir sayfaya yönlendirilir.

## <a name="page-layout-templates"></a>Sayfa düzeni şablonları

Kullanıcı akışları, Kullanıcı deneyimi sayfalarınıza profesyonel bir görünüm kazandırmak için içinden seçebileceğiniz çeşitli yerleşik şablonlar sağlar. Bu düzen şablonları Ayrıca kendi özelleştirmesi için başlangıç noktası olarak da kullanılabilir.

Sol menüdeki **Özelleştir** altında **sayfa düzenleri** ' ni seçin ve ardından **şablon**' u seçin.

![Azure portal Kullanıcı akışı sayfasında şablon seçme açılan listesi](media/customize-ui-overview/template-selection.png)

Sonra, listeden bir şablon seçin. Her şablon için oturum açma sayfalarının örnekleri aşağıda verilmiştir:

| Okyanus Mavisi | Kurşun Grisi | Klasik |
|:-:|:-:|:-:|
|![Kaydolma oturum açma sayfasında işlenen okyanus mavi şablonu örneği](media/customize-ui-overview/template-ocean-blue.png)|![Kaydolma oturum açma sayfasında oluşturulan Kurşun Grisi şablonu örneği](media/customize-ui-overview/template-slate-gray.png)|![Kaydolma oturum açma sayfasında işlenen klasik şablon örneği](media/customize-ui-overview/template-classic.png)|

Bir şablon seçtiğinizde, seçilen düzen Kullanıcı akışındaki tüm sayfalara uygulanır ve her sayfanın URI 'SI **özel sayfa URI 'si** alanında görünür.

## <a name="custom-html-and-css"></a>Özel HTML ve CSS

Özelleştirilmiş HTML ve CSS 'niz ile kendi ilke düzeninizi tasarlamak isterseniz, ilkenizde bulunan her Düzen adı için "özel sayfa içeriğini kullan" seçeneğini değiştirerek bunu yapabilirsiniz. Lütfen özel düzen yapılandırmalarına ilişkin aşağıdaki yönergeleri izleyin:

Azure AD B2C, [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/)adlı bir yaklaşım kullanarak müşterinizin tarayıcısında kodu çalıştırır.

Çalışma zamanında, içerik, Kullanıcı akışınız veya özel ilkenizde belirttiğiniz bir URL 'den yüklenir. Kullanıcı deneyimindeki her sayfa, içeriğini o sayfa için belirttiğiniz URL 'den yükler. URL 'nizden içerik yüklendikten sonra, Azure AD B2C tarafından yerleştirilen bir HTML parçası ile birleştirilir ve sayfa müşterinize görüntülenir.

Kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanmadan önce aşağıdaki kılavuzu gözden geçirin:

- Azure AD B2C HTML içeriğini sayfalarınıza **birleştirir** . Azure AD B2C sağladığı varsayılan içeriği kopyalamayın ve değiştirmeyi denemeyin. HTML içeriğinizi sıfırdan oluşturmak ve varsayılan içeriği başvuru olarak kullanmak en iyisidir.
- **JavaScript** , hem [Kullanıcı akışları](user-flow-javascript-overview.md) hem de [özel ilkeler](javascript-samples.md)için özel içeriğinize eklenebilir.
- Desteklenen **tarayıcı sürümleri** şunlardır:
  - Internet Explorer 11, 10 ve Microsoft Edge
  - Internet Explorer 9 ve 8 için sınırlı destek
  - Google Chrome 42,0 ve üzeri
  - Mozilla Firefox 38,0 ve üzeri
  - İOS ve macOS için Safari, sürüm 12 ve üzeri
- HTML 'nize **form etiketleri** eklemeyin. Form etiketleri, Azure AD B2C tarafından eklenen HTML tarafından oluşturulan POST işlemlerini kesintiye uğrayor.

### <a name="where-do-i-store-ui-content"></a>UI içeriğini nerede depolayabilirim?

Kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanırken, Kullanıcı arabirimi içeriğinizi CORS 'yi destekleyen genel kullanıma açık bir HTTPS uç noktası üzerinden barındırabilirsiniz. Örneğin, [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md), Web sunucuları, CDNS, AWS S3 veya dosya paylaşım sistemleri.

Önemli nokta, içeriği [CORS 'yi etkin](https://enable-cors.org/server.html)olan genel kullanıma açık bir HTTPS uç noktası üzerinde barındırmanıza bağlıdır. İçeriğinizi belirttiğinizde, mutlak bir URL kullanmanız gerekir.

> [!NOTE]
> HTML içeriği oluşturma, Azure Blob depolamaya içerik yükleme ve CORS 'yi yapılandırma hakkında ayrıntılı bilgi için UI Özelleştirme makalesindeki [özel sayfa içeriği](custom-policy-ui-customization.md#custom-page-content-walkthrough) Kılavuzu bölümüne bakın.

## <a name="get-started-with-custom-html-and-css"></a>Özel HTML ve CSS ile çalışmaya başlama

Bu yönergeleri izleyerek, Kullanıcı deneyimi sayfalarınızda kendi HTML ve CSS 'nizi kullanmaya başlayın.

- İçinde herhangi bir yerde bulunan boş bir öğeyle düzgün biçimlendirilmiş HTML içeriği oluşturun `<div id="api"></div>` `<body>` . Bu öğe Azure AD B2C içeriğin nereye ekleneceğini işaretler. Aşağıdaki örnek, en az bir sayfa göstermektedir:

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

- İçeriğinizi bir HTTPS uç noktasında (CORS ile izin verilir) barındırın. CORS yapılandırılırken hem GET hem de OPTIONS istek yöntemlerinin etkinleştirilmesi gerekir.
- Oluşturduğunuz içeriği kullanmak için bir Kullanıcı akışı veya özel ilke oluşturun veya düzenleyin.

### <a name="html-fragments-from-azure-ad-b2c"></a>Azure AD B2C HTML parçaları

Aşağıdaki tabloda, Azure AD B2C içeriklerde bulunan öğe ile birleştirmekte olan HTML parçaları listelenmektedir `<div id="api"></div>` .

| Sayfa ekleniyor | HTML açıklaması |
| ------------- | ------------------- |
| Kimlik sağlayıcı seçimi | Müşterinin kaydolma veya oturum açma sırasında aralarından seçim yapabileceğiniz kimlik sağlayıcılarının düğmelerinin bir listesini içerir. Bu düğmeler Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını (e-posta adresine veya Kullanıcı adına göre) içerir. |
| Yerel hesap kaydı | Bir e-posta adresine veya Kullanıcı adına göre yerel hesap kaydolma için bir form içerir. Form, metin girişi kutusu, parola giriş kutusu, radyo düğmesi, tek seçim açılan kutuları ve çoklu seçim onay kutuları gibi farklı giriş denetimleri içerebilir. |
| Sosyal hesap kaydolma | Facebook veya Google gibi bir sosyal kimlik sağlayıcısından mevcut bir hesap kullanılarak kaydolurken görünebilir. Bir kaydolma formu kullanılarak müşteriden ek bilgiler toplandığında kullanılır. |
| Birleşik kaydolma veya oturum açma | Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını kullanabilecek müşterilerin hem kaydolma hem de oturum açma sürümlerini işler. |
| Multi-factor authentication | Müşteriler, kaydolma veya oturum açma sırasında telefon numaralarını (metin veya ses kullanarak) doğrulayabilirler. |
| Hata | Müşteriye hata bilgilerini sağlar. |

## <a name="company-branding-preview"></a>Şirket markası (Önizleme)

Kullanıcı akış sayfalarınızı, Azure Active Directory [Şirket markalaması](../active-directory/fundamentals/customize-branding.md)kullanarak bir başlık logosu, arka plan resmi ve arka plan rengi ile özelleştirebilirsiniz.

Kullanıcı akış sayfalarınızı özelleştirmek için önce Azure Active Directory ' de şirket markasını yapılandırıp, ardından Kullanıcı akışlarınızın sayfa düzenleri Azure AD B2C ' nde etkinleştirin.

[!INCLUDE [preview note](../../includes/active-directory-b2c-public-preview.md)]

### <a name="configure-company-branding"></a>Şirket markası yapılandırma

**Şirket markalaması**içinde başlık logosunu, arka plan görüntüsünü ve arka plan rengini ayarlayarak başlayın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **Yönet**altında **Şirket markalaması**' nı seçin.
1. [Kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleme](../active-directory/fundamentals/customize-branding.md)bölümündeki adımları izleyin.

Şirket markasını Azure AD B2C ' de yapılandırırken şunları göz önünde bulundurun:

* Azure AD B2C Şirket markası şu anda **arka plan resmi**, **Başlık logosu**ve **arka plan rengi** özelleştirmesi ile sınırlıdır. Şirket markası bölmesindeki diğer özellikler, örneğin **Gelişmiş ayarlarda** *desteklenmez*.
* Kullanıcı akış sayfalarınızda arka plan rengi, arka plan görüntüsü yüklenmeden önce gösterilir. Daha yumuşak bir yükleme deneyimi için arka plan görüntinizdeki renklerle yakından eşleşen bir arka plan rengi seçmenizi öneririz.
* Ana Başlık logosu, bir kaydolma Kullanıcı akışı başlattıklarında kullanıcılarınıza gönderilen doğrulama e-postalarında görüntülenir.

### <a name="enable-branding-in-user-flow-pages"></a>Kullanıcı akış sayfalarında markalamayı etkinleştir

Şirket markasını yapılandırdıktan sonra, Kullanıcı akışlarınızda etkinleştirin.

1. Azure portal sol menüsünde **Azure AD B2C**' i seçin.
1. **İlkeler**altında **Kullanıcı akışları ' nı (ilkeler)** seçin.
1. Şirket markasını etkinleştirmek istediğiniz kullanıcı akışını seçin. Şirket markası, *oturum açma v1* ve *profil düzenlemesi v1* Kullanıcı akış türleri için **desteklenmez** .
1. **Özelleştir**altında **sayfa düzenleri**' ni seçin ve ardından marka yapmak istediğiniz düzeni seçin. Örneğin **Birleşik kaydolma veya oturum açma sayfası**' nı seçin.
1. **Sayfa düzeni sürümü (Önizleme)** için sürüm **1.2.0** veya üzerini seçin.
1. **Kaydet**’i seçin.

Kullanıcı akışındaki tüm sayfaları markalaştırmak isterseniz, Kullanıcı akışındaki her sayfa düzeni için sayfa düzeni sürümünü ayarlayın.

![Azure portal Azure AD B2C sayfa düzeni seçimi](media/customize-ui-overview/portal-02-page-layout-select.png)

Bu açıklamalı örnek, okyanus şablonu kullanan bir *kaydolma ve oturum açma* Kullanıcı akışı sayfasında özel bir başlık logosu ve arka plan görüntüsü gösterir:

![Azure AD B2C tarafından sunulan markalı kaydolma/oturum açma sayfası](media/customize-ui-overview/template-ocean-blue-branded.png)

### <a name="use-company-branding-assets-in-custom-html"></a>Özel HTML 'de Şirket marka varlıklarını kullanma

Şirket marka varlıklarınızı özel HTML 'de kullanmak için aşağıdaki etiketleri etiketin dışına ekleyin `<div id="api">` :

```HTML
<img data-tenant-branding-background="true" />
<img data-tenant-branding-logo="true" alt="Company Logo" />
```

Görüntü kaynağı arka plan resminin ve başlık logosunun yerine konur. [Özel HTML ve CSS ile çalışmaya başlama](#get-started-with-custom-html-and-css) bölümünde açıklandığı gıbı, CSS sınıflarını stil için kullanarak ve varlıkları sayfada konumlandırın.

## <a name="localize-content"></a>İçeriği yerelleştirin

Azure AD B2C kiracınızda [dil özelleştirmesini](user-flow-language-customization.md) etkinleştirerek HTML içeriğinizi yerelleştirebilirsiniz. Bu özelliği etkinleştirmek Azure AD B2C, OpenID Connect parametresini uç noktanıza iletmesine izin verir `ui_locales` . İçerik sunucunuz, dile özgü HTML sayfaları sağlamak için bu parametreyi kullanabilir.

İçerik, kullanılan yerel ayara bağlı olarak farklı yerlerden çekeklenebilir. CORS etkin uç noktanıza, belirli diller için içerik barındırmak üzere bir klasör yapısı ayarlarsınız. Joker karakter değerini kullanırsanız, doğru olanı çağıracaksınız `{Culture:RFC5646}` .

Örneğin, özel sayfa URI 'niz şöyle görünebilir:

```http
https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html
```

Buradan içerik çekerek sayfayı Fransızca olarak yükleyebilirsiniz:

```http
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="examples"></a>Örnekler

GitHub 'da [B2C-AzureBlobStorage-Client](https://github.com/azureadquickstarts/b2c-azureblobstorage-client) deposunda birkaç örnek şablon dosyası bulabilirsiniz.

Şablonlarda örnek HTML ve CSS dosyaları [/sample_templates](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates) dizininde bulunur.

## <a name="next-steps"></a>Sonraki adımlar

- **Kullanıcı akışları**kullanıyorsanız, Kullanıcı arabiriminizi öğreticiyle özelleştirmeye başlayabilirsiniz:

    [Azure Active Directory B2C ' de uygulamalarınızın Kullanıcı arabirimini özelleştirin](tutorial-customize-ui.md).
- **Özel ilkeler**kullanıyorsanız, şu makaleyle Kullanıcı arabirimini özelleştirmeye başlayabilirsiniz:

    [Azure Active Directory B2C özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirin](custom-policy-ui-customization.md).
