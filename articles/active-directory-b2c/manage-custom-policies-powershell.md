---
title: PowerShell ile özel ilkeleri yönetme
titleSuffix: Azure AD B2C
description: Azure AD B2C özel ilkelerinizin programlı yönetimi için Azure Active Directory (Azure AD) PowerShell cmdlet'ini kullanın. PowerShell ile özel ilkeler oluşturun, okuyun, güncelleyin ve silin.
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ebf0cfffa410d8dfe2f0e0b42a0fee0c16106fde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187415"
---
# <a name="manage-azure-ad-b2c-custom-policies-with-azure-powershell"></a>Azure PowerShell ile Azure AD B2C özel ilkelerini yönetme

Azure PowerShell, Azure AD B2C kiracınızda komut satırı ve komut dosyası tabanlı özel ilke yönetimi için birkaç cmdlet sağlar. Azure AD PowerShell modüllerini aşağıdakiler için nasıl kullanacağınızı öğrenin:

* Azure AD B2C kiracısında özel ilkeleri listele
* Kiracıdan bir ilke indirme
* İçeriğini üzerine yazarak varolan bir ilkeyi güncelleştirme
* Azure AD B2C kiracınıza yeni bir ilke yükleme
* Özel bir ilkeyi kiracıdan silme

## <a name="prerequisites"></a>Ön koşullar

* [Azure AD B2C kiracı](tutorial-create-tenant.md)ve [B2C IEF İlke Yöneticisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#b2c-ief-policy-administrator) rolü ile dizindeki bir kullanıcı için kimlik bilgileri
* Kiracınıza yüklenen [özel ilkeler](custom-policy-get-started.md)
* [Grafik **önizleme modülü** için Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0)

## <a name="connect-powershell-session-to-b2c-tenant"></a>PowerShell oturumunu B2C kiracısına bağlayın

Azure AD B2C kiracınızda özel ilkelerle çalışmak için öncelikle PowerShell oturumunuzu [Connect-AzureAD][Connect-AzureAD] komutunu kullanarak kiracıya bağlamanız gerekir.

Azure AD B2C kiracınızın adını değiştirerek `{b2c-tenant-name}` aşağıdaki komutu uygulayın. Dizindeki [B2C IEF İlke Yöneticisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#b2c-ief-policy-administrator) rolünü atanmış bir hesapla oturum açın.

```PowerShell
Connect-AzureAD -Tenant "{b2c-tenant-name}.onmicrosoft.com"
```

Başarılı bir oturum açma gösteren örnek komut çıktısı:

```Console
PS C:\> Connect-AzureAD -Tenant "contosob2c.onmicrosoft.com"

Account               Environment TenantId                             TenantDomain                 AccountType
-------               ----------- --------                             ------------                 -----------
azureuser@contoso.com AzureCloud  00000000-0000-0000-0000-000000000000 contosob2c.onmicrosoft.com   User
```

## <a name="list-all-custom-policies-in-the-tenant"></a>Kiracıdaki tüm özel ilkeleri listele

Özel ilkeleri keşfetmek, bir Azure AD B2C yöneticisinin işlemlerini gözden geçirmesine, yönetmesine ve iş mantığı eklemesine olanak tanır. Azure AD B2C kiracısında özel ilkelerin adlarının listesini döndürmek için [AzureADMSTrustFrameworkPolicy][Get-AzureADMSTrustFrameworkPolicy] komutunu kullanın.

```PowerShell
Get-AzureADMSTrustFrameworkPolicy
```

Örnek komut çıktısı:

```Console
PS C:\> Get-AzureADMSTrustFrameworkPolicy

Id
--
B2C_1A_TrustFrameworkBase
B2C_1A_TrustFrameworkExtensions
B2C_1A_signup_signin
B2C_1A_ProfileEdit
B2C_1A_PasswordReset
```

## <a name="download-a-policy"></a>Bir ilke indirin

İlke iI'lerinin listesini inceledikten sonra, içeriğini indirmek için [Get-AzureADMSTrustFrameworkPolicy][Get-AzureADMSTrustFrameworkPolicy] ile belirli bir ilkeyi hedefleyebilirsiniz.

```PowerShell
Get-AzureADMSTrustFrameworkPolicy [-Id <policyId>]
```

Bu örnekte, kimlik *B2C_1A_signup_signin* olan ilke indirilir:

```Console
PS C:\> Get-AzureADMSTrustFrameworkPolicy -Id B2C_1A_signup_signin
<TrustFrameworkPolicy xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06" PolicySchemaVersion="0.3.0.0" TenantId="contosob2c.onmicrosoft.com" PolicyId="B2C_1A_signup_signin" PublicPolicyUri="http://contosob2c.onmicrosoft.com/B2C_1A_signup_signin" TenantObjectId="00000000-0000-0000-0000-000000000000">
  <BasePolicy>
    <TenantId>contosob2c.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```

İlke içeriğini yerel olarak yeniden sağlamak için komut çıktısını `-OutputFilePath` bağımsız değişkeniçeren bir dosyaya aktarın ve ardından sık kullanılan düzenleyicide dosyayı açın.

Bir dosyaya çıktı gönderme örneği komutu:

```PowerShell
# Download and send policy output to a file
Get-AzureADMSTrustFrameworkPolicy -Id B2C_1A_signup_signin -OutputFilePath C:\RPPolicy.xml
```

## <a name="update-an-existing-policy"></a>Varolan bir ilkeyi güncelleştirme

Oluşturduğunuz veya indirdiğiniz bir ilke dosyasını düzenlemeden sonra, Güncelleştirilmiş ilkeyi [Set-AzureADMSTrustFrameworkPolicy][Set-AzureADMSTrustFrameworkPolicy] komutunu kullanarak Azure ADMSTrustFrameworkPolicy komutunu kullanarak Azure ADMsTrustFrameworkPolicy komutunu kullanarak Azure ADMTrustFrameworkPolicy komutunu kullanarak Azure AD B2C'de yayımlayabilirsiniz.

Komutu `Set-AzureADMSTrustFrameworkPolicy` Azure AD B2C kiracınızda zaten var olan bir ilkenin kimliğiyle birlikte yazarsanız, bu iipolitikasının içeriği üzerine yazılır.

```PowerShell
Set-AzureADMSTrustFrameworkPolicy [-Id <policyId>] -InputFilePath <inputpolicyfilePath> [-OutputFilePath <outputFilePath>]
```

Örnek komut:

```PowerShell
# Update an existing policy from file
Set-AzureADMSTrustFrameworkPolicy -Id B2C_1A_signup_signin -InputFilePath C:\B2C_1A_signup_signin.xml
```

Ek örnekler [için, Set-AzureADMSTrustFrameworkPolicy][Set-AzureADMSTrustFrameworkPolicy] komut başvurusuna bakın.

## <a name="upload-a-new-policy"></a>Yeni bir ilke yükleme

Üretimde çalışan özel bir ilkede değişiklik yaptığınızda, geri dönüş veya A/B test senaryoları için ilkenin birden çok sürümü yayımlanmak isteyebilirsiniz. Veya, varolan bir ilkenin bir kopyasını yapmak, birkaç küçük değişiklikle değiştirmek ve ardından farklı bir uygulama tarafından kullanılmak üzere yeni bir ilke olarak yüklemek isteyebilirsiniz.

Yeni bir ilke yüklemek için [Yeni AzureADMSTrustFrameworkPolicy][New-AzureADMSTrustFrameworkPolicy] komutunu kullanın:

```PowerShell
New-AzureADMSTrustFrameworkPolicy -InputFilePath <inputpolicyfilePath> [-OutputFilePath <outputFilePath>]
```

Örnek komut:

```PowerShell
# Add new policy from file
New-AzureADMSTrustFrameworkPolicy -InputFilePath C:\SignUpOrSignInv2.xml
```

## <a name="delete-a-custom-policy"></a>Özel bir ilkeyi silme

Temiz bir işlem yaşam döngüsünü korumak için, kullanılmayan özel ilkeleri düzenli aralıklarla kaldırmanızı öneririz. Örneğin, yeni bir ilke kümesine geçiş yaptıktan ve yeni ilkelerin işlevselliğini doğruladıktan sonra eski ilke sürümlerini kaldırmak isteyebilirsiniz. Ayrıca, bir dizi özel ilke yayımlamaya çalışır ve bir hata alırsanız, başarısız sürümün bir parçası olarak oluşturulan ilkeleri kaldırmak mantıklı olabilir.

Kiracınızdan bir ilkeyi silmek için [Azure'u KaldırADMSTrustFrameworkPolicy][Remove-AzureADMSTrustFrameworkPolicy] komutunu kullanın.

```PowerShell
Remove-AzureADMSTrustFrameworkPolicy -Id <policyId>
```

Örnek komut:

```PowerShell
# Delete an existing policy
Remove-AzureADMSTrustFrameworkPolicy -Id B2C_1A_signup_signin
```

## <a name="troubleshoot-policy-upload"></a>Sorun giderme ilkesi yükleme

Yeni bir özel ilke yayımlamaya veya varolan bir ilkeyi güncelleştirmeye çalıştığınızda, yanlış XML biçimlendirmeve ilke dosyası devralma zincirindeki hatalar doğrulama hatalarına neden olabilir.

Örneğin, hatalı Biçimlendirilmiş XML içeren içerikli bir ilkeyi güncelleştirme girişimi aşağıda verilmiştir (çıktı kısalık için kesilir):

```Console
PS C:\> Set-AzureADMSTrustFrameworkPolicy -Id B2C_1A_signup_signin -InputFilePath C:\B2C_1A_signup_signin.xml
Set-AzureADMSTrustFrameworkPolicy : Error occurred while executing PutTrustFrameworkPolicy
Code: AADB2C
Message: Validation failed: 1 validation error(s) found in policy "B2C_1A_SIGNUP_SIGNIN" of tenant "contosob2c.onmicrosoft.com".Schema validation error found at line
14 col 55 in policy "B2C_1A_SIGNUP_SIGNIN" of tenant "contosob2c.onmicrosoft.com": The element 'OutputClaims' in namespace
'http://schemas.microsoft.com/online/cpim/schemas/2013/06' cannot contain text. List of possible elements expected: 'OutputClaim' in namespace
'http://schemas.microsoft.com/online/cpim/schemas/2013/06'.
...
```

Sorun giderme özel ilkeleri hakkında bilgi için Bkz. [Sorun Giderme Azure AD B2C özel ilkeleri ve Kimlik Deneyimi Çerçevesi.](active-directory-b2c-guide-troubleshooting-custom.md)

## <a name="next-steps"></a>Sonraki adımlar

Sürekli tümleştirme/sürekli teslim (CI/CD) ardışık hattının bir parçası olarak özel ilkeleri dağıtmak için PowerShell'i kullanma hakkında bilgi [için](deploy-custom-policies-devops.md)bkz.

<!-- LINKS - External -->
[Connect-AzureAD]: https://docs.microsoft.com/powershell/module/azuread/get-azureadmstrustframeworkpolicy
[Get-AzureADMSTrustFrameworkPolicy]: https://docs.microsoft.com/powershell/module/azuread/get-azureadmstrustframeworkpolicy
[New-AzureADMSTrustFrameworkPolicy]: https://docs.microsoft.com/powershell/module/azuread/new-azureadmstrustframeworkpolicy
[Remove-AzureADMSTrustFrameworkPolicy]: https://docs.microsoft.com/powershell/module/azuread/remove-azureadmstrustframeworkpolicy
[Set-AzureADMSTrustFrameworkPolicy]: https://docs.microsoft.com/powershell/module/azuread/set-azureadmstrustframeworkpolicy
