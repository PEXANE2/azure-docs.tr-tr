---
title: Azure 'da bir sanal sabit sürücünün anlık görüntüsünü oluşturma
description: Bir Azure sanal makinesinin yedekleme veya sorun giderme sorunları için kullanılacak bir kopyasını oluşturmayı öğrenin.
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 10/08/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: bc74a3eea1f99de6080788d6f3fddcac823092dc
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75370910"
---
# <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Anlık görüntü, bir sanal sabit sürücünün (VHD) tam, salt okunurdur bir kopyasıdır. Yedekleme olarak kullanmak veya sanal makine (VM) sorunlarını gidermek için bir işletim sistemi veya veri diski VHD 'sinin anlık görüntüsünü alabilirsiniz.

Yeni bir VM oluşturmak için anlık görüntüyü kullanacaksanız, devam eden işlemlerin tümünü temizlemek için anlık görüntüyü almadan önce VM 'yi düzgün bir şekilde kapatmanız önerilir.

## <a name="use-the-azure-portal"></a>Azure Portal’ı kullanma 

Anlık görüntü oluşturmak için aşağıdaki adımları izleyin: 
1.  [Azure Portal](https://portal.azure.com) **kaynak oluştur**' u seçin.
2. **Anlık görüntü**arayın ve seçin.
3. **Anlık görüntü** penceresinde **Oluştur**' u seçin. **Anlık görüntü oluştur** penceresi görüntülenir.
4. Anlık görüntü için bir **ad** girin.
5. Mevcut bir kaynak grubunu seçin veya yeni bir [kaynak grubu](../../azure-resource-manager/management/overview.md#resource-groups) adı girin. 
6. Azure veri merkezi **Konumu** seçin.  
7. **Kaynak disk**için, anlık görüntü yapılacak yönetilen diski seçin.
8. Anlık görüntüyü depolamak için kullanılacak **hesap türünü** seçin. Anlık görüntünün yüksek performanslı bir diskte depolanması gerekmiyorsa **Standard_HDD**' yi seçin.
9. **Oluştur**’u seçin.

## <a name="use-powershell"></a>PowerShell'i kullanma

Aşağıdaki adımlarda, VHD diskinin nasıl kopyalanacağı ve anlık görüntü yapılandırmasının nasıl oluşturulacağı gösterilmektedir. Ardından [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) cmdlet 'ini kullanarak diskin anlık görüntüsünü alabilirsiniz. 

 

1. Bazı parametreleri ayarla: 

   ```azurepowershell-interactive
   $resourceGroupName = 'myResourceGroup' 
   $location = 'eastus' 
   $vmName = 'myVM'
   $snapshotName = 'mySnapshot'  
   ```

2. VM 'yi al:

   ```azurepowershell-interactive
   $vm = get-azvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
   ```

3. Anlık görüntü yapılandırmasını oluşturun. Bu örnekte, anlık görüntü işletim sistemi diski olur:

   ```azurepowershell-interactive
   $snapshot =  New-AzSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
   ```
   
   > [!NOTE]
   > Anlık görüntüsünü bölge dayanıklı depolamada depolamak isterseniz, [kullanılabilirlik alanlarını](../../availability-zones/az-overview.md) destekleyen bir bölgede oluşturun ve `-SkuName Standard_ZRS` parametresini ekleyin.   
   
4. Anlık görüntüyü al:

   ```azurepowershell-interactive
   New-AzSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
   ```


## <a name="next-steps"></a>Sonraki adımlar

Bir anlık görüntüden yönetilen disk oluşturarak ve ardından yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturun. Daha fazla bilgi için, [PowerShell ile anlık GÖRÜNTÜDEN VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)' daki örneğe bakın.
