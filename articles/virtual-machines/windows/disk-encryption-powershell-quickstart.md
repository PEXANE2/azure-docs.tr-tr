---
title: Azure PowerShell ile Windows VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir Windows sanal makinesi oluşturmak ve şifrelemek için Azure PowerShell'i nasıl kullanacağınızı öğrenirsiniz
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 6f564a9a4f13136bbe7e28a3600ca71892c82439
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82081600"
---
# <a name="quickstart-create-and-encrypt-a-windows-virtual-machine-in-azure-with-powershell"></a>Quickstart: PowerShell ile Azure'da bir Windows sanal makinesi oluşturun ve şifreleyin

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, Windows sanal makinesi (VM) oluşturmak, şifreleme anahtarlarının depolanması için bir Anahtar Kasası oluşturmak ve VM'yi şifrelemek için Azure PowerShell modülünün nasıl kullanılacağını gösterir. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[New-AzVM](/powershell/module/az.compute/new-azvm)ile bir Azure sanal makinesi oluşturun. Cmdlet'e kimlik bilgileri sağlamalısınız. 

```powershell
$cred = Get-Credential 

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image win2016datacenter -Size Standard_D2S_V3
```

VM'nizin dağıtılması birkaç dakika sürer. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Şifreleme anahtarları için yapılandırılan bir Key Vault oluşturma

Azure disk şifrelemesi şifreleme anahtarını Azure Anahtar Kasası'nda saklar. [Yeni-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault)ile Bir Anahtar Vault oluşturun. Anahtar Kasası'nın şifreleme anahtarlarını depolamasını sağlamak için -EnabledForDiskEncryption parametresini kullanın.

> [!Important]
> Her Key Vault'un benzersiz bir adı olmalıdır. Aşağıdaki örnek, *myKV*adlı bir Anahtar Vault oluşturur, ancak farklı bir şey adlandırmanız gerekir.

```powershell
New-AzKeyvault -name MyKV -ResourceGroupName myResourceGroup -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleme

VM'nizi [Set-AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension)ile şifreleyin. 

Set-AzVmDiskŞifreleme Uzantısı, Key Vault nesnenizden bazı değerler gerektirir. [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault)için anahtar tonoz benzersiz adını geçerek bu değerleri elde edebilirsiniz.

```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName MyVM -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId
```

Birkaç dakika sonra işlem aşağıdakileri döndürecektir:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

[Get-AzVmDiskEncryptionStatus'u](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus)çalıştırarak şifreleme işlemini doğrulayabilirsiniz.

```powershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Şifreleme etkinleştirildiğinde, döndürülen çıktıda aşağıdakileri görürsünüz:

```
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : NoDiskFound
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : Provisioning succeeded
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'yi ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini kullanabilirsiniz:

```powershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal bir makine oluşturdunuz, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz ve VM'yi şifrelediniz.  IaaS VM'leri için Azure Disk Şifrelemesi önkoşulları hakkında daha fazla bilgi edinmek üzere bir sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)