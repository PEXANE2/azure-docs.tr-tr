---
title: Azure Hizmet Kumaşı küme sertifikasıüzerinde devir
description: Sertifika ortak adı ile tanımlanan Hizmet Kumaşı küme sertifikasını nasıl devredin öğrenin.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 94cc6841886b1b0eb4271ac0f727a2e3561e0081
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451971"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>Service Fabric küme sertifikasını el ile devir
Service Fabric küme sertifikası nın süresi dolmak üzereyken, sertifikayı güncelleştirmeniz gerekir.  Küme, [ortak ada göre](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (küçük parmak izi yerine) sertifikaları kullanacak şekilde ayarlanmışsa, sertifika devri basittir.  Yeni bir son kullanma tarihi olan bir sertifika yetkilisinden yeni bir sertifika alın.  Azure portal Küme oluşturma iş akışı sırasında oluşturulan sertifikaları içerecek şekilde üretim Hizmeti Kumaş kümeleri için kendi imzalı sertifikalar desteklenmez. Yeni sertifika, eski sertifikayla aynı ortak ada sahip olmalıdır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Service Fabric kümesi, beyan edilen sertifikayı otomatik olarak kullanır ve gelecekteki son kullanma tarihine daha fazla girer; ana bilgisayara birden fazla doğrulama sertifikası yüklendiğinde. En iyi yöntem, Azure Kaynaklarını sağlamak için Bir Kaynak Yöneticisi şablonu kullanmaktır. Üretim dışı ortam için aşağıdaki komut dosyası, yeni bir sertifikayı anahtar kasasına yüklemek ve ardından sertifikayı sanal makine ölçeğine yüklemek için kullanılabilir: 

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

>[!NOTE]
> Hesaplamalar Sanal Makine Ölçeği Set Sırları her gizli bir sürümbenzersiz kaynak olduğu gibi, iki ayrı sırlar için aynı kaynak id desteklemez. 

## <a name="next-steps"></a>Sonraki Adımlar

* Küme [güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* [Küme sertifikalarını güncelleştirme ve yönetme](service-fabric-cluster-security-update-certs-azure.md)
