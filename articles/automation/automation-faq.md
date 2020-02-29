---
title: Azure Otomasyonu SSS | Microsoft Docs
description: Azure Otomasyonu hakkında sık sorulan soruların yanıtları.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 129a5316c2e7be329b479c79706791e993d20b74
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77925819"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure Otomasyonu sık sorulan sorular

Bu Microsoft SSS, Azure Otomasyonu hakkında sık sorulan soruların bir listesidir. Özellikleri hakkında başka sorularınız varsa, tartışma forumuna gidin ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.

## <a name="update-management-solution"></a>Güncelleştirme Yönetimi çözümü

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltmelere engel olabilir miyim?

Red Hat Enterprise Linux gibi bazı Linux değişkenlerde, işletim sistemi düzeyinde yükseltmeler paketler aracılığıyla gerçekleşebilir. Bu, işletim sistemi sürümü numarasının değiştiği Güncelleştirme Yönetimi çalışmasına yol açabilir. Güncelleştirme Yönetimi, bir yöneticinin Linux makinesinde yerel olarak kullanacağı paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu davranış bilerek yapılır.

İşletim sistemi sürümünün Güncelleştirme Yönetimi dağıtımlar aracılığıyla güncelleştirilmesini önlemek için **dışlama** özelliğini kullanın.

Red Hat Enterprise Linux, hariç tutulacak paket adı RedHat-Release-Server. x86_64.

### <a name="why-arent-criticalsecurity-updates-applied"></a>Kritik/güvenlik güncelleştirmeleri neden uygulanmadı?

Güncelleştirmeleri bir Linux makinesine dağıtırken, güncelleştirme sınıflandırmalarını seçebilirsiniz. Bu seçenek, belirtilen ölçütlere uyan makineye uygulanan güncelleştirmeleri filtreler. Bu filtre, güncelleştirme dağıtıldığında makinede yerel olarak uygulanır.

Güncelleştirme Yönetimi bulutta güncelleştirme zenginleştirmesini gerçekleştirdiğinden, bazı güncelleştirmeler, yerel makinede bu bilgilere sahip olmasa bile güvenlik etkisi olan Güncelleştirme Yönetimi olarak işaretlenir. Sonuç olarak, bir Linux makinesine kritik güncelleştirmeler uygularsanız, bu makinede güvenlik etkisi olması halinde işaretlenmemiş güncelleştirmeler olabilir ve bu nedenle güncelleştirmelerin uygulanmadığından emin olabilirsiniz. Ancak, Güncelleştirme Yönetimi ilgili güncelleştirme hakkında ek bilgi içerdiğinden, bu makineyi hala uyumsuz olarak raporlayabilir.

Güncelleştirme sınıflandırmasına göre güncelleştirmelerin dağıtımı, CentOS 'in RTM sürümlerinde çalışmaz. CentOS güncelleştirmelerini doğru bir şekilde dağıtmak için, güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmalar ' ı seçin. SUSE için, sınıflandırma olarak *yalnızca* **diğer güncelleştirmelerin** seçilmesi, zypper (Paket Yöneticisi) ile ilgili güvenlik güncelleştirmelerinin veya bağımlılıklarından önce kullanılması durumunda bazı güvenlik güncelleştirmelerinin de yüklenememesine neden olabilir. Bu davranış, zypper kısıtlamasıdır. Bazı durumlarda, güncelleştirme dağıtımını yeniden çalıştırmanız gerekebilir. Doğrulamak için güncelleştirme günlüğünü kontrol edin.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Güncelleştirmeleri Azure kiracılarının tamamında dağıtabilir miyim?

Başka bir Azure kiracı raporlarında, düzeltme eki uygulamanız gerektiğini Güncelleştirme Yönetimi makineniz varsa, bunları zamanlanmak için aşağıdaki geçici çözümü kullanmanız gerekir. Bir zamanlama oluşturmak için [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet 'ini `-ForUpdate` anahtarıyla kullanabilir ve [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet 'ini kullanarak diğer Kiracıdaki makineleri `-NonAzureComputer` parametresine geçirebilirsiniz. Aşağıdaki örnek bunun nasıl yapılacağını göstermektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz burada yanıtlanmazsa, ek sorular ve yanıtlar için aşağıdaki foruma bakabilirsiniz.

- [Azure Otomasyonu](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)

Güncelleştirme Yönetimi çözümü hakkında genel geri bildirimde bulunmak için lütfen [geri bildirim forumunu](https://feedback.azure.com/forums/905242-update-management)ziyaret edin.