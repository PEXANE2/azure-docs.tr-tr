---
title: İddia Sağlayıcıları - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'de özel bir ilkenin ClaimsProvider öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/29/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: dfb34085181e0b759d1d77485ff21b5bc59e0de3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189778"
---
# <a name="claimsproviders"></a>ClaimsProviders

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Talep sağlayıcı bir dizi [teknik profil](technicalprofiles.md)içerir. Her talep sağlayıcısının, uç noktaları ve talep sağlayıcıyla iletişim kurmak için gereken protokolleri belirleyen bir veya daha fazla teknik profili olmalıdır. Talep sağlayıcının birden çok teknik profili olabilir. Örneğin, talep sağlayıcı birden çok protokolü, farklı özelliklere sahip çeşitli uç noktaları desteklediği veya farklı güvence düzeylerinde farklı talepler saldığı için birden çok teknik profil tanımlanabilir. Hassas talepleri bir kullanıcı yolculuğunda serbest bırakmak kabul edilebilir, ancak başka bir kullanıcı yolculuğunda yayımlanamaz.

```XML
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

**ClaimsProviders** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| İddia Sağlayıcı | 1:n | Çeşitli kullanıcı yolculuklarında yararlanılabilen akredite bir talep sağlayıcısı. |

## <a name="claimsprovider"></a>İddia Sağlayıcı

**ClaimsProvider** öğesi aşağıdaki alt öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ---------- | ----------- |
| Domain | 0:1 | Talep sağlayıcısının etki alanı adını içeren bir dize. Örneğin, talep sağlayıcınız Facebook teknik profilini içeriyorsa, alan adı Facebook.com. Bu etki alanı adı, teknik profil tarafından geçersiz kılınmadığı sürece talep sağlayıcıda tanımlanan tüm teknik profiller için kullanılır. Etki alanı adı da bir **domain_hint**başvurulabilir. Daha fazla bilgi için, [Azure Active Directory B2C'yi kullanarak doğrudan oturum açma'yı](direct-signin.md)ayarla'nın **sosyal sağlayıcı bölümüne yeniden yönlendir'e** bakın. |
| DisplayName | 1:1 | Talep sağlayıcının adını içeren bir dize. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Talep sağlayıcı tarafından desteklenen bir dizi teknik profil |

**ClaimsProvider,** teknik profillerinizin talep sağlayıcıyla nasıl ilişkili olduğunu düzenler. Aşağıdaki örnek, Azure Etkin Dizin teknik profillerine sahip Azure Etkin Dizin talep sağlayıcısını gösterir:

```XML
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

Aşağıdaki örnek, **Facebook-OAUTH** teknik profiline sahip Facebook talep sağlayıcısını gösterir.

```XML
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
