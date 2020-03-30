---
title: Azure Otomasyonunda çocuk runbook'ları
description: Azure Otomasyonu'nda başka bir runbook'tan bir runbook başlatmak ve aralarında bilgi paylaşmak için farklı yöntemleri açıklar.
services: automation
ms.subservice: process-automation
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 42362a170f493afd51a5d4ee139620ad25b54e79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367372"
---
# <a name="child-runbooks-in-azure-automation"></a>Azure Otomasyonunda çocuk runbook'ları

Azure Otomasyonu'nda, diğer runbook'lar tarafından çağrılan ayrı bir işleve sahip yeniden kullanılabilir, modüler runbook'lar yazmak önerilen bir uygulamadır. Üst çalışma kitabı genellikle gerekli işlevselliği gerçekleştirmek için bir veya daha fazla alt runbook çağırır. Bir alt çalışma kitabını çağırmanın iki yolu vardır ve senaryolarınız için hangisinin en iyi olduğunu belirlemek için anlamanız gereken farklı farklılıklar vardır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Satır satır yürütmeyi kullanarak bir alt çalışma defterini çağırmak

Başka bir runbook'tan satır satır çağırmak için, runbook'un adını kullanın ve parametreleri için değerler sağlayın, tıpkı bir etkinlik veya cmdlet kullandığınız gibi. Aynı Otomasyon hesabındaki tüm runbook'lar bu şekilde kullanılmak üzere diğer tüm kitaplar tarafından kullanılabilir. Üst çalışma defteri, bir sonraki satıra geçmeden önce alt çalışma defterinin tamamlanmasını bekler ve çıktıdoğrudan üst öğeye döner.

Bir runbook'u satır içi olarak çağırdığınızda üst runbook ile aynı işi çalıştırır. Çocuk runbook iş geçmişinde hiçbir belirti yoktur. Alt çalışma kitabındaki tüm özel durumlar ve akış çıktıları üst öğeyle ilişkilidir. Bu davranış, daha az iş ile sonuçlanır ve bunların izlenmesini ve sorun gidermesini kolaylaştırır.

Bir runbook yayımlandığında, aradığı alt runbook'ların zaten yayımlanmış olması gerekir. Bunun nedeni, Azure Otomasyon'un bir runbook'u derlediğinde herhangi bir alt runbook ile bir ilişki oluşturmasıdır. Alt runbook'lar zaten yayımlanmamışsa, üst çalışma kitabı düzgün yayımlanmış gibi görünür, ancak başlatıldığında bir özel durum oluşturur. Bu durumda, alt çalışma kitaplarına düzgün bir şekilde başvurmak için üst çalışma kitabını yeniden yayımlayabilirsiniz. İlişkilendirme zaten oluşturulduğu için alt çalışma kitabı değiştirilirse, üst basamak çalışmasını yeniden yayımlamanız gerekmez.

Satır adı verilen bir alt runbook parametreleri karmaşık nesneler de dahil olmak üzere herhangi bir veri türü, olabilir. Azure portalını kullanarak veya [Start-AzAutomationRunbook](/powershell/module/Az.Automation/Start-AzAutomationRunbook) cmdlet ile runbook'u başlattığınızda olduğu gibi [JSON serileştirmesi](start-runbooks.md#runbook-parameters)yoktur.

### <a name="runbook-types"></a>Runbook türleri

Hangi runbook türleri birbirini arayabilir?

* PowerShell runbook ve [grafik çalışma kitabı,](automation-runbook-types.md#graphical-runbooks) her ikisi de PowerShell tabanlı olduğundan birbirlerini satır içi olarak adlandırabilir. [PowerShell runbook](automation-runbook-types.md#powershell-runbooks)
* PowerShell [İş Akışı çalışma kitabı](automation-runbook-types.md#powershell-workflow-runbooks) ve grafiksel PowerShell İş Akışı runbook'u, her ikisi de PowerShell İş Akışı tabanlı olduğundan birbirlerini satır içi olarak adlandırabilir.
* PowerShell türleri ve PowerShell İş Akışı türleri birbirini sıralı olarak `Start-AzAutomationRunbook`adlandıramıyor ve kullanması gerekir.

Yayımlama emri ne zaman önemlidir?

Runbook'ların yayımlama sırası yalnızca PowerShell İş Akışı ve grafiksel PowerShell İş Akışı runbook'ları için önemlidir.

Runbook'unuz satır satır yürütmeyi kullanarak grafik veya PowerShell İş Akışı alt çalışmasını aradığında, runbook'un adını kullanır. Komut dosyasının `.\\` yerel dizinde bulunduğunu belirtmek için adla başlanmalıdır.

### <a name="example"></a>Örnek

Aşağıdaki örnek, karmaşık bir nesneyi, bir tamsayı değerini ve boolean değerini kabul eden bir test alt kitabı başlatır. Alt runbook'un çıkışı bir değişkene atanır. Bu durumda, alt runbook bir PowerShell İş Akışı runbook olduğunu.

```azurepowershell-interactive
$vm = Get-AzVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Burada çocuk olarak bir PowerShell runbook kullanarak aynı örnektir.

```azurepowershell-interactive
$vm = Get-AzVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-a-cmdlet"></a>Cmdlet kullanarak çocuk runbook başlatma

> [!IMPORTANT]
> Runbook parametre ile `Start-AzAutomationRunbook` cmdlet ile `Wait` bir alt runbook çağırır ve alt runbook bir nesne sonucu üretir, işlem bir hata yla karşılaşabilir. Hatayı aşmak için [Get-AzAutomationJobOutputRecord](/powershell/module/az.automation/get-azautomationjoboutputrecord) cmdlet'i kullanarak sonuçları yoklama mantığını nasıl uygulayacağınızı öğrenmek için [nesne çıktılı Alt çalışma kitaplarına](troubleshoot/runbooks.md#child-runbook-object) bakın.

[Windows PowerShell ile bir runbook başlatmak için](start-runbooks.md#start-a-runbook-with-powershell)açıklandığı gibi bir runbook başlatmak için kullanabilirsiniz. `Start-AzAutomationRunbook` Bu cmdlet için iki kullanım şekli vardır. Bir modda, iş alt çalışma defteri için oluşturulduğunda cmdlet iş kimliğini döndürür. Komut dosyanızın *Bekle* parametresini belirterek etkinleştirdiği diğer modda, cmdlet alt iş bitene kadar bekler ve alt çalışma kitabından çıktıyı döndürür.

Bir alt çalışma defterinden iş bir cmdlet ile başladı ana runbook iş ayrı çalışır. Bu davranış, runbook satırını başlatmaktan daha fazla iş ile sonuçlanır ve işlerin izlenmesini zorlaştırır. Üst öğe, her birinin tamamlanmasını beklemeden birden fazla alt çalışma kitabını eşit olarak başlatabilir. Alt runbook'ları satır satır olarak çağıran bu paralel yürütme için, üst çalışma kitabının [paralel anahtar sözcüğü](automation-powershell-workflow.md#parallel-processing)kullanması gerekir.

Alt runbook çıktısı zamanlama nedeniyle güvenilir bir şekilde üst runbook dönmez. Buna ek olarak, `$VerbosePreference`, `$WarningPreference`, ve diğerleri gibi değişkenler alt runbook'lara yayılmayabilir. Bu sorunları önlemek için, alt runbook'ları `Start-AzAutomationRunbook` `Wait` parametreile birlikte ayrı Otomasyon işleri olarak başlatabilirsiniz. Bu teknik, alt çalışma defteri tamamlanana kadar üst çalışma kitabını engeller.

Üst çalışma kitabının beklerken engellenmesini istemiyorsanız, alt çalışma kitabını `Start-AzAutomationRunbook` `Wait` parametre olmadan kullanmaya başlayabilirsiniz. Bu durumda, runbook'unuzun işin tamamlanmasını beklemek için [Get-AzAutomationJob'u](/powershell/module/az.automation/get-azautomationjob) kullanması gerekir. Ayrıca sonuçları almak için [Get-AzAutomationJobOutput](/powershell/module/az.automation/get-azautomationjoboutput) ve [Get-AzAutomationJobOutputRecord](/powershell/module/az.automation/get-azautomationjoboutputrecord) kullanmanız gerekir.

Cmdlet ile başlayan bir alt çalışma kitabının [parametreleri, Runbook parametrelerinde](start-runbooks.md#runbook-parameters)açıklandığı gibi karma olarak sağlanır. Yalnızca basit veri türleri kullanılabilir. Runbook karmaşık veri türü içeren bir parametreye sahipse satır içi olarak çağrılmalıdır.

Alt runbook'ları ayrı işler olarak başlatırken abonelik bağlamı kaybolabilir. Alt çalışma kitabının belirli bir Azure aboneliğine karşı Az modülü cmdletlerini çalıştırabilmesi için, çocuğun bu aboneliği üst çalışma kitabından bağımsız olarak doğrulaması gerekir.

Aynı Otomasyon hesabındaki işler birden fazla abonelikle çalışıyorsa, bir işte abonelik seçmek diğer işler için seçili abonelik bağlamını değiştirebilir. Bu durumu önlemek `Disable-AzContextAutosave –Scope Process` için, her runbook başında kullanın. Bu eylem yalnızca bu runbook yürütme bağlamını kaydeder.

### <a name="example"></a>Örnek

Aşağıdaki örnek, parametreleri olan bir alt çalışma defterini başlatır ve parametreli `Start-AzAutomationRunbook` `Wait` cmdlet'i kullanarak tamamlanmasını bekler. Tamamlandıktan sonra, örnek çocuk runbook cmdlet çıktıtoplar. Kullanmak `Start-AzAutomationRunbook`için komut dosyasının Azure aboneliğinizde kimlik doğrulaması yapması gerekir.

```azurepowershell-interactive
# Ensure that the runbook does not inherit an AzContext
Disable-AzContextAutosave –Scope Process

# Connect to Azure with Run As account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Connect-AzAccount `
    -ServicePrincipal `
    -Tenant $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Get-AzSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzContext $AzureContext `
    –Parameters $params –Wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Bir alt çalışma defteri ni arama yöntemlerinin karşılaştırılması

Aşağıdaki tablo, başka bir runbook'tan runbook çağırmanın iki yolu arasındaki farkları özetler.

|  | Satır içi | Cmdlet |
|:--- |:--- |:--- |
| İş |Alt runbook'lar üst runbook'la aynı işte çalışır. |Alt runbook için ayrı bir iş oluşturulur. |
| Yürütme |Üst runbook devam etmeden önce alt runbook'un tamamlanmasını bekler. |Alt çalışma defteri, alt çalışma kitabı başlatıldıktan hemen sonra devam eder *veya* üst çalışma defteri alt işin tamamlanmasını bekler. |
| Çıktı |Üst runbook doğrudan alt runbook'tan çıkış alabilir. |Üst runbook çocuk runbook iş çıktı almak gerekir *veya* üst runbook doğrudan alt runbook çıktı alabilirsiniz. |
| Parametreler |Alt runbook parametre değerleri ayrı ayrı belirtilir ve herhangi bir veri türünü kullanabilir. |Alt çalışma kitabı parametreleri için değerlerin tek bir karma tabloda birleştirilmesi gerekir. Bu karma tablo yalnızca JSON serileştirmekullanan basit, dizi ve nesne veri türlerini içerebilir. |
| Otomasyon Hesabı |Üst çalışma defteri yalnızca aynı Otomasyon hesabında alt runbook kullanabilirsiniz. |Üst runbook'lar, herhangi bir Otomasyon hesabından, aynı Azure aboneliğinden ve hatta bağlantınız olan farklı bir abonelikten bir alt runbook kullanabilir. |
| Yayımlama |Üst runbook yayımlanmadan önce alt runbook yayımlanmalıdır. |Alt çalışma kitabı, üst çalışma kitabı başlatılmadan önce herhangi bir zamanda yayımlanır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Otomasyonu'nda runbook başlatma](start-runbooks.md)
* [Azure Otomasyonu'nda runbook çıktısı ve iletileri](automation-runbook-output-and-messages.md)