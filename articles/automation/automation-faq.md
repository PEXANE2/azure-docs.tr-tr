---
title: Azure Otomasyonu SSS | Microsoft Docs
description: Bu makalede, Azure Otomasyonu hakkında sık sorulan sorulara yanıtlar verilmektedir.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 76c8d09ef2ef0130ddac856a1f37f8b68d977494
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186240"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure Otomasyonu sık sorulan sorular

Bu Microsoft SSS, Azure Otomasyonu hakkında sık sorulan soruların bir listesidir. Özellikleri hakkında başka sorularınız varsa, tartışma forumuna gidin ve sorularınızı gönderin. Bir soru sıkça sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekleyeceğiz.

## <a name="update-management"></a>Güncelleştirme Yönetimi

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltmelere engel olabilir miyim?

Red Hat Enterprise Linux gibi bazı Linux değişkenlerde, işletim sistemi düzeyinde yükseltmeler paketler aracılığıyla gerçekleşebilir. Bu, işletim sistemi sürümü numarasının değiştiği Güncelleştirme Yönetimi çalışmasına yol açabilir. Güncelleştirme Yönetimi, bir yöneticinin bir Linux makinesinde yerel olarak kullandığı paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu davranış bilerek yapılır.

İşletim sistemi sürümünün Güncelleştirme Yönetimi dağıtımlar aracılığıyla güncelleştirilmesini önlemek için **dışlama** özelliğini kullanın.

Red Hat Enterprise Linux, hariç tutulacak paket adı `redhat-release-server.x86_64` .

### <a name="why-arent-criticalsecurity-updates-applied"></a>Kritik/güvenlik güncelleştirmeleri neden uygulanmadı?

Güncelleştirmeleri bir Linux makinesine dağıtırken, güncelleştirme sınıflandırmalarını seçebilirsiniz. Bu seçenek, belirtilen ölçütlere uyan güncelleştirmeleri filtreler. Bu filtre, güncelleştirme dağıtıldığında makinede yerel olarak uygulanır.

Güncelleştirme Yönetimi bulutta güncelleştirme zenginleştirmesini gerçekleştirdiğinden, yerel makinede bu bilgilere sahip olmasa da, Güncelleştirme Yönetimi bir güvenlik etkisi olan bazı güncelleştirmelere bayrak atayabilirsiniz. Kritik güncelleştirmeleri bir Linux makinesine uygularsanız, bu makinede güvenlik etkisi varmış gibi işaretlenmemiş güncelleştirmeler olabilir ve bu nedenle uygulanmaz. Ancak, Güncelleştirme Yönetimi ilgili güncelleştirme hakkında ek bilgi içerdiğinden, bu makineyi hala uyumsuz olarak raporlayabilir.

Güncelleştirme sınıflandırmasına göre güncelleştirmelerin dağıtımı, CentOS 'in RTM sürümlerinde çalışmaz. CentOS güncelleştirmelerini doğru bir şekilde dağıtmak için, güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmalar ' ı seçin. SUSE için, sınıflandırma olarak yalnızca **diğer güncelleştirmelerin** seçilmesi, zypper (Paket Yöneticisi) ile ilgili güvenlik güncelleştirmelerinin veya bağımlılıklarından önce kullanılması durumunda bazı ek güvenlik güncelleştirmelerinin yüklenememesine neden olabilir. Bu davranış, zypper kısıtlamasıdır. Bazı durumlarda, güncelleştirme dağıtımını yeniden çalıştırmanız ve ardından güncelleştirme günlüğünden dağıtımı doğrulamanız gerekebilir.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Güncelleştirmeleri Azure kiracılarının tamamında dağıtabilir miyim?

Güncelleştirme Yönetimi başka bir Azure kiracı raporlamada düzeltme eki gerektiren makineleriniz varsa, bunları zamanlanmak için aşağıdaki geçici çözümü kullanmanız gerekir. [Yeni-AzAutomationSchedule](/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) cmdlet 'ini `ForUpdateConfiguration` bir zamanlama oluşturmak için belirtilen parametreyle birlikte kullanabilirsiniz. [New-AzAutomationSoftwareUpdateConfiguration](/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet 'ini kullanabilir ve diğer Kiracıdaki makineleri `NonAzureComputer` parametresine geçirebilirsiniz. Aşağıdaki örnek bunun nasıl yapılacağını göstermektedir.

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz burada yanıtlanmazsa, ek sorular ve yanıtlar için aşağıdaki kaynaklara başvurabilirsiniz.

- [Azure Otomasyonu](/answers/topics/azure-automation.html)
- [Geri Bildirim forumu](https://feedback.azure.com/forums/905242-update-management)
