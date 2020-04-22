---
title: Azure Otomasyonu'nda zamanlamaları yönetme
description: Otomasyon zamanlamaları, Azure Otomasyonu'ndaki runbook'ları otomatik olarak başlamak üzere zamanlamak için kullanılır. Belirli bir zamanda veya yinelenen bir zamanlamada bir runbook'u otomatik olarak başlatabilmeniz için zamanlamanın nasıl oluşturulup yönetilenini açıklar.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3603e76186ce30fb491d829d3a804837f4ac2e6d
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732788"
---
# <a name="manage-schedules-in-azure-automation"></a>Azure Otomasyonu'nda zamanlamaları yönetme

Azure Otomasyonu'nda bir runbook'un belirli bir zamanda başlamasını planlamak için, bir veya daha fazla zamanlamaya bağlarsınız. Zamanlama, Azure portalındaki runbook'lar için bir kez veya yinelenen saatlik veya günlük zamanlamada çalışacak şekilde yapılandırılabilir. Bunları haftalık, aylık, haftanın belirli günleri veya ayın günleri veya ayın belirli bir günü için de zamanlayabilirsiniz. Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamalar şu anda Azure Otomasyon Uplc yapılandırmalarını desteklemiyor.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="powershell-cmdlets"></a>PowerShell Cmdlet'leri

Aşağıdaki tablodaki cmdletler, Azure Otomasyonu'nda PowerShell ile zamanlama oluşturmak ve yönetmek için kullanılır. [Azure PowerShell modülünün](/powershell/azure/overview)bir parçası olarak gönderiyaparlar.

| Cmdlet’ler | Açıklama |
|:--- |:--- |
| [Get-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationSchedule?view=azps-3.7.0) |Bir zamanlama alır. |
| [Al-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationscheduledrunbook?view=azps-3.7.0) |Zamanlanmış runbook'ları alır. |
| [Yeni-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) |Yeni bir zamanlama oluşturur. |
| [Kayıt-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-3.7.0) |Bir runbook'u bir zamanlamayla ilişkilendirer. |
| [Remove-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationSchedule?view=azps-3.7.0) |Bir zamanlama kaldırır. |
| [Set-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationSchedule?view=azps-3.7.0) |Varolan bir zamanlama için özellikleri ayarlar. |
| [Kayıt Dışı-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Unregister-AzAutomationScheduledRunbook?view=azps-3.7.0) |Bir runbook'u programdan ayırıyor. |

## <a name="creating-a-schedule"></a>Zamanlama oluşturma

Azure portalında veya PowerShell ile runbook'lar için yeni bir zamanlama oluşturabilirsiniz.

> [!NOTE]
> Azure Otomasyonu, yeni bir zamanlanmış iş çalıştırıldığında Otomasyon hesabınızdaki en son modülleri kullanır.  Runbook'larınızı ve otomatikleştirdikleri işlemleri etkilemeyi önlemek için, öncelikle zamanlamaları test etmek üzere ayrılmış bir Otomasyon hesabıyla birbirine bağlamış tüm runbook'ları test etmeniz gerekir.  Bu, zamanlanmış runbook'larınızın doğru çalışmaya devam ettiğini doğrular ve değilse, güncelleştirilmiş runbook sürümünü üretime geçirmeden önce gereken değişiklikleri daha da giderebilir ve uygulayabilirsiniz.
> Otomasyon hesabınız, **Modüllerden** [Azure Modüllerini Güncelleştir](../automation-update-azure-modules.md) seçeneğini seçerek modülleri el ile güncellemediğiniz sürece otomatik olarak yeni modül sürümlerini almaz.

### <a name="create-a-new-schedule-in-the-azure-portal"></a>Azure portalında yeni bir zamanlama oluşturma

1. Azure portalında, otomasyon hesabınızdan, soldaki **Paylaşılan Kaynaklar** bölümünün altındaki **Zamanlamalar'ı** seçin.
2. Sayfanın üst kısmında **bir zamanlama ekle'yi** tıklatın.
3. Yeni zamanlama bölmesine, bir ad yazın ve isteğe bağlı olarak yeni zamanlama için bir açıklama yazın.
4. Zamanlamanın bir kez mi yoksa yinelenen bir **zamanlamada mı** çalıştığını bir kez veya **Yinelenen'i**seçerek seçin. **Bir kez**seçerseniz, başlangıç saatini belirtin ve ardından **Oluştur'u**tıklatın. **Yinelenen'i**seçerseniz, bir başlangıç saati belirtin. **Her yinelemek**için, runbook'un saat, gün, hafta veya aya göre ne sıklıkta tekrarlanmasını istediğinizi seçin.
    1. **Haftayı**seçerseniz, haftanın günleri seçim yapabilirsiniz. İstediğiniz kadar gün seçin. Zamanlamanızın ilk çalışması, başlangıç saatinden sonra seçilen ilk gün gerçekleşir. Örneğin, bir hafta sonu programı seçmek için Cumartesi ve Pazar'ı seçin. 
    
       ![Hafta sonu yinelenen zamanlama ayarlama](../media/schedules/week-end-weekly-recurrence.png)

    2. **Ay'ı**seçerseniz, size farklı seçenekler verilir. Aylık **olaylar** seçeneği **için, Ay günlerini** veya **Hafta günlerini**seçin. **Ay günlerini**seçerseniz, istediğiniz kadar gün seçmenize olanak tanıyan bir takvim gösterilir. Geçerli ayda oluşmayan 31. Zamanlamanın son gün çalışmasını istiyorsanız, **ayın son gününde Çalıştır**altında **Evet'i** seçin. **Hafta günlerini**seçerseniz, Her seçenek **Recur** sunulur. **Birinci**, **İkinci**, **Üçüncü**, **Dördüncü**veya **Son'u**seçin. Son olarak tekrarlamak için bir gün seçin.

       ![Ayın birinci, onbeşinci ve son günü aylık program](../media/schedules/monthly-first-fifteenth-last.png)

5. Bittiğinde **Oluştur'u**tıklatın.

### <a name="create-a-new-schedule-with-powershell"></a>PowerShell ile yeni bir zamanlama oluşturma

Zamanlama oluşturmak için [Yeni-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) cmdlet'ini kullanın. Zamanlamanın başlangıç saatini ve çalışması gereken sıklığını belirtirsiniz. Aşağıdaki örnekler, birçok farklı zamanlama senaryosunun nasıl oluşturulurunun gösteriş olduğunu gösterir.

#### <a name="create-a-one-time-schedule"></a>Tek seferlik zamanlama oluşturma

Aşağıdaki örnek komutlar bir kez zamanlama oluşturur.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Yinelenen bir zamanlama oluşturma

Aşağıdaki örnek, bir yıl boyunca her gün saat 13:00'te çalışan yinelenen bir zamanlamanın nasıl oluşturulabildiğini gösterir.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Haftalık yinelenen zamanlama oluşturma

Aşağıdaki örnek, yalnızca hafta içi çalışan haftalık bir zamanlamanın nasıl oluşturulabildiğini gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Hafta sonları için haftalık yinelenen zamanlama oluşturma

Aşağıdaki örnek komutlar, yalnızca hafta sonları çalışan haftalık bir zamanlamanın nasıl oluşturulur unu gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-first-15th-and-last-days-of-the-month"></a>Ayın ilk, 15 ve son günleri için yinelenen bir zamanlama oluşturma

Aşağıdaki örnek, ayın 1, 15 ve son gününde çalışan yinelenen bir zamanlamanın nasıl oluşturulacagını gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="linking-a-schedule-to-a-runbook"></a>Zamanlamayı runbook'a bağlama

Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir. Runbook parametreleri varsa, o zaman onlar için değerler sağlayabilir. Zorunlu parametreler için değerler sağlamanız gerekir ve isteğe bağlı parametreler için değerler sağlayabilirsiniz. Bu değerler, runbook bu zamanlama tarafından başlatılan her zaman kullanılır. Aynı runbook'u başka bir zamanlamaya ekleyebilir ve farklı parametre değerleri belirtebilirsiniz.

### <a name="link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portalı ile bir runbook'a zamanlama bağlama

1. Azure portalında, otomasyon hesabınızdan, Süreç **Otomasyonu**altında **Runbook'ları** seçin.
2. Zamanlamak için runbook'un adına tıklayın.
3. Runbook şu anda bir zamanlamaya bağlı değilse, yeni bir zamanlama veya varolan bir zamanlamaya bağlantı oluşturma seçeneği sunulur.
4. Runbook'ta parametreler varsa, çalıştır **ayarlarını değiştir seçeneğini (Varsayılan:Azure)** seçebilirsiniz ve Parametreler bölmesi sunulur. Parametre bilgilerini buraya girebilirsiniz.

### <a name="link-a-schedule-to-a-runbook-with-powershell"></a>PowerShell ile bir runbook'a zamanlama bağlama

Bir zamanlamabağlamak için [Register-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-3.7.0) cmdlet kullanın. Parametreler parametresiyle runbook’un parametreleri için değer belirtebilirsiniz. Parametre değerlerini belirtme hakkında daha fazla bilgi için Azure [Otomasyonunda Runbook Başlatma'ya](../automation-starting-a-runbook.md)bakın.
Aşağıdaki örnekte, bir azure kaynak yöneticisi cmdlet kullanarak bir zamanlamanın bir runbook'a parametrelerle nasıl bağlanılmak üzere değiştirilen gösterilmektedir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-to-run-more-frequently"></a>Runbook'ları daha sık çalıştırmak için zamanlama

Azure Otomasyonu'ndaki bir zamanlamanın en sık yapılandırılabildiği aralık bir saattir. Bundan daha sık yürütmek için zamanlamaları gerekiyorsa, iki seçenek vardır:

* Runbook için bir [webhook](../automation-webhooks.md) oluşturun ve webhook'u aramak için [Azure Logic Apps'ı](../../logic-apps/logic-apps-overview.md) kullanın. Azure Logic Apps, zamanlama tanımlarken daha ayrıntılı parçalı ayrıntı sağlar.

* Her saat bir çalışan birbirinden 15 dakika içinde başlayan dört zamanlama oluşturun. Bu senaryo, runbook'un her 15 dakikada bir farklı zamanlamalarla çalışmasını sağlar.

## <a name="disabling-a-schedule"></a>Zamanlamayı devre dışı bırakma

Bir zamanlamayı devre dışı dışı kaldığınız zaman, ona bağlı herhangi bir runbook artık bu zamanlamada çalışmaz. Zamanlamayı el ile devre dışı bırakabilir veya bunları oluşturduğunuzda sıklığı olan zamanlamalar için bir son kullanma süresi ayarlayabilirsiniz. Sona erme süresine ulaşıldıktan sonra zamanlama devre dışı bırakılır.

### <a name="disable-a-schedule-from-the-azure-portal"></a>Azure portalından zamanlamayı devre dışı

1. Otomasyon hesabınızda, **Paylaşılan Kaynaklar**altında **Zamanlamalar'ı** seçin.
2. Ayrıntılar bölmesini açmak için zamanlamanın adını tıklatın.
3. **Değişiklik** **Etkin değil.**

> [!NOTE]
> Geçmişte başlangıç zamanı olan bir zamanlamayı devre dışı kılmış olmak istiyorsanız, kaydetmeden önce başlangıç tarihini gelecekte bir saate değiştirmeniz gerekir.

### <a name="disable-a-schedule-with-powershell"></a>PowerShell ile zamanlamayı devre dışı

Varolan bir zamanlamanın özelliklerini değiştirmek için [Set-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationSchedule?view=azps-3.7.0) cmdlet'i kullanın. Zamanlamayı devre dışı ksaymak `IsEnabled` için parametre için False'u belirtin.

Aşağıdaki örnek, Azure Kaynak Yöneticisi cmdlet kullanarak bir runbook için zamanlamanın nasıl devre dışı bırakılabildiğini gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonundaki runbook'lara başlamak için [bkz.](../automation-starting-a-runbook.md)