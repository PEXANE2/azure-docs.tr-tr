---
title: Azure Otomasyonu 'nda zamanlamalar
description: Otomasyon zamanlamaları, Azure Otomasyonu 'nda runbook 'ların otomatik olarak başlamasını zamanlamak için kullanılır. Bir runbook 'u belirli bir zamanda veya yinelenen bir zamanlamaya göre otomatik olarak başlatabilmeniz için ' de bir zamanlamanın nasıl oluşturulduğunu ve yönetileceğini açıklar.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c4898ba62abdc42d95b77b9a77387bfe71fb4771
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78386476"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure Otomasyonu’nda runbook zamanlama

Azure Otomasyonu 'nda bir runbook 'u belirli bir zamanda başlayacak şekilde zamanlamak için, onu bir veya daha fazla zamanlamaya bağlayın. Bir zamanlama, bir kez çalışacak veya Azure portal runbook 'lar için günlük bir zamanlamaya göre ya da bir kez çalıştırılacak şekilde yapılandırılabilir. Ayrıca, bunları haftalık, aylık, haftanın belirli günlerinde veya ayın belirli bir gününde zamanlayabilirsiniz. Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamalar Şu anda Azure Automation DSC yapılandırmasını desteklemez.

## <a name="powershell-cmdlets"></a>PowerShell Cmdlet'leri

Aşağıdaki tablodaki cmdlet 'ler, Azure Otomasyonu 'nda PowerShell ile zamanlamalar oluşturmak ve yönetmek için kullanılır. [Azure PowerShell modülün](/powershell/azure/overview)bir parçası olarak sevk ederler.

| Cmdlet’ler | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Bir zamanlama alır. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Yeni bir zamanlama oluşturur. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Bir zamanlamayı kaldırır. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Varolan bir zamanlamanın özelliklerini ayarlar. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Zamanlanan runbook 'ları alır. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Bir runbook 'u bir zamanlamaya göre ilişkilendirir. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Bir runbook 'un bir zamanlamaya göre ilişkilendirmesini kaldırın. |

## <a name="creating-a-schedule"></a>Bir zamanlama oluşturma

Azure portal veya PowerShell ile runbook 'lar için yeni bir zamanlama oluşturabilirsiniz.

> [!NOTE]
> Azure Otomasyonu, yeni bir zamanlanan iş çalıştırıldığında Otomasyon hesabınızdaki en son modülleri kullanır.  Runbook 'larınızı ve otomatikleştiredikleri işlemleri etkilememek için, öncelikle test için ayrılmış bir Otomasyon hesabı ile bağlantılı zamanlamalara sahip runbook 'ları test etmelisiniz.  Bu, zamanlanan runbook 'larınızın doğru şekilde çalışmaya devam etmesini doğrular ve yoksa, güncelleştirilmiş runbook sürümünü üretime geçirmeden önce gereken değişiklikleri daha fazla sorun giderme ve uygulama işlemleri gerçekleştirebilirsiniz.
> **Modüllerde** [Azure modüllerini Güncelleştir](../automation-update-azure-modules.md) seçeneğini belirleyerek Otomasyon hesabınız, el ile güncelleştirmediğiniz takdirde otomatik olarak modüllerin yeni sürümlerini almaz.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Azure portal yeni bir zamanlama oluşturmak için

1. Azure portal Otomasyon hesabınızdan, sol taraftaki bölüm **paylaşılan kaynakları** altında **zamanlamalar** ' ı seçin.
2. Sayfanın üst kısmındaki **zamanlama Ekle** ' ye tıklayın.
3. **Yeni zamanlama** bölmesinde, yeni zamanlama Için bir **ad** ve Isteğe bağlı olarak bir **Açıklama** yazın.
4. Zamanlamanın bir kez mı yoksa bir **kez** veya **yinelenen**seçerek yeniden gerçekleşen bir zamanlamaya göre mi çalışacağını seçin. Bir **kez** **Başlangıç zamanı**belirttikten sonra **Oluştur**' a tıklayın. **Yineleme**' yi seçip bir **Başlangıç saati** belirtin ve **yinelenme**sıklığı ' nı seçerseniz, runbook 'un ne sıklıkta **saat**, **gün**, **hafta**veya **aya**göre yinelenmesini istediğinizi seçin.
    1. **Hafta**' yı seçerseniz, tercih ettiğiniz haftanın günlerinin bir listesi sunulur. İstediğiniz kadar gün seçin. Zamanlamalarınızın ilk çalışması başlangıç zamanından sonra seçilen ilk günde olur. Örneğin, bir hafta sonu zamanlaması seçmek için **Cumartesi** ve **Pazar**' ı seçin.

       ![Hafta sonu yinelenen zamanlama ayarlanıyor](../media/schedules/week-end-weekly-recurrence.png)

    2. **Ay**' yı seçerseniz, farklı seçenekler sunulur. **Aylık yineleme** seçeneği Için, **ay günler** veya **hafta günü**seçeneklerinden birini belirleyin. **Ay günler**' i seçerseniz, istediğiniz kadar gün tercih etmenizi sağlayan bir takvim gösterilir. Geçerli ay içinde gerçekleşmeyen 31 gibi bir tarih seçerseniz, zamanlama çalıştırılmaz. Zamanlamanın son gün üzerinde çalışmasını istiyorsanız, **ayın son gününde Çalıştır**' ın altında **Evet** ' i seçin. **Hafta günleri**' ni seçerseniz, **yineleme her** seçeneği sunulur. **Birinci**, **ikinci**, **üçüncü**, **dördüncü**veya **son**' u seçin. Son olarak, tekrarlanacak bir gün seçin.

       ![Ayın ilk, on beşinci ve son gününde aylık zamanlama](../media/schedules/monthly-first-fifteenth-last.png)

5. İşiniz bittiğinde **Oluştur**' a tıklayın.

### <a name="to-create-a-new-schedule-with-powershell"></a>PowerShell ile yeni bir zamanlama oluşturmak için

Zamanlamalar oluşturmak için [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet 'ini kullanırsınız. Zamanlamanın başlangıç saatini ve çalışması gereken sıklığı belirtirsiniz. Aşağıdaki örneklerde birçok farklı zamanlama senaryosunun nasıl oluşturulacağı gösterilmektedir.

#### <a name="create-a-one-time-schedule"></a>Tek seferlik zamanlama oluşturma

Aşağıdaki örnek komutlar bir kerelik zamanlama oluşturur.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Yinelenen zamanlama oluşturma

Aşağıdaki örnek komutlarda bir yılda 1:00PM olmak üzere her gün çalışan yinelenen bir zamanlamanın nasıl oluşturulacağı gösterilmektedir.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Haftalık Yinelenen zamanlama oluşturma

Aşağıdaki örnek komutlarda, yalnızca hafta içi günleri çalıştıran haftalık bir zamanlamanın nasıl oluşturulacağı gösterilmektedir.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Hafta sonları için haftalık yinelenen zamanlama oluşturma

Aşağıdaki örnek komutlarda, yalnızca hafta sonları üzerinde çalışan haftalık bir zamanlamanın nasıl oluşturulacağı gösterilmektedir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-first-15th-and-last-days-of-the-month"></a>Ayın ilk, 15. ve son günleri için yinelenen bir zamanlama oluşturun

Aşağıdaki örnek komutlarda, ayın 1., 15. ve son gününde çalışan bir yinelenen zamanlamanın nasıl oluşturulacağı gösterilmektedir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzureRmAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="linking-a-schedule-to-a-runbook"></a>Bir runbook 'a zamanlama bağlama

Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir. Bir runbook 'un parametreleri varsa, bunlar için değer sağlayabilirsiniz. Zorunlu parametrelerin değerlerini sağlamanız ve isteğe bağlı parametrelerin değerlerini sağlayabilmeniz gerekir. Bu değerler, runbook 'un bu zamanlamaya göre başlatıldığı her seferinde kullanılır. Aynı runbook 'u başka bir zamanlamaya ekleyebilir ve farklı parametre değerleri belirtebilirsiniz.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portal ile bir zamanlamayı runbook 'a bağlamak için

1. Otomasyon hesabınızdan Azure portal, soldaki **Işlem Otomasyon** bölümünde bulunan **runbook 'lar** ' ı seçin.
2. Zamanlamak için Runbook 'un adına tıklayın.
3. Runbook Şu anda bir zamanlamaya bağlı değilse, yeni bir zamanlama oluşturma veya var olan bir zamanlamaya bağlanma seçeneği sunulur.
4. Runbook 'un parametreleri varsa, **çalışma ayarlarını değiştir (varsayılan: Azure)** seçeneğini belirleyin ve **Parametreler** bölmesi, bilgileri girebileceğiniz şekilde sunulur.

### <a name="to-link-a-schedule-to-a-runbook-with-powershell"></a>PowerShell ile bir runbook 'a zamanlama bağlamak için

Bir zamanlamayı bağlamak için [register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet 'ini kullanabilirsiniz. Parametreler parametresiyle runbook’un parametreleri için değer belirtebilirsiniz. Parametre değerlerini belirtme hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda runbook başlatma](../automation-starting-a-runbook.md).
Aşağıdaki örnek komutlarda, parametreleri olan Azure Resource Manager cmdlet 'ini kullanarak bir zamanlamanın bir runbook 'a nasıl bağlanacağını gösterilmektedir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Runbook 'ları daha sık zamanlama

Azure Automation 'da bir zamanlamanın en sık kullanılan aralığı bir saattir. Zamanlamaların daha sık yürütülmesi için zamanlamalara ihtiyacınız varsa iki seçenek vardır:

* Runbook için bir [Web kancası](../automation-webhooks.md) oluşturun ve Web kancasını çağırmak için [Azure Logic Apps](../../logic-apps/logic-apps-overview.md) kullanın. Azure Logic Apps, bir zamanlama tanımlanırken daha ayrıntılı ayrıntı düzeyi sağlar.

* Her saat bir kez çalışan 15 dakikadan kısa bir süre içinde başlayan dört zamanlama oluşturun. Bu senaryo, runbook 'un farklı zamanlamalarla 15 dakikada bir çalışmasına izin verir.

## <a name="disabling-a-schedule"></a>Zamanlamayı devre dışı bırakma

Bir zamanlamayı devre dışı bıraktığınızda, onunla bağlantılı tüm runbook bu zamanlamaya göre çalışmaz. Bir zamanlamayı el ile devre dışı bırakabilir veya bir zaman aşımı süresi oluştururken bir sıklık süresi ayarlayabilirsiniz. Süre sonu zamanına ulaşıldığında, zamanlama devre dışı bırakılır.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure portal bir zamanlamayı devre dışı bırakmak için

1. Azure portal Otomasyon hesabınızdan, sol taraftaki bölüm **paylaşılan kaynakları** altında **zamanlamalar** ' ı seçin.
2. Ayrıntılar bölmesini açmak için bir zamanlamanın adına tıklayın.
3. **Etkin özelliği** **Hayır**olarak değiştirin.

> [!NOTE]
> Geçmişte başlangıç saati olan bir zamanlamayı devre dışı bırakmak istiyorsanız, başlangıç tarihini kaydetmeden önce gelecekte bir zaman olarak değiştirmeniz gerekir.

### <a name="to-disable-a-schedule-with-powershell"></a>PowerShell ile bir zamanlamayı devre dışı bırakmak için

Var olan bir zamanlamanın özelliklerini değiştirmek için [set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet 'ini kullanabilirsiniz. Zamanlamayı devre dışı bırakmak için, **IsEnabled** parametresi için **false** belirtin.

Aşağıdaki örnek komutlarda bir Azure Resource Manager cmdlet 'i kullanarak bir runbook için zamanlamayı devre dışı bırakma gösterilmektedir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu 'nda runbook 'ları kullanmaya başlamak için bkz. [Azure Otomasyonu 'Nda runbook başlatma](../automation-starting-a-runbook.md)

