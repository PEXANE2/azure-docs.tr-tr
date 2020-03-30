---
title: Sertifika ortak adını kullanmak için kümeyi güncelleştirme
description: Hizmet Kumaşı kümesini sertifika parmak izlerini kullanmaktan sertifika ortak adını kullanmaya nasıl değiştirebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 66c49ccb7b7633d0eff392b676bb381118eb64a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610208"
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Kümeyi sertifika parmak izinden ortak ada değiştirme
Hiçbir iki sertifika aynı parmak izine sahip olamaz, bu da küme sertifikası rollover veya yönetimi zorlaştırır. Ancak, birden çok sertifika aynı ortak ada veya özneye sahip olabilir.  Dağıtılan bir kümeyi sertifika parmak izlerini kullanmaktan sertifika ortak adlarını kullanmaya geçmek sertifika yönetimini çok daha basit hale getirir. Bu makalede, sertifika parmak izi yerine sertifika ortak adını kullanmak için çalışan bir Hizmet Dokusu kümesini nasıl güncelleştirileştirileştirilir.

>[!NOTE]
> Şablonunuzda iki parmak izi beyan edildiyse, iki dağıtım gerçekleştirmeniz gerekir.  İlk dağıtım, bu makaledeki adımları izleyerek önce yapılır.  İlk dağıtım, şablondaki **parmak izi** özelliğinizi kullanılan sertifikaya ayarlar ve **thumbprintSecondary** özelliğini kaldırır.  İkinci dağıtım için bu makaledeki adımları izleyin.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-a-certificate"></a>Sertifika alın
İlk olarak, bir [sertifika yetkilisinden (CA)](https://wikipedia.org/wiki/Certificate_authority)sertifika alın.  Sertifikanın ortak adı kümenin ana bilgisayarı olmalıdır.  Örneğin, "myclustername.southcentralus.cloudapp.azure.com".  

Test amacıyla, ücretsiz veya açık sertifika yetkilisinden CA imzalı sertifika alabilirsiniz.

> [!NOTE]
> Azure portalında Hizmet Kumaşı kümesi dağıtılırken oluşturulanlar da dahil olmak üzere kendi imzalı sertifikalar desteklenmez.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Sertifikayı yükleyin ve ölçek kümesine yükleyin
Azure'da, Bir Hizmet Kumaşı kümesi sanal makine ölçeği kümesinde dağıtılır.  Sertifikayı anahtar kasasına yükleyin ve ardından kümenin üzerinde çalışan sanal makine ölçeğine yükleyin.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName `
    -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzKeyVaultCertificate -VaultName $vaultName -Name $certName `
    -FilePath $certFilename -Password $PasswordSec

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
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault `
    -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Verbose `
    -Name $VmssName -VirtualMachineScaleSet $vmss 
```

>[!NOTE]
> Ölçek kümesi sırlar, her sır sürümlü, benzersiz bir kaynak olduğundan, iki ayrı sırlar için aynı kaynak kimliğini desteklemez. 

## <a name="download-and-update-the-template-from-the-portal"></a>Şablonu portaldan indirin ve güncelleyin
Sertifika temel ölçek kümesine yüklendi, ancak bu sertifikayı ve ortak adını kullanmak için Hizmet Dokusu kümesini güncelleştirmeniz de gerekir.  Şimdi, küme dağıtımınız için şablonu indirin.  [Azure portalında](https://portal.azure.com) oturum açın ve kümeyi barındıran kaynak grubuna gidin.  **Ayarlar'da** **Dağıtımlar'ı**seçin.  En son dağıtımı seçin ve **şablonu görüntüle'yi**tıklatın.

![Şablonları görüntüleme][image1]

Şablon ve parametreleri JSON dosyalarını yerel bilgisayarınıza indirin.

İlk olarak, bir metin düzenleyicisi parametre dosyasını açın ve aşağıdaki parametre değeri ekleyin:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Ardından, şablon dosyasını bir metin düzenleyicisinde açın ve sertifika ortak adını desteklemek için üç güncelleştirme yapın.

1. **Parametreler** bölümünde, bir *sertifikaCommonName* parametresi ekleyin:
    ```json
    "certificateCommonName": {
        "type": "string",
        "metadata": {
            "description": "Certificate Commonname"
        }
    },
    ```

    Ayrıca *sertifikaThumbprint*kaldırmayı düşünün, artık Kaynak Yöneticisi şablonunda başvurulabilir.

2. **Microsoft.Compute/virtualMachineScaleSets** kaynağında, küçük parmak izi yerine sertifika ayarlarında ortak adı kullanmak için sanal makine uzantısını güncelleştirin.  **virtualMachineProfile**->**uzantılarındaProfil**->**extensions**->uzantıları**özellikleri**-> `"commonNames": ["[parameters('certificateCommonName')]"],` **ayarları**-> `"thumbprint": "[parameters('certificateThumbprint')]",`**sertifika,** eklemek ve kaldırın .
    ```json
        "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                            "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                            "nodeTypeRef": "[variables('vmNodeType0Name')]",
                            "dataPath": "D:\\SvcFab",
                            "durabilityLevel": "Bronze",
                            "enableParallelJobs": true,
                            "nicPrefixOverride": "[variables('subnet0Prefix')]",
                            "certificate": {
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        "typeHandlerVersion": "1.0"
                    }
                },
    ```

3.  **Microsoft.ServiceFabric/clusters** kaynağında, API sürümünü "2018-02-01" olarak güncelleyin.  Ayrıca, **commonAd** özelliğine sahip bir **sertifikaCommonNames** ayarı ekleyin ve aşağıdaki örnekte olduğu gibi **sertifika** ayarını (parmak izi özelliğiyle) kaldırın:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
            "addonFeatures": [
                "DnsService",
                "RepairManager"
            ],
            "certificateCommonNames": {
                "commonNames": [
                    {
                        "certificateCommonName": "[parameters('certificateCommonName')]",
                        "certificateIssuerThumbprint": ""
                    }
                ],
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
    ```

Ek bilgi için [bkz.](https://docs.microsoft.com/azure/service-fabric/service-fabric-create-cluster-using-cert-cn)

## <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Değişiklikleri yaptıktan sonra güncelleştirilmiş şablonu yeniden dağıtın.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="next-steps"></a>Sonraki adımlar
* Küme [güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* Küme sertifikasını nasıl [devratacaklarını](service-fabric-cluster-rollover-cert-cn.md) öğrenin
* [Küme sertifikalarını güncelleştirme ve yönetme](service-fabric-cluster-security-update-certs-azure.md)

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
