---
title: Azure sanal makine ölçek kümeleri ile otomatik örnek onarımları
description: Ölçek kümesindeki sanal makine örnekleri için otomatik onarımlar ilkesini yapılandırma hakkında bilgi edinin
author: avirishuv
manager: vashan
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: avverma
ms.openlocfilehash: f335b0fb3396103c321d740bcf6d125e60e95086
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274820"
---
# <a name="preview-automatic-instance-repairs-for-azure-virtual-machine-scale-sets"></a>Önizleme: Azure sanal makine ölçek kümeleri için otomatik örnek onarımları

Azure sanal makine ölçek kümeleri için otomatik örnek onarımlarını etkinleştirmek, sağlıklı bir örnek kümesini korurken uygulamalar için yüksek kullanılabilirlik elde etmenize yardımcı olur. Ölçek kümesindeki bir örnek, [uygulama sistem durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) ya da [Yük Dengeleyici Sistem durumu araştırmaları](../load-balancer/load-balancer-custom-probe-overview.md)tarafından bildirildiği şekilde sağlıksız olarak bulunursa, bu özellik sağlıksız örneği silerek örnek onarımı gerçekleştirir ve bunu değiştirmek için yeni bir tane oluşturur.

> [!NOTE]
> Bu önizleme özelliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.

## <a name="requirements-for-using-automatic-instance-repairs"></a>Otomatik örnek onarımları kullanma gereksinimleri

**Otomatik örnek onarımları önizlemeyi kabul et**

Otomatik örnek onarma önizlemesi için kabul etmek üzere REST API veya Azure PowerShell kullanın. Bu adımlar, aboneliğinizi önizleme özelliği için kaydeder. Bu özelliği kullanmak için yalnızca bir kerelik kurulum gereklidir. Aboneliğiniz zaten otomatik örnek onarımları önizlemesi için kaydedilmişse, yeniden kaydolmanız gerekmez. 

REST API kullanma 

1. Özellikleri kullanarak özelliğe kaydolun [-Kaydet](/rest/api/resources/features/register) 

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview/register?api-version=2015-12-01'
```

```json
{
  "properties": {
    "state": "Registering"
  },
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview",
  "type": "Microsoft.Features/providers/features",
  "name": "Microsoft.Compute/RepairVMScaleSetInstancesPreview"
}
```

2. *Durumun* *kayıtlı*olarak değiştirilmesi için birkaç dakika bekleyin. Bunu doğrulamak için aşağıdaki API 'YI kullanabilirsiniz.

```
GET on '/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview?api-version=2015-12-01'
```

```json
{
  "properties": {
    "state": "Registered"
  },
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview",
  "type": "Microsoft.Features/providers/features",
  "name": "Microsoft.Compute/RepairVMScaleSetInstancesPreview"
}
```

3. *Durum* *kayıtlı*olarak değiştirildikten sonra, aşağıdakileri çalıştırın.

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2015-12-01'
```

Azure PowerShell’i kullanma

1. Cmdlet [register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider) öğesini ve ardından [register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) öğesini kullanarak özelliğe kaydolun

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute

Register-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
```

2. *Registrationstate* 'in *kayıtlı*olarak değiştirilmesi için birkaç dakika bekleyin. Bunu doğrulamak için aşağıdaki cmdlet 'i kullanabilirsiniz.

```azurepowershell-interactive
Get-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
 ```

 Yanıt aşağıdaki gibi olmalıdır.

| FeatureName                           | Adı            | RegistrationState       |
|---------------------------------------|-------------------------|-------------------------|
| RepairVMScaleSetInstancesPreview      | Microsoft.Compute       | Kaydedildi              |

3. *Registrationstate* , *kayıtlı*olarak değiştirilecek şekilde değiştikten sonra aşağıdaki cmdlet 'i çalıştırın.

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute
```

**Ölçek kümesi için uygulama sistem durumu izlemeyi etkinleştir**

Ölçek kümesi, etkin örnekler için uygulama sistem durumu izleme içermelidir. Bu, [uygulama durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) ya da [yük dengeleyici durum araştırmaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanılarak yapılabilir. Tek seferde bunlardan yalnızca biri etkinleştirilebilir. Uygulama sistem durumu uzantısı veya yük dengeleyici, uygulama sistem durumunu tespit etmek için sanal makine örneklerinde yapılandırılan uygulama uç noktasının ping olarak araştırmasını sağlar. Bu sistem durumu, ölçek kümesi Orchestrator tarafından örnek durumunu izlemek ve gerektiğinde onarım gerçekleştirmek için kullanılır.

**Sistem durumunu sağlamak için uç noktayı yapılandırma**

Otomatik örnek onarım ilkesini etkinleştirmeden önce, ölçek kümesi örneklerinin uygulama sistem durumunu göstermek için yapılandırılmış uygulama uç noktasına sahip olduğundan emin olun. Bir örnek bu uygulama uç noktasında 200 (Tamam) durumunu döndürdüğünde, örnek "sağlıklı" olarak işaretlenir. Diğer tüm durumlarda, aşağıdaki senaryolar da dahil olmak üzere örnek "sağlıksız" olarak işaretlenir:

- Uygulama sistem durumu sağlamak için sanal makine örnekleri içinde yapılandırılmış bir uygulama uç noktası yok
- Uygulama uç noktası yanlış yapılandırıldığında
- Uygulama uç noktasına erişilemiyorsa

"Sağlıksız" olarak işaretlenmiş örnekler için otomatik onarımlar ölçek kümesi tarafından tetiklenir. Uç nokta yapılandırılırken, istenmeyen örnek onarımlarını önlemek için otomatik onarımlar ilkesini etkinleştirmeden önce uygulama uç noktasının doğru yapılandırıldığından emin olun.

**Tek yerleşim grubunu etkinleştir**

Bu önizleme Şu anda yalnızca tek bir yerleştirme grubu olarak dağıtılan ölçek kümeleri için kullanılabilir. *Tekplacementgroup* özelliği, ölçek kümesinin otomatik örnek onarımları özelliğini kullanması için *true* olarak ayarlanmalıdır. [Yerleştirme grupları](./virtual-machine-scale-sets-placement-groups.md#placement-groups)hakkında daha fazla bilgi edinin.

**API sürümü**

İşlem API 'SI sürüm 2018-10-01 veya üzeri için otomatik onarım ilkesi desteklenir.

**Kaynak veya abonelik taşınmasındaki kısıtlamalar**

Bu önizlemenin bir parçası olarak, otomatik onarımlar ilkesi etkinleştirildiğinde, kaynak veya abonelik taşınanlar ölçek kümeleri için şu anda desteklenmiyor.

**Service Fabric ölçek kümeleri için kısıtlama**

Bu önizleme özelliği şu anda Service Fabric ölçek kümeleri için desteklenmiyor.

## <a name="how-do-automatic-instance-repairs-work"></a>Otomatik örnek onarımları nasıl çalışır?

Otomatik örnek onarma özelliği, bir ölçek kümesindeki tek tek örneklerin sistem durumunu izlemeye dayanır. Ölçek kümesindeki sanal makine örnekleri, uygulama durumu [Uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük Dengeleyici Sistem durumu araştırmaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanılarak uygulama sistem durumunu göstermek üzere yapılandırılabilir. Bir örneğin sağlıksız olduğu bulunursa, ölçek kümesi sorunlu örneği silerek ve yeni bir tane oluşturarak onarım eylemi gerçekleştirir. Bu özellik, *Automaticrepairspolicy* nesnesi kullanılarak sanal makine ölçek kümesi modelinde etkinleştirilebilir.

### <a name="batching"></a>Toplu İşleme

Otomatik örnek onarma işlemleri toplu olarak gerçekleştirilir. Belirli bir zamanda, ölçek kümesindeki örneklerin %5 ' inden fazlası otomatik onarımlar ilkesi aracılığıyla onarılamamakta. Bu, aynı anda sağlıksız bir şekilde bulunursa çok sayıda örnek için eşzamanlı silme ve yeniden oluşturma yapmaktan kaçınmaya yardımcı olur.

### <a name="grace-period"></a>Yetkisiz kullanım süresi

Bir örnek, ölçek kümesinde gerçekleştirilen bir PUT, PATCH veya POST eylemi nedeniyle bir durum değişikliği işleminden geçtiğinde (örneğin, yeniden görüntü, yeniden dağıtma, güncelleştirme, vb.), söz konusu örnekteki herhangi bir onarım eylemi yalnızca yetkisiz kullanım süresi dolduktan sonra gerçekleştirilir. Yetkisiz kullanım süresi, örneğin sağlıklı duruma dönmesi için izin verilen sürenin miktarıdır. Yetkisiz kullanım süresi, durum değişikliği tamamlandıktan sonra başlar. Bu, zamanından önce veya yanlışlıkla onarım işlemlerinden kaçınmaya yardımcı olur. Kullanım süresi, ölçek kümesindeki yeni oluşturulan her örnek için kabul edilir (onarım işleminin sonucu olarak oluşturulan diğeri dahil). Kullanım süresi, ISO 8601 biçimindeki dakikalar içinde belirtilir ve *Automaticrepairspolicy. gracePeriod*özelliği kullanılarak ayarlanabilir. Yetkisiz kullanım süresi 30 dakika ile 90 dakika arasında değişebilir ve varsayılan değer olan 30 dakika olabilir.

Otomatik örnek onarımları işlemi aşağıdaki gibi işler:

1. [Uygulama durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük Dengeleyici Sistem durumu araştırmaları](../load-balancer/load-balancer-custom-probe-overview.md) , her bir örnek için uygulama sistem durumunu almak üzere ölçek kümesindeki her bir sanal makinenin içindeki uygulama uç noktasına ping yapar.
2. Uç nokta 200 (Tamam) durumu ile yanıt verirse, örnek "sağlıklı" olarak işaretlenir. Diğer tüm durumlarda (uç nokta ulaşılamaz durumdaysa dahil), örnek "sağlıksız" olarak işaretlenir.
3. Bir örneğin sağlıksız olduğu bulunursa, ölçek kümesi sağlıksız örneği silerek bir onarım eylemi tetikler ve bunu değiştirmek için yeni bir tane oluşturur.
4. Örnek onarımlar toplu olarak gerçekleştirilir. Belirli bir zamanda, ölçek kümesindeki toplam örneklerin %5 ' inden fazlası onarılamamakta. Ölçek kümesinde 20 ' den az örnek varsa, onarımlar tek seferde sağlıksız bir örnek için yapılır.
5. Yukarıdaki işlem, ölçek kümesindeki sağlıksız bir örnek onarılana kadar devam eder.

## <a name="instance-protection-and-automatic-repairs"></a>Örnek koruması ve otomatik onarımlar

Ölçek kümesindeki bir örnek, *[ölçek ayarlama eylemleri koruma ilkesi](./virtual-machine-scale-sets-instance-protection.md#protect-from-scale-set-actions)* uygulanarak korunduğunda, bu örnekte otomatik onarımlar yapılmaz.

## <a name="enabling-automatic-repairs-policy-when-creating-a-new-scale-set"></a>Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştirme

Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştirmek için, bu özelliğe yönelik tüm [gereksinimlerin](#requirements-for-using-automatic-instance-repairs) karşılandığından emin olun. Uç nokta yapılandırılırken, istenmeyen onarımlar tetiklenmemek için uygulama uç noktasının ölçek kümesi örnekleri için doğru şekilde yapılandırılması gerekir. Yeni oluşturulan ölçek kümeleri için tüm örnek onarımları yalnızca yetkisiz kullanım süresi dolduktan sonra gerçekleştirilir. Ölçek kümesinde otomatik örnek onarımını etkinleştirmek için, sanal makine ölçek kümesi modelinde *Automaticrepairspolicy* nesnesini kullanın.

### <a name="rest-api"></a>REST API

Aşağıdaki örnekte, bir ölçek kümesi modelinde otomatik örnek onarımın nasıl etkinleştirileceği gösterilmektedir. API sürüm 2018-10-01 veya üstünü kullanın.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT30M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

[Yeni-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) cmdlet 'i kullanılarak yeni bir ölçek kümesi oluşturulurken otomatik örnek onarma özelliği etkinleştirilebilir. Bu örnek betik, yapılandırma dosyasını kullanarak bir ölçek kümesinin ve ilişkili kaynakların oluşturulmasını adım adım göstermektedir: [tamamen sanal makine ölçek kümesi oluşturma](./scripts/powershell-sample-create-complete-scale-set.md). Ölçek kümesini oluşturmak için Configuration nesnesine *Enableautomaticrepair* ve *AutomaticRepairGracePeriod* parametrelerini ekleyerek otomatik örnek onarımları ilkesini yapılandırabilirsiniz. Aşağıdaki örnek, özelliği 30 dakikalık yetkisiz kullanım süresine sahip olacak şekilde sunar.

```azurepowershell-interactive
New-AzVmssConfig `
 -Location "EastUS" `
 -SkuCapacity 2 `
 -SkuName "Standard_DS2" `
 -UpgradePolicyMode "Automatic" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT30M"
```

## <a name="enabling-automatic-repairs-policy-when-updating-an-existing-scale-set"></a>Mevcut bir ölçek kümesini güncelleştirirken otomatik onarım ilkesini etkinleştirme

Mevcut bir ölçek kümesinde otomatik onarım ilkesini etkinleştirmeden önce, bu özelliğe yönelik tüm [gereksinimlerin](#requirements-for-using-automatic-instance-repairs) karşılandığından emin olun. Uç nokta yapılandırılırken, istenmeyen onarımlar tetiklenmemek için uygulama uç noktasının ölçek kümesi örnekleri için doğru şekilde yapılandırılması gerekir. Ölçek kümesinde otomatik örnek onarımını etkinleştirmek için, sanal makine ölçek kümesi modelinde *Automaticrepairspolicy* nesnesini kullanın.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, ilke için 40 dakikalık yetkisiz kullanım süresi sunar. API sürüm 2018-10-01 veya üstünü kullanın.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT40M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Var olan bir ölçek kümesindeki otomatik örnek onarma özelliğinin yapılandırmasını değiştirmek için [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet 'ini kullanın. Aşağıdaki örnek, yetkisiz kullanım süresini 40 dakikaya güncelleştirir.

```azurepowershell-interactive
Update-AzVmss `
 -ResourceGroupName "myResourceGroup" `
 -VMScaleSetName "myScaleSet" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT40M"
```

## <a name="troubleshoot"></a>Sorun giderme

**Otomatik onarım ilkesini etkinleştirme hatası**

"' Properties ' türündeki nesne üzerinde" automaticRepairsPolicy ' üyesi bulunamadı "iletisiyle bir ' Rozrequest ' hatası alırsanız, sonra sanal makine ölçek kümesi için kullanılan API sürümünü denetleyin. Bu özellik için API sürüm 2018-10-01 veya üzeri gereklidir.

**İlke etkin olsa bile örnek onarılmadı**

Örnek, yetkisiz kullanım süresi içinde olabilir. Bu, onarım gerçekleştirilmeden önce örnekteki durum değişikliğinden sonra beklenecek sürenin miktarıdır. Bu, zamanından önce veya yanlışlıkla onarımların önüne geçmek. Örnek için yetkisiz kullanım süresi tamamlandıktan sonra onarım eylemi gerçekleşmelidir.

**Ölçek kümesi örnekleri için uygulama sistem durumu görüntüleme**

Uygulamanın sistem durumunu görüntülemek için bir sanal makine ölçek kümesindeki örnekler için [örnek görünümü al API](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) 'sini kullanabilirsiniz. Azure PowerShell ile [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) cmdlet 'ini *-InstanceView* bayrağıyla birlikte kullanabilirsiniz. Uygulama sistem durumu, *Vmhealth*özelliği altında verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Ölçek kümeleriniz için [uygulama durumu uzantısının](./virtual-machine-scale-sets-health-extension.md) veya [Yük Dengeleyici Sistem durumu araştırmalarının](../load-balancer/load-balancer-custom-probe-overview.md) nasıl yapılandırılacağını öğrenin.
