---
title: Azure sanal makine ölçeği için Örnek Koruma örnekleri kümesi
description: Azure sanal makine ölçeği ayarörneklerini ölçekve ölçek ayarlı işlemlerden nasıl koruyacağınızı öğrenin.
author: avirishuv
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: avverma
ms.openlocfilehash: 021faad28fb575c4ffeb4d895ad451d8cd82b1a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254124"
---
# <a name="instance-protection-for-azure-virtual-machine-scale-set-instances"></a>Azure sanal makine ölçeği için Örnek Koruma örnekleri kümesi

Azure sanal makine ölçek kümeleri, Otomatik [Ölçeklendirme](virtual-machine-scale-sets-autoscale-overview.md)aracılığıyla iş yüklerinizin daha iyi elastikiyetini sağlar, böylece altyapınız küçüldüğünde ve ne zaman ölçeklendiğinde yapılandırabilirsiniz. Ölçek kümeleri, farklı [yükseltme ilkesi](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) ayarları yla çok sayıda VM'yi merkezi olarak yönetmenize, yapılandırmanıza ve güncelleştirmenize de olanak tanır. Yükseltme ilkesini Otomatik veya Rolling olarak ayarladıysanız, ölçek kümesi modelinde bir güncelleştirmeyi yapılandırabilirsiniz ve yeni yapılandırma her ölçek kümesi örneğine otomatik olarak uygulanır.

Uygulamanız trafiği işlerken, belirli örneklerin ölçek kümesi örneğinin geri kalanından farklı şekilde ele alınmasını istediğiniz durumlar olabilir. Örneğin, ölçek kümesindeki belirli örnekler uzun süren işlemler gerçekleştiriyor olabilir ve bu örneklerin işlemler tamamlanana kadar ölçeklendirilebilmesini istemezsinuz. Ayrıca, ölçek kümesinin diğer üyelerinden ek veya farklı görevleri gerçekleştirmek için ölçek kümesinde birkaç örnek özelleştirilmiş olabilirsiniz. Bu 'özel' VM'lerin ölçek kümesindeki diğer örneklerle değiştirilmemasını gerektirirsiniz. Örnek koruması, uygulamanız için bu ve diğer senaryoları etkinleştirmek için ek denetimler sağlar.

Bu makalede, ölçek kümesi örnekleri ile farklı örnek koruma yeteneklerini nasıl uygulayabileceğiniz ve kullanabileceğiniz açıklanmaktadır.

## <a name="types-of-instance-protection"></a>Örnek koruma türleri
Ölçek kümeleri iki tür örnek koruma özelliği sağlar:

-   **Ölçeklendirmeden koruyun**
    - Ölçek kümesi örneğinde **protectFromScaleIn** özelliği ile etkinleştirildi
    - Örneği başlatılan Otomatik ölçekten korur
    - Kullanıcı tarafından başlatılan örnek işlemleri (örnek silme dahil) **engellenmez**
    - Ölçek kümesinde başlatılan işlemler (yükseltme, yeniden görüntülenme, anlaşma, vb.) **engellenmez**

-   **Ölçek ayarlı eylemlerden koruyun**
    - Ölçek kümesi örneğinde **ProtectFromScaleSetActions** özelliği ile etkinleştirilmiş
    - Örneği başlatılan Otomatik ölçekten korur
    - Örneği ölçek kümesinde başlatılan işlemlerden korur (yükseltme, yeniden görüntülenme, işlem yeri, vb.)
    - Kullanıcı tarafından başlatılan örnek işlemleri (örnek silme dahil) **engellenmez**
    - Tam ölçek kümesinin silinmesi **engellenmez**

## <a name="protect-from-scale-in"></a>Ölçeklendirmeden koruyun
Örnek koruma, örnekler oluşturulduktan sonra ölçek kümesi örneklerine uygulanabilir. Koruma, [ölçek kümesi modelinde](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model)değil, yalnızca [örnek modelde](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) uygulanır ve değiştirilir.

Aşağıdaki örneklerde ayrıntılı olarak belirtildiği gibi ölçek seti örneklerinde ölçeklendirme koruması uygulamanın birden çok yolu vardır.

### <a name="azure-portal"></a>Azure portalında

Ölçeklendirme koruması, Azure portalı üzerinden ölçek kümesindeki bir örneğe uygulayabilirsiniz. Aynı anda birden fazla örneği ayarlayamazsınız. Korumak istediğiniz her örnek için adımları yineleyin.
 
1. Varolan sanal makine ölçek kümesine gidin.
1. **Ayarlar'ın**altındaki soldaki menüden **Örnekler'i** seçin.
1. Korumak istediğiniz örneğin adını seçin.
1. Koruma **İlkesi** sekmesini seçin.
1. Koruma **Politikası** bıçak, **ölçek-in seçeneğinden koru'yu** seçin.
1. **Kaydet'i**seçin. 

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, ölçek kümesindeki bir örneğe ölçek koruma uygular.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true
    }
  }        
}

```

> [!NOTE]
>Örnek koruma yalnızca 2019-03-01 ve üzeri API sürümüyle desteklenir

### <a name="azure-powershell"></a>Azure PowerShell

Ölçek belirleme örneğinize ölçeklendirme koruması uygulamak için [Update-AzVmssVM](/powershell/module/az.compute/update-azvmssvm) cmdlet'i kullanın.

Aşağıdaki örnek, örnek ID 0'a sahip ölçek kümesindeki bir örneğe ölçek koruma uygular.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Ölçek belirleme örneğinize ölçeklendirme koruması uygulamak için [az vmss güncelleştirmesini](/cli/azure/vmss#az-vmss-update) kullanın.

Aşağıdaki örnek, örnek ID 0'a sahip ölçek kümesindeki bir örneğe ölçek koruma uygular.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true
```

## <a name="protect-from-scale-set-actions"></a>Ölçek ayarlı eylemlerden koruyun
Örnek koruma, örnekler oluşturulduktan sonra ölçek kümesi örneklerine uygulanabilir. Koruma, [ölçek kümesi modelinde](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model)değil, yalnızca [örnek modelde](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) uygulanır ve değiştirilir.

Bir örneği ölçek kümesi eylemlerinden korumak da örneği Otomatik Ölçeklendirme'den korur.

Aşağıdaki örneklerde ayrıntılı olarak belirtildiği gibi ölçek kümesi eylemleri koruma ölçek seti örnekleri nde uygulamanın birden çok yolu vardır.

### <a name="azure-portal"></a>Azure portalında

Azure portalı üzerinden ölçek kümesi eylemlerine karşı koruma uygulayabilirsiniz. Aynı anda birden fazla örneği ayarlayamazsınız. Korumak istediğiniz her örnek için adımları yineleyin.
 
1. Varolan sanal makine ölçek kümesine gidin.
1. **Ayarlar'ın**altındaki soldaki menüden **Örnekler'i** seçin.
1. Korumak istediğiniz örneğin adını seçin.
1. Koruma **İlkesi** sekmesini seçin.
1. Koruma **İlkesi** bıçak, **ölçek ayarlanmış eylemlerden koruyun** seçeneğini seçin.
1. **Kaydet'i**seçin. 

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, ölçek kümesindeki bir örneğe ölçek kümesi eylemlerine karşı koruma uygular.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vMScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true,
      "protectFromScaleSetActions": true
    }
  }        
}

```

> [!NOTE]
>Örnek koruma yalnızca 2019-03-01 ve üzeri API sürümüyle desteklenir.</br>
Bir örneği ölçek kümesi eylemlerinden korumak da örneği Otomatik Ölçeklendirme'den korur. "protectFromScaleIn" belirtemezsiniz: "protectFromScaleSetActions" ayarı yaparken yanlış: true

### <a name="azure-powershell"></a>Azure PowerShell

Ölçek ayarlı eylemlere karşı koruma uygulamak için [Update-AzVmssVM](/powershell/module/az.compute/update-azvmssvm) cmdlet'i ölçek ayarörneğinize uygulayın.

Aşağıdaki örnek, ölçek kümesi eylemlerine karşı korumayı örnek ID 0'a sahip ölçek kümesindeki bir örneğe uygular.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true `
  -ProtectFromScaleSetAction $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Ölçek ayarlı eylemlere karşı korumauygulamak için [az vmss güncelleştirmesini](/cli/azure/vmss#az-vmss-update) ölçek ayarlı örneğinize kullanın.

Aşağıdaki örnek, ölçek kümesi eylemlerine karşı korumayı örnek ID 0'a sahip ölçek kümesindeki bir örneğe uygular.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true \
  --protect-from-scale-set-actions true
```

## <a name="troubleshoot"></a>Sorun giderme
### <a name="no-protectionpolicy-on-scale-set-model"></a>Ölçek kümesi modelinde koruma YokPolitikası
Örnek koruma yalnızca ölçek kümesi örneklerinde geçerlidir, ölçek kümesi modelinde değil.

### <a name="no-protectionpolicy-on-scale-set-instance-model"></a>Ölçek kümesi örnek modelinde koruma Politikası yok
Varsayılan olarak, koruma ilkesi oluşturulduğunda bir örne uygulanmaz.

Örnek oluşturma, örnekler oluşturulduktan sonra ayarlanan örnekleri ölçeklendirmek için örnek koruması uygulayabilirsiniz.

### <a name="not-able-to-apply-instance-protection"></a>Örnek koruma uygulayamaz
Örnek koruma yalnızca 2019-03-01 ve üzeri API sürümüyle desteklenir. Kullanılan API sürümünü denetleyin ve gerektiği gibi güncelleştirin. PowerShell veya CLI'nizi en son sürüme güncellemeniz de gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı sanal makine ölçeği kümelerinde nasıl [dağıtılayacaklarınıöğrenin.](virtual-machine-scale-sets-deploy-app.md)
