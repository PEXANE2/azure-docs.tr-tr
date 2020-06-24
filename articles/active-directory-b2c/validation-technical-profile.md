---
title: Özel ilkede doğrulama teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede doğrulama teknik profili kullanarak talepleri doğrulayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 2d4c538a9292698fecc8b44c055ab201748e292c
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85203002"
---
# <a name="define-a-validation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde doğrulama teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bir doğrulama teknik profili, [Azure Active Directory](active-directory-technical-profile.md) veya [REST API](restful-technical-profile.md)gibi herhangi bir protokolün sıradan bir teknik profilidir. Doğrulama teknik profili, çıkış taleplerini döndürür veya aşağıdaki verilerle 4xx HTTP durum kodunu döndürür. Daha fazla bilgi için bkz. [hata iletisi döndürme](restful-technical-profile.md#returning-validation-error-message)

```json
{
    "version": "1.0.0",
    "status": 409,
    "userMessage": "Your error message"
}
```

Doğrulama teknik profilinin çıkış taleplerinin kapsamı, doğrulama teknik profilini ve doğrulama teknik profillerini çağıran [kendi kendine onaylanan teknik profille](self-asserted-technical-profile.md) sınırlıdır. Sonraki düzenleme adımında çıkış taleplerini kullanmak istiyorsanız, çıkış taleplerini doğrulama teknik profilini çağıran kendi kendine onaylanan teknik profiline ekleyin.

Doğrulama teknik profilleri, **Validationteknisyen** öğesinde göründükleri sırada yürütülür. Doğrulama teknik profili bir hata harekete geçirirse veya başarılı olursa, sonraki doğrulama teknik profillerinin yürütülmesine devam edip etmeyeceğini doğrulama teknik profilinde yapılandırabilirsiniz.

Doğrulama teknik profili, **Validationteknisyen** öğesinde tanımlanan önkoşullara göre koşullu bir şekilde yürütülebilir. Örneğin, belirli bir talebin var olduğunu veya bir talebin belirtilen değere eşit olup olmadığını kontrol edebilirsiniz.

Kendi kendine onaylanan bir teknik profil, çıkış taleplerinin bazılarını veya tümünü doğrulamak için kullanılmak üzere bir doğrulama teknik profili tanımlayabilir. Başvurulan teknik profilin tüm giriş talepleri, başvuran doğrulama teknik profilinin çıkış talepleri ' nde görünmelidir.

> [!NOTE]
> Yalnızca kendi kendine onaylanan teknik profiller, doğrulama teknik profillerini kullanabilir. Kendi kendini onaylanan teknik profillerdeki çıkış taleplerini doğrulamanız gerekiyorsa, Kullanıcı yolculuğunda, doğrulama aşamasında teknik profile uyum sağlamak için ek bir düzenleme adımı kullanmayı göz önünde bulundurun.

## <a name="validationtechnicalprofiles"></a>Validationcealprofiles

**Validation, Alprofiles** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Validationteknisyen Alprofıle | 1: n | Başvuru yapan teknik profilin bazı veya tüm çıkış taleplerini doğrulamak için kullanılacak teknik bir profil. |

**Validation, Alprofile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlkede veya üst ilkede önceden tanımlanmış bir teknik profil tanıtıcısı. |
|ContinueOnError|No| Bu doğrulama teknik profili bir hata harekete geçirirse sonraki doğrulama teknik profillerinin doğrulanmasının gerekip gerekmediğini belirtir. Olası değerler: `true` veya `false` (varsayılan, diğer doğrulama profillerinin işlenmesi durdurulur ve bir hata döndürülür). |
|Devam onSuccess | No | Bu doğrulama teknik profili başarılı olursa sonraki doğrulama profillerinin doğrulanmasının gerekip gerekmediğini belirtir. Olası değerler: `true` veya `false` . Varsayılan olarak, `true` diğer doğrulama profillerinin işlenmesi devam edecektir. |

**Validation, Alprofile** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Üstbilgisinde | 0:1 | Doğrulama teknik profilinin yürütülmesi için karşılanması gereken önkoşulların bir listesi. |

**Önkoşul** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Yes | Önkoşul için gerçekleştirilecek denetim veya sorgunun türü. Belirtilen `ClaimsExist` talepler kullanıcının geçerli talep kümesinde varsa veya belirtilen `ClaimEquals` talep varsa ve değeri belirtilen değere eşitse eylemlerin gerçekleştirilmesi gerektiğini sağlamak için, belirtilen iki seçenekten biri de belirtilmiştir. |
| `ExecuteActionsIf` | Yes | Test true veya false olduğunda önkoşuldaki eylemlerin gerçekleştirilip gerçekleştirilmeyeceğini gösterir. |

**Önkoşul** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Description |
| ------- | ----------- | ----------- |
| Değer | 1: n | Denetim tarafından kullanılan veriler. Bu denetim türü ise `ClaimsExist` , bu alan, sorgulanacak bir ClaimTypeReferenceId belirtir. Denetim türü ise `ClaimEquals` , bu alan sorgu için bir ClaimTypeReferenceId belirtir. Başka bir değer öğesi, denetlenecek değeri içerdiğinde.|
| Eylem | 1:1 | Bir düzenleme adımı içindeki önkoşul denetimi doğru ise, alınması gereken eylem. **Eylemin** değeri olarak ayarlanır `SkipThisValidationTechnicalProfile` . İlişkili doğrulama teknik profilinin yürütülmemelidir. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, bu doğrulama teknik profillerini kullanır:

1. İlk doğrulama teknik profili, Kullanıcı kimlik bilgilerini denetler ve geçersiz Kullanıcı adı veya hatalı parola gibi bir hata oluşursa devam etmez.
2. Bir sonraki doğrulama teknik profili, userType talebi yoksa veya userType değeri ise yürütülür `Partner` . Doğrulama teknik profili, iç müşteri veritabanından kullanıcı profilini okumaya çalışır ve REST API hizmet kullanılamıyor ya da herhangi bir iç hata gibi bir hata oluşursa devam eder.
3. Son doğrulama teknik profili, userType talebi yoksa veya userType değeri ise yürütülür `Customer` . Doğrulama teknik profili, iç iş ortağı veritabanından kullanıcı profilini okumaya çalışır ve REST API hizmet kullanılamıyor ya da herhangi bir iç hata gibi bir hata oluşursa devam eder.

```xml
<ValidationTechnicalProfiles>
  <ValidationTechnicalProfile ReferenceId="login-NonInteractive" ContinueOnError="false" />
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromCustomertsDatabase" ContinueOnError="true" >
    <Preconditions>
      <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
        <Value>userType</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
      <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
        <Value>userType</Value>
        <Value>Partner</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
    </Preconditions>
  </ValidationTechnicalProfile>
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromPartnersDatabase" ContinueOnError="true" >
    <Preconditions>
      <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
        <Value>userType</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
      <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
        <Value>userType</Value>
        <Value>Customer</Value>
        <Action>SkipThisValidationTechnicalProfile</Action>
      </Precondition>
    </Preconditions>
  </ValidationTechnicalProfile>
</ValidationTechnicalProfiles>
```
