---
title: Azure'da sanal bir sabit diskin anlık görüntüsünü oluşturma
description: Yedekleme veya sorun giderme sorunları için kullanmak üzere Azure VM'nin bir kopyasını nasıl oluşturup oluşturabilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75370910"
---
# <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Anlık görüntü, sanal bir sabit diskin (VHD) tam, salt okunur kopyasıdır. Yedekleme olarak kullanmak için bir işletim sistemi veya veri diski VHD'nin anlık görüntüsünü alabilir veya sanal makine (VM) sorunlarını giderebilirsiniz.

Anlık görüntüyeni bir VM oluşturmak için kullanacaksanız, devam eden işlemleri temizlemek için anlık görüntü almadan önce VM'yi temiz bir şekilde kapatmanızı öneririz.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma 

Anlık görüntü oluşturmak için aşağıdaki adımları tamamlayın: 
1.  Azure [portalında](https://portal.azure.com) **kaynak oluştur'u**seçin.
2. **Anlık Görüntü'nü**arayın ve seçin.
3. Anlık **Görüntü** penceresinde **Oluştur'u**seçin. **Anlık görüntü oluştur** penceresi görüntülenir.
4. Anlık görüntü için bir **Ad** girin.
5. Varolan bir [Kaynak grubu](../../azure-resource-manager/management/overview.md#resource-groups) seçin veya yeni bir grubun adını girin. 
6. Azure veri merkezi **Konumu** seçin.  
7. **Kaynak disk**için, anlık görüntü için yönetilen diski seçin.
8. Anlık görüntüdepolamak için kullanılacak **Hesap türünü** seçin. Anlık görüntünün yüksek performanslı bir diskte depolanması gerekmediği sürece **Standard_HDD**seçin.
9. **Oluştur'u**seçin.

## <a name="use-powershell"></a>PowerShell kullanma

Aşağıdaki adımlar, VHD diskinin nasıl kopyalanılsüreceğini ve anlık görüntü yapılandırmasını nasıl oluşturup oluşturup oluşturup oluşturup oluşturup oluşturup oluşturabilirsiniz. Daha sonra [Yeni-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) cmdlet kullanarak diskin anlık görüntüsünü alabilirsiniz. 

 

1. Bazı parametrelerayarlayın: 

   ```azurepowershell-interactive
   $resourceGroupName = 'myResourceGroup' 
   $location = 'eastus' 
   $vmName = 'myVM'
   $snapshotName = 'mySnapshot'  
   ```

2. VM'yi alın:

   ```azurepowershell-interactive
   $vm = get-azvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
   ```

3. Anlık görüntü yapılandırmasını oluşturun. Bu örnekte, anlık görüntü işletim sistemi diskinin:

   ```azurepowershell-interactive
   $snapshot =  New-AzSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
   ```
   
   > [!NOTE]
   > Anlık görüntünüzün bölgeye dayanıklı depolama alanında depolanmasını istiyorsanız, [anlık durumu](../../availability-zones/az-overview.md) destekleyen `-SkuName Standard_ZRS` ve parametreyi içeren bir bölgede oluşturun.   
   
4. Anlık görüntü alın:

   ```azurepowershell-interactive
   New-AzSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
   ```


## <a name="next-steps"></a>Sonraki adımlar

Anlık görüntüden yönetilen bir disk oluşturup yeni yönetilen diski işletim sistemi diski olarak ekleyerek anlık görüntüden sanal bir makine oluşturun. Daha fazla bilgi için [PowerShell ile anlık görüntüden VM oluştur'daki](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)örneğe bakın.
