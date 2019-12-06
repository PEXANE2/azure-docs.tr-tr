---
title: Azure Otomasyonu 'nda alt runbook 'lar
description: Azure Otomasyonu 'nda bir runbook 'u başka bir runbook 'tan başlatmaya ve aralarında bilgi paylaşmaya yönelik farklı yöntemler açıklanmıştır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e7341a8c270d16497430a70c2a1b21354a775787
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850457"
---
# <a name="child-runbooks-in-azure-automation"></a>Azure Otomasyonu 'nda alt runbook 'lar

Azure Otomasyonu 'nda, başka runbook 'lar tarafından kullanılan ayrı bir işlevle yeniden kullanılabilir, modüler runbook 'ları yazmak için önerilen bir uygulamadır. Bir üst runbook, gerekli işlevselliği gerçekleştirmek için genellikle bir veya daha fazla alt runbook 'u çağırır. Bir alt runbook 'u bulmanın iki yolu vardır ve farklı senaryolarınız için hangisinin en iyi şekilde belirleyebilmeniz için anlamanız gereken farklı farklılıklar vardır.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Satır içi yürütme kullanarak alt runbook çağırma

Başka bir runbook'tan bir runbook'u satır içi olarak çağırmak için, bir etkinlik veya cmdlet'i kullanırken yaptığınız gibi runbook'un adını kullanır ve parametreleri için değerler belirtirsiniz.  Aynı Otomasyon hesabındaki tüm runbook 'lar bu şekilde kullanılmak üzere tüm diğerleri tarafından kullanılabilir. Üst runbook bir sonraki satıra geçmeden önce alt runbook'un tamamlanmasını bekler ve herhangi bir çıkış doğrudan üst öğeye döndürülür.

Bir runbook'u satır içi olarak çağırdığınızda üst runbook ile aynı işi çalıştırır. Çalıştığı alt runbook 'un iş geçmişinde hiçbir belirti yoktur. Alt runbook 'tan gelen tüm özel durumlar ve akış çıktıları üst öğesiyle ilişkilendirilir. Bu davranış, alt runbook tarafından oluşturulan tüm özel durumlar ve akış çıktılarından herhangi biri ana işle ilişkilendirildiğinden daha az iş ile sonuçlanır ve bunların izlenmesini ve sorun gidermeyi kolaylaştırır.

Bir runbook yayımlandığında, çağırdığı tüm alt runbook 'lar zaten yayımlanmalıdır. Bunun nedeni, Azure Otomasyonu 'nun bir runbook derlendiğinde alt runbook 'larla bir ilişki oluşturmesidir. Bu değillerse, üst runbook düzgün şekilde yayınla görünür ancak başlatıldığında bir özel durum oluşturur. Bu durumda, alt runbook 'lara doğru şekilde başvurmak için üst runbook 'u yeniden yayımlayabilirsiniz. İlişki zaten oluşturulduğundan, alt runbook 'lardan herhangi biri değiştirilirse üst runbook 'u yeniden yayımlamanız gerekmez.

Satır içi olarak adlandırılan bir alt runbook 'un parametreleri karmaşık nesneler de dahil olmak üzere herhangi bir veri türü olabilir. Runbook 'u Azure portal veya Start-AzureRmAutomationRunbook cmdlet 'i ile başlattığınızda olduğu gibi bir [JSON serileştirmesi](start-runbooks.md#runbook-parameters) yoktur.

### <a name="runbook-types"></a>Runbook türleri

Hangi türleri birbirine çağırabilirler:

* Bir [PowerShell runbook 'u](automation-runbook-types.md#powershell-runbooks) ve [grafik runbook 'ları](automation-runbook-types.md#graphical-runbooks) birbirini her satır içi çağırabilir (her ikisi de PowerShell tabanlıdır).
* PowerShell iş akışı [runbook 'u](automation-runbook-types.md#powershell-workflow-runbooks) ve grafik PowerShell iş akışı runbook 'ları her bir satır içi (her Ikisi de PowerShell iş akışı tabanlı) çağırabilir
* PowerShell türleri ve PowerShell Iş akışı türleri birbirini her satır içi çağırabilir ve start-AzureRmAutomationRunbook kullanması gerekir.

Siparişi ne zaman yayınlarsa:

* Runbook 'ların yayımlama sırası yalnızca PowerShell Iş akışı ve grafik PowerShell Iş akışı runbook 'ları için önemlidir.

Satır içi yürütme kullanarak bir grafik veya PowerShell Iş akışı alt runbook 'u çağırdığınızda, runbook 'un adını kullanırsınız.  Bir PowerShell alt runbook 'u çağırdığınızda, betiğin yerel dizinde bulunduğunu belirtmek için adını *.\\* ile başlatmanız gerekir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç parametre, karmaşık bir nesne, bir tamsayı ve Boole değeri kabul eden bir test alt runbook 'u başlatır. Alt runbook'un çıkışı bir değişkene atanır.  Bu durumda, alt Runbook bir PowerShell Iş akışı runbook 'ıdır.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Aşağıda, alt olarak bir PowerShell runbook 'u kullanan aynı örnek verilmiştir.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>Cmdlet kullanarak bir alt runbook 'u başlatma

> [!IMPORTANT]
> `-Wait` anahtarla `Start-AzureRmAutomationRunbook` cmdlet 'i olan bir alt runbook 'u çağırdıysanız ve alt runbook 'un sonuçları bir nesne ise, hatalarla karşılaşabilirsiniz. Hataya geçici bir çözüm bulmak için, sonuçları yoklamaya yönelik mantığı nasıl uygulayacağınızı öğrenmek ve [Get-Azurermautomationjoi Putrecord](/powershell/module/azurerm.automation/get-azurermautomationjoboutputrecord) öğesini kullanmak için bkz. [nesne çıkışı ile alt runbook 'lar](troubleshoot/runbooks.md#child-runbook-object) .

[Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) cmdlet 'Ini kullanarak [Windows PowerShell ile bir runbook başlatma](start-runbooks.md#start-a-runbook-with-powershell)bölümünde açıklanan şekilde bir runbook başlatabilirsiniz. Bu cmdlet için iki mod kullanılması vardır.  Tek bir modda cmdlet alt runbook için alt iş oluşturulduğunda iş kimliğini döndürür.  **-Wait** parametresini belirterek etkinleştirdiğiniz diğer modda cmdlet, alt iş bitene kadar bekler ve alt runbook 'tan çıkışı döndürür.

Bir cmdlet ile başlatılan bir alt runbook 'taki iş, üst runbook 'tan ayrı bir işte çalışır. Bu davranış, runbook 'un satır içi başlatılmasına kıyasla daha fazla iş ile sonuçlanır ve izlemeyi daha zor hale getirir. Üst öğe, her birinin tamamlanmasını beklemeden, zaman uyumsuz olarak birden fazla alt runbook başlatabilir. Alt runbook'ların satır içi olarak çağrıldığı paralel yürütme türü için, üst runbook'un [parallel anahtar kelimesini](automation-powershell-workflow.md#parallel-processing)kullanması gerekir.

Zamanlama nedeniyle alt runbook 'ların çıkışı üst runbook 'a güvenilir olarak döndürülmez. Ayrıca, $VerbosePreference, $WarningPreference ve diğerleri gibi bazı değişkenler alt runbook 'lara yayılmayabilir. Bu sorunlardan kaçınmak için, `-Wait` anahtarıyla `Start-AzureRmAutomationRunbook` cmdlet 'ini kullanarak alt runbook 'ları ayrı otomasyon işleri olarak başlatabilirsiniz. Bu, alt runbook tamamlanana kadar üst runbook 'u engeller.

Bekleme sırasında üst runbook 'un engellenmesini istemiyorsanız, `-Wait` anahtarı olmadan `Start-AzureRmAutomationRunbook` cmdlet 'ini kullanarak alt runbook 'u başlatabilirsiniz. Daha sonra iş tamamlanmasını beklemek için `Get-AzureRmAutomationJob` kullanmanız ve sonuçları almak için `Get-AzureRmAutomationJobOutput` ve `Get-AzureRmAutomationJobOutputRecord` gerekir.

Cmdlet ile başlatılan bir alt runbook için parametreler [Runbook Parametreleri](start-runbooks.md#runbook-parameters)bölümünde açıklandığı gibi karma tablosu olarak sağlanır. Yalnızca basit veri türleri kullanılabilir. Runbook karmaşık veri türü içeren bir parametreye sahipse satır içi olarak çağrılmalıdır.

Alt runbook 'lar ayrı işler olarak başlatıldığında abonelik bağlamı kaybolabilir. Alt runbook 'un Azure RM cmdlet 'lerini belirli bir Azure aboneliğine karşı yürütmesi için alt runbook 'un bu abonelikte üst runbook 'tan bağımsız olarak kimlik doğrulaması gerekir.

Aynı Otomasyon hesabı içindeki işler birden fazla abonelikle çalışıyorsa, bir iş içinde bir abonelik seçmek diğer işler için o anda seçili olan abonelik bağlamını değiştirebilir. Bu sorundan kaçınmak için her runbook 'un başlangıcında `Disable-AzureRmContextAutosave –Scope Processsave` kullanın. Bu eylem yalnızca bu runbook yürütmesinin bağlamını kaydeder.

### <a name="example"></a>Örnek

Aşağıdaki örnek, parametreleri olan bir alt runbook başlatır ve ardından Start-AzureRmAutomationRunbook-Wait parametresini kullanarak tamamlanmasını bekler. Tamamlandıktan sonra, çıkışı alt runbook 'tan toplanır. `Start-AzureRmAutomationRunbook`kullanmak için Azure aboneliğinizde kimlik doğrulaması yapmanız gerekir.

```azurepowershell-interactive
# Ensures you do not inherit an AzureRMContext in your runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRMContext $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Alt runbook çağırma yöntemlerinin karşılaştırılması

Aşağıdaki tabloda bir runbook'tan başka bir runbook çağırmanın iki yöntemi arasındaki farklar özetlenmiştir.

|  | Satır içi | Cmdlet |
|:--- |:--- |:--- |
| İş |Alt runbook'lar üst runbook'la aynı işte çalışır. |Alt runbook için ayrı bir iş oluşturulur. |
| Yürütme |Üst runbook devam etmeden önce alt runbook'un tamamlanmasını bekler. |Üst runbook, alt runbook başlatıldıktan hemen sonra devam eder *veya* üst Runbook alt işin bitmesini bekler. |
| Çıktı |Üst runbook doğrudan alt runbook'tan çıkış alabilir. |Üst Runbook alt runbook işinden çıkış almalıdır *veya* üst runbook, alt runbook 'tan doğrudan çıkış alabilir. |
| Parametreler |Alt runbook parametre değerleri ayrı ayrı belirtilir ve herhangi bir veri türünü kullanabilir. |Alt runbook parametrelerinin değerlerinin tek bir Hashtable 'da birleştirilmesi gerekir. Bu Hashtable yalnızca JSON serileştirmesi kullanan basit, dizi ve nesne veri türlerini içerebilir. |
| Otomasyon Hesabı |Üst runbook aynı Otomasyon hesabında yalnızca alt runbook 'u kullanabilir. |Üst runbook 'lar aynı Azure aboneliğindeki herhangi bir Otomasyon hesabından bir alt runbook kullanabilir ve hatta bağlantısı olan farklı bir aboneliğe sahip olabilirsiniz. |
| Yayımlama |Üst runbook yayımlanmadan önce alt runbook yayımlanmalıdır. |Üst runbook başlatılmadan önce alt runbook yayımlanmalıdır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Otomasyonu 'nda runbook başlatma](start-runbooks.md)
* [Azure Otomasyonu 'nda Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md)

