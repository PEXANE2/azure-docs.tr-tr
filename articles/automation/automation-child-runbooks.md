---
title: Azure Otomasyonu 'nda alt runbook 'lar
description: Azure Otomasyonu 'nda bir runbook 'u başka bir runbook 'tan başlatmaya ve aralarında bilgi paylaşmaya yönelik farklı yöntemler açıklanmıştır.
services: automation
ms.subservice: process-automation
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 6acf66e01c4f7b4bd2735687f542a0dbf472cfb4
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77500203"
---
# <a name="child-runbooks-in-azure-automation"></a>Azure Otomasyonu 'nda alt runbook 'lar

Başka runbook 'lar tarafından çağrılan ayrı bir işlevle yeniden kullanılabilir, modüler runbook 'lar yazmak için Azure Otomasyonu 'nda önerilen bir uygulamadır. Bir üst runbook, gerekli işlevselliği gerçekleştirmek için genellikle bir veya daha fazla alt runbook 'u çağırır. Alt runbook 'u bulmanın iki yolu vardır ve bu sayede, senaryolarınız için en uygun olanı belirleyebilmeniz için anlamanız gereken ayrı farklılıklar vardır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Satır içi yürütme kullanarak alt runbook çağırma

Başka bir runbook'tan bir runbook'u satır içi olarak çağırmak için, bir etkinlik veya cmdlet'i kullanırken yaptığınız gibi runbook'un adını kullanır ve parametreleri için değerler belirtirsiniz.  Aynı Otomasyon hesabındaki tüm runbook 'lar bu şekilde kullanılmak üzere tüm diğerleri tarafından kullanılabilir. Üst Runbook bir sonraki satıra geçmeden önce alt runbook 'un tamamlanmasını bekler ve herhangi bir çıkış doğrudan üst öğeye döndürülür.

Bir runbook'u satır içi olarak çağırdığınızda üst runbook ile aynı işi çalıştırır. Alt runbook 'un iş geçmişinde hiçbir belirti yoktur. Alt runbook 'tan gelen tüm özel durumlar ve akış çıkışları üst öğeyle ilişkilendirilir. Bu davranış, daha az iş ile sonuçlanır ve izleme ve sorun giderme işlemlerini kolaylaştırır.

Bir runbook yayımlandığında, çağırdığı tüm alt runbook 'lar zaten yayımlanmalıdır. Bunun nedeni, Azure Otomasyonu 'nun runbook 'u derlediğinde herhangi bir alt runbook ile ilişkilendirme oluşturmasının nedenidir. Alt runbook 'lar zaten yayımlanmamışsa, üst runbook düzgün şekilde yayınla görünür ancak başlatıldığında bir özel durum oluşturur. Bu durumda, alt runbook 'lara doğru şekilde başvurmak için üst runbook 'u yeniden yayımlayabilirsiniz. İlişki zaten oluşturulduğundan, herhangi bir alt runbook değiştirilirse üst runbook 'u yeniden yayımlamanız gerekmez.

Satır içi olarak adlandırılan bir alt runbook 'un parametreleri karmaşık nesneler de dahil olmak üzere herhangi bir veri türünde olabilir. Azure portal veya [Start-AzAutomationRunbook](/powershell/module/Az.Automation/Start-AzAutomationRunbook) cmdlet 'ini kullanarak runbook 'u başlattığınızda olduğu gibi, [JSON serileştirme](start-runbooks.md#runbook-parameters)yoktur.

### <a name="runbook-types"></a>Runbook türleri

Hangi runbook türleri birbirini çağırabilirler?

* Bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) 'u ve bir [grafik runbook](automation-runbook-types.md#graphical-runbooks) , her ikisi de PowerShell tabanlı olduğundan, her bir satır içi her satırı çağırabilir.
* PowerShell iş akışı [runbook](automation-runbook-types.md#powershell-workflow-runbooks) 'u ve bir grafik PowerShell iş akışı runbook 'u, her Ikisi de PowerShell iş akışı tabanlı olan bir satır içi olarak çağırabilir.
* PowerShell türleri ve PowerShell Iş akışı türleri birbirini her satır içi çağırabilir ve **Start-AzAutomationRunbook**kullanması gerekir.

Siparişi ne zaman yayımlasın?

Runbook 'ların yayımlama sırası yalnızca PowerShell Iş akışı ve grafik PowerShell Iş akışı runbook 'ları için önemlidir.

Runbook 'unuza satır içi yürütme kullanarak bir grafik veya PowerShell Iş akışı alt runbook 'u çağırdığında, runbook 'un adını kullanır. Ad "ile başlamalıdır. komut dosyasının yerel dizinde bulunduğunu belirtmek için, "\\.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir karmaşık nesne, bir tamsayı değeri ve bir Boolean değer kabul eden bir test alt runbook 'u başlatır. Alt runbook'un çıkışı bir değişkene atanır. Bu durumda, alt Runbook bir PowerShell Iş akışı runbook 'ıdır.

```azurepowershell-interactive
$vm = Get-AzVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Aşağıda, alt olarak bir PowerShell runbook 'u kullanan aynı örnek verilmiştir.

```azurepowershell-interactive
$vm = Get-AzVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-a-cmdlet"></a>Cmdlet kullanarak bir alt runbook 'u başlatma

> [!IMPORTANT]
> Runbook 'iniz *wait* parametresi ile **Start-azautomationrunbook** cmdlet 'i ile bir alt runbook 'u çağırır ve alt Runbook bir nesne sonucu üretirse, işlem bir hatayla karşılaşabilir. Hataya geçici bir çözüm bulmak için [Get-Azautomationjoi Putrecord](/powershell/module/az.automation/get-azautomationjoboutputrecord) cmdlet 'ini kullanarak sonuçları yoklamaya yönelik mantığı nasıl uygulayacağınızı öğrenmek için [nesne çıkışı ile alt runbook 'lara](troubleshoot/runbooks.md#child-runbook-object) bakın.

[Windows PowerShell ile bir runbook başlatmak için](start-runbooks.md#start-a-runbook-with-powershell)bölümünde açıklandığı gibi bir runbook 'u başlatmak için **Start-azautomationrunbook** 'u kullanabilirsiniz. Bu cmdlet için iki mod kullanılması vardır. Tek bir modda cmdlet alt runbook için alt iş oluşturulduğunda iş KIMLIĞINI döndürür. Betiğinizin bekleme parametresini belirterek izin *vermesini* sağlayan diğer modda, cmdlet alt iş bitene kadar bekler ve alt runbook 'tan çıkışı döndürür.

Bir cmdlet ile başlatılan bir alt runbook 'taki iş, üst runbook işinden ayrı bir işte çalışır. Bu davranış, runbook 'un satır içi başlatılmasına kıyasla daha fazla iş sağlar ve işlerin izlenmesini zorlaştırır. Üst öğe, her birinin tamamlanmasını beklemeden, zaman uyumsuz olarak birden fazla alt runbook başlatabilir. Alt runbook 'ların satır içi olarak çağrılması bu paralel yürütme için, üst runbook 'un [Parallel anahtar sözcüğünü](automation-powershell-workflow.md#parallel-processing)kullanması gerekir.

Zamanlama nedeniyle alt Runbook çıkışı üst runbook 'a güvenilir bir şekilde döndürülmüyor. Ayrıca, $VerbosePreference, $WarningPreference ve diğerleri gibi değişkenler alt runbook 'lara yayılmayabilir. Bu sorunlardan kaçınmak için, *bekleme* parametresiyle birlikte **Start-azautomationrunbook** kullanarak alt runbook 'ları ayrı otomasyon işleri olarak başlatabilirsiniz. Bu teknik, alt runbook tamamlanana kadar üst runbook 'u engeller.

Bekleme sırasında üst runbook 'un engellenmesini istemiyorsanız, *bekle* parametresi olmadan **Start-azautomationrunbook** kullanarak alt runbook 'u başlatabilirsiniz. Bu durumda, runbook 'unun iş tamamlanmasını beklemek için [Get-AzAutomationJob](/powershell/module/az.automation/get-azautomationjob) kullanması gerekir. Sonuçları almak için [Get-Azautomationjoi put](/powershell/module/az.automation/get-azautomationjoboutput) ve [Get-Azautomationjoi putrecord](/powershell/module/az.automation/get-azautomationjoboutputrecord) ' i de kullanmalıdır.

Bir cmdlet ile başlatılan bir alt runbook için parametreler [runbook parametreleri](start-runbooks.md#runbook-parameters)bölümünde açıklandığı gibi karma tablosu olarak sağlanır. Yalnızca basit veri türleri kullanılabilir. Runbook karmaşık veri türü içeren bir parametreye sahipse satır içi olarak çağrılmalıdır.

Alt runbook 'lar ayrı işler olarak başlatıldığında abonelik bağlamı kaybolabilir. Alt runbook 'un belirli bir Azure aboneliğine karşı az Module cmdlet 'leri yürütmesi için alt runbook 'un bu abonelikte üst runbook 'tan bağımsız olarak kimlik doğrulaması gerekir.

Aynı Otomasyon hesabı içindeki işler birden fazla abonelikle çalışıyorsa, bir iş içinde bir abonelik seçmek diğer işler için o anda seçili olan abonelik bağlamını değiştirebilir. Bu durumdan kaçınmak için her runbook 'un başlangıcında `Disable-AzContextAutosave –Scope Process` kullanın. Bu eylem yalnızca bu runbook yürütmesinin bağlamını kaydeder.

### <a name="example"></a>Örnek

Aşağıdaki örnek, parametreleri olan bir alt runbook başlatır ve sonra *bekleme* parametresiyle birlikte **Start-azautomationrunbook** cmdlet 'ini kullanarak tamamlanmasını bekler. Tamamlandıktan sonra örnek, alt runbook 'tan cmdlet çıkışını toplar. **Start-AzAutomationRunbook**'u kullanmak Için betiğin Azure aboneliğinizde kimlik doğrulaması yapması gerekir.

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

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Alt runbook çağırma yöntemlerinin karşılaştırılması

Aşağıdaki tabloda, başka bir runbook 'tan bir runbook 'u çağırmak için iki yol arasındaki farklar özetlenmektedir.

|  | Satır içi | Cmdlet |
|:--- |:--- |:--- |
| İş |Alt runbook'lar üst runbook'la aynı işte çalışır. |Alt runbook için ayrı bir iş oluşturulur. |
| Yürütme |Üst runbook devam etmeden önce alt runbook'un tamamlanmasını bekler. |Üst runbook, alt runbook başlatıldıktan hemen sonra devam eder *veya* üst Runbook alt işin bitmesini bekler. |
| Çıktı |Üst runbook doğrudan alt runbook'tan çıkış alabilir. |Üst Runbook alt runbook işinden çıkış almalıdır *veya* üst runbook, alt runbook 'tan doğrudan çıkış alabilir. |
| Parametreler |Alt runbook parametre değerleri ayrı ayrı belirtilir ve herhangi bir veri türünü kullanabilir. |Alt runbook parametrelerinin değerlerinin tek bir Hashtable 'da birleştirilmesi gerekir. Bu Hashtable yalnızca JSON serileştirmesi kullanan basit, dizi ve nesne veri türlerini içerebilir. |
| Otomasyon Hesabı |Üst runbook aynı Otomasyon hesabında yalnızca alt runbook 'u kullanabilir. |Üst runbook 'lar herhangi bir Otomasyon hesabından, aynı Azure aboneliğinden ve hatta bir bağlantınızın olduğu farklı bir abonelikte bulunan bir alt runbook 'u kullanabilir. |
| Yayımlama |Üst runbook yayımlanmadan önce alt runbook yayımlanmalıdır. |Alt runbook, üst runbook başlatılmadan önce yayımlanır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Otomasyonu 'nda runbook başlatma](start-runbooks.md)
* [Azure Otomasyonu 'nda Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md)