---
title: Azure Otomasyon SSS | Microsoft Dokümanlar
description: Azure Otomasyonu hakkında sık sorulan soruların yanıtları.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 129a5316c2e7be329b479c79706791e993d20b74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77925819"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure Automation sık sorulan sorular

Bu Microsoft SSS, Azure Otomasyonu hakkında sık sorulan soruların bir listesidir. Yetenekleri hakkında ek sorularınız varsa, tartışma forumuna gidin ve sorularınızı gönderin. Bir soru sık sık sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekliyoruz.

## <a name="update-management-solution"></a>Güncelleştirme Yönetimi çözümü

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltmeleri önleyebilir miyim?

Red Hat Enterprise Linux gibi bazı Linux türevlerinde, paketler aracılığıyla işletim sistemi düzeyinde yükseltmeler oluşabilir. Bu, işletim sistemi sürüm numarasının değiştiği Güncelleştirme Yönetimi çalıştırılmasına neden olabilir. Güncelleştirme Yönetimi, bir yöneticinin Linux makinesinde yerel olarak kullanacağı paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu davranış kasıtlıdır.

Update Management dağıtımları aracılığıyla işletim sistemi sürümünü güncelleştirmekten kaçınmak için **Dışlama** özelliğini kullanın.

Red Hat Enterprise Linux'ta, dışlanır paket adı redhat-release-server.x86_64'dir.

### <a name="why-arent-criticalsecurity-updates-applied"></a>Kritik/güvenlik güncelleştirmeleri neden uygulanmaz?

Güncelleştirmeleri bir Linux makinesine dağıttığınızda, güncelleştirme sınıflandırmalarını seçebilirsiniz. Bu seçenek, belirtilen ölçütleri karşılayan makineye uygulanan güncelleştirmeleri filtreler. Bu filtre, güncelleştirme dağıtıldığında makineye yerel olarak uygulanır.

Güncelleştirme Yönetimi bulutta güncelleştirme zenginleştirme gerçekleştirdiğinden, yerel makinede bu bilgilere sahip olmasa bile bazı güncelleştirmeler Güncelleştirme Yönetimi'nde güvenlik etkisi ne kadar dır olarak işaretlenebilir. Sonuç olarak, bir Linux makinesine kritik güncelleştirmeler uygularsanız, bu makine üzerinde güvenlik etkisi olduğu işaretlenmemiş güncelleştirmeler olabilir ve bu nedenle güncelleştirmeler uygulanmaz. Ancak, Güncelleştirme Yönetimi, ilgili güncelleştirme hakkında ek bilgilere sahip olduğundan, bu makineyi yine de uyumlu olmayan olarak bildirebilir.

Güncelleştirmesınıflandırması tarafından güncelleştirmeleri dağıtmak CentOS'un RTM sürümlerinde çalışmaz. CentOS güncelleştirmelerini düzgün bir şekilde dağıtmak için, güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmaları seçin. SUSE için, *yalnızca* **diğer güncelleştirmeleri** sınıflandırma olarak seçmek, önce zypper (paket yöneticisi) veya bağımlılıkları ile ilgili güvenlik güncelleştirmeleri gerekiyorsa bazı güvenlik güncelleştirmelerinin de yüklenmesine neden olabilir. Bu davranış zypper bir sınırlamadır. Bazı durumlarda, güncelleştirme dağıtımını yeniden çalıştırmanız gerekebilir. Doğrulamak için güncelleştirme günlüğünü kontrol edin.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Güncelleştirmeleri Azure kiracılarına dağıtabilir miyim?

Başka bir Azure kiracınızda düzeltme talimatı vermeniz gereken makineler varsa, bunları zamanlamak için aşağıdaki geçici çözüme sahip olmanız gerekir. Bir zamanlama oluşturmak için `-ForUpdate` anahtarla [Birlikte Yeni AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet'ini kullanabilir ve Yeni [AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet'i `-NonAzureComputer` kullanabilir ve diğer kiracıdaki makineleri parametreye geçirebilirsiniz. Aşağıdaki örnekte, bunun nasıl yapılacağını gösterilmektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz burada yanıtlanmamışsa, ek sorular ve yanıtlar için aşağıdaki foruma başvurabilirsiniz.

- [Azure Otomasyonu](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)

Güncelleme Yönetimi çözümü hakkında genel geri bildirim için lütfen [geri bildirim forumuna](https://feedback.azure.com/forums/905242-update-management)ziyaret edin.