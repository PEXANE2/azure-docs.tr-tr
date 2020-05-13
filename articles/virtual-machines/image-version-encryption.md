---
title: Önizleme-kendi anahtarlarınız ile şifrelenmiş bir görüntü sürümü oluşturma
description: Paylaşılan görüntü galerisinde, müşteri tarafından yönetilen şifreleme anahtarlarını kullanarak bir görüntü sürümü oluşturun.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/06/2020
ms.author: cynthn
ms.openlocfilehash: aeacfdc07e5349dfce45b209da1d78bddf870f33
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83269589"
---
# <a name="preview-use-customer-managed-keys-for-encrypting-images"></a>Önizleme: görüntüleri şifrelemek için müşteri tarafından yönetilen anahtarları kullanın

Galeri görüntüleri yönetilen diskler olarak depolanır, bu nedenle otomatik olarak sunucu tarafı şifreleme kullanılarak şifrelenir. Sunucu tarafı şifreleme, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanır, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure yönetilen diskleri temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz [. şifreleme API 'si: yeni nesil](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

Görüntülerinizin şifrelenmesi için platform tarafından yönetilen anahtarları kullanabilir veya kendi anahtarlarınızı kullanarak şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz, görüntülerinizdeki tüm diskleri şifrelemek ve şifrelerini çözmek için kullanılacak *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. 

Müşteri tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifreleme Azure Key Vault kullanır. [RSA anahtarlarınızı](../key-vault/keys/hsm-protected-keys.md) Key Vault içeri aktarabilir ya da Azure Key Vault yeni RSA anahtarları oluşturabilirsiniz.

Görüntüler için müşteri tarafından yönetilen anahtarları kullanmak için önce bir Azure Key Vault gerekir. Daha sonra bir disk şifreleme kümesi oluşturursunuz. Daha sonra görüntü sürümlerini oluştururken disk şifreleme kümesi kullanılır.

Disk şifreleme kümelerini oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [müşteri tarafından yönetilen anahtarlar](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys).

## <a name="limitations"></a>Sınırlamalar

Paylaşılan görüntü Galerisi görüntülerini şifrelemek için müşteri tarafından yönetilen anahtarlar kullanılırken çeşitli sınırlamalar vardır:  

- Şifreleme anahtarı kümeleri, yansımasıyla aynı abonelikte ve bölgede olmalıdır.

- Müşteri tarafından yönetilen anahtarlar kullanan resimleri paylaşamazsınız. 

- Müşteri tarafından yönetilen anahtarları kullanan görüntüleri diğer bölgelere çoğaltamaz.

- Bir diski veya görüntüyü şifrelemek için kendi anahtarlarınızı kullandıysanız, bu diskleri veya görüntüleri şifrelemek için platform tarafından yönetilen anahtarları kullanmaya geri dönemezsiniz.


> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarları kullanarak şifreleme Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="powershell"></a>PowerShell

Genel önizleme için önce özelliği kaydetmeniz gerekir.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

Kaydın tamamlanabilmesi birkaç dakika sürer. Özellik kaydının durumunu denetlemek için Get-AzProviderFeature komutunu kullanın.

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

RegistrationState kaydedildiğinde, sonraki adıma geçebilirsiniz.

Sağlayıcı kaydınızı denetleyin. Döndürdüğünden emin olun `Registered` .

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Format-table -Property ResourceTypes,RegistrationState
```

Döndürmezse `Registered` , sağlayıcıları kaydetmek için aşağıdakileri kullanın:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

Bir görüntü sürümü için ayarlanmış bir disk şifrelemesi belirtmek için, parametresiyle [New-Azgallerımagedefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion) komutunu kullanın `-TargetRegion` . 

```azurepowershell-interactive

$sourceId = <ID of the image version source>

$osDiskImageEncryption = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet'}

$dataDiskImageEncryption1 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet1';Lun=1}

$dataDiskImageEncryption2 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet2';Lun=2}

$dataDiskImageEncryptions = @($dataDiskImageEncryption1,$dataDiskImageEncryption2)

$encryption1 = @{OSDiskImage=$osDiskImageEncryption;DataDiskImages=$dataDiskImageEncryptions}

$region1 = @{Name='West US';ReplicaCount=1;StorageAccountType=Standard_LRS;Encryption=$encryption1}

$targetRegion = @{$region1}


# Create the image
New-AzGalleryImageVersion `
   -ResourceGroupName $rgname `
   -GalleryName $galleryName `
   -GalleryImageDefinitionName $imageDefinitionName `
   -Name $versionName -Location $location `
   -SourceImageId $sourceId `
   -ReplicaCount 2 `
   -StorageAccountType Standard_LRS `
   -PublishingProfileEndOfLifeDate '2020-12-01' `
   -TargetRegion $targetRegion
```

### <a name="create-a-vm"></a>VM oluşturma

Paylaşılan görüntü galerisinden bir VM oluşturabilir ve diskleri şifrelemek için müşterinin yönettiği anahtarları kullanabilirsiniz. Söz dizimi, bir görüntüden [Genelleştirilmiş](vm-generalized-image-version-powershell.md) veya [özel](vm-specialized-image-version-powershell.md) bir VM oluşturma ile aynıdır, genişletilmiş parametre kümesini kullanmanız ve `Set-AzVMOSDisk -Name $($vmName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage` VM yapılandırmasına eklemeniz gerekir.

Veri disklerinde, `-DiskEncryptionSetId $setID` [Add-AzVMDataDisk](/powershell/module/az.compute/add-azvmdatadisk)kullandığınızda parametresini eklemeniz gerekir.


## <a name="cli"></a>CLI 

Genel önizleme için önce özelliği kaydetmeniz gerekir.

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name SIGEncryption
```

Özellik kaydının durumunu denetleyin.

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name SIGEncryption | grep state
```

Bu döndürüldüğünde bir `"state": "Registered"` sonraki adıma geçebilirsiniz.

Kaydınızı denetleyin.

```azurecli-interactive
az provider show -n Microsoft.Compute | grep registrationState
```

Kayıtlı değilse, aşağıdakileri çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.Compute
```


Bir görüntü sürümü için ayarlanmış bir disk şifrelemesi belirtmek için, parametresiyle [az Image Gallery Create-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create) kullanın `--target-region-encryption` . Biçimi, `--target-region-encryption` işletim sistemi ve veri disklerini şifrelemek için bir anahtarlar ve bir boşluk ayrılmış listesidir. Şöyle görünmelidir: `<encryption set for the OS disk>,<Lun number of the data disk>, <encryption set for the data disk>, <Lun number for the second data disk>, <encryption set for the second data disk>` . 

İşletim sistemi diskinin kaynağı yönetilen bir disk veya VM ise, `--managed-image` görüntü sürümü kaynağını belirtmek için öğesini kullanın. Bu örnekte, kaynak, LUN 0 ' da bir işletim sistemi diskine ve veri diskine sahip olan yönetilen bir görüntüdür. İşletim sistemi diski DiskEncryptionSet1 ile şifrelenir ve veri diski DiskEncryptionSet2 ile şifrelenir.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --target-regions westus=2=standard_lrs \
   --target-region-encryption DiskEncryptionSet1,0,DiskEncryptionSet2 \
   --gallery-name MyGallery \
   --gallery-image-definition MyImage \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

İşletim sistemi diskinin kaynağı bir anlık görüntüdür, `--os-snapshot` işletim sistemi diskini belirtmek için kullanın. Aynı zamanda görüntü sürümünün parçası olması gereken veri diski anlık görüntüleri varsa, bunu `--data-snapshot-luns` LUN 'u belirtmek için kullanarak ve `--data-snapshots` anlık görüntüleri belirtecek şekilde ekleyin.

Bu örnekte, kaynaklar disk anlık görüntüleridir. LUN 0 ' da bir işletim sistemi diski ve bir veri diski vardır. İşletim sistemi diski DiskEncryptionSet1 ile şifrelenir ve veri diski DiskEncryptionSet2 ile şifrelenir.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --target-regions westus=2=standard_lrs \
   --target-region-encryption DiskEncryptionSet1,0,DiskEncryptionSet2 \
   --os-snapshot "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myOSSnapshot"
   --data-snapshot-luns 0
   --data-snapshots "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myDDSnapshot"
   --gallery-name MyGallery \
   --gallery-image-definition MyImage 
   
```

### <a name="create-the-vm"></a>Sanal makine oluşturma

Paylaşılan görüntü galerisinden bir VM oluşturabilir ve diskleri şifrelemek için müşterinin yönettiği anahtarları kullanabilirsiniz. Söz dizimi, bir görüntüden [Genelleştirilmiş](vm-generalized-image-version-cli.md) veya [özel](vm-specialized-image-version-cli.md) bir VM oluşturma ile aynıdır; yalnızca, `--os-disk-encryption-set` şifreleme kümesinin kimliğiyle parametresini eklemeniz gerekir. Veri diskleri için, `--data-disk-encryption-sets` veri diskleri için disk şifreleme kümelerinin boşlukla ayrılmış bir listesiyle ekleyin.


## <a name="portal"></a>Portal

Portalda görüntü sürümünüzü oluşturduğunuzda, depolama şifreleme kümeleriniz hakkında bilgi girmek için **şifreleme** sekmesini kullanabilirsiniz.

1. **Görüntü sürümü oluştur** sayfasında **şifreleme** sekmesini seçin.
2. **Şifreleme türü**' nde, **müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme**' yi seçin. 
3. Görüntüdeki her disk için, açılan listeden kullanılacak **disk şifrelemesi kümesini** seçin. 

### <a name="create-the-vm"></a>Sanal makine oluşturma

Paylaşılan görüntü galerisinden bir VM oluşturabilir ve diskleri şifrelemek için müşterinin yönettiği anahtarları kullanabilirsiniz. Portalda VM oluştururken, **diskler** sekmesinde, **şifreleme türü**için **müşteri tarafından yönetilen anahtarlarla bekleyen şifreleme** ' yi seçin. Daha sonra açılan listeden şifreleme kümesini seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Sunucu tarafı disk şifrelemesi](/windows/disk-encryption.md)hakkında daha fazla bilgi edinin.
