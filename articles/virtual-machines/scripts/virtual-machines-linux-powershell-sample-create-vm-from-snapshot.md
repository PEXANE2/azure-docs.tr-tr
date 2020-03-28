---
title: Anlık görüntüden VM oluşturma - PowerShell Sample
description: Azure PowerShell Komut Dosyası Örneği - Anlık görüntüden VM oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 21e31df7552dfe5e6368235173622695078626b9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75463649"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a>PowerShell ile anlık görüntüden sanal bir makine oluşturun

Bu betik bir işletim sistemi diskinin anlık görüntüsünden sanal makine oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, anlık görüntü özellikleri almak, anlık görüntü den yönetilen bir disk oluşturmak ve bir VM oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/get-azsnapshot) | Anlık görüntü adını kullanarak anlık görüntü alır. |
| [Yeni-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) | Bir disk yapılandırması oluşturur. Bu yapılandırma disk oluşturma işlemi ile kullanılır. |
| [Yeni-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk) | Yönetilen bir disk oluşturur. |
| [Yeni-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) | Yönetilen diski sanal makineye işletim sistemi diski olarak bağlar |
| [Yeni-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [Yeni-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) | Ağ arabirimi oluşturur. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Bir sanal makine oluşturur. |
|[Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.