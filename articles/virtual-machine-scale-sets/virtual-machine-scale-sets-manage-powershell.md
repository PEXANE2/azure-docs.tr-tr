---
title: Azure PowerShell ile sanal makine ölçek kümelerini yönetme
description: Bir örneği başlatma ve durdurma veya ölçek kümesi kapasitesini değiştirme gibi sanal makine ölçek kümelerini yönetmek için ortak Azure PowerShell cmdlet 'leri.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: management
ms.date: 05/29/2018
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 68b5aa21f861009dd78f48428fa0ffdc5b5ae3a3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124879"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Azure PowerShell ile sanal makine ölçek kümesini yönetme

Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede, bu görevleri gerçekleştirmenize izin veren bazı yaygın Azure PowerShell cmdlet 'leri ayrıntılı olarak açıklanır.

Bir sanal makine ölçek kümesi oluşturmanız gerekiyorsa, [Azure PowerShell bir ölçek kümesi oluşturabilirsiniz](quick-create-powershell.md).

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="view-information-about-a-scale-set"></a>Ölçek kümesi hakkındaki bilgileri görüntüleme
Bir ölçek kümesi hakkındaki genel bilgileri görüntülemek için [Get-AzVmss](/powershell/module/az.compute/get-azvmss)komutunu kullanın. Aşağıdaki örnek, *Myresourcegroup* kaynak grubundaki *myScaleSet* adlı ölçek kümesi hakkındaki bilgileri alır. Kendi adlarınızı aşağıdaki gibi girin:

```powershell
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki sanal makine örneğinin listesini görüntülemek için [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm)komutunu kullanın. Aşağıdaki örnekte, *myScaleSet* adlı ölçek kümesindeki ve *myresourcegroup* kaynak grubundaki tüm sanal makine örnekleri listelenmektedir. Bu adlar için kendi değerlerinizi sağlayın:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Belirli bir sanal makine örneği hakkında ek bilgileri görüntülemek için `-InstanceId` [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) ' ye parametreyi ekleyin ve görüntülenecek örneği belirtin. Aşağıdaki örnek *myScaleSet* ve *myresourcegroup* kaynak grubu ADLı *Ölçek kümesinde bulunan VM örneğiyle ilgili* bilgileri görüntüler. Kendi adlarınızı aşağıdaki gibi girin:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Yukarıdaki komutlarda, ölçek kümesi ve VM örnekleri hakkında bilgiler gösterildi. Ölçek kümesindeki örneklerin sayısını artırmak veya azaltmak için kapasiteyi değiştirebilirsiniz. Ölçek kümesi, gereken sayıda VM 'yi otomatik olarak oluşturur veya kaldırır, ardından VM 'Leri uygulama trafiğini alacak şekilde yapılandırır.

İlk olarak [Get-AzVmss](/powershell/module/az.compute/get-azvmss)ile bir ölçek kümesi nesnesi oluşturun ve ardından için yeni bir değer belirtin `sku.capacity` . Kapasite değişikliğini uygulamak için [Update-AzVmss](/powershell/module/az.compute/update-azvmss)komutunu kullanın. Aşağıdaki örnek, *Myresourcegroup* kaynak grubundaki *myScaleSet* değerini *5* örnek kapasiteye göre güncelleştirir. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
# Get current scale set
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Ölçek kümesinin kapasitesini azaltırsanız, en yüksek örnek kimliğine sahip VM 'Ler ilk önce kaldırılır.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri durdurma ve başlatma
Ölçek kümesindeki bir veya daha fazla sanal makineyi durdurmak için, [stop-AzVmss](/powershell/module/az.compute/stop-azvmss)kullanın. `-InstanceId` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM 'yi durdurmak için, her örnek KIMLIĞINI virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myresourcegroup* kaynak grubu adlı ölçek kümesinde *0* örneğini durduruyor. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Varsayılan olarak, durdurulan sanal makineler serbest bırakılır ve bunlar için işlem ücreti alınmaz. Durdurulan sanal makinenin sağlama durumunda kalmasını istiyorsanız, önceki komuta `-StayProvisioned` parametresini ekleyin. Sağlama durumunda tutulan durdurulmuş sanal makineler için normal işlem ücreti alınır.


### <a name="start-vms-in-a-scale-set"></a>Ölçek kümesindeki VM 'Leri başlatma
Bir ölçek kümesinde bir veya daha fazla VM başlatmak için [Start-AzVmss](/powershell/module/az.compute/start-azvmss)kullanın. `-InstanceId` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için, her örnek KIMLIĞINI virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myresourcegroup* kaynak grubu adlı ölçek kümesinde *0* örneğini başlatır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Start-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri yeniden başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makineyi yeniden başlatmak için [restart-AzVmss](/powershell/module/az.compute/restart-azvmss)komutunu kullanın. `-InstanceId` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM 'yi yeniden başlatmak için, her örnek KIMLIĞINI virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myresourcegroup* kaynak grubu adlı ölçek kümesinde *0* örneğini yeniden başlatır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Restart-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Ölçek kümesinden VM 'Leri kaldırma
Bir ölçek kümesindeki bir veya daha fazla VM 'yi kaldırmak için, [Remove-AzVmss](/powershell/module/az.compute/remove-azvmss)kullanın. `-InstanceId`Parametresi, kaldırılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Bir örnek KIMLIĞI belirtmezseniz, ölçek kümesindeki tüm sanal makineler kaldırılır. Birden çok VM 'yi kaldırmak için, her örnek KIMLIĞINI virgülle ayırın.

Aşağıdaki örnek, *myScaleSet* ve *myresourcegroup* kaynak grubu adlı ölçek kümesinde *0* örneğini kaldırır. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Remove-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Sonraki adımlar
Ölçek Kümeleri için diğer yaygın görevler, [bir uygulamanın nasıl dağıtılacağını](virtual-machine-scale-sets-deploy-app.md)ve [sanal makine örneklerinin nasıl yükseltileceğini](virtual-machine-scale-sets-upgrade-scale-set.md)içerir. [Otomatik ölçek kurallarını yapılandırmak](virtual-machine-scale-sets-autoscale-overview.md)için Azure PowerShell de kullanabilirsiniz.
