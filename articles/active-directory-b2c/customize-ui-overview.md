---
title: Kullanıcı arabirimini özelleştirme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C kullanan uygulamalarınız için kullanıcı arabirimini nasıl özelleştireceklerinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 37ddf57057b736cd76a74276e5593a865e7df8cc
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80666861"
---
# <a name="customize-the-user-interface-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki kullanıcı arabirimini özelleştirme

Azure Active Directory B2C'nin (Azure AD B2C) müşterilerinize görüntülemesi olan kullanıcı arabirimini markalamak ve özelleştirmek, uygulamanızda sorunsuz bir kullanıcı deneyimi sağlamaya yardımcı olur. Bu deneyimler arasında kaydolma, oturum açma, profil düzenleme ve parola sıfırlama yer almaktadır. Bu makalede, hem kullanıcı akışları hem de özel ilkeler için kullanıcı arabirimi (UI) özelleştirme yöntemleri tanıtılmaktadır.

## <a name="ui-customization-in-different-scenarios"></a>Farklı senaryolarda kullanıcı arabirimi özelleştirme

Kullanıcı nın kullanıcı deneyimlerinin kullanıcı deneyimlerinin kullanıcı arasını özelleştirmenin her biri farklı senaryolar için uygun olan çeşitli yolları vardır.

### <a name="user-flows"></a>Kullanıcı akışları

[Kullanıcı akışlarını](user-flow-overview.md)kullanıyorsanız, yerleşik *sayfa düzeni şablonlarını*kullanarak veya kendi HTML ve CSS'nizi kullanarak kullanıcı akış sayfalarınızın görünümünü değiştirebilirsiniz. Her iki yöntem de bu makalede daha sonra ele alınmıştır.

Kullanıcı akışları için Kullanıcı Arabirimi özelleştirmesini yapılandırmak için [Azure portalını](tutorial-customize-ui.md) kullanırsınız.

> [!TIP]
> Yalnızca kullanıcı akış sayfalarınızın afiş logosunu, arka plan görüntüsünü ve arka plan rengini değiştirmek istiyorsanız, bu makalede daha sonra açıklanan [Şirket markalama (önizleme)](#company-branding-preview) özelliğini deneyebilirsiniz.

### <a name="custom-policies"></a>Özel ilkeler

Uygulamanızda kaydolma veya oturum açma, parola sıfırlama veya profil düzenleme sağlamak için [özel ilkeler](custom-policy-overview.md) kullanıyorsanız, [Kullanıcı Arabirimi'ni özelleştirmek için ilke dosyalarını](custom-policy-ui-customization.md)kullanın.

Müşterinin kararına dayalı dinamik içerik sağlamanız gerekiyorsa, sorgu dizesinde gönderilen bir parametreye bağlı olarak [sayfa içeriğini dinamik olarak değiştirebilen](custom-policy-ui-customization.md#configure-dynamic-custom-page-content-uri) özel ilkeler kullanın. Örneğin, web veya mobil uygulamanızdan geçtiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfasındaki arka plan görüntüsünü değiştirebilirsiniz.

### <a name="javascript"></a>JavaScript

Hem [kullanıcı akışlarında](user-flow-javascript-overview.md) hem de [özel ilkelerde](page-layout.md)istemci tarafındaki JavaScript kodunu etkinleştirebilirsiniz.

### <a name="sign-in-only-ui-customization"></a>Yalnızca kullanıcı tarafından oturum açma Kullanıcı Birası özelleştirmesi

Yalnızca oturum açma sağlıyorsanız, beraberindeki parola sıfırlama sayfası ve doğrulama e-postalarıyla birlikte, Azure AD [oturum açma sayfası](../active-directory/fundamentals/customize-branding.md)için kullanılan aynı özelleştirme adımlarını kullanın.

Müşteriler oturum açmadan önce profillerini düzenlemeye çalışırsa, Azure AD oturum açma sayfasını özelleştirmek için kullanılan aynı adımları kullanarak özelleştirdiğiniz bir sayfaya yönlendirilirler.

## <a name="page-layout-templates"></a>Sayfa düzeni şablonları

Kullanıcı akışları, kullanıcı deneyimi sayfalarınıza profesyonel bir görünüm kazandırmak için seçebileceğiniz birkaç yerleşik şablon sağlar. Bu düzen şablonları da olabilir ve kendi özelleştirme için başlangıç noktası olarak hizmet vermektedir.

Sol menüde **Özelleştir'in** altında **Sayfa düzenlerini** seçin ve ardından **Şablon'u**seçin.

![Azure portalının kullanıcı akış sayfasında şablon seçimi açılır](media/customize-ui-overview/template-selection.png)

Ardından, listeden bir şablon seçin. Aşağıda her şablon için oturum açma sayfalarından örnekler verilmiştir:

| Okyanus Mavisi | Kayrak Gri | Klasik |
|:-:|:-:|:-:|
|![Kaydol sayfasında işlenen Ocean Blue şablonu örneği](media/customize-ui-overview/template-ocean-blue.png)|![Kaydol sayfasında oturum açmada işlenen Kayrak Grisi şablonu örneği](media/customize-ui-overview/template-slate-gray.png)|![Kaydol sayfasında işlenen Klasik şablon örneği](media/customize-ui-overview/template-classic.png)|

Bir şablon seçtiğinizde, seçili düzen kullanıcı akışınızdaki tüm sayfalara uygulanır ve her sayfanın URI'si **Özel sayfa URI** alanında görünür.

## <a name="custom-html-and-css"></a>Özel HTML ve CSS

Özelleştirilmiş HTML ve CSS'nizle kendi ilke düzeninizi tasarlamak istiyorsanız, bunu ilkenizde bulunan Düzen adlarının her biri için "Özel sayfa içeriğini kullan" geçişini değiştirerek yapabilirsiniz. Özel düzen yapılandırmaları ile ilgili aşağıdaki yönergeleri izleyin:

Azure AD B2C, [Çapraz Kaynak Paylaşımı (CORS)](https://www.w3.org/TR/cors/)adlı bir yaklaşım kullanarak müşterinizin tarayıcısında kod çalıştırır.

Çalışma zamanında, içerik kullanıcı akışınızda veya özel politikanızda belirttiğiniz bir URL'den yüklenir. Kullanıcı deneyimindeki her sayfa, içeriğini o sayfa için belirttiğiniz URL'den yükler. Url'nizden içerik yüklendikten sonra, Azure AD B2C tarafından eklenen bir HTML parçasıyla birleştirilir ve sayfa müşterinize gösterilir.

UI'yi özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanmadan önce aşağıdaki kılavuzu gözden geçirin:

- Azure AD B2C, HTML içeriğini sayfalarınızda **birleştirir.** Azure AD B2C'nin sağladığı varsayılan içeriği kopyalamayın ve değiştirmeyi deneyin. HTML içeriğinizi sıfırdan oluşturmak ve varsayılan içeriği referans olarak kullanmak en iyisidir.
- **JavaScript,** hem [kullanıcı akışları](user-flow-javascript-overview.md) hem de [özel ilkeler](javascript-samples.md)için özel içeriğinize eklenebilir.
- Desteklenen **tarayıcı sürümleri** şunlardır:
  - Internet Explorer 11, 10 ve Microsoft Edge
  - Internet Explorer 9 ve 8 için sınırlı destek
  - Google Chrome 42.0 ve üzeri
  - Mozilla Firefox 38.0 ve üzeri
  - iOS ve macOS için Safari, sürüm 12 ve üzeri
- HTML'nize **form etiketleri** eklemeyin. Form etiketleri, Azure AD B2C tarafından enjekte edilen HTML tarafından oluşturulan POST işlemlerini etkiler.

### <a name="where-do-i-store-ui-content"></a>Kullanıcı bira sıyrık içeriğini nerede saklarım?

Kullanıcı Arabirimi'ni özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanırken, Kullanıcı Arabirimi içeriğinizi CORS'leri destekleyen herkese açık herhangi bir HTTPS bitiş noktasında barındırabilirsiniz. Örneğin, [Azure Blob depolama,](../storage/blobs/storage-blobs-introduction.md)web sunucuları, CDN'ler, AWS S3 veya dosya paylaşım sistemleri.

Önemli olan nokta, [içeriği, CORS etkinleştirilmiş,](https://enable-cors.org/server.html)herkese açık bir HTTPS bitiş noktasında barındırmanızdır. İçeriğinizde belirttiğinizde mutlak bir URL kullanmanız gerekir.

> [!NOTE]
> HTML içeriği oluşturma, Azure Blob depolama alanına içerik yükleme ve CORS'ü yapılandırma hakkında daha fazla bilgi için, Kullanıcı Arabirimi özelleştirme makalesindeki [Özel sayfa içeriği gözden geçirme](custom-policy-ui-customization.md#custom-page-content-walkthrough) bölümüne bakın.

## <a name="get-started-with-custom-html-and-css"></a>Özel HTML ve CSS ile başlayın

Bu yönergeleri izleyerek kullanıcı deneyimi sayfalarınızda kendi HTML ve CSS'nizi kullanmaya başlayın.

- Bir yerde bulunan boş `<div id="api"></div>` bir öğe ile iyi `<body>`biçimlendirilmiş HTML içeriği oluşturun. Bu öğe, Azure AD B2C içeriğinin eklendiği yeri işaretler. Aşağıdaki örnekte en az sayfa gösterilmektedir:

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

- Azure AD B2C'nin sayfanıza ekleyen UI öğelerini stilize etmek için CSS'yi kullanın. Aşağıdaki örnekte, kaydolma enjekte edilen HTML öğelerinin ayarlarını da içeren basit bir CSS dosyası gösterilmektedir:

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

- İçeriğinizi HTTPS bitiş noktasında barındırın (CORS'e izin verilir). CORS'i yapılandırırken hem GET hem de OPTIONS istek yöntemleri etkinleştirilmelidir.
- Oluşturduğunuz içeriği kullanmak için bir kullanıcı akışı veya özel ilke oluşturun veya bunları düzenlendi.

### <a name="html-fragments-from-azure-ad-b2c"></a>Azure AD B2C'den HTML parçaları

Aşağıdaki tabloda, Azure AD B2C'nin içeriğinizde `<div id="api"></div>` bulunan öğeyle birleştirdiği HTML parçaları listelenebilmiştir.

| Eklenen sayfa | HTML açıklaması |
| ------------- | ------------------- |
| Kimlik sağlayıcı seçimi | Kimlik sağlayıcıları için müşterinin kaydolma veya oturum açma sırasında seçebileceği düğmelerin listesini içerir. Bu düğmeler, Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını (e-posta adresine veya kullanıcı adına göre) içerir. |
| Yerel hesap kaydolma | E-posta adresine veya kullanıcı adına dayalı yerel hesap kaydolma için bir form içerir. Form, metin giriş kutusu, parola giriş kutusu, radyo düğmesi, tek seçimli açılır kutular ve çok seçimli onay kutuları gibi farklı giriş denetimleri içerebilir. |
| Sosyal hesap kayıt | Facebook veya Google gibi bir sosyal kimlik sağlayıcısından mevcut bir hesabı kullanarak kaydolurken görünebilir. Bir kayıt formu kullanarak müşteriden ek bilgi toplanması gerektiğinde kullanılır. |
| Birleştirilmiş kaydolma veya oturum açma | Facebook, Google veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını kullanabilen müşterilerin hem kaydolma hem de kaydolma işlemlerini işler. |
| Multi-factor authentication | Müşteriler kaydolma veya oturum açma sırasında telefon numaralarını (metin veya ses kullanarak) doğrulayabilirler. |
| Hata | Müşteriye hata bilgileri sağlar. |

## <a name="company-branding-preview"></a>Şirket markalama (önizleme)

Azure Active Directory [Company markasını](../active-directory/fundamentals/customize-branding.md)kullanarak kullanıcı akış sayfalarınızı bir başlık logosu, arka plan resmi ve arka plan rengi yle özelleştirebilirsiniz.

Kullanıcı akış sayfalarınızı özelleştirmek için, önce Azure Active Directory'de şirket markasını yapılandırır, ardından Azure AD B2C'deki kullanıcı akışlarınızın sayfa düzenlerinde etkinleştirin.

[!INCLUDE [preview note](../../includes/active-directory-b2c-public-preview.md)]

### <a name="configure-company-branding"></a>Şirket markası yapılandırma

**Şirket markası**içinde banner logosunu, arka plan görüntüsünü ve arka plan rengini ayarlayarak başlayın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. **Yönet**altında, **Şirket markasını**seçin.
1. [Kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekle](../active-directory/fundamentals/customize-branding.md)adımlarını izleyin.

Azure AD B2C'de şirket markasını yapılandırırken bunları aklınızda bulundurun:

* Azure AD B2C'deki şirket markası şu anda **arka plan resmi,** **banner logosu**ve arka **plan rengi** özelleştirmesi ile sınırlıdır. Şirket marka bölmesindeki diğer özellikler, örneğin **Gelişmiş ayarlardakiler** *desteklenmez.*
* Kullanıcı akış sayfalarınızda, arka plan görüntüsü yüklenmeden önce arka plan rengi gösterilir. Daha sorunsuz bir yükleme deneyimi için arka plan resminizdeki renklerle yakından eşleşen bir arka plan rengi seçmenizi tavsiye ettik.
* Banner logosu, kullanıcılarınızın kaydolma kullanıcı akışını başlattıklarında gönderdiğiniz doğrulama e-postalarında görünür.

### <a name="enable-branding-in-user-flow-pages"></a>Kullanıcı akış sayfalarında markalamayı etkinleştirme

Şirket markasını yapılandırdıktan sonra, kullanıcı akışlarınızda etkinleştirin.

1. Azure portalının sol menüsünde **Azure AD B2C'yi**seçin.
1. **İlkeler** **altında, Kullanıcı akışlarını (ilkeler)** seçin.
1. Şirket markasını etkinleştirmek istediğiniz kullanıcı akışını seçin. *V1'de Oturum Açma* ve *V1* kullanıcı akış türleri için şirket markası **desteklenmez.**
1. **Özelleştir'in** **altında, Sayfa düzenlerini**seçin ve ardından markalamak istediğiniz düzeni seçin. Örneğin, **Birleşik Kaydol'u seçin veya sayfada oturum açın.**
1. Sayfa **Düzeni Sürümü (Önizleme)** için **sürüm 1.2.0** veya üzeri sürümü seçin.
1. **Kaydet'i**seçin.

Kullanıcı akışındaki tüm sayfaları damgalamak istiyorsanız, kullanıcı akışındaki her sayfa düzeni için sayfa düzeni sürümünü ayarlayın.

![Azure portalında Azure AD B2C'de sayfa düzeni seçimi](media/customize-ui-overview/portal-02-page-layout-select.png)

Bu açıklamalı örnek, Kaydol ve Ocean Blue şablonu kullanan kullanıcı akış sayfasında oturum aç'ta özel bir banner *logosunu ve* arka plan resmini gösterir:

![Azure AD B2C tarafından sunulan markalı kaydolma/kaydolma sayfası](media/customize-ui-overview/template-ocean-blue-branded.png)

### <a name="use-company-branding-assets-in-custom-html"></a>Şirket marka varlıklarını özel HTML'de kullanma

Şirket marka varlıklarınızı özel HTML'de kullanmak için `<div id="api">` etiketin dışına aşağıdaki etiketleri ekleyin:

```HTML
<img data-tenant-branding-background="true" />
<img data-tenant-branding-logo="true" alt="Company Logo" />
```

Görüntü kaynağı arka plan görüntüsü ve afiş logosu ile değiştirilir. Özel HTML [ve CSS bölümüyle başlayın,](#get-started-with-custom-html-and-css) sayfadaki varlıkları stilve konumlandırmak için CSS sınıflarını kullanın.

## <a name="localize-content"></a>İçeriği yerelleştirin

Azure AD B2C kiracınızda [dil özelleştirmesini](user-flow-language-customization.md) etkinleştirerek HTML içeriğinizi yerelleştirirsiniz. Bu özelliği etkinleştirmek, Azure AD B2C'nin `ui-locales` OpenID Connect parametresini bitiş noktanıza iletmesine olanak tanır. İçerik sunucunuz bu parametreyi dile özgü HTML sayfaları sağlamak için kullanabilir.

İçerik, kullanılan yerel ekibe göre farklı yerlerden çekilebilir. CORS özellikli bitiş noktanızda, belirli diller için içeriği barındıracak bir klasör yapısı ayarlarsınız. Joker karakter değerini `{Culture:RFC5646}`kullanırsanız doğru olanı ararsınız.

Örneğin, özel sayfanız URI şu şekilde görünebilir:

```HTTP
https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html
```

Aşağıdaki lerden içerik çekerek sayfayı Fransızca yükleyebilirsiniz:

```HTTP
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="examples"></a>Örnekler

GitHub'daki [B2C-AzureBlobStorage-Client](https://github.com/azureadquickstarts/b2c-azureblobstorage-client) deposunda birkaç örnek şablon dosyası bulabilirsiniz.

Şablonlarda örnek HTML ve CSS dosyaları [/sample_templates](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates) dizininde bulunur.

## <a name="next-steps"></a>Sonraki adımlar

- **Kullanıcı akışlarını**kullanıyorsanız, kullanıcı arama nızı öğreticiyle özelleştirmeye başlayabilirsiniz:

    [Azure Active Directory B2C'de uygulamalarınızın kullanıcı arabirimini özelleştirin.](tutorial-customize-ui.md)
- **Özel ilkeler**kullanıyorsanız, kullanıcı arasını makaleyle özelleştirmeye başlayabilirsiniz:

    [Azure Active Directory B2C'de özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirin.](custom-policy-ui-customization.md)
