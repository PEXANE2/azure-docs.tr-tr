---
title: Azure Powershell ile bir Linux VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, Bir Linux sanal makinesi oluşturmak ve şifrelemek için Azure Powershell'i nasıl kullanacağınızı öğrenirsiniz
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: a2cb8919ac0752c42f22e064d6201c7120fbc9b6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78970543"
---
# <a name="quickstart-create-and-encrypt-a-linux-vm-in-azure-with-azure-powershell"></a>Hızlı başlatma: Azure PowerShell ile Azure'da bir Linux VM oluşturma ve şifreleme

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, Bir Linux sanal makinesi (VM) oluşturmak, şifreleme anahtarlarının depolanması için bir Anahtar Kasası oluşturmak ve VM'yi şifrelemek için Azure PowerShell modülünün nasıl kullanılacağını gösterir. Bu hızlı başlangıç, Canonical'ın Ubuntu 16.04 LTS pazar yeri görüntüsünü ve VM Standard_D2S_V3 boyutunu kullanır. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır:

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Yukarıda oluşturduğunuz VM yapılandırma nesnesini ona aktararak [Yeni AzVM](/powershell/module/az.compute/new-azvm)içeren bir Azure sanal makinesi oluşturun.

```powershell-interactive
$cred = = Get-Credential

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:16.04-LTS:latest -Size Standard_D2S_V3
```

VM'nizin dağıtılması birkaç dakika sürer. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Şifreleme anahtarları için yapılandırılan bir Key Vault oluşturma

Azure disk şifrelemesi şifreleme anahtarını Azure Anahtar Kasası'nda saklar. [Yeni-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault)ile Bir Anahtar Vault oluşturun. Anahtar Kasası'nın şifreleme anahtarlarını depolamasını sağlamak için -EnabledForDiskEncryption parametresini kullanın.

> [!Important]
> Her anahtar kasası Azure'da benzersiz bir ada sahip olmalıdır. Aşağıdaki örneklerde, seçtiğiniz adla <benzersiz anahtar tonoz> değiştirin.

```azurepowershell-interactive
New-AzKeyvault -name "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup" -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleme

VM'nizi [Set-AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension)ile şifreleyin. 

Set-AzVmDiskŞifreleme Uzantısı, Key Vault nesnenizden bazı değerler gerektirir. [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault)için anahtar tonoz benzersiz adını geçerek bu değerleri elde edebilirsiniz.

```azurepowershell-interactive
$KeyVault = Get-AzKeyVault -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup"

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName "MyVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

Birkaç dakika sonra işlem aşağıdakileri döndürecektir:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

[Get-AzVmDiskEncryptionStatus'u](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus)çalıştırarak şifreleme işlemini doğrulayabilirsiniz.

```azurepowershell-interactive
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Şifreleme etkinleştirildiğinde, döndürülen çıktıda aşağıdakileri görürsünüz:

```
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : NotMounted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'yi ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini kullanabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal bir makine oluşturdunuz, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz ve VM'yi şifrelediniz.  Linux VM'leri için Azure Disk Şifrelemesi hakkında daha fazla bilgi edinmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)