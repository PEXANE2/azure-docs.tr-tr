---
title: Azure Otomasyonundaki Zamanlamalar
description: Otomasyon zamanlamaları, Azure Otomasyonu'ndaki runbook'ları otomatik olarak başlamak üzere zamanlamak için kullanılır. Belirli bir zamanda veya yinelenen bir zamanlamada bir runbook'u otomatik olarak başlatabilmeniz için zamanlamanın nasıl oluşturulup yönetilenini açıklar.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c4898ba62abdc42d95b77b9a77387bfe71fb4771
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252668"
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure Otomasyonu’nda runbook zamanlama

Azure Otomasyonu'nda bir runbook'un belirli bir zamanda başlamasını planlamak için, bir veya daha fazla zamanlamaya bağlarsınız. Zamanlama, Azure portalındaki runbook'lar için bir kez veya yinelenen saatlik veya günlük zamanlamada çalışacak şekilde yapılandırılabilir. Bunları haftalık, aylık, haftanın belirli günleri veya ayın günleri veya ayın belirli bir günü için de zamanlayabilirsiniz. Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamalar şu anda Azure Otomasyon Uplc yapılandırmalarını desteklemiyor.

## <a name="powershell-cmdlets"></a>PowerShell Cmdlet'leri

Aşağıdaki tablodaki cmdletler, Azure Otomasyonu'nda PowerShell ile zamanlama oluşturmak ve yönetmek için kullanılır. [Azure PowerShell modülünün](/powershell/azure/overview)bir parçası olarak gönderiyaparlar.

| Cmdlet’ler | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Bir zamanlama alır. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Yeni bir zamanlama oluşturur. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Bir zamanlama kaldırır. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Varolan bir zamanlama için özellikleri ayarlar. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/get-azurermautomationscheduledrunbook) |Zamanlanmış runbook'ları alır. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Bir runbook'u bir zamanlamayla ilişkilendirer. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Bir runbook'u programdan ayırıyor. |

## <a name="creating-a-schedule"></a>Zamanlama oluşturma

Azure portalında veya PowerShell ile runbook'lar için yeni bir zamanlama oluşturabilirsiniz.

> [!NOTE]
> Azure Otomasyonu, yeni bir zamanlanmış iş çalıştırıldığında Otomasyon hesabınızdaki en son modülleri kullanır.  Runbook'larınızı ve otomatikleştirdikleri işlemleri etkilemeyi önlemek için, öncelikle zamanlamaları test etmek üzere ayrılmış bir Otomasyon hesabıyla birbirine bağlamış tüm runbook'ları test etmeniz gerekir.  Bu, zamanlanmış runbook'larınızın doğru çalışmaya devam ettiğini doğrular ve değilse, güncelleştirilmiş runbook sürümünü üretime geçirmeden önce gereken değişiklikleri daha da giderebilir ve uygulayabilirsiniz.
> Otomasyon hesabınız, **Modüllerden** [Azure Modüllerini Güncelleştir](../automation-update-azure-modules.md) seçeneğini seçerek modülleri el ile güncellemediğiniz sürece otomatik olarak yeni modül sürümlerini almaz.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Azure portalında yeni bir zamanlama oluşturmak için

1. Azure portalında, otomasyon hesabınızdan, soldaki **Paylaşılan Kaynaklar** bölümünün altındaki **Zamanlamalar'ı** seçin.
2. Sayfanın üst kısmında **bir zamanlama ekle'yi** tıklatın.
3. Yeni **zamanlama** bölmesine, bir **Ad** ve isteğe bağlı olarak yeni zamanlama için bir **Açıklama** yazın.
4. Zamanlamanın bir kez mi yoksa yinelenen bir **zamanlamada mı** çalıştığını bir kez veya **Yinelenen'i**seçerek seçin. **Bir Kez** Başlangıç **saatini**belirtirseniz ve sonra **Oluştur'u**tıklatın. **Yinelenen'i**seçerseniz, **Bir Başlangıç saati** belirtin ve Her Biri Için **Yineleme**, runbook'un **saat,** **gün,** **hafta**veya **aya**göre ne sıklıkta tekrarlanmasını istediğinizin sıklığını seçin.
    1. **Haftayı**seçerseniz, seçim yapabileceğiniz haftanın günlerinin bir listesi size verilir. İstediğiniz kadar gün seçin. Zamanlamanızın ilk çalışması, başlangıç saatinden sonra seçilen ilk gün gerçekleşir. Örneğin, bir hafta sonu programı seçmek için **Cumartesi** ve **Pazar'ı**seçin.

       ![Hafta sonu yinelenen zamanlama ayarlama](../media/schedules/week-end-weekly-recurrence.png)

    2. **Ay'ı**seçerseniz, size farklı seçenekler verilir. Aylık **olaylar** seçeneği **için, Ay günlerini** veya **Hafta günlerini**seçin. **Ay günlerini**seçerseniz, istediğiniz kadar gün seçmenize olanak tanıyan bir takvim gösterilir. Geçerli ayda oluşmayan 31. Zamanlamanın son gün çalışmasını istiyorsanız, **ayın son gününde Çalıştır**altında **Evet'i** seçin. **Hafta günlerini**seçerseniz, Her seçenek **Recur** sunulur. **Birinci**, **İkinci**, **Üçüncü**, **Dördüncü**veya **Son'u**seçin. Son olarak tekrarlamak için bir gün seçin.

       ![Ayın birinci, onbeşinci ve son günü aylık program](../media/schedules/monthly-first-fifteenth-last.png)

5. Bittiğinde **Oluştur'u**tıklatın.

### <a name="to-create-a-new-schedule-with-powershell"></a>PowerShell ile yeni bir zamanlama oluşturmak için

Zamanlama oluşturmak için [Yeni AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet'ini kullanırsınız. Zamanlamanın başlangıç saatini ve çalışması gereken sıklığını belirtirsiniz. Aşağıdaki örnekler, birçok farklı zamanlama senaryosunun nasıl oluşturulurunun gösteriş olduğunu gösterir.

#### <a name="create-a-one-time-schedule"></a>Tek seferlik zamanlama oluşturma

Aşağıdaki örnek komutlar bir kez zamanlama oluşturur.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Yinelenen bir zamanlama oluşturma

Aşağıdaki örnek komutlar, bir yıl boyunca her gün saat 13:00'te çalışan yinelenen bir zamanlamanın nasıl oluşturulur unu gösterir.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Haftalık yinelenen zamanlama oluşturma

Aşağıdaki örnek komutlar, yalnızca hafta içi çalışan haftalık bir zamanlamanın nasıl oluşturulabildiğini gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Hafta sonları için haftalık yinelenen zamanlama oluşturma

Aşağıdaki örnek komutlar, yalnızca hafta sonları çalışan haftalık bir zamanlamanın nasıl oluşturulur unu gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzureRmAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-first-15th-and-last-days-of-the-month"></a>Ayın ilk, 15 ve son günleri için yinelenen bir zamanlama oluşturma

Aşağıdaki örnek komutlar, ayın 1, 15 ve son gününde çalışan yinelenen bir zamanlamanın nasıl oluşturulacagını gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzureRmAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="linking-a-schedule-to-a-runbook"></a>Zamanlamayı runbook'a bağlama

Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir. Runbook parametreleri varsa, o zaman onlar için değerler sağlayabilir. Zorunlu parametreler için değerler sağlamanız gerekir ve isteğe bağlı parametreler için değerler sağlayabilirsiniz. Bu değerler, runbook bu zamanlama tarafından başlatılan her zaman kullanılır. Aynı runbook'u başka bir zamanlamaya ekleyebilir ve farklı parametre değerleri belirtebilirsiniz.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portalı ile bir runbook'a zamanlama bağlamak için

1. Azure portalında, otomasyon hesabınızdan, soldaki **Süreç Otomasyonu** bölümünün altındaki **Runbook'ları** seçin.
2. Zamanlamak için runbook'un adına tıklayın.
3. Runbook şu anda bir zamanlamaya bağlı değilse, yeni bir zamanlama veya varolan bir zamanlamaya bağlantı oluşturma seçeneği sunulur.
4. Runbook'ta parametreler varsa, çalıştır **ayarlarını değiştir seçeneğini (Varsayılan:Azure)** seçebilirsiniz ve bilgileri girebileceğiniz **Parametreler** bölmesi sunulur.

### <a name="to-link-a-schedule-to-a-runbook-with-powershell"></a>PowerShell ile bir runbook'a zamanlama bağlamak için

Bir zamanlamaya bağlanmak için [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet'i kullanabilirsiniz. Parametreler parametresiyle runbook’un parametreleri için değer belirtebilirsiniz. Parametre değerlerini belirtme hakkında daha fazla bilgi için Azure [Otomasyonunda Runbook Başlatma'ya](../automation-starting-a-runbook.md)bakın.
Aşağıdaki örnek komutlar, parametrelere sahip bir Azure Kaynak Yöneticisi cmdlet kullanarak bir zamanlamanın runbook'a nasıl bağlanıştırılabildiğini gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Runbook'ları daha sık zamanlama

Azure Otomasyonu'ndaki bir zamanlamanın en sık yapılandırılabildiği aralık bir saattir. Bundan daha sık yürütmek için zamanlamaları gerekiyorsa, iki seçenek vardır:

* Runbook için bir [webhook](../automation-webhooks.md) oluşturun ve webhook'u aramak için [Azure Logic Apps'ı](../../logic-apps/logic-apps-overview.md) kullanın. Azure Logic Apps, zamanlama tanımlarken daha ayrıntılı parçalı ayrıntı sağlar.

* Her saat bir çalışan birbirinden 15 dakika içinde başlayan dört zamanlama oluşturun. Bu senaryo, runbook'un her 15 dakikada bir farklı zamanlamalarla çalışmasını sağlar.

## <a name="disabling-a-schedule"></a>Zamanlamayı devre dışı bırakma

Bir zamanlamayı devre dışı dışı kaldığınız zaman, ona bağlı herhangi bir runbook artık bu zamanlamada çalışmaz. Zamanlamayı el ile devre dışı bırakabilir veya bunları oluşturduğunuzda sıklığı olan zamanlamalar için bir son kullanma süresi ayarlayabilirsiniz. Sona erme süresine ulaşıldıktan sonra zamanlama devre dışı bırakılır.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure portalından bir zamanlamayı devre dışı kılabilir

1. Azure portalında, Otomasyon hesabınızdan, soldaki **Paylaşılan Kaynaklar** bölümünün altındaki **Zamanlamalar'ı** seçin.
2. Ayrıntılar bölmesini açmak için zamanlamanın adını tıklatın.
3. **Değişiklik** **Etkin değil.**

> [!NOTE]
> Geçmişte başlangıç zamanı olan bir zamanlamayı devre dışı kılmış olmak istiyorsanız, kaydetmeden önce başlangıç tarihini gelecekte bir saate değiştirmeniz gerekir.

### <a name="to-disable-a-schedule-with-powershell"></a>PowerShell ile bir programı devre dışı kalmak için

Varolan bir zamanlamanın özelliklerini değiştirmek için [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet'i kullanabilirsiniz. Zamanlamayı devre dışı ksaymak **için, IsEnabled** parametresi için **yanlış** belirtin.

Aşağıdaki örnek komutlar, Azure Kaynak Yöneticisi cmdlet kullanarak bir runbook için zamanlamanın nasıl devre dışı bırakılabildiğini gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonunda runbook'lara başlamak için [bkz.](../automation-starting-a-runbook.md)

