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
ms.openlocfilehash: ed2b13077ff5809899097254dde0fa8406e1db3b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85601402"
---
Azure PowerShell, Azure CLı veya Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırakabilirsiniz. 

>[!IMPORTANT]
>Linux VM 'lerde Azure disk şifrelemesi ile şifrelemeyi devre dışı bırakmak yalnızca veri birimlerinde desteklenir. İşletim sistemi birimi şifrelendiyse veri veya işletim sistemi birimlerinde desteklenmez.  

- **Azure PowerShell ile disk şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [Disable-Azvmdiskencryptıon](/powershell/module/az.compute/disable-azvmdiskencryption) cmdlet 'ini kullanın. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' [-VolumeType DATA]
     ```

- **Azure CLI ile şifrelemeyi devre dışı bırakın:** Şifrelemeyi devre dışı bırakmak için [az VM ENCRYPTION Disable](/cli/azure/vm/encryption#az-vm-encryption-disable) komutunu kullanın. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type DATA
     ```
- **Kaynak Yöneticisi şablonuyla şifrelemeyi devre dışı bırak:** Şifrelemeyi devre dışı bırakmak için [çalışan bir LINUX VM şablonunda şifrelemeyi devre dışı bırak '](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-linux-vm-without-aad) i kullanın.
     1. **Azure’a dağıt**’a tıklayın.
     2. Abonelik, kaynak grubu, konum, VM, yasal koşullar ve Sözleşme ' yi seçin.
