---
title: Bulut hizmeti dağıtma (genişletilmiş destek)-PowerShell
description: PowerShell kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtma
ms.topic: tutorial
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 08a8dde815a6dea5d69e5e2a385cbaa03fba681a
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99832703"
---
# <a name="deploy-a-cloud-service-extended-support-using-azure-powershell"></a>Azure PowerShell kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtma

Bu makalede, `Az.CloudService` Azure 'da birden çok rol (WebRole ve WorkerRole) ve Uzak Masaüstü uzantısı olan Cloud Services (genişletilmiş destek) dağıtmak için PowerShell modülünün nasıl kullanılacağı gösterilmektedir. 

> [!IMPORTANT]
> Cloud Services (genişletilmiş destek) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Başlamadan önce

Cloud Services (genişletilmiş destek) için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin ve ilişkili kaynakları oluşturun. 

## <a name="deploy-a-cloud-services-extended-support"></a>Cloud Services Dağıtma (genişletilmiş destek)
1. Install az. CloudService PowerShell Module  

    ```powershell
    Install-Module -Name Az.CloudService 
    ```

2. Yeni bir kaynak grubu oluşturma. Mevcut bir kaynak grubu kullanılıyorsa bu adım isteğe bağlıdır.   

    ```powershell
    New-AzResourceGroup -ResourceGroupName “ContosOrg” -Location “East US” 
    ```

3. Bulut hizmeti paketi (. cspkg) ve hizmet yapılandırma (. cscfg) dosyalarını depolamak için kullanılacak bir depolama hesabı ve kapsayıcı oluşturun. Depolama hesabı adı için benzersiz bir ad kullanmanız gerekir. 

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName “ContosOrg” -Name “contosostorageaccount” -Location “East US” -SkuName “Standard_RAGRS” -Kind “StorageV2” 
    $container = New-AzStorageContainer -Name “contosocontainer” -Context $storageAccount.Context -Permission Blob 
    ```

4. Bulut hizmeti paketinizi (cspkg) depolama hesabına yükleyin.

    ```powershell
    $tokenStartTime = Get-Date 
    $tokenEndTime = $tokenStartTime.AddYears(1) 
    $cspkgBlob = Set-AzStorageBlobContent -File “./ContosoApp/ContosoApp.cspkg” -Container “contosocontainer” -Blob “ContosoApp.cspkg” -Context $storageAccount.Context 
    $cspkgToken = New-AzStorageBlobSASToken -Container “contosocontainer” -Blob $cspkgBlob.Name -Permission rwd -StartTime $tokenStartTime -ExpiryTime $tokenEndTime -Context $storageAccount.Context 
    $cspkgUrl = $cspkgBlob.ICloudBlob.Uri.AbsoluteUri + $cspkgToken 
    ```
 

5.  Bulut hizmeti yapılandırmanızı (cscfg) depolama hesabına yükleyin. 

    ```powershell
    $cscfgBlob = Set-AzStorageBlobContent -File “./ContosoApp/ContosoApp.cscfg” -Container contosocontainer -Blob “ContosoApp.cscfg” -Context $storageAccount.Context 
    $cscfgToken = New-AzStorageBlobSASToken -Container “contosocontainer” -Blob $cscfgBlob.Name -Permission rwd -StartTime $tokenStartTime -ExpiryTime $tokenEndTime -Context $storageAccount.Context 
    $cscfgUrl = $cscfgBlob.ICloudBlob.Uri.AbsoluteUri + $cscfgToken 
    ```

6. Sanal ağı ve alt ağı oluşturun. Mevcut bir ağ ve alt ağ kullanılıyorsa bu adım isteğe bağlıdır. Bu örnek, hem bulut hizmeti rolleri (WebRole hem de WorkerRole) için tek bir sanal ağ ve alt ağ kullanır. 

    ```powershell
    $subnet = New-AzVirtualNetworkSubnetConfig -Name "ContosoWebTier1" -AddressPrefix "10.0.0.0/24" -WarningAction SilentlyContinue 
    $virtualNetwork = New-AzVirtualNetwork -Name “ContosoVNet” -Location “East US” -ResourceGroupName “ContosOrg” -AddressPrefix "10.0.0.0/24" -Subnet $subnet 
    ```
 
7. Genel IP adresi oluşturun ve (isteğe bağlı olarak) genel IP adresinin DNS etiketi özelliğini ayarlayın. Statik IP kullanıyorsanız, hizmet yapılandırma dosyasında bir Ayrılmış IP olarak başvurulması gerekir.  

    ```powershell
    $publicIp = New-AzPublicIpAddress -Name “ContosIp” -ResourceGroupName “ContosOrg” -Location “East US” -AllocationMethod Dynamic -IpAddressVersion IPv4 -DomainNameLabel “contosoappdns” -Sku Basic 
    ```

8. Ağ profili nesnesi oluşturun ve genel IP adresini platform tarafından oluşturulan yük dengeleyicinin ön ucu ile ilişkilendirin.  

    ```powershell
    $publicIP = Get-AzPublicIpAddress -ResourceGroupName ContosOrg -Name ContosIp  
    $feIpConfig = New-AzCloudServiceLoadBalancerFrontendIPConfigurationObject -Name 'ContosoFe' -PublicIPAddressId $publicIP.Id 
    $loadBalancerConfig = New-AzCloudServiceLoadBalancerConfigurationObject -Name 'ContosoLB' -FrontendIPConfiguration $feIpConfig 
    $networkProfile = @{loadBalancerConfiguration = $loadBalancerConfig} 
    ```
 
9. Anahtar Kasası oluşturun. Bu Key Vault, bulut hizmeti (genişletilmiş destek) rolleriyle ilişkili sertifikaları depolamak için kullanılacaktır. ' Dağıtım için Azure sanal makinelerine ' ve ' şablon dağıtımı için ' Azure Resource Manager erişim için ' erişim ilkelerini ' (portalda) etkinleştirdiğinizden emin olun. Key Vault, bulut hizmeti ile aynı bölgede ve abonelikte yer almalıdır ve benzersiz bir ada sahip olmalıdır. Daha fazla bilgi için bkz. [Azure Cloud Services sertifikaları kullanma (genişletilmiş destek)](certificates-and-key-vault.md).

    ```powershell
    New-AzKeyVault -Name "ContosKeyVault” -ResourceGroupName “ContosOrg” -Location “East US” 
    ```

10. Key Vault erişim ilkesini güncelleştirin ve Kullanıcı hesabınıza sertifika izinleri verin. 

    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosKeyVault' -ResourceGroupName 'ContosOrg' -UserPrincipalName 'user@domain.com' -PermissionsToCertificates create,get,list,delete 
    ```

    Alternatif olarak, erişim ilkesini ObjectID aracılığıyla (çalıştırılarak elde edilebilir `Get-AzADUser` ) ayarlayın 
    
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosKeyVault' -ResourceGroupName 'ContosOrg' -ObjectId 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' -PermissionsToCertificates create,get,list,delete 
    ```
 

11. Bu örnekte, bir Key Vault otomatik olarak imzalanan bir sertifika ekleyeceğiz. Sertifika parmak izinin, bulut hizmeti rollerinde dağıtım için bulut hizmeti yapılandırma (. cscfg) dosyasına eklenmesi gerekir. 

    ```powershell
    $Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName "Self" -ValidityInMonths 6 -ReuseKeyOnRenewal 
    Add-AzKeyVaultCertificate -VaultName "ContosKeyVault" -Name "ContosCert" -CertificatePolicy $Policy 
    ```
 
12. Bellek içi bir nesne için bir işletim sistemi profili oluşturun. İşletim sistemi profili, bulut hizmeti rolleriyle ilişkili sertifikaları belirtir. Bu, önceki adımda oluşturulan sertifika ile aynı olacaktır. 

    ```powershell
    $keyVault = Get-AzKeyVault -ResourceGroupName ContosOrg -VaultName ContosKeyVault 
    $certificate = Get-AzKeyVaultCertificate -VaultName ContosKeyVault -Name ContosCert 
    $secretGroup = New-AzCloudServiceVaultSecretGroupObject -Id $keyVault.ResourceId -CertificateUrl $certificate.SecretId 
    $osProfile = @{secret = @($secretGroup)} 
    ```

13. Bellek içi nesne için bir rol profili oluşturun. Rol profili, ad, kapasite ve katman gibi bir rol SKU 'ya özgü özellikleri tanımlar. Bu örnekte, iki rol tanımlıyoruz: frontendRole ve backendRole. Rol profili bilgileri, yapılandırma (cscfg) dosya ve hizmet tanımı (csdef) dosyasında tanımlanan rol yapılandırması ile eşleşmelidir. 

    ```powershell
    $frontendRole = New-AzCloudServiceRoleProfilePropertiesObject -Name 'ContosoFrontend' -SkuName 'Standard_D1_v2' -SkuTier 'Standard' -SkuCapacity 2 
    $backendRole = New-AzCloudServiceRoleProfilePropertiesObject -Name 'ContosoBackend' -SkuName 'Standard_D1_v2' -SkuTier 'Standard' -SkuCapacity 2 
    $roleProfile = @{role = @($frontendRole, $backendRole)} 
    ```

14. Seçim Bulut hizmetinize eklemek istediğiniz bir uzantı profili bellek içi nesnesi oluşturun. Bu örnekte, RDP uzantısı ekleyeceğiz. 

    ```powershell
    $credential = Get-Credential 
    $expiration = (Get-Date).AddYears(1) 
    $rdpExtension = New-AzCloudServiceRemoteDesktopExtensionObject -Name 'RDPExtension' -Credential $credential -Expiration $expiration -TypeHandlerVersion '1.2.1' 

    $storageAccountKey = Get-AzStorageAccountKey -ResourceGroupName "ContosOrg" -Name "contosostorageaccount"
    $configFile = "<WAD public configuration file path>"
    $wadExtension = New-AzCloudServiceDiagnosticsExtension -Name "WADExtension" -ResourceGroupName "ContosOrg" -CloudServiceName "ContosCS" -StorageAccountName "contosostorageaccount" -StorageAccountKey $storageAccountKey[0].Value -DiagnosticsConfigurationPath $configFile -TypeHandlerVersion "1.5" -AutoUpgradeMinorVersion $true 
    $extensionProfile = @{extension = @($rdpExtension, $wadExtension)} 
    ```
    ConfigFile 'ın yalnızca PublicConfig etiketleri olması gerektiğini ve aşağıdaki gibi bir ad alanı içermesi gerektiğini unutmayın:
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        ...............
    </PublicConfig>
    ```
15. Seçim Bulut hizmetinize eklemek istediğiniz etiketleri PowerShell karma tablosu olarak tanımlayın. 

    ```powershell
    $tag=@{"Owner" = "Contoso"} 
    ```

17. SAS URL 'Leri & profil nesneleri kullanarak bulut hizmeti dağıtımı oluşturun.

    ```powershell
    $cloudService = New-AzCloudService ` 
    -Name “ContosoCS” ` 
    -ResourceGroupName “ContosOrg” ` 
    -Location “East US” ` 
    -PackageUrl $cspkgUrl ` 
    -ConfigurationUrl $cscfgUrl ` 
    -UpgradeMode 'Auto' ` 
    -RoleProfile $roleProfile ` 
    -NetworkProfile $networkProfile  ` 
    -ExtensionProfile $extensionProfile ` 
    -OSProfile $osProfile `
    -Tag $tag 
    ```

## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.
- [Cloud Services (genişletilmiş destek) örnekleri deposunu](https://github.com/Azure-Samples/cloud-services-extended-support) ziyaret edin
