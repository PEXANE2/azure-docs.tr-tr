---
title: Azure'da Windows VM'deki diskleri şifreleme
description: Azure PowerShell'i kullanarak gelişmiş güvenlik için Windows VM'deki sanal diskleri şifreleme
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 096fe779c077424b01df3ead5965cb799866a03b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033539"
---
# <a name="encrypt-virtual-disks-on-a-windows-vm"></a>Windows VM'deki sanal diskleri şifreleme
Gelişmiş sanal makine (VM) güvenliği ve uyumluluğu için Azure'daki sanal diskler şifrelenebilir. Diskler, Azure Anahtar Kasası'nda sabitlenen şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarlarını kontrol edebilir ve bunların kullanımını denetleyebilirsiniz. Bu makalede, Azure PowerShell kullanarak Windows VM'deki sanal disklerin nasıl şifrelenir. Ayrıca [Linux sanal makineleri şifreleyebilirsiniz.](../linux/disk-encryption-overview.md)

 

## <a name="overview-of-disk-encryption"></a>Disk şifrelemeye genel bakış
Windows VM'deki sanal diskler BitLocker kullanılarak istirahatte şifrelenir. Azure'da sanal diskleri şifrelemek ücretsizdir. Şifreleme anahtarları yazılım koruması kullanılarak Azure Key Vault'ta depolanır veya FIPS 140-2 seviye 2 standartlarına göre sertifikalı Donanım Güvenlik Modülleri'nde (HSM) anahtarlarınızı içe aktarabilir veya oluşturabilirsiniz. Şifreleme anahtarları, VM'nize bağlı sanal diskleri şifrelemek ve şifresini çözmek için kullanılır. Bu şifreleme anahtarlarının denetimini siz de saklarsınız ve bunların kullanımını denetleyebilirsiniz. 

VM şifreleme işlemi aşağıdaki gibidir:

1. Azure Anahtar Kasası'nda şifreleme anahtarı oluşturun.
1. Şifreleme anahtarını diskleri şifrelemek için kullanılabilir şekilde yapılandırın.
1. Sanal diskleriniz için disk şifrelemeyi etkinleştirin.
1. Gerekli şifreleme anahtarları Azure Key Vault'tan istenir.
1. Sanal diskler sağlanan şifreleme anahtarı kullanılarak şifrelenir.


## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar

Desteklenen senaryolar ve disk şifreleme gereksinimleri:

* Azure Marketi görüntülerinden veya özel VHD görüntülerinden yeni Windows VM'lerinde şifreleme etkinleştirme.
* Azure'daki mevcut Windows VM'lerinde şifreleme yi etkinleştirme.
* Depolama Alanları kullanılarak yapılandırılan Windows VM'lerinde şifrelemeyi etkinleştirme.
* Windows VM'ler için işletim sistemi ve veri sürücülerinde şifrelemenin devre dışı bırakılması.
* A, D, DS, G ve GS serisi VM'ler gibi standart kademe VM'ler.

    > [!NOTE]
    > Tüm kaynaklar (Key Vault, Storage hesabı ve VM dahil) aynı Azure bölgesinde ve abonelikte olmalıdır.

Disk şifreleme şu anda aşağıdaki senaryolarda desteklenmez:

* Temel kademe VM'ler.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* Zaten şifrelenmiş bir VM'de şifreleme anahtarlarını güncelleştirin.
* Şirket içi Anahtar Yönetim Hizmeti ile entegrasyon.


## <a name="create-an-azure-key-vault-and-keys"></a>Azure Anahtar Kasası ve tuşları oluşturma
Başlamadan önce Azure PowerShell modülünün en son sürümünün yüklendiğinden emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). Aşağıdaki komut örneklerinde, tüm örnek parametreleri *myResourceGroup,* *myKeyVault,* *myVM*ve benzeri adlarınız, konumunuz ve anahtar değerleriniz ile değiştirin.

İlk adım, şifreleme anahtarlarınızı depolamak için bir Azure Anahtar Kasası oluşturmaktır. Azure Anahtar Kasaları, bunları uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamanızı sağlayan anahtarları, sırları veya parolaları depolayabilir. Sanal disk şifrelemesi için, sanal disklerinizi şifrelemek veya şifresini çözmek için kullanılan bir şifreleme anahtarını depolamak için bir Anahtar Kasası oluşturursunuz. 

[Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)ile Azure aboneliğinizdeki Azure Anahtar Kasası sağlayıcısını etkinleştirin ve [ardından New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *Doğu ABD* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzResourceGroup -Location $location -Name $rgName
```

Şifreleme anahtarlarını ve depolama ve VM kendisi gibi ilişkili bilgi işlem kaynaklarını tutan Azure Anahtar Kasası'nın tümü aynı bölgede olmalıdır. [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) ile bir Azure Anahtar Kasası oluşturun ve Disk şifrelemeyle kullanılmak üzere Key Vault'u etkinleştirin. *KeyVaultName* için benzersiz bir Key Vault adını aşağıdaki gibi belirtin:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Şifreleme anahtarlarını yazılım veya Donanım Güvenlik Modeli (HSM) korumalarını kullanarak depolayabilirsiniz.  Standart bir Key Vault yalnızca yazılım korumalı anahtarları saklar. HSM kullanmak için ek bir ücret karşılığında premium Key Vault gerekir. Premium Key Vault oluşturmak için, bir önceki adımda *-Sku "Premium"* parametresini ekleyin. Aşağıdaki örnekte, standart bir Key Vault oluşturduğumuzdan yazılım korumalı anahtarlar kullanmaktadır. 

Her iki koruma modeli için de, Sanal Disklerin şifresini çözmek için VM önyükleme yaptığında, Azure platformuna şifreleme anahtarlarını istemek için erişim izni verilmesi gerekir. [Ekle-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey)ile Key Vault'unuzda bir şifreleme anahtarı oluşturun. Aşağıdaki örnek *myKey*adlı bir anahtar oluşturur:

```azurepowershell-interactive
Add-AzKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Şifreleme işlemini test etmek [için, New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)ile bir VM oluşturun. Aşağıdaki örnek, *Windows Server 2016 Datacenter* görüntüsünü kullanarak *myVM* adında bir VM oluşturur. Kimlik bilgileri istendiğinde, VM'niz için kullanılacak kullanıcı adı ve parolayı girin:

```azurepowershell-interactive
$cred = Get-Credential

New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-a-virtual-machine"></a>Sanal makineyi şifreleme
Azure Anahtar Kasası tuşunu kullanarak [VM'nizi Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) ile şifreleyin. Aşağıdaki örnek tüm anahtar bilgileri alır ve *myVM*adlı VM şifreler:

```azurepowershell-interactive
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

VM şifrelemesine devam etme istemini kabul edin. İşlem sırasında VM yeniden başlatılır. Şifreleme işlemi tamamlandıktan ve VM yeniden başlatıldıktan sonra [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus)ile şifreleme durumunu gözden geçirin:

```azurepowershell-interactive
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

Çıktı aşağıdaki örneğe benzer:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Anahtar Kasası yönetme hakkında daha fazla bilgi için sanal [makineler için Bir Anahtar Kasası Ayarlama'ya](key-vault-setup.md)bakın.
* Azure'a yüklemek için şifrelenmiş özel bir VM hazırlama gibi disk şifreleme hakkında daha fazla bilgi için [Azure Disk Şifrelemesi'ne](../../security/fundamentals/encryption-overview.md)bakın.
