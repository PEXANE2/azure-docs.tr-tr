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
ms.date: 01/28/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ecece3a00a788b67f6c831804bf5b00372fef93d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99056120"
---
# <a name="customize-the-user-interface-in-azure-active-directory-b2c"></a>Azure Active Directory B2C Kullanıcı arabirimini özelleştirme

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Azure Active Directory B2C (Azure AD B2C) müşterilerinizin gösterdiği Kullanıcı arabirimini markalamayı ve özelleştirmeyi, uygulamanızda sorunsuz bir kullanıcı deneyimi sağlamaya yardımcı olur. Bu deneyimlere kaydolma, oturum açma, profil düzenlemesi ve parola sıfırlama dahildir. Bu makalede, sayfa şablonunu ve şirket markasını kullanarak Azure AD B2C sayfalarınızı özelleştirirsiniz. 

> [!TIP]
> Kullanıcı akış sayfalarınızın diğer yönlerini sayfa şablonunun, başlık logosunun, arka plan resminin veya arka plan renginin ötesinde özelleştirmek için bkz. [HTML şablonuyla Kullanıcı arabirimini özelleştirme](customize-ui-with-html.md).

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="overview"></a>Genel Bakış

Azure AD B2C, Kullanıcı deneyimi sayfalarınıza profesyonel bir görünüm kazandırmak için arasından seçim yapabileceğiniz birkaç yerleşik şablon sağlar. Bu sayfa şablonları, [Şirket markası](#company-branding) özelliğini kullanarak kendi özelleştirmesi için başlangıç noktası olarak da kullanılabilir.

### <a name="ocean-blue"></a>Okyanus Mavisi

Kaydolma oturum açma sayfasında işlenen okyanus mavi şablonu örneği:

![Okyanus mavi şablonu ekran görüntüsü](media/customize-ui/template-ocean-blue.png)

### <a name="slate-gray"></a>Kurşun Grisi

Kaydolma oturum açma sayfasında oluşturulan kurşun gri şablon örneği:

![Kurşun Grisi şablonu ekran görüntüsü](media/customize-ui/template-slate-gray.png)

### <a name="classic"></a>Klasik

Kaydolma oturum açma sayfasında işlenen klasik şablon örneği:

![Klasik şablon ekran görüntüsü](media/customize-ui/template-classic.png)

### <a name="company-branding"></a>Şirket markası

Azure AD B2C sayfalarınızı Azure Active Directory [Şirket markalaması](../active-directory/fundamentals/customize-branding.md)kullanarak bir başlık logosu, arka plan resmi ve arka plan rengi ile özelleştirebilirsiniz. Şirket markası kaydolma, oturum açma, profil düzenlemesi ve parola sıfırlama işlemlerini içerir. 

Aşağıdaki örnek, okyanus şablonu kullanarak özel bir logo, arka plan görüntüsü ile *kaydolma ve oturum açma* sayfasını göstermektedir:

![Azure AD B2C tarafından sunulan markalı kaydolma/oturum açma sayfası](media/customize-ui/template-ocean-blue-branded.png)


## <a name="select-a-page-template"></a>Bir sayfa şablonu seçin

::: zone pivot="b2c-user-flow"

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **Kullanıcı akışları ' nı** seçin.
1. Özelleştirmek istediğiniz kullanıcı akışını seçin.
1. Sol menüdeki **Özelleştir** altında **sayfa düzenleri** ' ni seçin ve ardından bir **şablon** seçin.
    ![Azure portal Kullanıcı akışı sayfasında şablon seçme açılan listesi](./media/customize-ui/select-page-template.png)

Bir şablon seçtiğinizde, seçili şablon Kullanıcı akışındaki tüm sayfalara uygulanır. Her sayfanın URI 'SI, **özel sayfa URI 'si** alanında görünür.

::: zone-end

::: zone pivot="b2c-custom-policy"

Bir sayfa şablonu seçmek için, `LoadUri` [içerik tanımlarının](contentdefinitions.md)öğesini ayarlayın. Aşağıdaki örnek, içerik tanımı tanımlayıcılarını ve karşılık gelen öğesini gösterir `LoadUri` . 

Okyanus Mavisi:

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/AzureBlue/exception.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/AzureBlue/multifactor-1.0.0.cshtml</LoadUri>
  </ContentDefinition>
</ContentDefinitions>
```

Kurşun Grisi:

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/MSA/exception.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/MSA/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/MSA/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/MSA/unified.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/MSA/multifactor-1.0.0.cshtml</LoadUri>
  </ContentDefinition>
</ContentDefinitions>
```

Klasik 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/default/exception.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/default/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/default/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/default/multifactor-1.0.0.cshtml</LoadUri>
  </ContentDefinition>
</ContentDefinitions>
```
::: zone-end


## <a name="configure-company-branding"></a>Şirket markası yapılandırma

Kullanıcı akış sayfalarınızı özelleştirmek için önce Azure Active Directory Şirket markasını yapılandırın, ardından Kullanıcı Azure AD B2C akışlarınızda etkinleştirin.

**Şirket markalaması** içinde başlık logosunu, arka plan görüntüsünü ve arka plan rengini ayarlayarak başlayın.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **Yönet** altında **Şirket markalaması**' nı seçin.
1. [Kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleme](../active-directory/fundamentals/customize-branding.md)bölümündeki adımları izleyin.

Şirket markasını Azure AD B2C ' de yapılandırırken şunları göz önünde bulundurun:

* Azure AD B2C Şirket markası şu anda **arka plan resmi**, **Başlık logosu** ve **arka plan rengi** özelleştirmesi ile sınırlıdır. Şirket markası bölmesindeki diğer özellikler, örneğin **Gelişmiş ayarlar** *desteklenmez*.
* Kullanıcı akış sayfalarınızda arka plan rengi, arka plan görüntüsü yüklenmeden önce gösterilir. Daha yumuşak bir yükleme deneyimi için arka plan görüntinizdeki renklerle yakından eşleşen bir arka plan rengi seçmenizi öneririz.
* Ana Başlık logosu, bir kaydolma Kullanıcı akışı başlattıklarında kullanıcılarınıza gönderilen doğrulama e-postalarında görüntülenir.



## <a name="enable-company-branding-in-user-flow-pages"></a>Kullanıcı akış sayfalarında şirket markasını etkinleştirin

::: zone pivot="b2c-user-flow"

Şirket markasını yapılandırdıktan sonra, Kullanıcı akışlarınızda etkinleştirin.

1. Azure portal sol menüsünde **Azure AD B2C**' i seçin.
1. **İlkeler** altında **Kullanıcı akışları ' nı (ilkeler)** seçin.
1. Şirket markasını etkinleştirmek istediğiniz kullanıcı akışını seçin. Şirket markası, standart *oturum açma* ve standart *profil düzenlemesi* Kullanıcı akış türleri için **desteklenmez** .
1. **Özelleştir** altında **sayfa düzenleri**' ni seçin ve ardından marka yapmak istediğiniz sayfayı seçin. Örneğin **Birleşik kaydolma veya oturum açma sayfası**' nı seçin.
1. **Sayfa düzeni sürümü (Önizleme)** için sürüm **1.2.0** veya üzerini seçin.
1. **Kaydet**’i seçin.

Kullanıcı akışındaki tüm sayfaları markalaştırmak isterseniz, Kullanıcı akışındaki her sayfa düzeni için sayfa düzeni sürümünü ayarlayın.

![Azure portal Azure AD B2C sayfa düzeni seçimi](media/customize-ui/portal-02-page-layout-select.png)

::: zone-end

::: zone pivot="b2c-custom-policy"

Şirket markasını yapılandırdıktan sonra, özel ilkenizde etkinleştirin. [Sayfa düzeni sürümünü](contentdefinitions.md#migrating-to-page-layout) `contract` özel ilkenizde *Tüm* içerik tanımlarının sayfa sürümüyle yapılandırın. Değerin biçimi şu kelimeyi içermelidir: `contract` _urn: com: Microsoft: AAD: B2C: Elements:**anlaşma**:p Age-Name: Version_. Özel ilkeleriniz içinde eski bir **Datauri** değeri kullanan bir sayfa düzeni belirtmek için. Daha fazla bilgi için sayfa sürümüne sahip [sayfa düzenine geçiş](contentdefinitions.md#migrating-to-page-layout) yapmayı öğrenin.

Aşağıdaki örnek, içerik tanımlarını ilgili sayfa sözleşmesiyle ve *okyanus mavi* sayfa şablonuyla gösterir: 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/AzureBlue/exception.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
     <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
     <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
     <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/AzureBlue/multifactor-1.0.0.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C ' de uygulamanızın kullanıcı arabirimini özelleştirme](customize-ui-with-html.md)bölümünde uygulamalarınızın Kullanıcı arabirimini nasıl özelleştirebileceğinizi hakkında daha fazla bilgi edinin.
