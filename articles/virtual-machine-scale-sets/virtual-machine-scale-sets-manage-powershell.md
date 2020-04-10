---
title: Azure PowerShell ile Sanal Makine Ölçeği Kümelerini Yönetme
description: Bir örneğin nasıl başlatıp durduruleceği veya ölçek kümesi kapasitesini değiştirme gibi Sanal Makine Ölçeği Kümelerini yönetmek için ortak Azure PowerShell cmdlets' leri.
author: ju-shim
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: jushiman
ms.openlocfilehash: 5794fb40b49a575c12f6855292c41014fabf3b8c
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011608"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Azure PowerShell ile sanal makine ölçeği kümesini yönetme

Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede, bu görevleri gerçekleştirmenize olanak tanıyan ortak Azure PowerShell cmdlet'lerinden bazıları ayrıntılı olarak anlatılıyor.

Sanal makine ölçeği kümesi oluşturmanız gerekiyorsa, [Azure PowerShell ile bir ölçek kümesi oluşturabilirsiniz.](quick-create-powershell.md)

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="view-information-about-a-scale-set"></a>Ölçek kümesi hakkındaki bilgileri görüntüleme
Bir ölçek kümesi hakkındaki genel bilgileri görüntülemek için [Get-AzVmss'i](/powershell/module/az.compute/get-azvmss)kullanın. Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myScaleSet* adlı ölçek kümesi hakkında bilgi alır. Kendi adlarınızı aşağıdaki gibi girin:

```powershell
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki VM örneğinin listesini görüntülemek için [Get-AzVmssVM'yi](/powershell/module/az.compute/get-azvmssvm)kullanın. Aşağıdaki örnekte, *myScaleSet* adlı ölçek kümesindeki ve *myResourceGroup* kaynak grubundaki tüm VM örnekleri listelenebilmiştir. Bu adlar için kendi değerlerinizi sağlayın:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Belirli bir VM örneği hakkında ek `-InstanceId` bilgileri görüntülemek için [Get-AzVmssVM'ye](/powershell/module/az.compute/get-azvmssvm) parametre ekleyin ve görüntüleyecek bir örnek belirtin. Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde VM örnek *0* hakkındaki bilgileri görüntüler. Kendi adlarınızı aşağıdaki gibi girin:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Önceki komutlar ölçek kümeniz ve VM örnekleri hakkında bilgi gösterdi. Ölçek kümesindeki örnek sayısını artırmak veya azaltmak için kapasiteyi değiştirebilirsiniz. Ölçek kümesi otomatik olarak gerekli VM sayısını oluşturur veya kaldırır, ardından uygulama trafiği almak için VM'leri yapılandırır.

İlk olarak, [Get-AzVmss](/powershell/module/az.compute/get-azvmss)ile bir ölçek kümesi nesnesi oluşturmak, sonra için `sku.capacity`yeni bir değer belirtin. Kapasite değişikliğini uygulamak için [Update-AzVmss'i](/powershell/module/az.compute/update-azvmss)kullanın. Aşağıdaki örnek, *myResourceGroup* kaynak grubundaki *myScaleSet'i* *5* örneklik kapasiteyle güncelleştirir. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
# Get current scale set
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Bir ölçek kümesinin kapasitesini azaltırsanız, önce en yüksek örnek lid'lere sahip VM'ler kaldırılır.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri durdurma ve başlatma
Bir ölçek kümesinde bir veya daha fazla VM'yi durdurmak için [Stop-AzVmss](/powershell/module/az.compute/stop-azvmss)kullanın. `-InstanceId` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM'yi durdurmak için, her örnek kimliğini virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde *örnek 0'ı* durdurur. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Varsayılan olarak, durdurulan sanal makineler serbest bırakılır ve bunlar için işlem ücreti alınmaz. Durdurulan sanal makinenin sağlama durumunda kalmasını istiyorsanız, önceki komuta `-StayProvisioned` parametresini ekleyin. Sağlama durumunda tutulan durdurulmuş sanal makineler için normal işlem ücreti alınır.


### <a name="start-vms-in-a-scale-set"></a>VM'leri ölçek kümesinde başlatma
Bir ölçek kümesinde bir veya daha fazla VM başlatmak için [Başlat-AzVmss'i](/powershell/module/az.compute/start-azvmss)kullanın. `-InstanceId` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için, her örnek kimliğini virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde *örnek 0'ı* başlatır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Start-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri yeniden başlatma
Bir ölçek kümesinde bir veya daha fazla VM'yi yeniden başlatmak için [Yeniden Başlat-AzVmss'i](/powershell/module/az.compute/restart-azvmss)kullanın. `-InstanceId` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM'yi yeniden başlatmak için, her örnek kimliği virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesinde örnek *0'ı* yeniden başlatır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Restart-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>VM'leri ölçek kümesinden kaldırma
Bir ölçek kümesindeki bir veya daha fazla VM'yi kaldırmak için [Kaldır-AzVmss'i](/powershell/module/az.compute/remove-azvmss)kullanın. `-InstanceId` Parametre kaldırmak için bir veya daha fazla VMs belirtmenize olanak sağlar. Örnek bir kimlik belirtmezseniz, ölçek kümesindeki tüm VM'ler kaldırılır. Birden çok VM kaldırmak için, her örnek kimliğini virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myResourceGroup* kaynak grubu adlı ölçek kümesindeki *örnek 0'ı* kaldırır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Remove-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleri için diğer yaygın görevler, [bir uygulamanın](virtual-machine-scale-sets-deploy-app.md)nasıl dağıtılanmasını ve [VM örneklerini yükseltmeyi](virtual-machine-scale-sets-upgrade-scale-set.md)içerir. [Otomatik ölçek kurallarını yapılandırmak](virtual-machine-scale-sets-autoscale-overview.md)için Azure PowerShell'i de kullanabilirsiniz.
