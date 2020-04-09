---
title: Azure uygulama kimliği (PowerShell) oluşturma | Azure
titleSuffix: Microsoft identity platform
description: Azure PowerShell kullanarak Azure Active Directory uygulamasıyla hizmet sorumlusu oluşturma ve rol tabanlı erişim denetimi aracılığıyla bu uygulamaya kaynaklar için erişim verme işlemleri açıklanır. Bir sertifikayla uygulamanın kimliğinin nasıl doğrulandığı gösterilir.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.date: 10/10/2019
ms.author: ryanwi
ms.reviewer: tomfitz
ms.openlocfilehash: 7bd8c3b25c23ba8586e38ec8eb7d1baefaa21633
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884197"
---
# <a name="how-to-use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Nasıl olunur: Sertifikası olan bir hizmet sorumlusu oluşturmak için Azure PowerShell'i kullanın

Kaynaklara erişmesi gereken bir uygulamanız veya betiğiniz olduğunda, uygulama için bir kimlik ayarlayabilir ve uygulamanın kimliğini kendi kimlik bilgileriyle doğrulayabilirsiniz. Bu kimlik, hizmet sorumlusu olarak bilinir. Bu yaklaşım şunları yapmanızı sağlar:

* Uygulama kimliğine kendi izinlerinizden farklı izinler atayabilirsiniz. Normalde, bu izinler tam olarak uygulamaya gereken izinlerle sınırlı olur.
* Katılımsız bir betik yürütürken kimlik doğrulaması için sertifika kullanabilirsiniz.

> [!IMPORTANT]
> Hizmet ilkesi oluşturmak yerine, uygulama kimliğiniz için Azure kaynakları için yönetilen kimlikler kullanmayı düşünün. Kodunuz yönetilen kimlikleri destekleyen bir hizmette çalışıyorsa ve Azure Etkin Dizini (Azure AD) kimlik doğrulamasını destekleyen kaynaklara erişiyorsa, yönetilen kimlikler sizin için daha iyi bir seçenektir. Şu anda hangi hizmetlerin desteklediği de dahil olmak üzere Azure kaynaklarıiçin yönetilen kimlikler hakkında daha fazla bilgi edinmek için Azure [kaynakları için yönetilen kimlikler nedir?](../managed-identities-azure-resources/overview.md)

Bu makalede, sertifikayla kimlik doğrulaması yapan bir hizmet sorumlusunun nasıl oluşturulduğu gösterilir. Parolası olan bir hizmet sorumlusu ayarlamak için bkz. [Azure PowerShell ile Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps).

Bu makale için PowerShell'in [en son sürümünü](/powershell/azure/install-az-ps) kullanıyor olmalısınız.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="required-permissions"></a>Gerekli izinler

Bu makaleyi tamamlamak için hem Azure REKLAM'ınızda hem de Azure aboneliğinizde yeterli izinlere sahip olmalısınız. Özellikle, Azure AD'de bir uygulama oluşturabilmeli ve hizmet ilkesini bir role atayabiliyor olmalısınız.

Hesabınızın yeterli izinlere sahip olup olmadığını denetlemenin en kolay yolu portalı kullanmaktır. Bkz. [Gerekli izinleri denetleme](howto-create-service-principal-portal.md#required-permissions).

## <a name="assign-the-application-to-a-role"></a>Uygulamayı bir role atama
Aboneliğinizdeki kaynaklara erişmek için uygulamayı bir role atamanız gerekir. Hangi rolün uygulama için doğru izinleri sunduğuna karar verin. Kullanılabilir roller hakkında bilgi edinmek için Bkz. [RBAC: Built in Roles](/azure/role-based-access-control/built-in-roles).

Kapsamı abonelik, kaynak grubu veya kaynak düzeyinde ayarlayabilirsiniz. İzinler daha düşük kapsam düzeylerine devralınır. Örneğin, bir kaynak grubu için *Okuyucu* rolüne bir uygulama eklemek, kaynak grubunu ve içerdiği kaynakları okuyabileceği anlamına gelir. Uygulamanın yeniden başlatma, başlatma ve durdurma gibi eylemleri yürütmesine izin vermek için *Katılımcı* rolünü seçin.

## <a name="create-service-principal-with-self-signed-certificate"></a>Otomatik olarak imzalanan bir sertifikayla hizmet sorumlusu oluşturma

Aşağıdaki örnekte basit bir senaryo ele alınmıştır. Kendi imzalı sertifikası olan bir hizmet ilkesi oluşturmak için [New-AzADServicePrincipal'ı](/powershell/module/az.resources/new-azadserviceprincipal) kullanır ve [Reader](/azure/role-based-access-control/built-in-roles#reader) rolünü hizmet ilkesine atamak için [New-AzRoleAssignment'ı](/powershell/module/az.resources/new-azroleassignment) kullanır. Rol atamasının kapsamı şu anda seçili olan Azure aboneliğinizdir. Farklı bir abonelik seçmek için [Set-AzContext'ı](/powershell/module/Az.Accounts/Set-AzContext)kullanın.

> [!NOTE]
> Yeni SelfSignedCertificate cmdlet ve PKI modülü şu anda PowerShell Core'da desteklenmez. 

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $sp.ApplicationId
```

Örnek, yeni hizmet sorumlusunun Azure AD boyunca yayılması için biraz zaman tanımak için 20 saniye uyur. Betiğiniz yeteri kadar uzun beklemiyorsa, "{DIR-ID} dizininde {ID} sorumlusu yok" hatasını görürsünüz. Bu hatayı gidermek için bir dakika bekleyin ve **Yeni-AzRoleAssignment** komutunu yeniden çalıştırın.

**ResourceGroupName** parametresini kullanıp rol atamasının kapsamı olarak belirli bir kaynak grubunu belirtebilirsiniz. Ayrıca kapsam olarak belirli bir kaynağı belirtmek için **ResourceType** ve **ResourceName** parametrelerini kullanabilirsiniz. 

**Windows 10'unuz veya Windows Server 2016'nız yoksa**, Microsoft Betik Merkezi'nden [Otomatik olarak imzalanan sertifika oluşturucusunu](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) indirmeniz gerekir. İçindekileri ayıklayın ve ihtiyacınız olan cmdlet'i içeri aktarın.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

Sertifikayı oluşturmak için betikte aşağıdaki iki satırı değiştirin.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell betiği aracılığıyla sertifika sağlama

Hizmet sorumlusu olarak her oturum açtığınızda, AD uygulamanız için dizinin kiracı kimliğini sağlamanız gerekir. Kiracı, Azure AD'nin bir örneğidir.

```powershell
$TenantId = (Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -eq "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Sertifika Yetkilisinin sertifikasıyla hizmet sorumlusu oluşturma

Aşağıdaki örnekte, hizmet sorumlusu oluşturmak için bir Sertifika Yetkilisinin verdiği sertifika kullanılır. Atamanın kapsamı belirtilen Azure aboneliğidir. Okuyucu [rolüne](../../role-based-access-control/built-in-roles.md#reader) hizmet ilkesini ekler. Rol ataması sırasında hata oluştursa, atamayı yeniden dener.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzAccount
 Import-Module Az.Resources
 Set-AzContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell betiği aracılığıyla sertifika sağlama
Hizmet sorumlusu olarak her oturum açtığınızda, AD uygulamanız için dizinin kiracı kimliğini sağlamanız gerekir. Kiracı, Azure AD'nin bir örneğidir.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

Uygulama kimliği ve kiracı kimliği büyük/küçük harfe duyarlı değildir, dolayısıyla bunları doğrudan betiğinize ekleyebilirsiniz. Kiracı kimliğini almanız gerekiyorsa şunu kullanın:

```powershell
(Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
```

Uygulama kimliğini almanız gerekiyorsa şunu kullanın:

```powershell
(Get-AzADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Kimlik bilgilerini değiştirme

Bir AD uygulamasının kimlik bilgilerini değiştirmek için, bir güvenlik uzlaşması veya kimlik bilgisi süresi nin dolması nedeniyle [Remove-AzADAppCredential](/powershell/module/az.resources/remove-azadappcredential) ve [New-AzADAppCredential](/powershell/module/az.resources/new-azadappcredential) cmdlets'i kullanın.

Uygulamanın tüm kimlik bilgilerini kaldırmak için şunu kullanın:

```powershell
Get-AzADApplication -DisplayName exampleapp | Remove-AzADAppCredential
```

Sertifika değeri eklemek için, bu makalede gösterildiği gibi otomatik olarak imzalanan bir sertifika oluşturun. Ardından şunu kullanın:

```powershell
Get-AzADApplication -DisplayName exampleapp | New-AzADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Hata ayıklama

Hizmet sorumlusu oluştururken şu hataları alabilirsiniz:

* **"Authentication_Unauthorized"** veya **"Bağlamda hiç abonelik bulunamadı."** - Hesabınızda bir uygulamayı kaydetmek için Azure AD'de [gerekli izinler](#required-permissions) olmadığında bu hatayı görürsünüz. Genellikle, bu hatayı yalnızca Azure Etkin Dizininizdeki yönetici kullanıcılar uygulamaları kaydedebiliyor ve hesabınız yönetici olmadığında görürsünüz. Yöneticinizden sizi yönetici rolüne atamasını veya kullanıcıların uygulamaları kaydetmesini sağlamasını isteyin.

* Hesabınızın **"'Microsoft.Authorization/roleAssignments/write' (Microsoft.Authorization/roleAssignments/write) kapsamını '/abonelikler/{guid}') üzerinden gerçekleştirme yetkisi yoktur."** - Hesabınızda bir kimliğe rol atamak için yeterli izin yoksa bu hatayı görürsünüz. Abonelik yöneticinizden sizi Kullanıcı Erişimi Yöneticisi rolüne atamasını isteyin.

## <a name="next-steps"></a>Sonraki adımlar

* Parolası olan bir hizmet sorumlusu ayarlamak için bkz. [Azure PowerShell ile Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps).
* Uygulamaların ve hizmet sorumlularının daha ayrıntılı açıklaması için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](app-objects-and-service-principals.md).
* Azure AD kimlik doğrulaması hakkında daha fazla bilgi için Azure [AD kimlik doğrulama senaryolarına](authentication-scenarios.md)bakın.
