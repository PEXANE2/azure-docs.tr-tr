---
title: Azure sanal makine ölçeği için bildirimi sonlandırma örnekleri
description: Azure sanal makine ölçeği seti örnekleri için sonlandırma bildirimini nasıl etkinleştirin
author: avirishuv
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: avverma
ms.openlocfilehash: 6023e9bf7539b79446d0135ba731b61be166dd6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250757"
---
# <a name="terminate-notification-for-azure-virtual-machine-scale-set-instances"></a>Azure sanal makine ölçeği için bildirimi sonlandırma örnekleri
Ölçek kümesi örnekleri, örnek sonlandırma bildirimlerini almayı seçebilir ve sonlandırma işlemi için önceden tanımlanmış bir gecikme zaman ayarı ayarlayabilir. Sonlandırma bildirimi, yeniden başlatma ve yeniden dağıtma gibi etkili işlemleriçin bildirimler sağlayan azure Meta veri hizmeti - [Zamanlanmış Olaylar](../virtual-machines/windows/scheduled-events.md)aracılığıyla gönderilir. Çözüm, Zamanlanan Olaylar listesine başka bir olay - Sonlandırma - ekler ve sonlandırma olayının ilişkili gecikmesi, kullanıcılar tarafından ölçek kümesi model yapılandırmalarında belirtildiği gecikme sınırına bağlıdır.

Özelliğe kaydolduktan sonra, ölçek kümesi örneklerinin, örnek silinmeden önce belirtilen zaman aşımının süresinin dolmasını beklemesi gerekmez. Sonlandırma bildirimini aldıktan sonra, sonlandırma süresi dolmadan önce örnek herhangi bir zamanda silinmeyi seçebilir.

## <a name="enable-terminate-notifications"></a>Sonlandırma bildirimlerini etkinleştirme
Aşağıdaki örneklerde açıklandığı gibi, ölçek seti örneklerinizde sonlandırma bildirimlerini etkinleştirmenin birden çok yolu vardır.

### <a name="azure-portal"></a>Azure portalında

Yeni bir ölçek kümesi oluştururken aşağıdaki adımlar sonlandırma bildirimini etkinleştirir. 

1. Sanal **makine ölçek kümelerine**gidin.
1. Yeni bir ölçek kümesi oluşturmak için **+ Ekle'yi** seçin.
1. **Yönetim** sekmesine gidin. 
1. Örnek **sonlandırma** bölümünü bulun.
1. **Örneğin sonlandırma bildirimi**için, **On'u**seçin.
1. **Sonlandırma gecikmesi (dakika)** için, istenen varsayılan zaman anına ayarlayın.
1. Yeni ölçek kümesini oluşturmayı **bitirdiğinizde, Gözden Geçir + oluştur** düğmesini seçin. 

> [!NOTE]
> Azure portalında varolan ölçek kümelerinde sonlandırma bildirimleri ayarlayamıyorsunuz

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, ölçek kümesi modelinde sonlandırma bildirimini sağlar.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "virtualMachineProfile": {
            "scheduledEventsProfile": {
                "terminateNotificationProfile": {
                    "notBeforeTimeout":"PT5M",
                    "enable":true
                }
            }
        }
    }        
}

```

Yukarıdaki blok, ölçek kümenizdeki tüm örneklerde sonlandırma işlemi için 5 dakikalık bir zaman alakart gecikmesi *(PT5M*tarafından belirtildiği gibi) belirtir. *NotBeforeTimeout,* ISO 8601 formatında 5 ile 15 dakika arasında herhangi bir değer alabilir. Yukarıda açıklanan *sonlandırmaBildirimiProfili* altında *notBeforeTimeout* özelliğini değiştirerek sonlandırma işlemi için varsayılan zaman öncesini değiştirebilirsiniz.

Scale set modelinde *scheduledEventsProfile'ı* etkinleştirdikten ve *notBeforeTimeout'u*ayarladıktan sonra, değişiklikleri yansıtacak şekilde tek tek örnekleri [en son modele](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) güncelleştirin.

> [!NOTE]
>Ölçek kümesi örneklerindeki bildirimleri sonlandırma yalnızca 2019-03-01 ve üzeri API sürümüyle etkinleştirilebilir

### <a name="azure-powershell"></a>Azure PowerShell
Yeni bir ölçek kümesi oluştururken, [Yeni-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) cmdlet kullanarak belirlenen ölçekte sonlandırma bildirimleri etkinleştirebilirsiniz.

Bu örnek komut dosyası yapılandırma dosyasını kullanarak bir ölçek kümesi ve ilişkili kaynakların oluşturulması ile yürür: [Tam bir sanal makine ölçek kümesi oluşturun.](./scripts/powershell-sample-create-complete-scale-set.md) Ölçek kümesi oluşturmak için yapılandırma nesnesine *Sonlandırma Zamanlanmış Olayları* ve *SonlandırmaZamanlıEventNotBeforeTimeoutInMinutes* parametrelerini ekleyerek yapılandırma sonlandırma bildirimi sağlayabilirsiniz. Aşağıdaki örnek, özelliği 10 dakikalık bir gecikme süresiyle etkinleştirir.

```azurepowershell-interactive
New-AzVmssConfig `
  -Location "VMSSLocation" `
  -SkuCapacity 2 `
  -SkuName "Standard_DS2" `
  -UpgradePolicyMode "Automatic" `
  -TerminateScheduledEvents $true `
  -TerminateScheduledEventNotBeforeTimeoutInMinutes 10
```

Varolan bir ölçek kümesinde sonlandırma bildirimlerini etkinleştirmek için [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet'i kullanın.

```azurepowershell-interactive
Update-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -TerminateScheduledEvents $true
  -TerminateScheduledEventNotBeforeTimeoutInMinutes 15
```
Yukarıdaki örnek, varolan bir ölçek kümesinde bildirimleri sonlandırmayı sağlar ve sonlandırma olayı için 15 dakikalık bir zaman ayarı ayarlar.

Ölçek kümesi modelinde zamanlanmış olayları etkinleştirdikten ve zaman ayarı yaptıktan sonra, değişiklikleri yansıtacak şekilde tek tek örnekleri [en son modele](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) güncelleştirin.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Aşağıdaki örnek, yeni bir ölçek kümesi oluştururken sonlandırma bildirimini etkinleştirmek içindir.

```azurecli-interactive
az group create --name <myResourceGroup> --location <VMSSLocation>
az vmss create \
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --image UbuntuLTS \
  --admin-username <azureuser> \
  --generate-ssh-keys \
  --terminate-notification-time 10
```

Yukarıdaki örnekte önce bir kaynak grubu oluşturulur, ardından 10 dakikalık varsayılan zaman ayarı için sonlandırma bildirimleri etkinleştirilmiş yeni bir ölçek kümesi oluşturulur.

Aşağıdaki örnek, varolan bir ölçek kümesinde sonlandırma bildirimini etkinleştirmek içindir.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --enable-terminate-notification true \
  --terminate-notification-time 10
```

## <a name="get-terminate-notifications"></a>Sonlandırma bildirimlerini alın

Sonlandırma bildirimleri, Azure Meta veri hizmeti olan [Zamanlanmış Etkinlikler](../virtual-machines/windows/scheduled-events.md)aracılığıyla teslim edilir. Azure Meta veri hizmeti, Sanal Makineler'in VM içinden erişilebilen bir REST Endpoint kullanarak sanal makineleri çalıştırmayla ilgili bilgileri ortaya çıkarır. Bilgiler, VM dışında açıkta olmaması için routable olmayan bir IP üzerinden kullanılabilir.

Etkinlikler için ilk kez istekte bulunduğunuzda ölçek kümeniz için Zamanlanmış Etkinlikler etkinleştirilir. İki dakikaya kadar olan ilk aramanızda gecikmeli yanıt bekleyebilirsiniz. Yaklaşan bakım olaylarını ve devam eden bakım etkinliklerinin durumunu algılamak için bitiş noktasını düzenli aralıklarla sorgula.

Ölçek kümesi örnekleri 24 saat boyunca istekte bulunmazsa, zamanlanmış Etkinlikler ölçek kümeniz için devre dışı bırakılır.

### <a name="endpoint-discovery"></a>Bitiş noktası bulma
VNET özellikli VM'ler için Meta veri hizmeti, 169.254.169.254 sayılı statik bir IP'den edinilebilir.

Zamanlanan Etkinlikler'in en son sürümü için tam bitiş noktası:
> 'http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01'

### <a name="query-response"></a>Sorgu yanıtı
Yanıt, zamanlanmış olaylar dizisi içerir. Boş bir dizi, şu anda zamanlanan olay olmadığı anlamına gelir.

Zamanlanan olayların olduğu durumlarda, yanıt bir dizi olay içerir. Bir "Sonlandırma" olayı için yanıt aşağıdaki gibi görünür:
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Terminate",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled",
            "NotBefore": {timeInUTC},
        }
    ]
}
```
*DocumentIncarnation* bir ETag ve Olaylar yük son sorgudan bu yana değişti olup olmadığını incelemek için kolay bir yol sağlar.

Yukarıdaki alanların her biri hakkında daha fazla bilgi için [Windows](../virtual-machines/windows/scheduled-events.md#event-properties) ve [Linux](../virtual-machines/linux/scheduled-events.md#event-properties)için Zamanlanmış Etkinlikler belgelerine bakın.

### <a name="respond-to-events"></a>Olaylara yanıt verme
Yaklaşan bir olayı öğrendikten ve zarif kapatma mantığınız tamamlandıktan sonra, EventId ile meta veri hizmetine BIR POST çağrısı yaparak olağanüstü olayı onaylayabilirsiniz. POST çağrısı Azure'a VM silme ile devam edebileceğini gösterir.

Aşağıda json POST istek gövdesinde bekleniyor. İstek, Başlangıç İsteklerinin bir listesini içermelidir. Her StartRequest, hızlandırmak istediğiniz olay için EventId'yi içerir:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

Ölçek kümesindeki her VM'nin yalnızca o VM ile ilgili EventID'yi onayladığından emin olun. Bir VM [örnek meta veriler aracılığıyla](virtual-machine-scale-sets-instance-ids.md#instance-metadata-vm-name)kendi VM adını alabilirsiniz. Bu ad "{scale-set-name}_{instance-id}" formunu alır ve yukarıda açıklanan sorgu yanıtının 'Kaynaklar' bölümünde görüntülenir.

PowerShell ve [Python'u](../virtual-machines/linux/scheduled-events.md#python-sample)kullanarak olayları sorgulamak [PowerShell](../virtual-machines/windows/scheduled-events.md#powershell-sample) ve yanıtlamak için örnek komut dosyalarına da başvurabilirsiniz.

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi yöntemler
-   Bildirimleri yalnızca 'silme' işlemlerinde sonlandırma – Tüm silme işlemleri (el ile silme veya Otomatik Ölçekle başlatılan ölçek-in) ölçek kümeniz *zamanlanmışEventsProfile* etkinse Sonlandırma olayları oluşturur. Yeniden başlatma, yeniden görüntülenme, yeniden dağıtma ve durdurma/anlaşma yeri gibi diğer işlemler Sonlandırma olayları oluşturmaz. Düşük öncelikli VM'ler için sonlandırma bildirimleri etkinleştirilenemez.
-   Zaman aşımı için zorunlu bekleme yok – Sonlandırma işlemini, olay alındıktan sonra ve etkinliğin *NotBefore* süresi dolmadan önce istediğiniz zaman başlatabilirsiniz.
-   Zaman silme sırasında zorunlu silme – Bir olay oluşturulduktan sonra zaman adedini genişletme olanağı yoktur. Zaman aşımı sona erdiğinde, bekleyen sonlandırma olayı işlenir ve VM silinir.
-   Değiştirilebilir zaman önce değeri – Ölçek kümesi modelindeki *notBeforeTimeout* özelliğini değiştirerek ve VM örneklerini en son modele güncelleyerek, bir örnek silinmeden önce zaman ekme değerini değiştirebilirsiniz.
-   Bekleyen tüm silmeleri onaylayın – onaylanmayan VM_1 bekleyen bir silme varsa ve VM_2'da başka bir sonlandırma olayını onayladıysanız, VM_1 için sonlandırma olayı onaylanana veya zaman aşımı geçene kadar VM_2 silinmez. VM_1 için sonlandırma olayını onayladıktan sonra hem VM_1 hem de VM_2 silinir.
-   Tüm eşzamanlı silmeleri onaylayın – Yukarıdaki örneği genişletme, VM_1 ve VM_2 aynı *NotBefore* zamanına sahipse, her iki sonlandırma olayı nın da onaylanması veya zaman aşımı sona ermeden önce VM'nin silinmemesi gerekir.

## <a name="troubleshoot"></a>Sorun giderme
### <a name="failure-to-enable-scheduledeventsprofile"></a>ZamanlanmışEtkinliklerin etkinleştirilmemesiProfil
"VirtualMachineProfile' türündeki nesnede üye 'scheduledEventsProfile' bulamadım" belirten bir hata iletisiyle 'BadRequest' hatası alırsanız, ölçek kümesi işlemleri için kullanılan API sürümünü denetleyin. Hesaplama API sürümü **2019-03-01** veya daha yüksek gereklidir. 

### <a name="failure-to-get-terminate-events"></a>Sonlandırma olaylarının alınamaması
Zamanlanmış Olaylar aracılığıyla herhangi bir **Sonlandırma** olayı almıyorsanız, olayları almak için kullanılan API sürümünü denetleyin. Metadata Service API sürümü **2019-01-01** veya daha yüksek Sonlandırma olayları için gereklidir.
>'http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01'

### <a name="getting-terminate-event-with-incorrect-notbefore-time"></a>Yanlış NotBefore zaman ile Sonlandırma olayı alma  
Scale set modelinde *scheduledEventsProfile'ı* etkinleştirdikten ve *notBeforeTimeout'u*ayarladıktan sonra, değişiklikleri yansıtacak şekilde tek tek örnekleri [en son modele](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı sanal makine ölçeği kümelerinde nasıl [dağıtılayacaklarınıöğrenin.](virtual-machine-scale-sets-deploy-app.md)
