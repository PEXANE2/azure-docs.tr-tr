---
title: Azure sanal makine ölçek kümeleri ile otomatik örnek onarımları
description: VM örnekleri için otomatik onarım ilkesini ölçek kümesinde nasıl yapılandırılabildiğini öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78274820"
---
# <a name="preview-automatic-instance-repairs-for-azure-virtual-machine-scale-sets"></a>Önizleme: Azure sanal makine ölçek kümeleri için otomatik örnek onarımları

Azure sanal makine ölçek kümeleri için otomatik örnek onarımlarını etkinleştirmek, bir dizi sağlıklı örnek sağlayarak uygulamalar için yüksek kullanılabilirlik elde etmenize yardımcı olur. Ölçek kümesindeki bir [örneğin, Uygulama Durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya Yük [dengeleyici sabunu sondaları](../load-balancer/load-balancer-custom-probe-overview.md)tarafından bildirilen sağlıksız olduğu tespit edilirse, bu özellik sağlıksız örneği silerek ve değiştirmek için yeni bir örnek oluşturarak otomatik olarak örnek onarımı gerçekleştirir.

> [!NOTE]
> Bu önizleme özelliği hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.

## <a name="requirements-for-using-automatic-instance-repairs"></a>Otomatik örnek onarımları kullanma gereksinimleri

**Otomatik örnek onarım önizlemesini tercih edin**

Otomatik örnek onarım önizlemesini tercih etmek için REST API veya Azure PowerShell'i kullanın. Bu adımlar, önizleme özelliği için aboneliğinizi kaydeder. Bu özelliği kullanmak için yalnızca bir kerelik kurulum gerektiğini unutmayın. Aboneliğiniz otomatik örnek onarım önizlemesi için zaten kayıtlıysa, yeniden kaydolmanız gerekmez. 

REST API kullanma 

1. Özellikler ' i kullanarak özellik için kaydolun [- Register](/rest/api/resources/features/register) 

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

2. *Devletin* *Kayıtlı*olarak değişmesi için birkaç dakika bekleyin. Bunu onaylamak için aşağıdaki API'yi kullanabilirsiniz.

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

3. *Devlet* *Kayıtlı*olarak değiştirildikten sonra aşağıdakileri çalıştırın.

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2015-12-01'
```

Azure PowerShell’i kullanma

1. Cmdlet [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider) ve ardından [Register-AzureRmProviderFeature'ı](/powershell/module/azurerm.resources/register-azurermproviderfeature) kullanarak özellik için kaydolun

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute

Register-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
```

2. *RegistrationState'in* *Kayıtlı*olarak değişmesi için birkaç dakika bekleyin. Bunu onaylamak için aşağıdaki cmdlet'i kullanabilirsiniz.

```azurepowershell-interactive
Get-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
 ```

 Yanıt aşağıdaki gibi olmalıdır.

| Özellik Adı                           | Sağlayıcı Adı            | Kayıt Durumu       |
|---------------------------------------|-------------------------|-------------------------|
| RepairVMScaleSetInstancesPreview      | Microsoft.Compute       | Kaydedildi              |

3. *RegistrationState'in* *Kayıtlı*olarak değiştirilmesinden sonra aşağıdaki cmdlet'i çalıştırın.

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute
```

**Ölçek kümesi için uygulama durumu izlemesini etkinleştirme**

Ölçek kümesi, örneğin uygulama durumu izleme etkin olmalıdır. Bu uygulama sağlık [uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sağlık probları](../load-balancer/load-balancer-custom-probe-overview.md)kullanılarak yapılabilir. Bunlardan yalnızca biri aynı anda etkinleştirilebilir. Uygulama durumu uzantısı veya yük dengeleyici, uygulama durumu durumunu belirlemek için sanal makine örneklerinde yapılandırılan uygulama bitiş noktasını pingler. Bu sistem durumu, örnek durumu izlemek ve gerektiğinde onarım yapmak için ölçek kümesi orchestrator tarafından kullanılır.

**Sistem durumu durumu sağlamak için bitiş noktasını yapılandırma**

Otomatik örnek onarım ilkesini etkinleştirmeden önce, ölçek kümesi örneklerinin uygulama sistem durumu durumunu yayarlar için uygulama bitiş noktası nı yapılandırdığından emin olun. Bir örnek bu uygulama bitiş noktasında 200 (Tamam) durumunu döndürdüğünde, örnek "Sağlıklı" olarak işaretlenir. Diğer tüm durumlarda, örnek aşağıdaki senaryolar da dahil olmak üzere "Sağlıksız" olarak işaretlenir:

- Uygulama durumu durumunu sağlamak için sanal makine örnekleri içinde yapılandırılan bir uygulama bitiş noktası olmadığında
- Uygulama bitiş noktası yanlış yapılandırıldığında
- Uygulama bitiş noktasına ulaşılamadığında

"Sağlıksız" olarak işaretlenmiş örneklerde, otomatik onarımlar ölçek kümesi tarafından tetiklenir. Bitiş noktası yapılandırılırken, istenmeyen örnek onarımlarını önlemek için otomatik onarım ilkesini etkinleştirmeden önce uygulama bitiş noktasının doğru şekilde yapılandırıldığından emin olun.

**Tek yerleşim grubunu etkinleştirme**

Bu önizleme şu anda yalnızca tek bir yerleşim grubu olarak dağıtılan ölçek kümeleri için kullanılabilir. Özellik *singlePlacementGroup* otomatik örnek onarım özelliği ni kullanmak için ölçek ayarınız için *doğru* olarak ayarlanmalıdır. [Yerleşim grupları](./virtual-machine-scale-sets-placement-groups.md#placement-groups)hakkında daha fazla bilgi edinin.

**API sürümü**

Otomatik onarım ilkesi, 2018-10-01 veya daha yüksek işlem API sürümü için desteklenir.

**Kaynak veya abonelik hamlelerinde kısıtlamalar**

Bu önizlemenin bir parçası olarak, otomatik onarım ilkesi etkinleştirildiğinde, kaynak veya abonelik hareketleri şu anda ölçek kümeleri için desteklenmez.

**Hizmet kumaş ölçek setleri için kısıtlama**

Bu önizleme özelliği şu anda servis kumaş ölçek kümeleri için desteklenmez.

## <a name="how-do-automatic-instance-repairs-work"></a>Otomatik örnek onarımları nasıl çalışır?

Otomatik örnek onarım özelliği, bir ölçek kümesindeki tek tek örneklerin sistem durumu izlemesine dayanır. Bir ölçek kümesindeki VM [örnekleri, Uygulama Durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sistem sondaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanılarak uygulama durumu durumunu yayarlar şekilde yapılandırılabilir. Bir örneğin sağlıksız olduğu tespit edilirse, ölçek kümesi sağlıksız örneği silerek ve değiştirmek için yeni bir örnek oluşturarak onarım eylemi gerçekleştirir. Bu özellik *otomatikRepairsPolicy* nesnesi kullanılarak sanal makine ölçeği kümesi modelinde etkinleştirilebilir.

### <a name="batching"></a>Toplu İşleme

Otomatik örnek onarım işlemleri toplu olarak gerçekleştirilir. Herhangi bir zamanda, ölçek kümesindeki örneklerin en fazla %5'i otomatik onarım ilkesi yle onarılır. Bu, aynı anda sağlıksız bulunursa, aynı anda silinmesini ve çok sayıda örneğin yeniden oluşturulmasını önlemeye yardımcı olur.

### <a name="grace-period"></a>Yetkisiz kullanım süresi

Bir örnek, ölçek kümesinde gerçekleştirilen bir PUT, PATCH veya POST eylemi (örneğin yeniden görüntüle, yeniden dağıtma, güncelleştirme, vb.) nedeniyle bir durum değişikliği işleminden geçtiğinde, bu örnekteki herhangi bir onarım eylemi yalnızca yetkisiz kullanım süresi bekledikten sonra gerçekleştirilir. Yetkisiz kullanım süresi, örneğin sağlıklı duruma dönmesine izin veren süredir. Yetkisiz kullanım süresi, durum değişikliği tamamlandıktan sonra başlar. Bu, herhangi bir erken veya kazara onarım işlemlerini önlemeye yardımcı olur. Yetkisiz kullanım süresi, ölçek kümesinde yeni oluşturulan herhangi bir örnek için (onarım işlemi sonucunda oluşturulan örnek dahil) onurlandırıldı. Yetkisiz kullanım süresi ISO 8601 formatında dakikalar içinde belirtilir ve *özellik otomatikRepairsPolicy.gracePeriod*kullanılarak ayarlanabilir. Yetkisiz kullanım süresi 30 dakika ile 90 dakika arasında değişebilir ve varsayılan değeri 30 dakikadır.

Otomatik örnek onarım işlemi aşağıdaki gibi çalışır:

1. [Uygulama Durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sağlık probları](../load-balancer/load-balancer-custom-probe-overview.md) her örnek için uygulama durumu durumu almak için ayarlanmış ölçekte her sanal makine içinde uygulama bitiş noktası ping.
2. Bitiş noktası 200 (Tamam) durumuyla yanıt veriyorsa, örnek "Sağlıklı" olarak işaretlenir. Diğer tüm durumlarda (bitiş noktası erişilemez sa yılabilir sa da dahil olmak üzere), örnek "Sağlıksız" olarak işaretlenir.
3. Bir örneğin sağlıksız olduğu tespit edildiğinde, ölçek kümesi sağlıksız örneği silerek ve değiştirmek için yeni bir örnek oluşturarak bir onarım eylemini tetikler.
4. Örnek onarımlar toplu olarak gerçekleştirilir. Herhangi bir zamanda, ölçek kümesindeki toplam örneklerin en fazla %5'i onarılır. Bir ölçek kümesinde 20'den az örnek varsa, onarımlar aynı anda sağlıksız bir örnek için yapılır.
5. Yukarıdaki işlem, ölçek kümesindeki tüm sağlıksız örnek onarılana kadar devam ediyor.

## <a name="instance-protection-and-automatic-repairs"></a>Örnek koruma ve otomatik onarımlar

Ölçek kümesindeki bir *[örnek, ölçek ayarlı eylemlerden koruma ilkesine](./virtual-machine-scale-sets-instance-protection.md#protect-from-scale-set-actions)* karşı koruma uygulanarak korunuyorsa, bu örnekte otomatik onarımlar gerçekleştirilmez.

## <a name="enabling-automatic-repairs-policy-when-creating-a-new-scale-set"></a>Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştirme

Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştirmek için, bu özelliğe katılmanın tüm [gereksinimlerinin](#requirements-for-using-automatic-instance-repairs) karşılandığından emin olun. Uygulama bitiş noktası, bitiş noktası yapılandırılırken istenmeyen onarımları tetiklememek için ölçek kümesi örnekleri için doğru şekilde yapılandırılmalıdır. Yeni oluşturulan ölçek kümeleri için, herhangi bir örnek onarım yalnızca yetkisiz kullanım süresi süresi bekledikten sonra gerçekleştirilir. Bir ölçek kümesinde otomatik örnek onarımını etkinleştirmek için, sanal makine ölçeği kümesi modelinde *automaticRepairsPolicy* nesnesini kullanın.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, ölçek kümesi modelinde otomatik örnek onarımını nasıl etkinleştirileceğinizi gösterir. API sürümünü 2018-10-01 veya daha yüksek kullanın.

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

[Yeni-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) cmdlet kullanılarak yeni bir ölçek kümesi oluşturulurken otomatik örnek onarım özelliği etkinleştirilebilir. Bu örnek komut dosyası yapılandırma dosyasını kullanarak bir ölçek kümesi ve ilişkili kaynakların oluşturulması ile yürür: [Tam bir sanal makine ölçek kümesi oluşturun.](./scripts/powershell-sample-create-complete-scale-set.md) Ölçek kümesini oluşturmak için yapılandırma nesnesine *EnableAutomaticRepair* ve *AutomaticRepairGracePeriod* parametrelerini ekleyerek otomatik örnek onarım ilkesini yapılandırabilirsiniz. Aşağıdaki örnek, özelliği 30 dakikalık bir yetkisiz kullanım süresiyle etkinleştirmektedir.

```azurepowershell-interactive
New-AzVmssConfig `
 -Location "EastUS" `
 -SkuCapacity 2 `
 -SkuName "Standard_DS2" `
 -UpgradePolicyMode "Automatic" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT30M"
```

## <a name="enabling-automatic-repairs-policy-when-updating-an-existing-scale-set"></a>Varolan bir ölçek kümesini güncellerken otomatik onarım ilkesini etkinleştirme

Varolan bir ölçek kümesinde otomatik onarım ilkesini etkinleştirmeden önce, bu özelliğe katılmanın tüm [gereksinimlerinin](#requirements-for-using-automatic-instance-repairs) karşılandığından emin olun. Uygulama bitiş noktası, bitiş noktası yapılandırılırken istenmeyen onarımları tetiklememek için ölçek kümesi örnekleri için doğru şekilde yapılandırılmalıdır. Bir ölçek kümesinde otomatik örnek onarımını etkinleştirmek için, sanal makine ölçeği kümesi modelinde *automaticRepairsPolicy* nesnesini kullanın.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, 40 dakikalık yetkisiz kullanım süresine sahip ilkeyi sağlar. API sürümünü 2018-10-01 veya daha yüksek kullanın.

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

Varolan bir ölçek kümesinde otomatik örnek onarım özelliğinin yapılandırmasını değiştirmek için [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet'i kullanın. Aşağıdaki örnek, yetkisiz kullanım süresini 40 dakikaya güncelleştirir.

```azurepowershell-interactive
Update-AzVmss `
 -ResourceGroupName "myResourceGroup" `
 -VMScaleSetName "myScaleSet" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT40M"
```

## <a name="troubleshoot"></a>Sorun giderme

**Otomatik onarım ilkesinin etkinleştirilmemesi**

"'Özellikler' türünesnesi üzerinde üye 'automaticRepairsPolicy' bulamadım" belirten bir iletiyle bir 'BadRequest' hatası alırsanız, sanal makine ölçeği kümesi için kullanılan API sürümünü denetleyin. BU özellik için API sürümü 2018-10-01 veya daha yüksek olması gerekir.

**İlke etkinleştirildiğinde bile örnek onarılamadı**

Örnek yetkisiz kullanım süresi içinde olabilir. Bu, onarım gerçekleştirmeden önce örnekte herhangi bir durum değişikliğinden sonra bekleme süresidir. Bu herhangi bir erken veya kazara onarım önlemek içindir. Onarım eylemi, yetkisiz kullanım süresi tamamlandıktan sonra gerçekleşmelidir.

**Ölçek kümesi örnekleri için uygulama sistem durumu durumunu görüntüleme**

Uygulama durumu durumunu görüntülemek için sanal makine ölçeğinde örnek [görüntüapi](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) sini kullanabilirsiniz. Azure PowerShell ile cmdlet [Get-AzVmssVM'i](/powershell/module/az.compute/get-azvmssvm) *-InstanceView* bayrağı ile kullanabilirsiniz. Uygulama sağlık durumu özelliği *vmHealth*altında sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

Ölçek kümeleriniz için [Uygulama Durumu uzantısını](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sistem sondalarını](../load-balancer/load-balancer-custom-probe-overview.md) nasıl yapılandıracağımız hakkında bilgi edinin.
