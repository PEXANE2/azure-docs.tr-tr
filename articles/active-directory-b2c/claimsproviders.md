---
title: ClaimsProviders-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkenin ClaimsProvider öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/29/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1d1928de8c9731b54966e566d4dddf9c01073d41
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85201268"
---
# <a name="claimsproviders"></a>ClaimsProviders

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bir talep sağlayıcı bir dizi [Teknik profil](technicalprofiles.md)içerir. Her talep sağlayıcının, uç noktaları ve talep sağlayıcısıyla iletişim kurmak için gereken protokolleri tespit eden bir veya daha fazla teknik profili olmalıdır. Bir talep sağlayıcısının birden çok teknik profili olabilir. Örneğin, talep sağlayıcı birden çok protokolü, farklı yeteneklere sahip çeşitli uç noktaları desteklediğinden veya farklı güvence düzeylerinde farklı talepler yayınlamadığından birden çok teknik profil tanımlanabilir. Gizli talepler tek bir Kullanıcı yolculuğunda serbest bırakılması kabul edilebilir, ancak başka bir Kullanıcı yolculuğunda olabilir.

```xml
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
  ...
</ClaimsProviders>
```

**Claimsproviders** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1: n | Çeşitli Kullanıcı yararlanılabilir olabilecek, acalacaklandırılan bir talep sağlayıcısı. |

## <a name="claimsprovider"></a>ClaimsProvider

**ClaimsProvider** öğesi aşağıdaki alt öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ---------- | ----------- |
| Domain | 0:1 | Talep sağlayıcısı için etki alanı adını içeren bir dize. Örneğin, talep sağlayıcınız Facebook teknik profilini içeriyorsa, etki alanı adı Facebook.com olur. Bu etki alanı adı, teknik profil tarafından geçersiz kılınmadıkça talep sağlayıcısında tanımlanan tüm teknik profiller için kullanılır. Etki alanı adına bir **domain_hint**de başvurulabilir. Daha fazla bilgi için, [Azure Active Directory B2C kullanarak doğrudan oturum açma ayarlama](direct-signin.md)konusunun **sosyal sağlayıcıya yeniden yönlendirme** bölümüne bakın. |
| DisplayName | 1:1 | Talep sağlayıcısının adını içeren bir dize. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Talep sağlayıcısı tarafından desteklenen bir teknik profiller kümesi |

**ClaimsProvider** , teknik profillerinizin talep sağlayıcısıyla ilişkisini düzenler. Aşağıdaki örnekte, Azure Active Directory teknik profilleriyle Azure Active Directory talep sağlayıcısı gösterilmektedir:

```xml
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Aşağıdaki örnekte, Facebook **-OAUTH** teknik profiliyle Facebook talep sağlayıcısı gösterilmektedir.

```xml
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
