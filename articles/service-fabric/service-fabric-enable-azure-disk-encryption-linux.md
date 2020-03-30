---
title: Linux kümeleri için disk şifrelemeyi etkinleştirme
description: Bu makalede, Azure Kaynak Yöneticisi ve Azure Anahtar Kasası kullanarak Linux'taki Azure Hizmet Dokusu küme düğümleri için disk şifrelemenin nasıl etkinleştirilen açıklanmaktadır.
ms.topic: article
ms.date: 03/22/2019
ms.openlocfilehash: c600d822d20b0e5a0ca613935b1dfa4be838fcec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252824"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-linux"></a>Linux'ta Azure Service Fabric küme düğümleri için disk şifrelemesini etkinleştirme 
> [!div class="op_single_selector"]
> * [Linux için Disk Şifreleme](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Windows için Disk Şifreleme](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Bu eğitimde, Linux'taki Azure Service Fabric küme düğümlerinde disk şifrelemeyi nasıl etkinleştireceğinizi öğreneceksiniz. Düğüm türlerinin ve sanal makine ölçek kümelerinin her biri için aşağıdaki adımları izlemeniz gerekir. Düğümleri şifrelemek için, sanal makine ölçeği kümelerinde Azure Disk Şifreleme özelliğini kullanırız.

Kılavuz aşağıdaki konuları kapsar:

* Linux'ta Service Fabric küme sanal makine ölçek kümelerinde disk şifrelemesini etkinleştirirken dikkat edilmesi gereken temel kavramlar.
* Linux'taki Service Fabric küme düğümlerinde disk şifrelemesini etkinleştirmeden önce izlenecek adımlar.
* Linux'taki Service Fabric küme düğümlerinde disk şifrelemesini etkinleştirmek için izlenecek adımlar.



[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

 **Kendi kendine kayıt**

Sanal makine ölçeği kümesi için disk şifreleme önizlemesi kendi kendine kayıt gerektirir. Aşağıdaki adımları kullanın:

1. Şu komutu çalıştırın: 
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Durum *Kayıtlı'yı*okuyana kadar yaklaşık 10 dakika bekleyin. Aşağıdaki komutu çalıştırarak durumu kontrol edebilirsiniz:
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Key Vault**

1. Ölçek kümesiyle aynı abonelik te ve bölgede bir anahtar kasaoluşturun. Ardından PowerShell cmdlet'ini kullanarak anahtar kasasındaki **EnabledForDiskEncryption** erişim ilkesini seçin. Azure portalındaki Key Vault UI'sini aşağıdaki komutla kullanarak da ilkeyi ayarlayabilirsiniz:
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
* Hem şifreleme hem de devre dışı sayılsa şifreleme, Linux'taki sanal makine ölçeği kümelerinde işletim sistemi ve veri hacimleri için desteklenir. 
* Sanal makine ölçek kümeleri için sanal makine (VM) yeniden görüntülenme ve yükseltme işlemleri geçerli önizlemede desteklenmez.


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

Özel bir şablon yazmanız gerekiyorsa, Azure Hizmet Dokusu küme oluşturma [şablonu örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) sayfasındaki şablonlardan birini kullanmanızı şiddetle öneririz. 

Zaten özel bir şablonunuz varsa, şablondaki ve parametre dosyasındaki sertifikayla ilgili üç parametrenin de aşağıdaki gibi adlandırıldığını iki kez denetleyin. Ayrıca, değerlerin aşağıdaki gibi null olduğundan emin olun:

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

Linux'taki sanal makine ölçek kümeleri için yalnızca veri diski şifrelemesi desteklendirilebildiği için, Kaynak Yöneticisi şablonu kullanarak bir veri diski eklemeniz gerekir. Veri diski sağlama şablonunuzu aşağıdaki gibi güncelleştirin:

```Json
   
   "storageProfile": { 
            "imageReference": { 
              "publisher": "[parameters('vmImagePublisher')]", 
              "offer": "[parameters('vmImageOffer')]", 
              "sku": "[parameters('vmImageSku')]", 
              "version": "[parameters('vmImageVersion')]" 
            }, 
            "osDisk": { 
              "caching": "ReadOnly", 
              "createOption": "FromImage", 
              "managedDisk": { 
                "storageAccountType": "[parameters('storageAccountType')]" 
              } 
           }, 
                "dataDisks": [ 
                { 
                    "diskSizeGB": 1023, 
                    "lun": 0, 
                    "createOption": "Empty" 
   
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

Burada eşdeğer CLI komutu. Beyan ifadelerindeki değerleri uygun değerlerle değiştirin. CLI, önceki PowerShell komutunun desteklediği diğer tüm parametreleri destekler.

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

### <a name="mount-a-data-disk-to-a-linux-instance"></a>Veri diskini Linux örneğine monte etme
Sanal makine ölçeği kümesinde şifrelemeye devam etmeden önce, eklenen veri diskinin doğru şekilde monte edilmiş olduğundan emin olun. Linux kümesi VM'de oturum açın ve **LSBLK** komutunu çalıştırın. Çıktı, **Mount Point** sütununa eklenen veri diskini göstermelidir.


### <a name="deploy-application-to-a-service-fabric-cluster-in-linux"></a>Uygulamayı Linux'taki Service Fabric kümesine dağıtma
Bir uygulamayı kümenize dağıtmak için Quickstart'taki adımları ve kılavuzu [izleyin: Linux kaplarını Service Fabric'e dağıtın.](service-fabric-quickstart-containers-linux.md)


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

### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-linux"></a>Linux'ta ayarlanmış sanal makine ölçeği için disk şifreleme etkinleştirilmişse doğrulama
Tüm sanal makine ölçeği kümesinin veya bir ölçek kümesindeki herhangi bir örneğin durumunu almak için aşağıdaki komutları çalıştırın.
Ayrıca, Linux kümesi VM'de oturum açabilir ve **LSBLK** komutunu çalıştırabilirsiniz. Çıktı, **Mount Point** sütununa eklenen veri diskini göstermeli ve **Tür** sütununda *Crypt'i*okumalıdır.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli
az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```

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
Bu noktada, güvenli bir kümeye sahip olmalı ve Service Fabric küme düğümleri ve sanal makine ölçek kümeleri için disk şifrelemesini nasıl etkinleştirip devre dışı kacağınıbilmeniz gerekir. Linux'taki Service Fabric küme düğümleri ile ilgili benzer bir kılavuz [için Windows için Disk Şifreleme'ye](service-fabric-enable-azure-disk-encryption-windows.md)bakın. 
