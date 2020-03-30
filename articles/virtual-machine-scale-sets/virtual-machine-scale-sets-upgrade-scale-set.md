---
title: Azure sanal makine ölçeği kümesini değiştirme
description: REST API'leri, Azure PowerShell ve Azure CLI ile ayarlanan Azure sanal makine ölçeğini nasıl değiştirip güncelleştirebilirsiniz öğrenin
author: mimckitt
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: mimckitt
ms.openlocfilehash: 66fd656b5175547641150a048e57c978dc06d291
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476833"
---
# <a name="modify-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesini değiştirme

Uygulamalarınızın kullanım ömrü boyunca sanal makine ölçek kümenizi değiştirmeniz veya güncelleştirmeniz gerekebilir. Bu güncelleştirmeler, ölçek kümesinin yapılandırmasını nasıl güncelleştireceğini veya uygulama yapılandırmasını nasıl değiştireceğini içerebilir. Bu makalede, REST API'leri, Azure PowerShell veya Azure CLI ile varolan bir ölçek kümesinin nasıl değiştirilen şekilde değiştirilen.

## <a name="fundamental-concepts"></a>Temel kavramlar

### <a name="the-scale-set-model"></a>Ölçek kümesi modeli
Ölçek kümesi, ölçek kümesinin bir bütün olarak *istenilen* durumunu yakalayan bir "ölçek kümesi modeline" sahiptir. Bir ölçek kümesi için modeli sorgulamak için, 

- [Compute/virtualmachinescalesets](/rest/api/compute/virtualmachinescalesets/get) ile REST API/aşağıdaki gibi olsun:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- [Get-AzVmss](/powershell/module/az.compute/get-azvmss)ile Azure PowerShell :

    ```powershell
    Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
    ```

- [Az vmss gösterilen](/cli/azure/vmss)Azure CLI :

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet
    ```

- [resources.azure.com](https://resources.azure.com) veya dile özel Azure [SDK'larını](https://azure.microsoft.com/downloads/)da kullanabilirsiniz.

Çıktının tam sunumu, komuta sağladığınız seçeneklere bağlıdır. Aşağıdaki örnek, Azure CLI'den yoğunlaştırılmış örnek çıktısını gösterir:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Bu özellikler bir bütün olarak ayarlanan ölçek için geçerlidir.


### <a name="the-scale-set-instance-view"></a>Ölçek kümesi örnek görünümü
Ölçek kümesi, ölçek kümesinin geçerli *çalışma süresi* durumunu bir bütün olarak yakalayan bir "ölçek kümesi örneği görünümüne" de sahiptir. Bir ölçek kümesi için örnek görünümünü sorgulamak için şunları kullanabilirsiniz:

- Aşağıdaki gibi [compute/virtualmachinescalesets/getinstanceview](/rest/api/compute/virtualmachinescalesets/getinstanceview) ile REST API:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/instanceView?api-version={apiVersion}
    ```

- [Get-AzVmss](/powershell/module/az.compute/get-azvmss)ile Azure PowerShell :

    ```powershell
    Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceView
    ```

- [Az vmss get-instance-view](/cli/azure/vmss)ile Azure CLI :

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
    ```

- [resources.azure.com](https://resources.azure.com) veya dile özel Azure [SDK'larını](https://azure.microsoft.com/downloads/) da kullanabilirsiniz

Çıktının tam sunumu, komuta sağladığınız seçeneklere bağlıdır. Aşağıdaki örnek, Azure CLI'den yoğunlaştırılmış örnek çıktısını gösterir:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
}
```

Bu özellikler, ölçek kümesine uygulanan uzantıların durumu gibi ölçek kümesindeki VM'lerin geçerli çalışma zamanı durumunun bir özetini sağlar.


### <a name="the-scale-set-vm-model-view"></a>Ölçek kümesi VM model görünümü
Ölçek kümesinin model görünümüne sahip olmasına benzer şekilde, ölçek kümesindeki her VM örneğinin kendi model görünümü vardır. Belirli bir VM örneğinin model görünümünü ölçek kümesinde sorgulamak için şunları kullanabilirsiniz:

- [Compute/virtualmachinescalesetvms/get](/rest/api/compute/virtualmachinescalesetvms/get) with REST API aşağıdaki gibidir:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId?api-version={apiVersion}
    ```

- [Get-AzVmssVm](/powershell/module/az.compute/get-azvmssvm)ile Azure PowerShell :

    ```powershell
    Get-AzVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- [Az vmss gösterilen](/cli/azure/vmss)Azure CLI :

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- [resources.azure.com](https://resources.azure.com) veya [Azure SDK'larını](https://azure.microsoft.com/downloads/)da kullanabilirsiniz.

Çıktının tam sunumu, komuta sağladığınız seçeneklere bağlıdır. Aşağıdaki örnek, Azure CLI'den yoğunlaştırılmış örnek çıktısını gösterir:

```azurecli
$ az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Bu özellikler, bir ölçek kümesi içinde bir VM örneğinin yapılandırmasını açıklar, bir bütün olarak kümeedilen ölçek yapılandırmasını değil. Örneğin, ölçek kümesi modeli `overprovision` bir özellik olarak, bir ölçek kümesi içinde bir VM örneği için model yok iken. Bu fark, aşırı sağlamanın ölçek kümesindeki tek tek VM örnekleri değil, bir bütün olarak ayarlanan ölçek için bir özellik olmasıdır (aşırı sağlama hakkında daha fazla bilgi için ölçek [kümeleri için Tasarım hususlarına](virtual-machine-scale-sets-design-overview.md#overprovisioning)bakın).


### <a name="the-scale-set-vm-instance-view"></a>Ölçek kümesi VM örnek görünümü
Bir ölçek kümesinin örnek görünümüne benzer şekilde, ölçek kümesindeki her VM örneğinin kendi örnek görünümü vardır. Bir ölçek kümesi içinde belirli bir VM örneği için örnek görünümünü sorgulamak için şunları kullanabilirsiniz:

- Aşağıdaki gibi [compute/virtualmachinescalesetvms/getinstanceview](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) ile REST API:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId/instanceView?api-version={apiVersion}
    ```

- [Get-AzVmssVm](/powershell/module/az.compute/get-azvmssvm)ile Azure PowerShell :

    ```powershell
    Get-AzVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -InstanceView
    ```

- [Az vmss get-instance-view](/cli/azure/vmss) ile Azure CLI

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- [resources.azure.com](https://resources.azure.com) veya [Azure SDK'larını](https://azure.microsoft.com/downloads/) da kullanabilirsiniz

Çıktının tam sunumu, komuta sağladığınız seçeneklere bağlıdır. Aşağıdaki örnek, Azure CLI'den yoğunlaştırılmış örnek çıktısını gösterir:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
}
```

Bu özellikler, ölçek kümesine uygulanan uzantıları içeren bir ölçek kümesi içindeki bir VM örneğinin geçerli çalışma zamanı durumunu açıklar.


## <a name="how-to-update-global-scale-set-properties"></a>Genel ölçek kümesi özelliklerini güncelleştirme
Genel ölçek kümesi özelliğini güncelleştirmek için ölçek kümesi modelindeki özelliği güncelleştirmeniz gerekir. Bu güncelleştirmeyi şu yoluyla yapabilirsiniz:

- Aşağıdaki gibi [compute / virtualmachinescalesets / createorupdate](/rest/api/compute/virtualmachinescalesets/createorupdate) ile REST API:

    ```rest
    PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Genel ölçek kümesi özelliklerini güncelleştirmek için REST API'daki özellikleri içeren bir Kaynak Yöneticisi şablonu dağıtabilirsiniz.

- [Güncelleme-AzVmss](/powershell/module/az.compute/update-azvmss)ile Azure PowerShell:

    ```powershell
    Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet {scaleSetConfigPowershellObject}
    ```

- [Az vmss güncellemesi](/cli/azure/vmss)ile Azure CLI :
    - Bir özelliği değiştirmek için:

        ```azurecli
        az vmss update --set {propertyPath}={value}
        ```

    - Ölçek kümesindeki bir liste özelliğine nesne eklemek için: 

        ```azurecli
        az vmss update --add {propertyPath} {JSONObjectToAdd}
        ```

    - Bir nesneyi ölçek kümesindeki bir liste özelliğinden kaldırmak için: 

        ```azurecli
        az vmss update --remove {propertyPath} {indexToRemove}
        ```

    - Daha önce `az vmss create` komutile ölçek kümesi ni dağıttıysanız, `az vmss create` ölçek kümesini güncelleştirmek için komutu yeniden çalıştırabilirsiniz. `az vmss create` Değiştirmek istediğiniz özellikler dışında, komuttaki tüm özelliklerin öncekiyle aynı olduğundan emin olun.

- [resources.azure.com](https://resources.azure.com) veya [Azure SDK'larını](https://azure.microsoft.com/downloads/)da kullanabilirsiniz.

Ölçek kümesi modeli güncelleştirildikten sonra, yeni yapılandırma ölçek kümesinde oluşturulan tüm yeni VM'ler için geçerlidir. Ancak, ölçek kümesindeki varolan VM'lerin modelleri yine de en son genel ölçek kümesi modeliyle güncel olmalıdır. Her VM için modelde, VM'nin en son genel ölçek kümesi modeliyle güncel olup olmadığını gösteren bir boolean özelliği `latestModelApplied` vardır (VM'nin`true` en son modelle güncel olduğu anlamına gelir).


## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>En son ölçek kümesi modeliyle VM'leri güncel getirme
Ölçek kümelerinin, VM'lerin en son ölçek kümesi modeliyle nasıl güncel getirildiğini belirleyen bir "yükseltme ilkesi" vardır. Yükseltme ilkesi için üç mod şunlardır:

- **Otomatik** - Bu modda, ölçek kümesi VM'lerin indirildiği sırası hakkında hiçbir garanti veretmez. Ölçek kümesi tüm VM'leri aynı anda indirebilir. 
- **Yuvarlama** - Bu modda, ölçek kümesi güncelleştirmeyi toplu iş lerle birlikte toplu olarak kullanıma açar.
- **El Kitabı** - Bu modda, ölçek kümesi modelini güncelleştirdiğinizde, varolan VM'lere hiçbir şey olmaz.
 
Varolan VM'leri güncelleştirmek için, varolan her VM'nin "el ile yükseltmesini" yapmanız gerekir. Bu manuel yükseltmeyi şunları ile yapabilirsiniz:

- Aşağıdaki gibi [compute/ virtualmachinescalesets / updateinstances](/rest/api/compute/virtualmachinescalesets/updateinstances) ile REST API:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/manualupgrade?api-version={apiVersion}
    ```

- [Update-AzVmssInstance](/powershell/module/az.compute/update-azvmssinstance)ile Azure PowerShell :
    
    ```powershell
    Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- [Az vmss güncelleme örnekleri](/cli/azure/vmss) ile Azure CLI

    ```azurecli
    az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
    ```

- Dile özel [Azure SDK'larını](https://azure.microsoft.com/downloads/)da kullanabilirsiniz.

>[!NOTE]
> Service Fabric kümeleri yalnızca *Otomatik* modu kullanabilir, ancak güncelleştirme farklı şekilde işlenir. Daha fazla bilgi için [Service Fabric uygulama yükseltmelerine](../service-fabric/service-fabric-application-upgrade.md)bakın.

Yükseltme ilkesini izlemeyen genel ölçek kümesi özelliklerinde bir değişiklik türü vardır. Ölçek kümesi işletim sistemi ve Veri disk profilindeki değişiklikler (yönetici kullanıcı adı ve parola gibi) yalnızca *2017-12-01* veya sonraki API sürümünde değiştirilebilir. Bu değişiklikler yalnızca ölçek kümesi modelindeki değişiklikten sonra oluşturulan VM'ler için geçerlidir. Varolan VM'leri güncel getirmek için, varolan her VM'nin "yeniden görüntülenmesini" yapmanız gerekir. Bu yeniden görüntüyü şu yoluyla yapabilirsiniz:

- Aşağıdaki gibi [compute/ virtualmachinescalesets / reimage](/rest/api/compute/virtualmachinescalesets/reimage) ile REST API:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/reimage?api-version={apiVersion}
    ```

- [Set-AzVmssVm](https://docs.microsoft.com/powershell/module/az.compute/set-azvmssvm)ile Azure PowerShell :

    ```powershell
    Set-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -Reimage
    ```

- [Az vmss reimage](https://docs.microsoft.com/cli/azure/vmss)ile Azure CLI :

    ```azurecli
    az vmss reimage --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Dile özel [Azure SDK'larını](https://azure.microsoft.com/downloads/)da kullanabilirsiniz.


## <a name="properties-with-restrictions-on-modification"></a>Değişiklik le ilgili kısıtlamaları olan özellikler

### <a name="create-time-properties"></a>Oluşturma zamanı özellikleri
Bazı özellikler yalnızca ölçek kümesini oluşturduğunuzda ayarlanabilir. Bu özellikler şunlardır:

- Kullanılabilirlik Alanları
- Resim başvuru yayıncısı
- Resim referans teklifi
- Görüntü referans sku
- Yönetilen işletim sistemi disk depolama hesabı türü

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Yalnızca geçerli değere göre değiştirilebilen özellikler
Geçerli değere bağlı olarak istisnalar dışında bazı özellikler değiştirilebilir. Bu özellikler şunlardır:

- **singlePlacementGroup** - singlePlacementGroup doğruysa, yanlış olarak değiştirilebilir. Ancak, singlePlacementGroup yanlışsa, doğru olarak **değiştirilemeyebilir.**
- **subnet** - Bir ölçek kümesinin alt ağı, özgün alt ağ ve yeni alt ağ aynı sanal ağda olduğu sürece değiştirilebilir.

### <a name="properties-that-require-deallocation-to-change"></a>Deallocation'nin değiştirilmesini gerektiren özellikler
Bazı özellikler yalnızca ölçek kümesindeki VM'ler ayrılırsa belirli değerlerle değiştirilebilir. Bu özellikler şunlardır:

- **SKU adı**- Yeni VM SKU ölçek kümesinin şu anda üzerinde olduğu donanımda desteklenmiyorsa, SKU adını değiştirmeden önce kümedeki VM'leri ayarlamanız gerekir. Daha fazla bilgi için [Azure VM'yi nasıl yeniden boyutlandırırıgöreceğinizi](../virtual-machines/windows/resize-vm.md)öğrenin.


## <a name="vm-specific-updates"></a>VM'ye özel güncellemeler
Genel ölçek kümesi özellikleri yerine belirli VM'lere bazı değişiklikler uygulanabilir. Şu anda desteklenen tek VM özel güncelleştirmesi, ölçek kümesindeki VM'lere/VM'lere veri disklerini eklemek/ayırmaktır. Bu özellik önizlemede. Daha fazla bilgi için [önizleme belgelerine](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk)bakın.


## <a name="scenarios"></a>Senaryolar

### <a name="application-updates"></a>Uygulama güncellemeleri
Bir uygulama uzantılar aracılığıyla ayarlanan bir ölçekte dağıtılırsa, uzantı yapılandırmasına yapılan bir güncelleştirme, uygulamanın yükseltme ilkesine uygun olarak güncelleştirmesine neden olur. Örneğin, Özel Komut Dosyası Uzantısı'nda çalışacak bir komut dosyasının yeni bir sürümünüz varsa, yeni komut dosyasına işaret etmek için *fileUris* özelliğini güncelleştirebilirsiniz. Bazı durumlarda, uzantı yapılandırması değişmemiş olsa bile güncelleştirmeyi zorlamak isteyebilirsiniz (örneğin, komut dosyasının URI'sinde değişiklik yapmadan komut dosyasını güncellediniz). Bu gibi durumlarda, bir güncelleştirmezorlamak için *forceUpdateTag* değiştirebilirsiniz. Azure platformu bu özelliği yorumlamaz. Değeri değiştirirseniz, uzantınnasıl çalıştığı üzerinde hiçbir etkisi yoktur. Bir değişiklik sadece uzantısı yeniden çalıştırmak için zorlar. *forceUpdateTag*hakkında daha fazla bilgi [için, uzantılar için REST API belgelerine](/rest/api/compute/virtualmachineextensions/createorupdate)bakın. *ForceUpdateTag'ın* sadece özel komut dosyası uzantısı ile değil, tüm uzantılarla kullanılabileceğini unutmayın.

Uygulamaların özel bir görüntü aracılığıyla dağıtılması da yaygındır. Bu senaryo aşağıdaki bölümde ele alınmıştır.

### <a name="os-updates"></a>İşletim Sistemi Güncellemeleri
Azure platform görsellerini kullanıyorsanız, *imageReference'ı* değiştirerek görüntüyü güncelleştirebilirsiniz (daha fazla bilgi, [REST API belgelerine](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)bakın).

>[!NOTE]
> Platform görüntüleriile, görüntü başvuru sürümü için "en son" belirtmek yaygındır. Oluşturduğunuzda, ölçeklendirdiğinizde ve yeniden görüntülediğinizde, VM'ler en son kullanılabilir sürümle oluşturulur. Ancak, yeni görüntü sürümleri yayımlandıkça işletim sistemi görüntüsünün zaman içinde otomatik olarak güncelleştirilen anlamına **gelmez.** Otomatik işletim sistemi yükseltmeleri sağlayan ayrı bir özellik şu anda önizlemededir. Daha fazla bilgi için [Otomatik İşletim Sistemi Yükseltmeleri belgelerine](virtual-machine-scale-sets-automatic-upgrade.md)bakın.

Özel görüntüler kullanıyorsanız, *imageReference* ID'yi güncelleyerek görüntüyü güncelleyebilirsiniz (daha fazla bilgi, [REST API belgelerine](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)bakın).

## <a name="examples"></a>Örnekler

### <a name="update-the-os-image-for-your-scale-set"></a>Ölçek kümeniz için işletim sistemi görüntüsünü güncelleştirme
Ubuntu LTS 16.04'ün eski bir sürümünü çalıştıran bir ölçek kümeniz olabilir. Ubuntu LTS 16.04'ün *sürüm 16.04.201801090*gibi daha yeni bir sürümüne güncellemek istiyorsunuz. Resim başvurusu sürümü özelliği listenin bir parçası değildir, bu nedenle bu özellikleri aşağıdaki komutlardan biriyle doğrudan değiştirebilirsiniz:

- [Update-AzVmss](/powershell/module/az.compute/update-azvmss) ile Azure PowerShell aşağıdaki gibidir:

    ```powershell
    Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -ImageReferenceVersion 16.04.201801090
    ```

- [Az vmss güncellemesi](/cli/azure/vmss)ile Azure CLI :

    ```azurecli
    az vmss update --resource-group myResourceGroup --name myScaleSet --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090
    ```

Alternatif olarak, ölçek kümenizin kullandığı görüntüyü değiştirmek isteyebilirsiniz. Örneğin, ölçek kümeniz tarafından kullanılan özel bir resmi güncelleştirmek veya değiştirmek isteyebilirsiniz. Resim referans kimliği özelliğini güncelleyerek ölçek kümenizin kullandığı görüntüyü değiştirebilirsiniz. Resim başvuru kimliği özelliği listenin bir parçası değildir, bu nedenle bu özelliği aşağıdaki komutlardan biriyle doğrudan değiştirebilirsiniz:

- [Update-AzVmss](/powershell/module/az.compute/update-azvmss) ile Azure PowerShell aşağıdaki gibidir:

    ```powershell
    Update-AzVmss `
        -ResourceGroupName "myResourceGroup" `
        -VMScaleSetName "myScaleSet" `
        -ImageReferenceId /subscriptions/{subscriptionID}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myNewImage
    ```

- [Az vmss güncellemesi](/cli/azure/vmss)ile Azure CLI :

    ```azurecli
    az vmss update \
        --resource-group myResourceGroup \
        --name myScaleSet \
        --set virtualMachineProfile.storageProfile.imageReference.id=/subscriptions/{subscriptionID}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myNewImage
    ```


### <a name="update-the-load-balancer-for-your-scale-set"></a>Ölçek setinizin yük dengeleyicisini güncelleştirin
Azure Yük Dengeleyicisi ile bir ölçek belirlediğinizi ve Azure Yük Dengeleyicisini Azure Uygulama Ağ Geçidi ile değiştirmek istediğinizi varsayalım. Ölçek kümesiiçin yük dengeleyicisi ve Uygulama Ağ Geçidi özellikleri listenin bir parçasıdır, bu nedenle özellikleri doğrudan değiştirmek yerine liste öğelerini kaldırmak veya eklemek için komutları kullanabilirsiniz:

- Azure Powershell:

    ```powershell
    # Get the current model of the scale set and store it in a local PowerShell object named $vmss
    $vmss=Get-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet"
    
    # Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
    $ipconf = New-AzVmssIPConfig "myNic" -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name
    
    # Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
    $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf
    
    # Update the model of the scale set with the new configuration in the local PowerShell object
    Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -virtualMachineScaleSet $vmss
    ```

- Azure CLI:

    ```azurecli
    # Remove the load balancer backend pool from the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
    
    # Remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0
    
    # Add the application gateway backend pool to the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}'
    ```

>[!NOTE]
> Bu komutlar, ölçek kümesinde yalnızca bir IP yapılandırması ve yük dengeleyicisi olduğunu varsayar. Birden fazla varsa, *0*dışında bir liste dizini kullanmanız gerekebilir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure CLI](virtual-machine-scale-sets-manage-cli.md) veya [Azure PowerShell](virtual-machine-scale-sets-manage-powershell.md)ile ölçek kümelerinde ortak yönetim görevlerini de gerçekleştirebilirsiniz.
