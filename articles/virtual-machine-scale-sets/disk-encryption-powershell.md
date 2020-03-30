---
title: Azure PowerShell ile Azure ölçek kümeleri için diskleri şifreleme
description: Windows sanal makine ölçeği kümesindeki VM örneklerini ve ekli diskleri şifrelemek için Azure PowerShell'i nasıl kullanacağınızı öğrenin
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: mbaldwin
ms.openlocfilehash: bd7f92c104e06896f4b3c8bb2adef45983cf5d4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278993"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell"></a>Azure PowerShell ile ayarlanmış sanal makine ölçeğinde işletim sistemi ve bağlı veri disklerini şifreleme

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır.  Bu makalede, sanal makine ölçeği kümesi oluşturmak ve şifrelemek için Azure PowerShell'i nasıl kullanacağınızı gösterilmektedir. Azure Disk Şifrelemesini sanal makine ölçeği kümesine uygulama hakkında daha fazla bilgi [için, Sanal Makine Ölçeği Kümeleri için Azure Disk](disk-encryption-overview.md)Şifrelemesi'ne bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifreleme siiçin etkin leştirilmiş bir Azure Anahtar Kasası oluşturma

Azure Key Vault, uygulamalarınızda ve hizmetlerinizde bunları güvenli bir şekilde uygulamanızı sağlayan anahtarları, sırları veya parolaları depolayabilir. Şifreleme anahtarları yazılım koruması kullanılarak Azure Key Vault'ta depolanır veya fips 140-2 seviye 2 standartlarına göre onaylı Donanım Güvenlik Modülleri'nde (HSM) anahtarlarınızı içe aktarabilir veya oluşturabilirsiniz. Bu şifreleme anahtarları, VM'nize bağlı sanal diskleri şifrelemek ve şifresini çözmek için kullanılır. Bu şifreleme anahtarlarının denetimini siz elde elabilirsiniz ve bunların kullanımını denetleyebilirsiniz.

[New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault)ile Bir Anahtar Vault oluşturun. Anahtar Kasasının disk şifreleme için kullanılmasına izin vermek için *EnabledForDiskEncryption* parametresini ayarlayın. Aşağıdaki örnekte, kaynak grubu adı, Anahtar Kasa Adı ve konum için değişkenler de tanımlanamamıştır. Kendi benzersiz Key Vault adınızı sağlayın:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzResourceGroup -Name $rgName -Location $location
New-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Varolan bir Anahtar Kasası kullanma

Bu adım yalnızca disk şifrelemeyle kullanmak istediğiniz varolan bir Key Vault'unuzun olması gerekir. Önceki bölümde bir Anahtar Kasası oluşturduysanız bu adımı atlayın.

[Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-AzKeyVaultAccessPolicy)ile disk şifreleme için ayarlanan ölçekle aynı abonelik te ve bölgede varolan bir Key Vault'u etkinleştirebilirsiniz. *$vaultName* değişkeninde varolan Key Vault'unuzun adını aşağıdaki gibi tanımlayın:


```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

İlk olarak, VM örnekleri için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Şimdi [Yeni-AzVmss](/powershell/module/az.compute/new-azvmss)ile ayarlanmış sanal bir makine ölçeği oluşturun. Her bir sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem TCP bağlantı noktası 80 üzerinden trafiği dağıtmak hem de TCP bağlantı noktası 3389 üzerinden uzak masaüstü trafiğine hem de TCP bağlantı noktası 5985 üzerinden PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir:

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştirme

VM örneklerini bir ölçek kümesinde şifrelemek için, önce [Get-AzKeyVault](/powershell/module/az.keyvault/Get-AzKeyVault)ile Key Vault URI ve kaynak kimliği hakkında bazı bilgiler alın. Bu değişkenler daha sonra [Set-AzVmssDiskEncryptionExtension](/powershell/module/az.compute/Set-AzVmssDiskEncryptionExtension)ile şifreleme işlemini başlatmak için kullanılır:


```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType "All"
```

İstendiğinde, VM örneklerini ayarladan ölçekte disk şifreleme işlemine devam etmek için *y* yazın.

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>Anahtarı sarmak için KEK kullanarak şifrelemeyi etkinleştirme

Sanal makine ölçeği kümesini şifrelerken ek güvenlik için bir Anahtar Şifreleme Anahtarı da kullanabilirsiniz.

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $vaultName -Name $keyEncryptionKeyName).Key.kid;

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $keyVaultResourceId -VolumeType "All"
```

> [!NOTE]
>  Disk-şifreleme-anahtar tonoz parametresi değeri için sözdizimi tam tanımlayıcı dizedir:</br>
/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> Anahtar şifreleme anahtarı parametresinin değeri için sözdizimi, KEK'e tam URI'dir:</br>
https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]

## <a name="check-encryption-progress"></a>Şifreleme ilerlemesini denetleme

Disk şifrelemedurumunu kontrol etmek için [Get-AzVmssDiskEncryption](/powershell/module/az.compute/Get-AzVmssDiskEncryption)kullanın:


```azurepowershell-interactive
Get-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

VM örnekleri şifrelendiğinde, *EncryptionSummary* kodu aşağıdaki örnek çıktıda gösterildiği gibi *ProvisioningState/succeeded* raporlarını bildirir:

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="disable-encryption"></a>Şifrelemeyi devre dışı

Artık şifreli VM örnekleri diskleri kullanmak istemiyorsanız, [Devre Dışı-AzVmssDiskEncryption](/powershell/module/az.compute/Disable-AzVmssDiskEncryption) ile şifrelemeyi aşağıdaki gibi devre dışı kullanabilirsiniz:


```azurepowershell-interactive
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Sonraki adımlar

- Bu makalede, sanal makine ölçeği kümesini şifrelemek için Azure PowerShell'i kullandınız. [Azure CLI](disk-encryption-cli.md) veya Azure [Kaynak Yöneticisi şablonlarını](disk-encryption-azure-resource-manager.md)da kullanabilirsiniz.
- Azure Disk Şifreleme'nin başka bir uzantı sağlandıktan sonra uygulanmasını istiyorsanız, [uzantı sıralamasını](virtual-machine-scale-sets-extension-sequencing.md)kullanabilirsiniz.
