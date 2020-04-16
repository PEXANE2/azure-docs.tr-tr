---
title: Azure Otomasyon SSS | Microsoft Dokümanlar
description: Azure Otomasyonu hakkında sık sorulan soruların yanıtları.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 3fa29f3df5f0434c4c61e8d12adbb3f55156a29f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405954"
---
# <a name="azure-automation-frequently-asked-questions"></a>Azure Automation sık sorulan sorular

Bu Microsoft SSS, Azure Otomasyonu hakkında sık sorulan soruların bir listesidir. Yetenekleri hakkında ek sorularınız varsa, tartışma forumuna gidin ve sorularınızı gönderin. Bir soru sık sık sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekliyoruz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="update-management-solution"></a>Güncelleştirme Yönetimi çözümü

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyinde yükseltmeleri önleyebilir miyim?

Red Hat Enterprise Linux gibi bazı Linux türevlerinde, paketler aracılığıyla işletim sistemi düzeyinde yükseltmeler oluşabilir. Bu, işletim sistemi sürüm numarasının değiştiği Güncelleştirme Yönetimi'ne yol açabilir. Güncelleştirme Yönetimi, bir yöneticinin Linux makinesinde yerel olarak kullandığı paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu davranış kasıtlıdır.

Update Management dağıtımları aracılığıyla işletim sistemi sürümünü güncelleştirmekten kaçınmak için **Dışlama** özelliğini kullanın.

Red Hat Enterprise Linux'ta, dışlanır paket adı . `redhat-release-server.x86_64`

### <a name="why-arent-criticalsecurity-updates-applied"></a>Kritik/güvenlik güncelleştirmeleri neden uygulanmaz?

Güncelleştirmeleri bir Linux makinesine dağıttığınızda, güncelleştirme sınıflandırmalarını seçebilirsiniz. Bu seçenek, belirtilen ölçütleri karşılayan güncelleştirmeleri filtreler. Bu filtre, güncelleştirme dağıtıldığında makineye yerel olarak uygulanır.

Güncelleştirme Yönetimi bulutta güncelleştirme zenginleştirme gerçekleştirdiğinden, yerel makinede bu bilgilere sahip olmasa bile Güncelleştirme Yönetimi'ndeki bazı güncelleştirmeleri güvenlik etkisi ne olduğunu belirtebilirsiniz. Bir Linux makinesine kritik güncelleştirmeler uygularsanız, bu makine üzerinde güvenlik etkisi olduğu işaretlenmemiş ve bu nedenle uygulanmayan güncelleştirmeler olabilir. Ancak, Güncelleştirme Yönetimi, ilgili güncelleştirme hakkında ek bilgilere sahip olduğundan, bu makineyi yine de uyumlu olmayan olarak bildirebilir.

Güncelleştirmesınıflandırması tarafından güncelleştirmeleri dağıtmak CentOS'un RTM sürümlerinde çalışmaz. CentOS güncelleştirmelerini düzgün bir şekilde dağıtmak için, güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmaları seçin. SUSE için, yalnızca **diğer güncelleştirmeleri** sınıflandırma olarak seçmek, önce zypper (paket yöneticisi) veya bağımlılıkları ile ilgili güvenlik güncelleştirmeleri gerekiyorsa bazı ek güvenlik güncelleştirmelerinin yüklenmesine neden olabilir. Bu davranış zypper bir sınırlamadır. Bazı durumlarda, güncelleştirme dağıtımını yeniden çalıştırmanız ve ardından güncelleştirme günlüğü aracılığıyla dağıtımı doğrulamanız gerekebilir.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>Güncelleştirmeleri Azure kiracılarına dağıtabilir miyim?

Güncelleştirme Yönetimi'ne bildiren başka bir Azure kiracısında düzeltme yapılması gereken makineleriniz varsa, bunları zamanlamak için aşağıdaki geçici çözümden yararlanmalısınız. Bir zamanlama oluşturmak için belirtilen `ForUpdateConfiguration` parametre ile [Yeni-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) cmdlet kullanabilirsiniz. [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet'i kullanabilir ve diğer kiracıdaki makineleri `NonAzureComputer` parametreye geçirebilirsiniz. Aşağıdaki örnekte, bunun nasıl yapılacağını gösterilmektedir.

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz burada yanıtlanmamışsa, ek sorular ve yanıtlar için aşağıdaki kaynaklara başvurabilirsiniz.

- [Azure Otomasyonu](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)
- [Geri Bildirim forumu](https://feedback.azure.com/forums/905242-update-management)
