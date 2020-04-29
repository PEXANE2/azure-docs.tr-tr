---
title: Azure Otomasyonu SSS | Microsoft Docs
description: Azure Otomasyonu hakkında sık sorulan soruların yanıtları.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 3fa29f3df5f0434c4c61e8d12adbb3f55156a29f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81405954"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure Otomasyonu sık sorulan sorular

Bu Microsoft SSS, Azure Otomasyonu hakkında sık sorulan soruların bir listesidir. Özellikleri hakkında başka sorularınız varsa, tartışma forumuna gidin ve sorularınızı gönderin. Bir soru sıkça sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekleyeceğiz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="update-management-solution"></a>Güncelleştirme Yönetimi çözümü

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltmelere engel olabilir miyim?

Red Hat Enterprise Linux gibi bazı Linux değişkenlerde, işletim sistemi düzeyinde yükseltmeler paketler aracılığıyla gerçekleşebilir. Bu, işletim sistemi sürümü numarasının değiştiği Güncelleştirme Yönetimi çalışmasına yol açabilir. Güncelleştirme Yönetimi, bir yöneticinin bir Linux makinesinde yerel olarak kullandığı paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu davranış bilerek yapılır.

İşletim sistemi sürümünün Güncelleştirme Yönetimi dağıtımlar aracılığıyla güncelleştirilmesini önlemek için **dışlama** özelliğini kullanın.

Red Hat Enterprise Linux, hariç tutulacak paket adı `redhat-release-server.x86_64`.

### <a name="why-arent-criticalsecurity-updates-applied"></a>Kritik/güvenlik güncelleştirmeleri neden uygulanmadı?

Güncelleştirmeleri bir Linux makinesine dağıtırken, güncelleştirme sınıflandırmalarını seçebilirsiniz. Bu seçenek, belirtilen ölçütlere uyan güncelleştirmeleri filtreler. Bu filtre, güncelleştirme dağıtıldığında makinede yerel olarak uygulanır.

Güncelleştirme Yönetimi bulutta güncelleştirme zenginleştirmesini gerçekleştirdiğinden, yerel makinede bu bilgilere sahip olmasa da, Güncelleştirme Yönetimi bir güvenlik etkisi olan bazı güncelleştirmelere bayrak atayabilirsiniz. Kritik güncelleştirmeleri bir Linux makinesine uygularsanız, bu makinede güvenlik etkisi varmış gibi işaretlenmemiş güncelleştirmeler olabilir ve bu nedenle uygulanmaz. Ancak, Güncelleştirme Yönetimi ilgili güncelleştirme hakkında ek bilgi içerdiğinden, bu makineyi hala uyumsuz olarak raporlayabilir.

Güncelleştirme sınıflandırmasına göre güncelleştirmelerin dağıtımı, CentOS 'in RTM sürümlerinde çalışmaz. CentOS güncelleştirmelerini doğru bir şekilde dağıtmak için, güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmalar ' ı seçin. SUSE için, sınıflandırma olarak yalnızca **diğer güncelleştirmelerin** seçilmesi, zypper (Paket Yöneticisi) ile ilgili güvenlik güncelleştirmelerinin veya bağımlılıklarından önce kullanılması durumunda bazı ek güvenlik güncelleştirmelerinin yüklenememesine neden olabilir. Bu davranış, zypper kısıtlamasıdır. Bazı durumlarda, güncelleştirme dağıtımını yeniden çalıştırmanız ve ardından güncelleştirme günlüğünden dağıtımı doğrulamanız gerekebilir.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Güncelleştirmeleri Azure kiracılarının tamamında dağıtabilir miyim?

Güncelleştirme Yönetimi başka bir Azure kiracı raporlamada düzeltme eki gerektiren makineleriniz varsa, bunları zamanlanmak için aşağıdaki geçici çözümü kullanmanız gerekir. [Yeni-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) cmdlet 'ini bir zamanlama oluşturmak için belirtilen `ForUpdateConfiguration` parametreyle birlikte kullanabilirsiniz. [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet 'ini kullanabilir ve diğer Kiracıdaki makineleri `NonAzureComputer` parametresine geçirebilirsiniz. Aşağıdaki örnek bunun nasıl yapılacağını göstermektedir.

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz burada yanıtlanmazsa, ek sorular ve yanıtlar için aşağıdaki kaynaklara başvurabilirsiniz.

- [Azure Otomasyonu](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)
- [Geri Bildirim forumu](https://feedback.azure.com/forums/905242-update-management)
