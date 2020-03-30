---
title: Windows kümeleri için disk şifrelemeyi etkinleştirme
description: Bu makalede, Azure Kaynak Yöneticisi'nde Azure Anahtar Kasası'nı kullanarak Azure Hizmet Dokusu küme düğümleri için disk şifrelemenin nasıl etkinleştirilen olduğu açıklanmaktadır.
ms.topic: article
ms.date: 03/22/2019
ms.openlocfilehash: b08cdb63aa6f334c5a6f7c230b1624d232206c3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78251811"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-windows"></a>Windows'da Azure Hizmet Kumaş küme düğümleri için disk şifrelemesini etkinleştirme 
> [!div class="op_single_selector"]
> * [Windows için Disk Şifreleme](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Linux için Disk Şifreleme](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Bu eğitimde, Windows'daki Service Fabric küme düğümlerinde disk şifrelemeyi nasıl etkinleştireceğinizi öğreneceksiniz. Düğüm türlerinin ve sanal makine ölçek kümelerinin her biri için aşağıdaki adımları izlemeniz gerekir. Düğümleri şifrelemek için, sanal makine ölçeği kümelerinde Azure Disk Şifreleme özelliğini kullanırız.

Kılavuz aşağıdaki konuları kapsar:

* Windows'da Service Fabric küme sanal makine ölçek kümelerinde disk şifrelemesini etkinleştirirken dikkat edilmesi gereken temel kavramlar.
* Windows'daki Service Fabric küme düğümlerinde disk şifrelemesini etkinleştirmeden önce izlenecek adımlar.
* Windows'daki Service Fabric küme düğümlerinde disk şifrelemesini etkinleştirmek için izlenecek adımlar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

**Kendi kendine kayıt** 

Sanal makine ölçeği kümesi için disk şifreleme önizlemesi kendi kendine kayıt gerektirir. Aşağıdaki adımları kullanın: 

1. İlk olarak, aşağıdaki komutu çalıştırın:
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Durum *Kayıtlı'yı*okuyana kadar yaklaşık 10 dakika bekleyin. Aşağıdaki komutu çalıştırarak durumu kontrol edebilirsiniz: 
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Key Vault** 

1. Ölçek kümesiyle aynı abonelik ve bölgede bir anahtar kasası oluşturun ve ardından PowerShell cmdlet'ini kullanarak anahtar **kasasındaki EnabledForDiskEncryption** erişim ilkesini seçin. Azure portalındaki Key Vault UI'sini aşağıdaki komutla kullanarak da ilkeyi ayarlayabilirsiniz:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. Yeni şifreleme komutlarına sahip [Olan Azure CLI'nin](/cli/azure/install-azure-cli)en son sürümünü yükleyin.
3. [Azure PowerShell sürümünden Azure SDK'nın](https://github.com/Azure/azure-powershell/releases) en son sürümünü yükleyin. Aşağıda, sanal makine ölçeğinde , şifrelemeyi etkinleştirmek ([ayarlamak) (set)](/powershell/module/az.compute/set-azvmssdiskencryptionextension)şifreleme durumunu ve ölçek kümesi örneğindeki şifrelemeyi kaldırmak[(devre dışı)](/powershell/module/az.compute/disable-azvmssdiskencryption)şifrelemeyi etkinleştirmek için Azure Disk Şifreleme cmdlets'i ayarlanır.[get](/powershell/module/az.compute/get-azvmssvmdiskencryption)

| Komut | Sürüm |  Kaynak  |
| ------------- |-------------| ------------|
| Al-AzVmssDiskŞifreleme Durumu   | 1.0.0 veya sonrası | Az.Compute |
| Al-AzVmssVMDiskŞifreleme Durumu   | 1.0.0 veya sonrası | Az.Compute |
| Devre Dışı-AzVmssDiskŞifreleme   | 1.0.0 veya sonrası | Az.Compute |
| Al-AzVmssDiskŞifreleme   | 1.0.0 veya sonrası | Az.Compute |
| Al-AzVmssVMDiskŞifreleme   | 1.0.0 veya sonrası | Az.Compute |
| Set-AzVmssDiskŞifreleme Uzantısı   | 1.0.0 veya sonrası | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Disk şifreleme için desteklenen senaryolar
* Sanal makine ölçek kümeleri için şifreleme yalnızca yönetilen disklerle oluşturulan ölçek kümeleri için desteklenir. Yerel (veya yönetilmeyen) disk ölçeği kümeleri için desteklenmez.
* Şifreleme, Windows'daki sanal makine ölçeği kümelerinde işletim sistemi ve veri birimleri için desteklenir. Devre dışı kşifreleme, Windows'daki sanal makine ölçeği kümeleri için işletim sistemi ve veri birimleri için de desteklenir.
* Sanal makine ölçek kümeleri için sanal makine yeniden görüntülenme ve yükseltme işlemleri geçerli önizlemede desteklenmez.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Yeni bir küme oluşturun ve disk şifrelemeyi etkinleştirin

Azure Kaynak Yöneticisi şablonu ve kendi imzalı sertifika kullanarak bir küme oluşturmak ve disk şifrelemesini etkinleştirmek için aşağıdaki komutları kullanın.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma 
Aşağıdaki komutlarla oturum açın:
```powershell
Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Zaten sahip olduğunuz özel şablonu kullanma 

İhtiyaçlarınıza uygun özel bir şablon yazmanız gerekiyorsa, [Azure Hizmet Dokusu küme oluşturma şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) sayfasında bulunan şablonlardan biriyle başlamanızı şiddetle öneririz. [Küme şablonu bölümünüzü özelleştirmek][customize-your-cluster-template] için aşağıdaki kılavuza bakın.

Zaten özel bir şablonunuz varsa, şablondaki ve parametre dosyasındaki sertifikayla ilgili üç parametrenin de aşağıdaki gibi adlandırıldığını ve değerlerin aşağıdaki gibi geçersiz olduğunu iki kez denetleyin:

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```azurecli

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

### <a name="deploy-an-application-to-a-service-fabric-cluster-in-windows"></a>Windows'daki Bir Hizmet Kumaşı kümesine uygulama dağıtma
Bir uygulamayı kümenize dağıtmak için, Dağıt'taki adımları ve kılavuzu izleyin [ve PowerShell'i kullanarak uygulamaları kaldırın.](service-fabric-deploy-remove-applications.md)


### <a name="enable-disk-encryption-for-the-virtual-machine-scale-sets-created-previously"></a>Daha önce oluşturulan sanal makine ölçek kümeleri için disk şifrelemeyi etkinleştirme

Önceki adımlarda oluşturduğunuz sanal makine ölçeği kümeleri için disk şifrelemesini etkinleştirmek için aşağıdaki komutları çalıştırın:
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-windows"></a>Windows'da ayarlanan sanal makine ölçeği için disk şifrelemeetkinse doğrulama
Aşağıdaki komutları çalıştırarak bir ölçek kümesinde tüm sanal makine ölçeği kümesinin veya herhangi bir örneğin durumunu alın.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


Ayrıca, sanal makine ölçeği kümesinde oturum açabilir ve sürücülerin şifrelenmiş olduğundan emin olabilirsiniz.

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Service Fabric kümesinde ayarlanan sanal makine ölçeği için disk şifrelemeyi devre dışı bırak 
Aşağıdaki komutları çalıştırarak ayarlanan sanal makine ölçeği için disk şifrelemesini devre dışı bırak. Disk şifrelemesini devre dışı bırakmanın tek bir örnek için değil, tüm sanal makine ölçeği kümesi için geçerli olduğunu unutmayın.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```azurecli

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, güvenli bir kümeye sahip olmalı ve Service Fabric küme düğümleri ve sanal makine ölçek kümeleri için disk şifrelemesini nasıl etkinleştirip devre dışı kacağınıbilmeniz gerekir. Linux'taki Service Fabric küme düğümleri ile ilgili benzer kılavuz için [Linux için Disk Şifreleme bölümüne](service-fabric-enable-azure-disk-encryption-linux.md)bakın.

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template
