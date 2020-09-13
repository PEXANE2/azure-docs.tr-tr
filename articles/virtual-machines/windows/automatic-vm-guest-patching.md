---
title: Azure 'da Windows VM 'Leri için otomatik VM Konuk düzeltme eki uygulama
description: Azure 'da Windows sanal makinelerini otomatik düzeltme eki uygulama hakkında bilgi edinin
author: mayanknayar
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.topic: how-to
ms.date: 09/09/2020
ms.author: manayar
ms.openlocfilehash: 47ac9fa91f391442691661a3ba03dd1f0d918601
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89669064"
---
# <a name="preview-automatic-vm-guest-patching-for-windows-vms-in-azure"></a>Önizleme: Azure 'da Windows VM 'Leri için otomatik VM Konuk düzeltme eki uygulama

Windows VM 'leriniz için otomatik sanal makine konuk düzeltme eki uygulamayı etkinleştirmek, güncelleştirme yönetimini güvenli bir şekilde ve sanal makinelere güvenlik uyumluluğunu sürdürmek için otomatik olarak yama sağlar

Otomatik VM Konuk düzeltme eki uygulama aşağıdaki özelliklere sahiptir:
- *Kritik* veya *güvenlik* olarak sınıflandırılan yamalar sanal makineye otomatik olarak indirilir ve uygulanır.
- Düzeltme ekleri, VM 'nin saat diliminde yoğun olmayan saatlerde uygulanır.
- Düzeltme Eki düzenleme Azure tarafından yönetilir ve düzeltme ekleri, kullanılabilirlik-ilk ilkelere göre uygulanır.
- Platform sistem durumu sinyalleriyle belirlenen sanal makine sistem durumu, düzeltme eki uygulama başarısızlıklarını algılamak için izlenir.
- Tüm VM boyutları için geçerlidir.

> [!IMPORTANT]
> Otomatik VM Konuk düzeltme eki uygulama şu anda genel önizleme aşamasındadır. Aşağıda açıklanan genel önizleme işlevlerini kullanmak için bir katılım prosedürü gereklidir.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-does-automatic-vm-guest-patching-work"></a>Otomatik VM Konuk düzeltme eki uygulama nasıl çalışır?

VM 'de otomatik VM Konuk düzeltme eki uygulama etkinse, kullanılabilir *kritik* ve *GÜVENLIK* düzeltme ekleri VM 'de otomatik olarak indirilir ve uygulanır. Bu işlem, Windows Update aracılığıyla yeni düzeltme noktaları bırakıldığında her ay otomatik olarak kapatılır. Düzeltme Eki değerlendirmesi ve yükleme otomatiktir ve işlem VM 'nin gerektiği şekilde yeniden başlatılmasını içerir.

VM, bu VM için ilgili düzeltme eklerini belirlemede düzenli olarak değerlendirilir. Düzeltme ekleri, sanal makine için yoğun olmayan saatlerde sanal makineye herhangi bir gün yüklenebilir. Bu otomatik değerlendirme, eksik olan tüm düzeltme eklerinin olası en erken fırsatta bulunmasını sağlar.

Düzeltme ekleri, aşağıda açıklanan kullanılabilirlik ilk Orchestration sonrasında aylık Windows Update sürümünün 30 gün içinde yüklenir. Düzeltme ekleri, VM 'nin saat dilimine bağlı olarak VM için yalnızca yoğun olmayan saatlerde yüklenir. Yamaların otomatik olarak yüklenmesi için sanal makinenin yoğun olmayan saatlerde çalışıyor olması gerekir. Bir sanal makine düzenli bir değerlendirme sırasında kapatılmışsa, sanal makine otomatik olarak değerlendirilir ve geçerli düzeltme ekleri, sanal makine açıldığında sonraki dönemsel değerlendirme sırasında otomatik olarak yüklenir.

Diğer düzeltme eki sınıflandırmaları ile düzeltme eklerini yüklemek veya kendi özel bakım pencerenizde düzeltme eki yüklemeyi zamanlamak için [güncelleştirme yönetimi](tutorial-config-management.md#manage-windows-updates)kullanabilirsiniz.

### <a name="availability-first-patching"></a>Kullanılabilirlik-ilk düzeltme eki uygulama

Düzeltme eki yükleme işlemi, otomatik VM Konuk düzeltme eki etkin olan tüm VM 'Ler için Azure tarafından genel olarak düzenlenir. Bu düzenleme, Azure tarafından sunulan farklı kullanılabilirlik seviyeleri genelinde kullanılabilirlik-ilk ilkeleri izler.

Güncelleştirme yapılmakta olan bir sanal makine grubu için, Azure platformu güncelleştirmeleri düzenler:

**Bölgeler arasında:**
- Aylık güncelleştirme, genel dağıtım başarısızlıklarını engellemek için Azure genelinde küresel olarak düzenlenir.
- Bir aşamada bir veya daha fazla bölge olabilir ve bir güncelleştirme, bir aşamadaki uygun VM 'Ler başarıyla güncelleşirse bir sonraki aşamalara gider.
- Coğrafi eşleştirilmiş bölgeler eşzamanlı olarak güncellenmez ve aynı bölgesel aşamada olamaz.
- Bir güncelleştirmenin başarısı, sanal makinenin sistem durumu gönderi güncelleştirmesi izlenerek ölçülür. VM sistem durumu, VM için platform sistem durumu göstergeleri aracılığıyla izlenir.

**Bir bölge içinde:**
- Farklı Kullanılabilirlik Alanları sanal makineler eşzamanlı olarak güncellenmez.
- Bir abonelik içindeki tüm VM 'Lerin eş zamanlı güncelleştirmelerini önlemek için bir kullanılabilirlik kümesinin parçası olmayan VM 'Ler en iyi çaba temelinde toplu olarak oluşturulur.

**Bir kullanılabilirlik kümesi içinde:**
- Ortak bir kullanılabilirlik kümesindeki tüm VM 'Ler eşzamanlı olarak güncellenmez.
-   Ortak bir kullanılabilirlik kümesindeki VM 'Ler güncelleştirme etki alanı sınırları içinde güncelleştirilir ve birden çok güncelleştirme etki alanındaki VM 'Ler eşzamanlı olarak güncellenmez.

Belirli bir sanal makıne için düzeltme eki yükleme tarihi, aylık düzeltme eki uygulama döngülerine göre farklı bir toplu işte alınabilir.

## <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
Önizlemede yalnızca belirli işletim sistemi platformu görüntülerinden oluşturulan VM 'Ler desteklenir. Özel görüntüler şu anda önizlemede desteklenmiyor.

Aşağıdaki platform SKU 'Ları Şu anda desteklenmektedir (ve daha fazla düzenli olarak eklenir):

| Publisher               | İşletim sistemi teklifi      |  Sku               |
|-------------------------|---------------|--------------------|
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter |
| Microsoft Corporation   | WindowsServer | 2016-veri merkezi    |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-Server-Core |
| Microsoft Corporation   | WindowsServer | 2019-veri merkezi |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-Server-Core |

## <a name="patch-orchestration-modes"></a>Düzeltme Eki düzenleme modları
Azure 'da Windows VM 'Leri artık aşağıdaki düzeltme eki düzenleme modlarını desteklemektedir:

**AutomaticByPlatform:**
- Bu mod, Windows sanal makinesi için otomatik VM Konuk düzeltme eki uygulamayı sağlar ve sonraki düzeltme eki yüklemesi Azure tarafından düzenlenir.
- Bu modun ayarlanması ayrıca çoğaltmayı önlemek için Windows sanal makinesinde yerel otomatik güncelleştirmeleri devre dışı bırakır.
- Bu mod yalnızca yukarıda desteklenen işletim sistemi platformu görüntüleri kullanılarak oluşturulan VM 'Ler için desteklenir.
- Bu modu kullanmak için, özelliğini ayarlayın `osProfile.windowsConfiguration.enableAutomaticUpdates=true` ve  `osProfile.windowsConfiguration.patchSettings.patchMode=AutomaticByPlatfom` VM şablonundaki özelliği ayarlayın.

**Otomatik olarak:**
- Bu mod, Windows sanal makinesinde otomatik güncelleştirme yapılmasını ve Otomatik Güncelleştirmeler aracılığıyla sanal makineye düzeltme ekleri yüklenmesini mümkün.
- Bu mod, başka bir yama modu belirtilmemişse varsayılan olarak ayarlanır.
- Bu modu kullanmak için özelliği ayarlayın `osProfile.windowsConfiguration.enableAutomaticUpdates=true` ve  `osProfile.windowsConfiguration.patchSettings.patchMode=AutomaticByOS` VM şablonundaki özelliği ayarlayın.

**El ile:**
- Bu mod, Windows sanal makinesinde otomatik güncelleştirmeleri devre dışı bırakır.
- Bu mod, özel düzeltme eki uygulama çözümleri kullanılırken ayarlanmalıdır.
- Bu modu kullanmak için özelliği ayarlayın `osProfile.windowsConfiguration.enableAutomaticUpdates=false` ve  `osProfile.windowsConfiguration.patchSettings.patchMode=Manual` VM şablonundaki özelliği ayarlayın.

> [!NOTE]
>Özelliği `osProfile.windowsConfiguration.enableAutomaticUpdates` Şu anda yalnızca VM ilk oluşturulduğunda ayarlanabilir. El Ile bir otomatik moda veya otomatik moddan El Ile moduna geçiş Şu anda desteklenmiyor. AutomaticByOS modundan Automaticbyplatfod moduna geçiş destekleniyor

## <a name="requirements-for-enabling-automatic-vm-guest-patching"></a>Otomatik VM Konuk düzeltme eki uygulamayı etkinleştirme gereksinimleri

- Sanal makinede [Azure VM aracısının](../extensions/agent-windows.md) yüklü olması gerekir.
- Windows Update hizmeti sanal makinede çalışıyor olmalıdır.
- Sanal makine Windows Update uç noktalarına erişebilmelidir. Sanal makineniz Windows Server Update Services (WSUS) kullanacak şekilde yapılandırıldıysa, ilgili WSUS sunucusu uç noktalarına erişilebilir olması gerekir.
- Işlem API 'SI sürüm 2020-06-01 veya üstünü kullanın.

Önizleme işlevselliğinin etkinleştirilmesi, aşağıda açıklandığı gibi, abonelik başına *InGuestAutoPatchVMPreview* özelliği için tek seferlik bir kabul gerektirir.

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte, aboneliğiniz için önizlemenin nasıl etkinleştirileceği açıklanmaktadır:

```
POST on `/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/InGuestAutoPatchVMPreview/register?api-version=2015-12-01`
```

Özellik kaydı 15 dakikaya kadar sürebilir. Kayıt durumunu denetlemek için:

```
GET on `/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/InGuestAutoPatchVMPreview?api-version=2015-12-01`
```

Aboneliğiniz için özellik kaydedildikten sonra, değişikliği işlem kaynak sağlayıcısına yayarak katılım işlemini doldurun.

```
POST on `/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2020-06-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
Aboneliğiniz için Önizlemeyi etkinleştirmek üzere [register-AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature) cmdlet 'ini kullanın.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName InGuestAutoPatchVMPreview -ProviderNamespace Microsoft.Compute
```

Özellik kaydı 15 dakikaya kadar sürebilir. Kayıt durumunu denetlemek için:

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName InGuestAutoPatchVMPreview -ProviderNamespace Microsoft.Compute
```

Aboneliğiniz için özellik kaydedildikten sonra, değişikliği işlem kaynak sağlayıcısına yayarak katılım işlemini doldurun.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Aboneliğiniz için Önizlemeyi etkinleştirmek üzere [az Feature Register](/cli/azure/feature#az-feature-register) kullanın.

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name InGuestAutoPatchVMPreview
```

Özellik kaydı 15 dakikaya kadar sürebilir. Kayıt durumunu denetlemek için:

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name InGuestAutoPatchVMPreview
```

Aboneliğiniz için özellik kaydedildikten sonra, değişikliği işlem kaynak sağlayıcısına yayarak katılım işlemini doldurun.

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```
## <a name="enable-automatic-vm-guest-patching"></a>Otomatik VM Konuk düzeltme eki uygulamayı etkinleştir
Otomatik VM Konuk düzeltme eki uygulamayı etkinleştirmek için, sanal makine şablon tanımında *Osprofile. windowsConfiguration. enableAutomaticUpdates* özelliğinin *true* olarak ayarlandığından emin olun. Bu özellik yalnızca VM oluşturulurken ayarlanabilir.

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte, otomatik VM Konuk düzeltme eki uygulama 'nın nasıl etkinleştirileceği açıklanmaktadır:

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVirtualMachine?api-version=2020-06-01`
```

```json
{
  "properties": {
    "osProfile": {
      "windowsConfiguration": {
        "provisionVMAgent": true,
        "enableAutomaticUpdates": true,
        "patchSettings": {
          "patchMode": "AutomaticByPlatform"
        }
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
VM oluştururken veya güncelleştirirken otomatik VM Konuk düzeltme eki uygulamayı etkinleştirmek için [set-AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem) cmdlet 'ini kullanın.

```azurepowershell-interactive
Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate -PatchMode "AutomaticByPlatform"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Yeni bir VM oluştururken otomatik VM Konuk düzeltme eki uygulamayı etkinleştirmek için [az VM Create](/cli/azure/vm#az-vm-create) kullanın. Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *myvm* adlı BIR VM için otomatik VM Konuk düzeltme eki uygulamayı yapılandırır:

```azurecli-interactive
az vm create --resource-group myResourceGroup --name myVM --image Win2019Datacenter --enable-agent --enable-auto-update --patch-mode AutomaticByPlatform
```

Var olan bir VM 'yi değiştirmek için [az VM Update](/cli/azure/vm#az-vm-update) kullanın

```azurecli-interactive
az vm update --resource-group myResourceGroup --name myVM --set osProfile.windowsConfiguration.enableAutomaticUpdates=true osProfile.windowsConfiguration.patchSettings.patchMode=AutomaticByPlatform
```

## <a name="enablement-and-assessment"></a>Etkinleştirme ve değerlendirme

> [!NOTE]
>VM 'nin yoğun olmayan saatlerde etkinleştirilmesi durumunda, etkinleştirme tamamlanana kadar, VM 'de otomatik VM Konuk güncelleştirmelerini etkinleştirmek üç saatten fazla sürebilir. Değerlendirme ve düzeltme eki yüklemesi yalnızca yoğun olmayan saatlerde Gerçekleşmede, yama uygulamak için sanal makinenizin yoğun olmayan saatlerde da çalışıyor olması gerekir.

Sanal makine için otomatik sanal makine konuk düzeltme eki uygulama etkinleştirildiğinde, VM 'ye türünde bir VM Uzantısı `Microsoft.CPlat.Core.WindowsPatchExtension` yüklenir. Bu uzantı, otomatik VM Konuk düzeltme eki uygulama işleminin bir parçası olarak Azure platformu tarafından yönetildiğinden, bu uzantının el ile yüklenmesi veya güncellenmesi gerekmez.

VM 'nin yoğun olmayan saatlerde etkinleştirilmesi durumunda, etkinleştirme tamamlanana kadar, VM 'de otomatik VM Konuk güncelleştirmelerini etkinleştirmek üç saatten fazla sürebilir. Uzantı Ayrıca VM için yoğun olmayan saatlerde yüklenir ve güncelleştirilir. Etkinleştirme tamamlanmadan önce VM 'nin yoğun olmayan saatleri sona erdirmek için etkinleştirme işlemi, sonraki kullanılabilir yoğun süre boyunca sürdürülür. VM daha önce otomatik Windows Update AutomaticByOS düzeltme eki modunda açıksa, uzantı yüklendiğinde otomatik Windows Update sanal makine için devre dışıdır.

Otomatik VM Konuk düzeltme eki 'nin tamamlanıp tamamlanmadığını ve düzeltme eki uygulama 'nın VM 'de yüklü olup olmadığını doğrulamak için VM 'nin örnek görünümünü gözden geçirebilirsiniz. Etkinleştirme işlemi tamamlandıktan sonra uzantı yüklenir ve VM 'nin değerlendirme sonuçları altında kullanılabilir olacaktır `patchStatus` . VM 'nin örnek görünümüne aşağıda açıklandığı gibi birden çok yolla erişilebilir.

### <a name="rest-api"></a>REST API

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVirtualMachine/instanceView?api-version=2020-06-01`
```
### <a name="azure-powershell"></a>Azure PowerShell
VM 'niz için örnek görünümüne erişmek üzere parametresiyle [Get-AzVM](/powershell/module/az.compute/get-azvm) cmdlet 'ini kullanın `-Status` .

```azurepowershell-interactive
Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM" -Status
```

PowerShell Şu anda yalnızca yama uzantısı hakkında bilgi sağlamaktadır. Hakkındaki bilgiler `patchStatus` yakında PowerShell aracılığıyla da kullanılabilir.

### <a name="azure-cli-20"></a>Azure CLI 2.0
VM 'niz için örnek görünümüne erişmek için [az VM Get-instance-View](/cli/azure/vm#az-vm-get-instance-view) kullanın.

```azurecli-interactive
az vm get-instance-view --resource-group myResourceGroup --name myVM
```

### <a name="understanding-the-patch-status-for-your-vm"></a>VM 'niz için düzeltme eki durumunu anlama

`patchStatus`Örnek görünümü yanıtının bölümü, sanal makinenizin en son değerlendirmesi ve son düzeltme eki yüklemesiyle ilgili ayrıntıları sağlar.

SANAL makinenizin değerlendirme sonuçları, bölümünde incelenebilir `availablePatchSummary` . Otomatik VM Konuk düzeltme eki uygulama özelliği etkin olan bir sanal makine için bir değerlendirme düzenli olarak yürütülür. Bir değerlendirme sonrasında kullanılabilir düzeltme eklerinin sayısı `criticalAndSecurityPatchCount` ve `otherPatchCount` sonuçları. Otomatik VM Konuk düzeltme eki uygulama, *kritik* ve *güvenlik* düzeltme eki sınıflandırmaları kapsamında değerlendirilen tüm düzeltme eklerini yükler. Diğer değerlendirilmiş düzeltme ekleri atlanır.

VM 'niz için düzeltme eki yükleme sonuçları, bölümünde incelenebilir `lastPatchInstallationSummary` . Bu bölümde, yüklenen, bekleyen, başarısız veya Atlanan düzeltme eklerinin sayısı dahil olmak üzere VM 'deki son düzeltme eki yükleme girişimine ilişkin ayrıntılar sağlanmaktadır. Düzeltme ekleri yalnızca VM için yoğun olmayan saatler bakım penceresi sırasında yüklenir. Bekleyen ve başarısız düzeltme ekleri, sonraki yoğun olmayan saatlerde bakım penceresi sırasında otomatik olarak yeniden denenir.

## <a name="on-demand-patch-assessment"></a>İsteğe bağlı düzeltme eki değerlendirmesi
VM 'niz için otomatik VM Konuk düzeltme eki uygulama özelliği zaten etkinse, VM 'de sanal makinenin yoğun olmayan saatlerde düzenli aralıklarla bir düzeltme eki değerlendirmesi yapılır. Bu işlem otomatiktir ve en son değerlendirmenin sonuçları, bu belgede daha önce açıklandığı gibi VM 'nin örnek görünümü aracılığıyla incelenebilir. Ayrıca, istediğiniz zaman VM 'niz için isteğe bağlı bir yama değerlendirmesi tetikleyebilirsiniz. Düzeltme Eki değerlendirmesi işleminin tamamlanması birkaç dakika sürebilir ve en son değerlendirmenin durumu VM 'nin örnek görünümünde güncelleştirilir.

> [!NOTE]
>İsteğe bağlı düzeltme eki değerlendirmesi, düzeltme ekinin yüklü olarak otomatik olarak tetiklemez. VM için değerlendirilen ve geçerli düzeltme ekleri, bu belgede daha önce açıklanan kullanılabilirlik için ilk düzeltme eki uygulama işleminin ardından VM 'nin yoğun olmayan saatlerde yüklenecektir.

### <a name="rest-api"></a>REST API
```
POST on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVirtualMachine/assessPatches?api-version=2020-06-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
Sanal makineniz için kullanılabilir düzeltme eklerini değerlendirmek üzere [Invoke-AzVmPatchAssessment](/powershell/module/az.compute/invoke-azvmpatchassessment) cmdlet 'ini kullanın.

```azurepowershell-interactive
Invoke-AzVmPatchAssessment -ResourceGroupName "myResourceGroup" -VMName "myVM"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Sanal makineniz için kullanılabilir düzeltme eklerini değerlendirmek için [az VM değerlendir-Patches](/cli/azure/vm#az-vm-assess-patches) kullanın.

```azurecli-interactive
az vm assess-patches --resource-group myResourceGroup --name myVM
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows sanal makinelerini oluşturma ve yönetme hakkında daha fazla bilgi edinin](tutorial-manage-vm.md)
