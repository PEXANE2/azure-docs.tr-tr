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
ms.openlocfilehash: 8156c563573183e51e06650914117f8787922e93
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81603674"
---
# <a name="automatic-instance-repairs-for-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri için otomatik örnek onarımları

Azure sanal makine ölçek kümeleri için otomatik örnek onarımlarını etkinleştirmek, bir dizi sağlıklı örnek sağlayarak uygulamalar için yüksek kullanılabilirlik elde etmenize yardımcı olur. Ölçek kümesindeki bir [örneğin, Uygulama Durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya Yük [dengeleyici sabunu sondaları](../load-balancer/load-balancer-custom-probe-overview.md)tarafından bildirilen sağlıksız olduğu tespit edilirse, bu özellik sağlıksız örneği silerek ve değiştirmek için yeni bir örnek oluşturarak otomatik olarak örnek onarımı gerçekleştirir.

## <a name="requirements-for-using-automatic-instance-repairs"></a>Otomatik örnek onarımları kullanma gereksinimleri

**Ölçek kümesi için uygulama durumu izlemesini etkinleştirme**

Ölçek kümesi, örneğin uygulama durumu izleme etkin olmalıdır. Bu uygulama sağlık [uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sağlık probları](../load-balancer/load-balancer-custom-probe-overview.md)kullanılarak yapılabilir. Bunlardan yalnızca biri aynı anda etkinleştirilebilir. Uygulama durumu uzantısı veya yük dengeleyici, uygulama durumu durumunu belirlemek için sanal makine örneklerinde yapılandırılan uygulama bitiş noktasını pingler. Bu sistem durumu, örnek durumu izlemek ve gerektiğinde onarım yapmak için ölçek kümesi orchestrator tarafından kullanılır.

**Sistem durumu durumu sağlamak için bitiş noktasını yapılandırma**

Otomatik örnek onarım ilkesini etkinleştirmeden önce, ölçek kümesi örneklerinin uygulama sistem durumu durumunu yayarlar için uygulama bitiş noktası nı yapılandırdığından emin olun. Bir örnek bu uygulama bitiş noktasında 200 (Tamam) durumunu döndürdüğünde, örnek "Sağlıklı" olarak işaretlenir. Diğer tüm durumlarda, örnek aşağıdaki senaryolar da dahil olmak üzere "Sağlıksız" olarak işaretlenir:

- Uygulama durumu durumunu sağlamak için sanal makine örnekleri içinde yapılandırılan bir uygulama bitiş noktası olmadığında
- Uygulama bitiş noktası yanlış yapılandırıldığında
- Uygulama bitiş noktasına ulaşılamadığında

"Sağlıksız" olarak işaretlenmiş örneklerde, otomatik onarımlar ölçek kümesi tarafından tetiklenir. Bitiş noktası yapılandırılırken, istenmeyen örnek onarımlarını önlemek için otomatik onarım ilkesini etkinleştirmeden önce uygulama bitiş noktasının doğru şekilde yapılandırıldığından emin olun.

**Tek yerleşim grubunu etkinleştirme**

Bu özellik şu anda yalnızca tek bir yerleşim grubu olarak dağıtılan ölçek kümeleri için kullanılabilir. Özellik *singlePlacementGroup* otomatik örnek onarım özelliği ni kullanmak için ölçek ayarınız için *doğru* olarak ayarlanmalıdır. [Yerleşim grupları](./virtual-machine-scale-sets-placement-groups.md#placement-groups)hakkında daha fazla bilgi edinin.

**API sürümü**

Otomatik onarım ilkesi, 2018-10-01 veya daha yüksek işlem API sürümü için desteklenir.

**Kaynak veya abonelik hamlelerinde kısıtlamalar**

Otomatik onarım özelliği etkinleştirildiğinde, kaynak veya abonelik hareketleri şu anda ölçek kümeleri için desteklenmez.

**Hizmet kumaş ölçek setleri için kısıtlama**

Bu özellik şu anda servis kumaş ölçek kümeleri için desteklenmez.

## <a name="how-do-automatic-instance-repairs-work"></a>Otomatik örnek onarımları nasıl çalışır?

Otomatik örnek onarım özelliği, bir ölçek kümesindeki tek tek örneklerin sistem durumu izlemesine dayanır. Bir ölçek kümesindeki VM [örnekleri, Uygulama Durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sistem sondaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanılarak uygulama durumu durumunu yayarlar şekilde yapılandırılabilir. Bir örneğin sağlıksız olduğu tespit edilirse, ölçek kümesi sağlıksız örneği silerek ve değiştirmek için yeni bir örnek oluşturarak onarım eylemi gerçekleştirir. En son sanal makine ölçek kümesi modeli yeni örneği oluşturmak için kullanılır. Bu özellik *otomatikRepairsPolicy* nesnesi kullanılarak sanal makine ölçeği kümesi modelinde etkinleştirilebilir.

### <a name="batching"></a>Toplu İşleme

Otomatik örnek onarım işlemleri toplu olarak gerçekleştirilir. Herhangi bir zamanda, ölçek kümesindeki örneklerin en fazla %5'i otomatik onarım ilkesi yle onarılır. Bu, aynı anda sağlıksız bulunursa, aynı anda silinmesini ve çok sayıda örneğin yeniden oluşturulmasını önlemeye yardımcı olur.

### <a name="grace-period"></a>Yetkisiz kullanım süresi

Bir örnek, ölçek kümesinde gerçekleştirilen bir PUT, PATCH veya POST eylemi (örneğin yeniden görüntüle, yeniden dağıtma, güncelleştirme, vb.) nedeniyle bir durum değişikliği işleminden geçtiğinde, bu örnekteki herhangi bir onarım eylemi yalnızca yetkisiz kullanım süresi bekledikten sonra gerçekleştirilir. Yetkisiz kullanım süresi, örneğin sağlıklı duruma dönmesine izin veren süredir. Yetkisiz kullanım süresi, durum değişikliği tamamlandıktan sonra başlar. Bu, herhangi bir erken veya kazara onarım işlemlerini önlemeye yardımcı olur. Yetkisiz kullanım süresi, ölçek kümesinde yeni oluşturulan herhangi bir örnek için (onarım işlemi sonucunda oluşturulan örnek dahil) onurlandırıldı. Yetkisiz kullanım süresi ISO 8601 formatında dakikalar içinde belirtilir ve *özellik otomatikRepairsPolicy.gracePeriod*kullanılarak ayarlanabilir. Yetkisiz kullanım süresi 30 dakika ile 90 dakika arasında değişebilir ve varsayılan değeri 30 dakikadır.

### <a name="suspension-of-repairs"></a>Onarımların Askıya Alınması 

Sanal makine ölçek kümeleri, gerekirse otomatik örnek onarımlarını geçici olarak askıya alma olanağı sağlar. Sanal makine ölçeği kümesinin örnek görünümünde özellik *orkestrasyonHizmetleri* altında otomatik onarımlar için *serviceState* otomatik onarımların geçerli durumunu gösterir. Bir ölçek kümesi otomatik onarımlara opted edildiğinde, parametre *serviceState* değeri *Running*olarak ayarlanır. Bir ölçek kümesi için otomatik onarımlar askıya alındığınızda, parametre *serviceState* *Askıya Alındı*olarak ayarlanır. *automaticRepairsPolicy* bir ölçek kümesinde tanımlanır ancak otomatik onarım özelliği etkin değilse, parametre *serviceState* *Çalışmıyor*olarak ayarlanır.

Bir ölçek kümesinde sağlıksız olanları değiştirmek için yeni oluşturulan örnekleri tekrar tekrar onarım işlemleri gerçekleştiren sonra bile sağlıksız kalmaya devam ederse, o zaman bir güvenlik önlemi olarak platform *Askıya Alınan*otomatik onarım için *serviceState* günceller . *Çalıştır'a*otomatik onarımlar için *serviceState* değerini ayarlayarak otomatik onarımlara yeniden devam edebilirsiniz. Ölçek kümeniz için [otomatik onarım ilkesinin servis durumunu görüntüleme ve güncelleştirme](#viewing-and-updating-the-service-state-of-automatic-instance-repairs-policy) bölümünde ayrıntılı yönergeler sağlanır. 

Otomatik örnek onarım işlemi aşağıdaki gibi çalışır:

1. [Uygulama Durumu uzantısı](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sağlık probları](../load-balancer/load-balancer-custom-probe-overview.md) her örnek için uygulama durumu durumu almak için ayarlanmış ölçekte her sanal makine içinde uygulama bitiş noktası ping.
2. Bitiş noktası 200 (Tamam) durumuyla yanıt veriyorsa, örnek "Sağlıklı" olarak işaretlenir. Diğer tüm durumlarda (bitiş noktası erişilemez sa yılabilir sa da dahil olmak üzere), örnek "Sağlıksız" olarak işaretlenir.
3. Bir örneğin sağlıksız olduğu tespit edildiğinde, ölçek kümesi sağlıksız örneği silerek ve değiştirmek için yeni bir örnek oluşturarak bir onarım eylemini tetikler.
4. Örnek onarımlar toplu olarak gerçekleştirilir. Herhangi bir zamanda, ölçek kümesindeki toplam örneklerin en fazla %5'i onarılır. Bir ölçek kümesinde 20'den az örnek varsa, onarımlar aynı anda sağlıksız bir örnek için yapılır.
5. Yukarıdaki işlem, ölçek kümesindeki tüm sağlıksız örnek onarılana kadar devam ediyor.

## <a name="instance-protection-and-automatic-repairs"></a>Örnek koruma ve otomatik onarımlar

Bir ölçek kümesindeki bir örnek [koruma ilkelerinden](./virtual-machine-scale-sets-instance-protection.md)biri uygulanarak korunuyorsa, bu örnekte otomatik onarımlar gerçekleştirilmez. Bu, her iki koruma ilkesi için de geçerlidir: *Ölçeklendirmeden koruyun* ve *ölçek ayarlı eylemlerden koruyun.* 

## <a name="terminatenotificationandautomaticrepairs"></a>Bildirimi ve otomatik onarımları sonlandırma

Sonlandırma [bildirimi](./virtual-machine-scale-sets-terminate-notification.md) özelliği bir ölçek kümesinde etkinleştirilmişse, otomatik onarım işlemi sırasında sağlıksız bir örneğin silinmesi sonlandırma bildirimi yapılandırmasını izler. Sonlandırma bildirimi Azure meta veri hizmeti (zamanlanan olaylar) aracılığıyla gönderilir ve örnek silme, yapılandırılan gecikme süresi boyunca geciktirilir. Ancak, sağlıksız bir yerine yeni bir örnek oluşturulması gecikme zaman sonu tamamlamak için beklemez.

## <a name="enabling-automatic-repairs-policy-when-creating-a-new-scale-set"></a>Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştirme

Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştirmek için, bu özelliğe katılmanın tüm [gereksinimlerinin](#requirements-for-using-automatic-instance-repairs) karşılandığından emin olun. Uygulama bitiş noktası, bitiş noktası yapılandırılırken istenmeyen onarımları tetiklememek için ölçek kümesi örnekleri için doğru şekilde yapılandırılmalıdır. Yeni oluşturulan ölçek kümeleri için, herhangi bir örnek onarım yalnızca yetkisiz kullanım süresi süresi bekledikten sonra gerçekleştirilir. Bir ölçek kümesinde otomatik örnek onarımını etkinleştirmek için, sanal makine ölçeği kümesi modelinde *automaticRepairsPolicy* nesnesini kullanın.

Bu [hızlı başlatma şablonunu,](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-automatic-repairs-slb-health-probe) yük dengeleyici sistem sondası ve 30 dakikalık yetkisiz kullanım süresiyle etkinleştirilmiş otomatik örnek onarımları içeren sanal bir makine ölçeği ni dağıtmak için de kullanabilirsiniz.

### <a name="azure-portal"></a>Azure portal
 
Yeni bir ölçek kümesi oluştururken otomatik onarım ilkesini etkinleştiren aşağıdaki adımlar.
 
1. Sanal **makine ölçek kümelerine**gidin.
1. Yeni bir ölçek kümesi oluşturmak için **+ Ekle'yi** seçin.
1. **Sağlık** sekmesine gidin. 
1. **Sağlık** bölümünü bulun.
1. Monitör **uygulama durumu** seçeneğini etkinleştirin.
1. Otomatik **onarım ilkesi** bölümünü bulun.
1. **Otomatik onarım** seçeneğini **açın.**
1. **Yetkisiz kullanım süresinde (min)** yetkisiz kullanım süresini dakika içinde belirtin, izin verilen değerler 30 ile 90 dakika arasındadır. 
1. Yeni ölçek kümesini oluşturmayı **bitirdiğinizde, Gözden Geçir + oluştur** düğmesini seçin.

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

### <a name="azure-cli-20"></a>Azure CLI 2.0

Aşağıdaki örnek, *[az vmss oluşturmak](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)* kullanarak yeni bir ölçek kümesi oluştururken otomatik onarım ilkesi sağlar. Önce bir kaynak grubu oluşturun, ardından 30 dakika olarak ayarlanmış otomatik onarım ilkesi yetkisiz kullanım süresiyle yeni bir ölçek kümesi oluşturun.

```azurecli-interactive
az group create --name <myResourceGroup> --location <VMSSLocation>
az vmss create \
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --image UbuntuLTS \
  --admin-username <azureuser> \
  --generate-ssh-keys \
  --load-balancer <existingLoadBalancer> \
  --health-probe <existingHealthProbeUnderLoaderBalancer> \
  --automatic-repairs-period 30
```

Yukarıdaki örnekte, örneklerin uygulama durumu durumunu izlemek için varolan bir yük dengeleyicisi ve sistem durumu sondası kullanır. Bunun yerine izleme için bir uygulama sistem durumu uzantısı kullanmayı tercih ederseniz, bir ölçek kümesi oluşturabilir, uygulama sistem durumu uzantısını yapılandırabilir ve bir sonraki bölümde açıklandığı gibi *az vmss güncelleştirmesini*kullanarak otomatik örnek onarım ilkesini etkinleştirebilirsiniz.

## <a name="enabling-automatic-repairs-policy-when-updating-an-existing-scale-set"></a>Varolan bir ölçek kümesini güncellerken otomatik onarım ilkesini etkinleştirme

Varolan bir ölçek kümesinde otomatik onarım ilkesini etkinleştirmeden önce, bu özelliğe katılmanın tüm [gereksinimlerinin](#requirements-for-using-automatic-instance-repairs) karşılandığından emin olun. Uygulama bitiş noktası, bitiş noktası yapılandırılırken istenmeyen onarımları tetiklememek için ölçek kümesi örnekleri için doğru şekilde yapılandırılmalıdır. Bir ölçek kümesinde otomatik örnek onarımını etkinleştirmek için, sanal makine ölçeği kümesi modelinde *automaticRepairsPolicy* nesnesini kullanın.

Varolan ölçek kümesinin modelini güncelleştirdikten sonra, en son modelin ölçeğin tüm örneklerine uygulandığından emin olun. [VM'leri en son ölçek kümesi modeliyle nasıl güncel getireceğiniz](./virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)hakkındaki yönergeye bakın.

### <a name="azure-portal"></a>Azure portal

Azure portalı üzerinden ayarlanmış varolan bir ölçek kümesinin otomatik onarım ilkesini değiştirebilirsiniz. 
 
1. Varolan sanal makine ölçek kümesine gidin.
1. Soldaki menüdeki **Ayarlar'ın** altında Sistem **Durumu ve onarım'ı**seçin.
1. Monitör **uygulama durumu** seçeneğini etkinleştirin.
1. Otomatik **onarım ilkesi** bölümünü bulun.
1. **Otomatik onarım** seçeneğini **açın.**
1. **Yetkisiz kullanım süresinde (min)** yetkisiz kullanım süresini dakika içinde belirtin, izin verilen değerler 30 ile 90 dakika arasındadır. 
1. İşiniz bittiğinde **Kaydet**‘i seçin. 

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

### <a name="azure-cli-20"></a>Azure CLI 2.0

Aşağıda, *[az vmss güncelleştirmesini](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-update)* kullanarak varolan bir ölçek kümesinin otomatik örnek onarım ilkesini güncelleştirmek için bir örnek verilmiştir.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --enable-automatic-repairs true \
  --automatic-repairs-period 30
```

## <a name="viewing-and-updating-the-service-state-of-automatic-instance-repairs-policy"></a>Otomatik örnek onarım ilkesinin servis durumunu görüntüleme ve güncelleştirme

### <a name="rest-api"></a>REST API 

ServiceState'in mülkiyeti altında otomatik onarımlar için *serviceState'i* görüntülemek üzere ayarlanmış sanal makine ölçeği için 2019-12-01 veya daha yüksek API sürümüyle [Örnek Görünümü Al'ı](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview) kullanın. *orchestrationServices* 

```http
GET '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version=2019-12-01'
```

```json
{
  "orchestrationServices": [
    {
      "serviceName": "AutomaticRepairs",
      "serviceState": "Running"
    }
  ]
}
```

Otomatik onarım durumunu ayarlamak için ayarlanmış sanal makine ölçeğinde 2019-12-01 veya daha yüksek API sürümü ile *setOrchestrationServiceState* API kullanın. Ölçek kümesi otomatik onarım özelliğine alındıktan sonra, ölçek kümeniz için otomatik onarımları askıya almak veya devam ettirmek için bu API'yi kullanabilirsiniz. 

 ```http
 POST '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/setOrchestrationServiceState?api-version=2019-12-01'
 ```

```json
{
  "orchestrationServices": [
    {
      "serviceName": "AutomaticRepairs",
      "serviceState": "Suspend"
    }
  ]
}
```

### <a name="azure-cli"></a>Azure CLI 

Otomatik örnek onarımları için *serviceState'i* görüntülemek için [örnek görüntü](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-get-instance-view) cmdlet'i kullanın. 

```azurecli-interactive
az vmss get-instance-view \
    --name MyScaleSet \
    --resource-group MyResourceGroup
```

*ServiceState'i* otomatik örnek onarımları için güncelleştirmek için [ayar-orkestrasyon-servis durumu](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-set-orchestration-service-state) cmdlet'i kullanın. Ölçek kümesi otomatik onarım özelliğine alındıktan sonra, ölçek kümeniz için otomatik onarımları askıya almak veya devam ettirmek için bu cmdlet'i kullanabilirsiniz. 

```azurecli-interactive
az vmss set-orchestration-service-state \
    --service-name AutomaticRepairs \
    --action Resume \
    --name MyScaleSet \
    --resource-group MyResourceGroup
```
### <a name="azure-powershell"></a>Azure PowerShell

Otomatik örnek onarımları için *ServiceState'i* görüntülemek için *InstanceView* parametresi olan [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss?view=azps-3.7.0) cmdlet'i kullanın.

```azurepowershell-interactive
Get-AzVmss `
    -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -InstanceView
```

Otomatik örnek onarımları için *serviceState'i* güncelleştirmek için Set-AzVmssOrchestrationServiceState cmdlet'i kullanın. Ölçek kümesi otomatik onarım özelliğine alındıktan sonra, ölçek kümeniz için otomatik onarımları askıya almak veya devam ettirmek için bu cmdlet'i kullanabilirsiniz.

```azurepowershell-interactive
Set-AzVmssOrchestrationServiceState `
    -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -ServiceName "AutomaticRepairs" `
    -Action "Suspend"
```

## <a name="troubleshoot"></a>Sorun giderme

**Otomatik onarım ilkesinin etkinleştirilmemesi**

"'Özellikler' türünesnesi üzerinde üye 'automaticRepairsPolicy' bulamadım" belirten bir iletiyle bir 'BadRequest' hatası alırsanız, sanal makine ölçeği kümesi için kullanılan API sürümünü denetleyin. BU özellik için API sürümü 2018-10-01 veya daha yüksek olması gerekir.

**İlke etkinleştirildiğinde bile örnek onarılamadı**

Örnek yetkisiz kullanım süresi içinde olabilir. Bu, onarım gerçekleştirmeden önce örnekte herhangi bir durum değişikliğinden sonra bekleme süresidir. Bu herhangi bir erken veya kazara onarım önlemek içindir. Onarım eylemi, yetkisiz kullanım süresi tamamlandıktan sonra gerçekleşmelidir.

**Ölçek kümesi örnekleri için uygulama sistem durumu durumunu görüntüleme**

Uygulama durumu durumunu görüntülemek için sanal makine ölçeğinde örnek [görüntüapi](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) sini kullanabilirsiniz. Azure PowerShell ile cmdlet [Get-AzVmssVM'i](/powershell/module/az.compute/get-azvmssvm) *-InstanceView* bayrağı ile kullanabilirsiniz. Uygulama sağlık durumu özelliği *vmHealth*altında sağlanır.

Azure portalında, sistem durumu durumunu da görebilirsiniz. Varolan bir ölçek kümesine gidin, soldaki menüden **Örnekler'i** seçin ve her ölçek kümesi örneğinin sistem durumu için **Sistem Durumu** sütununa bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Ölçek kümeleriniz için [Uygulama Durumu uzantısını](./virtual-machine-scale-sets-health-extension.md) veya [Yük dengeleyici sistem sondalarını](../load-balancer/load-balancer-custom-probe-overview.md) nasıl yapılandıracağımız hakkında bilgi edinin.
