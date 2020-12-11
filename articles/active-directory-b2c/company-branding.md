---
title: Kuruluşunuzun oturum açma sayfasına marka ekleyin
titleSuffix: Azure AD B2C
description: Kuruluşunuzun markasını Azure Active Directory B2C sayfalarına eklemeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/10/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: bee81876b066b9fc1ea69c418ee4d0839db27b3c
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97111500"
---
# <a name="add-branding-to-your-organizations-azure-active-directory-b2c-pages"></a>Kuruluşunuzun Azure Active Directory B2C sayfalarına marka ekleme

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Azure AD B2C sayfalarınızı Azure Active Directory [Şirket markalaması](../active-directory/fundamentals/customize-branding.md)kullanarak bir başlık logosu, arka plan resmi ve arka plan rengi ile özelleştirebilirsiniz. Şirket markası kaydolma, oturum açma, profil düzenlemesi ve parola sıfırlama işlemlerini içerir. 

> [!TIP]
> Kullanıcı akış sayfalarınızın diğer yönlerini başlık logosunun, arka plan resminin veya arka plan renginin ötesinde özelleştirmek için bkz. [HTML şablonuyla Kullanıcı arabirimini özelleştirme](customize-ui-with-html.md).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


Kullanıcı akış sayfalarınızı özelleştirmek için önce Azure Active Directory ' de şirket markasını yapılandırıp, ardından Kullanıcı akışlarınızın sayfa düzenleri Azure AD B2C ' nde etkinleştirin.

## <a name="configure-company-branding"></a>Şirket markası yapılandırma

**Şirket markalaması** içinde başlık logosunu, arka plan görüntüsünü ve arka plan rengini ayarlayarak başlayın.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **Yönet** altında **Şirket markalaması**' nı seçin.
1. [Kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleme](../active-directory/fundamentals/customize-branding.md)bölümündeki adımları izleyin.

Şirket markasını Azure AD B2C ' de yapılandırırken şunları göz önünde bulundurun:

* Azure AD B2C Şirket markası şu anda **arka plan resmi**, **Başlık logosu** ve **arka plan rengi** özelleştirmesi ile sınırlıdır. Şirket markası bölmesindeki diğer özellikler, örneğin **Gelişmiş ayarlarda** *desteklenmez*.
* Kullanıcı akış sayfalarınızda arka plan rengi, arka plan görüntüsü yüklenmeden önce gösterilir. Daha yumuşak bir yükleme deneyimi için arka plan görüntinizdeki renklerle yakından eşleşen bir arka plan rengi seçmenizi öneririz.
* Ana Başlık logosu, bir kaydolma Kullanıcı akışı başlattıklarında kullanıcılarınıza gönderilen doğrulama e-postalarında görüntülenir.

::: zone pivot="b2c-user-flow"

## <a name="enable-branding-in-user-flow-pages"></a>Kullanıcı akış sayfalarında markalamayı etkinleştir

Şirket markasını yapılandırdıktan sonra, Kullanıcı akışlarınızda etkinleştirin.

1. Azure portal sol menüsünde **Azure AD B2C**' i seçin.
1. **İlkeler** altında **Kullanıcı akışları ' nı (ilkeler)** seçin.
1. Şirket markasını etkinleştirmek istediğiniz kullanıcı akışını seçin. Şirket markası, standart *oturum açma* ve standart *profil düzenlemesi* Kullanıcı akış türleri için **desteklenmez** .
1. **Özelleştir** altında **sayfa düzenleri**' ni seçin ve ardından marka yapmak istediğiniz düzeni seçin. Örneğin **Birleşik kaydolma veya oturum açma sayfası**' nı seçin.
1. **Sayfa düzeni sürümü (Önizleme)** için sürüm **1.2.0** veya üzerini seçin.
1. **Kaydet**’i seçin.

Kullanıcı akışındaki tüm sayfaları markalaştırmak isterseniz, Kullanıcı akışındaki her sayfa düzeni için sayfa düzeni sürümünü ayarlayın.

![Azure portal Azure AD B2C sayfa düzeni seçimi](media/company-branding/portal-02-page-layout-select.png)

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="select-a-page-layout"></a>Sayfa düzeni seçin

Şirket markalamasını etkinleştirmek için özel ilkenizde tüm içerik tanımlarının sayfa sürümüyle birlikte [bir sayfa düzeni sürümü tanımlamanız](contentdefinitions.md#migrating-to-page-layout) gerekir `contract` .  Değerin biçimi şu kelimeyi içermelidir: `contract` _urn: com: Microsoft: AAD: B2C: Elements:**anlaşma**:p Age-Name: Version_. Özel ilkeleriniz içinde eski bir **Datauri** değeri kullanan bir sayfa düzeni belirtmek için.

Sayfa sürümüne sahip [sayfa düzenine geçiş](contentdefinitions.md#migrating-to-page-layout) yapmayı öğrenin.

Aşağıdaki örnekte, sayfa sözleşmesiyle birlikte içerik tanımı tanımlayıcıları ve karşılık gelen **veri URI 'si** gösterilmektedir: 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

::: zone-end

Aşağıdaki örnek, okyanus şablonunu kullanan bir *kaydolma ve oturum açma* Kullanıcı akışı sayfasında özel bir başlık logosu ve arka plan görüntüsü gösterir:

![Azure AD B2C tarafından sunulan markalı kaydolma/oturum açma sayfası](media/company-branding/template-ocean-blue-branded.png)

## <a name="use-company-branding-assets-in-custom-html"></a>Özel HTML 'de Şirket marka varlıklarını kullanma

Şirket marka varlıklarınızı [özel HTML](customize-ui-with-html.md)'de kullanmak için aşağıdaki etiketleri etiketin dışına ekleyin `<div id="api">` :

```HTML
<img data-tenant-branding-background="true" />
<img data-tenant-branding-logo="true" alt="Company Logo" />
```

Görüntü kaynağı arka plan resminin ve başlık logosunun yerine konur.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C ' de uygulamanızın kullanıcı arabirimini özelleştirme](customize-ui-with-html.md)bölümünde uygulamalarınızın Kullanıcı arabirimini nasıl özelleştirebileceğinizi hakkında daha fazla bilgi edinin.