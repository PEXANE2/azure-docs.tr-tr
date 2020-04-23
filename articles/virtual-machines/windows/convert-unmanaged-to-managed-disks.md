---
title: Windows sanal makinesini yönetilmeyen disklerden yönetilen disklere dönüştürme
description: Kaynak Yöneticisi dağıtım modelinde PowerShell'i kullanarak windows VM'i yönetilen disklerden yönetilen disklere dönüştürme
author: roygara
ms.service: virtual-machines-windows
ms.subservice: disks
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: rogarana
ms.openlocfilehash: d8069b174b7a69cc2e6c47171159569c56a15563
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82081957"
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Windows sanal makinesini yönetilmeyen disklerden yönetilen disklere dönüştürme

Yönetilmeyen diskler kullanan varolan Windows sanal makineleriniz (VM'ler) varsa, Azure Yönetilen [Diskler](managed-disks-overview.md) hizmeti aracılığıyla Yönetilen diskleri kullanmak için Sanal M'leri dönüştürebilirsiniz. Bu işlem hem işletim sistemi diskini hem de ekteki tüm veri disklerini dönüştürür.

 

## <a name="before-you-begin"></a>Başlamadan önce


* [Yönetilen Disklere geçiş için](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks)Gözden Geçirme Planı.

* [Yönetilen Disklere geçiş le ilgili SSS'yi gözden geçirin.](faq-for-disks.md#migrate-to-managed-disks)

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]

* Özgün VHD’ler ve dönüştürme öncesinde VM tarafından kullanılan depolama hesabı silinmez. Ücretler uygulanmaya devam eder. Bunlar için ücret alınmasını önlemek istiyorsanız, dönüştürmenin tamamlandığını doğruladıktan sonra özgün VHD bloblarını silin. Bu eklenmemiş diskleri silmek için bulmanız gerekiyorsa, makalemize bakın [Eklenmemiş Azure yönetilen ve yönetilmeyen diskleri bulun ve silin.](find-unattached-disks.md)


## <a name="convert-single-instance-vms"></a>Tek örnekli VM'leri dönüştürme
Bu bölümde, tek örnekli Azure VM'lerin yönetilen disklerden yönetilen disklere nasıl dönüştürülileceği ele alilmektedir. (VM'leriniz kullanılabilirlik kümesindeyse, bir sonraki bölüme bakın.) 

1. [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) cmdlet kullanarak VM'yi bulun. Aşağıdaki örnek, kaynak grubunda adı `myVM` geçen VM'yi yerle `myResourceGroup`bir eder: 

   ```azurepowershell-interactive
   $rgName = "myResourceGroup"
   $vmName = "myVM"
   Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
   ```

2. [ConvertTo-AzVMManagedDisk](https://docs.microsoft.com/powershell/module/az.compute/convertto-azvmmanageddisk) cmdlet kullanarak VM'yi yönetilen disklere dönüştürün. Aşağıdaki işlem, işletim sistemi diski ve tüm veri diskleri de dahil olmak üzere önceki VM'yi dönüştürür ve Sanal Makine'yi başlatır:

   ```azurepowershell-interactive
   ConvertTo-AzVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
   ```



## <a name="convert-vms-in-an-availability-set"></a>Kullanılabilirlik kümesinde VM'leri dönüştürme

Yönetilen disklere dönüştürmek istediğiniz VM'ler kullanılabilirlik kümesindeyse, öncelikle kullanılabilirlik kümesini yönetilen kullanılabilirlik kümesine dönüştürmeniz gerekir.

1. [Update-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/update-azavailabilityset) cmdlet kullanarak ayarlanan kullanılabilirliği dönüştürün. Aşağıdaki örnek, adlı `myAvailabilitySet` `myResourceGroup`kaynak grubunda adı geçen kullanılabilirlik kümesini güncelleştirir:

   ```azurepowershell-interactive
   $rgName = 'myResourceGroup'
   $avSetName = 'myAvailabilitySet'

   $avSet = Get-AzAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
   Update-AzAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
   ```

   Kullanılabilirlik kümenizin bulunduğu bölgede yalnızca 2 yönetilen hata etki alanı varsa, ancak yönetilmeyen hata etki alanı sayısı 3 ise, bu komut "Belirtilen hata etki alanı sayısı 3 aralık1 ila 2'ye düşmesi gerekir" gibi bir hata gösterir. Hatayı gidermek için hata etki alanını 2 `Sku` `Aligned` olarak güncelleştirin ve aşağıdaki gibi güncelleştirin:

   ```azurepowershell-interactive
   $avSet.PlatformFaultDomainCount = 2
   Update-AzAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
   ```

2. Kullanılabilirlik kümesindeki VM'leri bulunve dönüştürün. Aşağıdaki komut dosyası anlaşma [stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) cmdlet kullanarak her VM yerini alır, [ConvertTo-AzVMManagedDisk](https://docs.microsoft.com/powershell/module/az.compute/convertto-azvmmanageddisk)kullanarak dönüştürür ve dönüştürme işlemi dışında otomatik olarak yeniden başlatın:

   ```azurepowershell-interactive
   $avSet = Get-AzAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

   foreach($vmInfo in $avSet.VirtualMachinesReferences)
   {
     $vm = Get-AzVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
   }
   ```


## <a name="troubleshooting"></a>Sorun giderme

Dönüştürme sırasında bir hata varsa veya bir VM önceki dönüşümdeki sorunlar nedeniyle başarısız `ConvertTo-AzVMManagedDisk` durumdaysa, cmdlet'i yeniden çalıştırın. Basit bir yeniden deneme genellikle durumu engeller.
Dönüştürmeden önce, tüm VM uzantılarının 'Sağlama başarılı' durumunda olduğundan emin olun veya dönüştürme hata kodu 409 ile başarısız olur.

## <a name="convert-using-the-azure-portal"></a>Azure portalını kullanarak dönüştürme

Ayrıca, Azure portalını kullanarak yönetilmeyen diskleri yönetilen disklere dönüştürebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Portaldaki VM listesinden VM'yi seçin.
3. VM için bıçakta menüden **Diskler'i** seçin.
4. **Diskler** bıçağının üst kısmında, **yönetilen disklere Geçir'i**seçin.
5. VM'niz bir kullanılabilirlik kümesindeyse, yönetilen **disklere Geçir** bilgili bıçak ta önce kullanılabilirlik kümesini dönüştürmeniz gerektiğine dair bir uyarı olacaktır. Uyarı, kullanılabilirlik kümesini dönüştürmek için tıklatabileceğiniz bir bağlantıya sahip olmalıdır. Kullanılabilirlik kümesi dönüştürüldükten veya VM'niz kullanılabilirlik kümesinde değilse, disklerinizi yönetilen disklere geçirme işlemini başlatmak için **Geçir'i** tıklatın.

Geçiş tamamlandıktan sonra VM durdurulur ve yeniden başlatılır.

## <a name="next-steps"></a>Sonraki adımlar

[Standart yönetilen diskleri premium'a dönüştürme](convert-disk-storage.md)

[Anlık görüntüleri](snapshot-copy-managed-disk.md)kullanarak VM'nin salt okunur kopyasını alın.

