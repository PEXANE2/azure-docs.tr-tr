---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/19/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 1e3cdfccc36549d2aa127636b15b698a4e19ccf0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610582"
---
Azure PowerShell, Azure CLı veya Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırakabilirsiniz. Windows VM'de hem işletim sistemi hem de veri diskleri şifrelenmiş durumdayken yalnızca veri diskinde şifrelemenin devre dışı bırakılması istenen sonucu vermeyebilir. Bunun yerine tüm disklerde şifrelemeyi devre dışı bırakın.

- **Azure PowerShell ile disk şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Azure CLI ile şifrelemeyi devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type "all"
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırak:** 

    1. [WINDOWS VM 'leri çalıştırırken disk şifrelemesini devre dışı bırak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) ' dan **Azure 'a dağıt** ' a tıklayın.
    2. Abonelik, kaynak grubu, konum, VM, birim türü, yasal koşullar ve Sözleşme ' yi seçin.
    3.  Çalışan bir Windows VM 'de disk şifrelemeyi devre dışı bırakmak için **satın al** ' a tıklayın 
