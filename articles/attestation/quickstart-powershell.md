---
title: Azure kanıtlama 'nı Azure PowerShell ile ayarlama
description: Azure PowerShell kullanarak bir kanıtlama sağlayıcısı ayarlama ve yapılandırma.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 70e86e01a9d37a27620d451bcd5d035dfcb4573d
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89237391"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell ile Azure kanıtlama ayarlama

Azure PowerShell kullanarak bir kanıtlama sağlayıcısı oluşturmak ve yapılandırmak için aşağıdaki adımları izleyin. Azure PowerShell nasıl yükleneceği ve çalıştırılacağı hakkında bilgi için bkz. [Azure PowerShell genel bakış](/powershell/azure/?view=azps-2.8.0&viewFallbackFrom=azps-2.4.0) .

PowerShell Galerisi, kullanım dışı Aktarım Katmanı Güvenliği (TLS) 1,0 ve 1,1 sürümlerini içerir. TLS 1,2 veya sonraki bir sürüm önerilir. Bu nedenle, aşağıdaki hataları alabilirsiniz:

- Uyarı: ' ' paket kaynağı çözümlenemedi https://www.powershellgallery.com/api/v2
- PackageManagement\Install-Package: belirtilen arama ölçütleri ve modül adı için eşleşme bulunamadı 

PowerShell Galerisi etkileşim kurmaya devam etmek için, Install-Module komutlarından önce aşağıdaki komutu çalıştırın

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
```

## <a name="install-azattestation-powershell-module"></a>Install az. kanıtlama PowerShell Module

Azure PowerShell olan makinede, Azure kanıtlama için cmdlet 'leri içeren az. kanıtlama PowerShell modülünü yükler.  

### <a name="initial-installation"></a>İlk yükleme

Tüm mevcut PowerShell pencerelerini sonlandırın.

"Geçerli kullanıcı" yüklemek için, yükseltilmiş olmayan bir PowerShell penceresi başlatın ve şunu çalıştırın:

```powershell
Install-Module -Name Az.Attestation -AllowClobber -Scope CurrentUser
```

"Tüm kullanıcılar" ı yüklemek için yükseltilmiş bir PowerShell penceresi başlatın ve şunu çalıştırın:

```powershell
Install-Module -Name Az.Attestation -AllowClobber -Scope AllUsers
```

Yükseltilmiş PowerShell konsolunu kapatın.

### <a name="update-the-installation"></a>Yüklemeyi güncelleştirme

Tüm mevcut PowerShell pencerelerini sonlandırın.

"Geçerli kullanıcı" öğesini güncelleştirmek için, yükseltilmiş olmayan bir PowerShell penceresi başlatın ve şunu çalıştırın:

```powershell
Update-Module -Name Az.Attestation
```

"Tüm kullanıcılar" ı güncelleştirmek için yükseltilmiş bir PowerShell penceresi başlatın ve şunu çalıştırın:

```powershell
Update-Module -Name Az.Attestation
```

Yükseltilmiş PowerShell konsolunu kapatın.

### <a name="get-installed-modules"></a>Yüklü modülleri al

Kanıtlama işlemlerini desteklemek için gereken az modüllerin en düşük sürümü:
- Az 4.5.0
- Az. Accounts 1.9.2
- Az. kanıtlama 0.1.8

Tüm az modüllerin yüklü sürümünü doğrulamak için aşağıdaki komutu çalıştırın 

```powershell
Get-InstalledModule
```
Sürümler en düşük gereksinimle eşleşmez, Update-Module komutlarını çalıştırın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

PowerShell konsolunda Azure 'da oturum açın (yükseltilmiş erişim ayrıcalıkları olmadan).

```powershell
Connect-AzAccount
```

Gerekirse, Azure kanıtlama için kullanılacak aboneliğe geçiş yapın.

```powershell
Set-AzContext -Subscription <subscription id>  
```

## <a name="register-microsoftattestation-resource-provider"></a>Microsoft. kanıtlama kaynak sağlayıcısını Kaydet

Microsoft. kanıtlama kaynak sağlayıcısını abonelikte kaydettirin. Azure kaynak sağlayıcıları ve kaynak sağlayıcılarının nasıl yapılandırılacağı ve yönetileceği hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md). Bir kaynak sağlayıcısı kaydetmenin bir abonelik için yalnızca bir kez gerekli olduğunu unutmayın.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Attestation
```
## <a name="regional-availability-of-azure-attestation"></a>Azure kanıtlama 'nın bölgesel kullanılabilirliği

```powershell
(Get-AzResourceProvider -ProviderNamespace Microsoft.Attestation)[0].Locations
```

## <a name="create-an-azure-resource-group"></a>Azure Kaynak grubu oluşturma

Kanıtlama sağlayıcısı için bir kaynak grubu oluşturun. Diğer Azure kaynaklarının (istemci uygulama örneğini içeren bir sanal makine dahil) aynı kaynak grubuna yerleştirileceğini unutmayın.

```powershell
$location = "uksouth" 
$attestationResourceGroup = "<attestation provider resource group name>"
New-AzResourceGroup -Name $attestationResourceGroup -Location $location 
```

## <a name="create-and-manage-an-attestation-provider"></a>Kanıtlama sağlayıcısı oluşturma ve yönetme

New-AzAttestation bir kanıtlama sağlayıcısı oluşturur.

```powershell
$attestationProvider = "<attestation provider name>" 
New-AzAttestation -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Location $location
```

PolicySignerCertificateFile, bir güvenilen İmzalama anahtarları kümesini belirten bir dosyadır. PolicySignerCertificateFile parametresi için bir dosya adı belirtilmişse, kanıtlama sağlayıcısı yalnızca imzalı JWT biçimindeki ilkelerle yapılandırılabilir. Else ilkesi, metin veya işaretsiz JWT biçiminde yapılandırılabilir.

```powershell
New-AzAttestation -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Location $location -PolicySignersCertificateFile "C:\test\policySignersCertificates.pem"
```

PolicySignersCertificateFile örnek için bkz. [ilke imzalayan sertifika örnekleri](policy-signer-examples.md).

Get-AzAttestation, Status ve AttestURI gibi kanıtlama sağlayıcısı özelliklerini alır. AttestURI ' yi daha sonra gerekli olacağı için bir yere göz atın.

```azurepowershell
Get-AzAttestation -Name $attestationProvider -ResourceGroupName $attestationResourceGroup  
```

Yukarıdaki komut aşağıdaki gibi bir çıktı üretmelidir: 

```
Id:/subscriptions/MySubscriptionID/resourceGroups/MyResourceGroup/providers/Microsoft.Attestation/attestationProviders/MyAttestationProvider
Location: MyLocation
ResourceGroupName: MyResourceGroup
Name: MyAttestationProvider
Status: Ready
TrustModel: AAD
AttestUri: https://MyAttestationProvider.us.attest.azure.net 
Tags: 
TagsTable: 
```

Kanıtlama sağlayıcıları Remove-AzAttestation cmdlet 'i kullanılarak silinebilir.  

' ' AzurePowerShell Remove-AzAttestation-Name $attestationProvider-ResourceGroupName $attestationResourceGroup
```

## Policy management

In order to manage policies, an Azure AD user requires the following permissions for "Actions":
- Microsoft.Attestation/attestationProviders/attestation/read
- Microsoft.Attestation/attestationProviders/attestation/write
- Microsoft.Attestation/attestationProviders/attestation/delete

These permissions can be assigned to an AD user through a role such as "Owner" (wildcard permissions), "Contributor" (wildcard permissions) or "Attestation Contributor" (specific permissions for Azure Attestation only).  

In order to read policies, an Azure AD user requires the following permission for "Actions":
- Microsoft.Attestation/attestationProviders/attestation/read

This permission can be assigned to an AD user through a role such as "Reader" (wildcard permissions) or "Attestation Reader" (specific permissions for Azure Attestation only).

Below PowerShell cmdlets provide policy management for an attestation provider (one TEE at a time).

Get-AzAttestationPolicy returns the current policy for the specified TEE. The cmdlet displays policy in both text and JWT format of the policy.

```powershell
$teeType = "<tee Type>"
Get-AzAttestationPolicy   -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Tee $teeType 
```

Desteklenen t türleri "sgxenclave" ve "vbsenclave".

Set-AttestationPolicy belirtilen t için yeni bir ilke ayarlar. Cmdlet, ilkeyi metin veya JWT biçiminde kabul eder ve PolicyFormat parametresi tarafından denetlenir. PolicyFormat için varsayılan değer "Text" değeridir. 

```powershell
$policyFormat = "<policy format>"
$policy=Get-Content -path "C:\test\policy.txt" -Raw
Set-AzAttestationPolicy   -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Tee $teeType -Policy $policy -PolicyFormat $policyFormat 
```

Bir kanıtlama sağlayıcısının oluşturulması sırasında PolicySignerCertificateFile sağlanırsa, ilkeler yalnızca imzalı JWT biçiminde yapılandırılabilir. Else ilkesi, metin veya işaretsiz JWT biçiminde yapılandırılabilir.

JWT biçimindeki kanıtlama ilkesi, "AttestationPolicy" adlı bir talep içermelidir. İmzalı ilke için, JWT 'nin mevcut ilke imzalayan sertifikalara karşılık gelen özel anahtarla imzalanmış olması gerekir.

İlke örnekleri için bkz. [kanıtlama ilkesi örnekleri](policy-examples.md).

Reset-AzAttestationPolicy belirtilen t için ilkeyi varsayılan olarak sıfırlar.

```powershell
Reset-AzAttestationPolicy -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Tee $teeType 
```

## <a name="policy-signer-certificates-management"></a>İlke imzalayan sertifikaları yönetimi

PowerShell cmdlet 'lerinin aşağıdaki bir kanıtlama sağlayıcısı için ilke imzalayan sertifikaları yönetimi sağlar:

```powershell
Get-AzAttestationPolicySigners -Name $attestationProvider -ResourceGroupName $attestationResourceGroup

Add-AzAttestationPolicySigner -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Signer <signer>

Remove-AzAttestationPolicySigner -Name $attestationProvider -ResourceGroupName $attestationResourceGroup -Signer <signer>
```

İlke imzalayan sertifikası, "Maa-policyCertificate" adlı talebe sahip imzalı bir JWT. Talebin değeri, eklenecek güvenilir imzalama anahtarını içeren bir JWK değeridir. JWT, mevcut ilke imzalayan sertifikalarına karşılık gelen özel anahtarla imzalanmalıdır.

İlke imzalayan sertifikasının tüm anlam düzenlemesi 'nin PowerShell dışında yapılması gerektiğini unutmayın. PowerShell 'e göre düşünüldüğünde, basit bir dizedir.

İlke imzalayan sertifika örneği için bkz. [ilke imzalayan sertifika örnekleri](policy-signer-examples.md).

Cmdlet 'ler ve parametreleri hakkında daha fazla bilgi için bkz. [Azure kanıtlama PowerShell cmdlet 'leri](/powershell/module/az.attestation/?view=azps-4.3.0#attestation) 

## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [Kod örneklerini kullanarak bir SGX kuşatma atturuntest](https://docs.microsoft.com/samples/browse/?expanded=azure&terms=attestation)
