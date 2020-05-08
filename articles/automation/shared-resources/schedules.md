---
title: Azure Otomasyonu 'nda zamanlamaları yönetme
description: Bir runbook 'u belirli bir zamanda veya yinelenen bir zamanlamaya göre otomatik olarak başlatabilmeniz için Azure Otomasyonu 'nda bir zamanlama oluşturmayı ve yönetmeyi öğrenin.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4cd6d4236b95a17f404df13e8b50daf989cf6072
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82652102"
---
# <a name="manage-schedules-in-azure-automation"></a>Azure Otomasyonu 'nda zamanlamaları yönetme

Azure Otomasyonu 'nda bir runbook 'u belirli bir zamanda başlayacak şekilde zamanlamak için, onu bir veya daha fazla zamanlamaya bağlayın. Bir zamanlama, Azure portal runbook 'lar için bir kez veya yinelenen bir saatlik ya da günlük zamanlamaya göre çalışacak şekilde yapılandırılabilir. Ayrıca, bunları haftalık, aylık, haftanın belirli günlerinde veya ayın belirli bir gününde zamanlayabilirsiniz. Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir.

> [!NOTE]
> Zamanlamalar Şu anda Azure Automation DSC yapılandırmasını desteklememektedir.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="powershell-cmdlets-used-to-access-schedules"></a>Zamanlamalara erişmek için kullanılan PowerShell cmdlet 'leri

Aşağıdaki tablodaki cmdlet 'ler, PowerShell ile otomasyon zamanlamaları oluşturur ve yönetir. Bunlar [az modüllerin](modules.md#az-modules)bir parçası olarak sevk ederler. 

| Cmdlet’ler | Açıklama |
|:--- |:--- |
| [Get-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationSchedule?view=azps-3.7.0) |Bir zamanlama alır. |
| [Get-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationscheduledrunbook?view=azps-3.7.0) |Zamanlanan runbook 'ları alır. |
| [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) |Yeni bir zamanlama oluşturur. |
| [Register-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-3.7.0) |Bir runbook 'u bir zamanlamaya göre ilişkilendirir. |
| [Remove-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationSchedule?view=azps-3.7.0) |Bir zamanlamayı kaldırır. |
| [Set-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationSchedule?view=azps-3.7.0) |Varolan bir zamanlamanın özelliklerini ayarlar. |
| [Unregister-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Unregister-AzAutomationScheduledRunbook?view=azps-3.7.0) |Bir runbook 'un bir zamanlamaya göre ilişkilendirmesini kaldırın. |

## <a name="create-a-schedule"></a>Zamanlama Oluştur

Runbook 'larınız için Azure portal veya PowerShell ile yeni bir zamanlama oluşturabilirsiniz. Runbook 'larınızı ve otomatikleştiredikleri işlemleri etkilememek için, öncelikle test için ayrılmış bir Otomasyon hesabı ile bağlantılı zamanlamaları olan runbook 'ları test etmelisiniz. Test, zamanlanan runbook 'larınızın doğru şekilde çalışmaya devam etmesini doğrular. Bir sorun görürseniz, güncelleştirilmiş runbook sürümünü üretime geçirmeden önce gereken değişiklikleri sorun giderme ve uygulama işlemleri yapabilirsiniz.

> [!NOTE]
> [Azure modüllerini Güncelleştir](../automation-update-azure-modules.md) seçeneğini **modüller**' i seçerek Otomasyon hesabınız, el ile güncelleştirmediğiniz takdirde otomatik olarak modüllerin yeni sürümlerini almaz. Azure Otomasyonu, yeni bir zamanlanan iş çalıştırıldığında Otomasyon hesabınızdaki en son modülleri kullanır. 

### <a name="create-a-new-schedule-in-the-azure-portal"></a>Azure portal yeni bir zamanlama oluşturun

1. Azure portal Otomasyon hesabınızdan, sol taraftaki bölüm **paylaşılan kaynakları** altında **zamanlamalar** ' ı seçin.
1. Sayfanın üst kısmında **bir zamanlama Ekle ' yi** seçin.
1. **Yeni zamanlama** bölmesinde, bir ad girin ve isteğe bağlı olarak yeni zamanlama için bir açıklama girin.
1. Zamanlamanın bir kez veya tekrardan **birini seçerek bir** kez mi yoksa yeniden oluşturma zamanlamasında mi çalışacağını **seçin.** Bir **kez**seçerseniz, bir başlangıç saati belirtip **Oluştur**' u seçin. **Yinelenen**seçeneğini belirlerseniz, bir başlangıç saati belirtin. **Her**zaman yineleme için, runbook 'un ne sıklıkta yinelenmesini istediğinizi seçin. Saatlik, gün, hafta veya aya göre seçin.
    1. **Hafta**' yı seçerseniz, haftanın günleri arasından seçim yapmanız için sunulur. İstediğiniz kadar gün seçin. Zamanlamalarınızın ilk çalışması başlangıç zamanından sonra seçilen ilk günde olur. Örneğin, bir hafta sonu zamanlaması seçmek için Cumartesi ve Pazar ' ı seçin.
    
       ![Hafta sonu yinelenen zamanlama ayarlanıyor](../media/schedules/week-end-weekly-recurrence.png)

    2. **Ay**' yı seçerseniz, farklı seçenekler sunulur. **Aylık yineleme** seçeneği Için, **ay günler** veya **hafta günü**seçeneklerinden birini belirleyin. **Ay günler**' i seçerseniz, istediğiniz kadar gün seçim yapabilmeniz için bir takvim belirir. Geçerli ay içinde gerçekleşmeyen 31 gibi bir tarih seçerseniz, zamanlama çalıştırılmaz. Zamanlamanın son gün üzerinde çalışmasını istiyorsanız, **ayın son gününde Çalıştır**' ın altında **Evet** ' i seçin. **Hafta günleri**' ni seçerseniz, **yineleme her** seçeneği görüntülenir. **Birinci**, **ikinci**, **üçüncü**, **dördüncü**veya **son**' u seçin. Son olarak, tekrarlanacak bir gün seçin.

       ![Ayın ilk, on beşinci ve son gününde aylık zamanlama](../media/schedules/monthly-first-fifteenth-last.png)

1. İşiniz bittiğinde **Oluştur**' u seçin.

### <a name="create-a-new-schedule-with-powershell"></a>PowerShell ile yeni bir zamanlama oluşturma

Zamanlamalar oluşturmak için [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) cmdlet 'ini kullanın. Zamanlamanın başlangıç saatini ve çalışması gereken sıklığı belirtirsiniz. Aşağıdaki örneklerde birçok farklı zamanlama senaryosunun nasıl oluşturulacağı gösterilmektedir.

#### <a name="create-a-one-time-schedule"></a>Tek seferlik zamanlama oluşturma

Aşağıdaki örnek bir kerelik zamanlama oluşturur.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Yinelenen zamanlama oluşturma

Aşağıdaki örnek, bir yılda her gün 1:00 PM 'de çalışan yinelenen bir zamanlamanın nasıl oluşturulacağını göstermektedir.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Haftalık Yinelenen zamanlama oluşturma

Aşağıdaki örnek, yalnızca hafta içi günleri çalıştıran haftalık bir zamanlamanın nasıl oluşturulacağını göstermektedir.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Hafta sonları için haftalık yinelenen zamanlama oluşturma

Aşağıdaki örnek, yalnızca hafta sonları üzerinde çalışan haftalık bir zamanlamanın nasıl oluşturulacağını gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-the-first-fifteenth-and-last-days-of-the-month"></a>Ayın ilk, on beşinci ve son günleri için yinelenen bir zamanlama oluşturun

Aşağıdaki örnek, bir ayın ilk, on beşinci ve son gününde çalışan bir yinelenen zamanlamanın nasıl oluşturulacağını gösterir.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="link-a-schedule-to-a-runbook"></a>Bir runbook 'a zamanlama bağlama

Bir runbook, birden çok zaman çizelgesine bağlanabilir ve bir zaman çizelgesine birden çok runbook bağlı olabilir. Bir runbook 'un parametreleri varsa, bunlar için değer sağlayabilirsiniz. Zorunlu parametrelerin değerlerini sağlamanız gerekir ve isteğe bağlı parametrelerin değerlerini de sağlayabilirsiniz. Bu değerler, runbook 'un bu zamanlamaya göre başlatıldığı her seferinde kullanılır. Aynı runbook 'u başka bir zamanlamaya ekleyebilir ve farklı parametre değerleri belirtebilirsiniz.

### <a name="link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure portal ile bir zamanlamayı runbook 'a bağlama

1. Otomasyon hesabınızdan Azure portal, **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.
1. Zamanlanacak runbook 'un adını seçin.
1. Runbook Şu anda bir zamanlamaya bağlı değilse, yeni bir zamanlama oluşturma veya var olan bir zamanlamaya bağlanma seçeneği sunulur.
1. Runbook 'un parametreleri varsa, **çalışma ayarlarını değiştir (varsayılan: Azure)** ve **Parametreler** bölmesi görüntülenir seçeneğini belirleyebilirsiniz. Burada parametre bilgilerini girebilirsiniz.

### <a name="link-a-schedule-to-a-runbook-with-powershell"></a>PowerShell ile bir runbook 'a zamanlama bağlama

Bir zamanlamayı bağlamak için [register-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-3.7.0) cmdlet 'ini kullanın. Parametreler parametresiyle runbook’un parametreleri için değer belirtebilirsiniz. Parametre değerlerini belirtme hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda runbook başlatma](../automation-starting-a-runbook.md).
Aşağıdaki örnek, parametreleri olan Azure Resource Manager cmdlet 'ini kullanarak bir zamanlamayı runbook 'a bağlamayı gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="schedule-runbooks-to-run-more-frequently"></a>Runbook 'ları daha sık çalışacak şekilde zamanlayın

Azure Otomasyonu 'nda bir zamanlamanın yapılandırılabileceği en sık kullanılan Aralık bir saattir. Zamanlamaları bundan daha sık çalıştırmak istiyorsanız, iki seçenek vardır:

* Runbook için bir [Web kancası](../automation-webhooks.md) oluşturun ve Web kancasını çağırmak için [Azure Logic Apps](../../logic-apps/logic-apps-overview.md) kullanın. Azure Logic Apps, zamanlama tanımlamak için daha ayrıntılı ayrıntı düzeyi sağlar.

* Her birinin 15 dakikadan kısa bir süre içinde başlayıp her saat bir kez çalıştırılan dört zamanlama oluşturun. Bu senaryo, runbook 'un farklı zamanlamalarla 15 dakikada bir çalışmasına izin verir.

## <a name="disable-a-schedule"></a>Zamanlamayı devre dışı bırak

Bir zamanlamayı devre dışı bıraktığınızda, onunla bağlantılı tüm runbook bu zamanlamaya göre çalışmaz. Bir zamanlamayı el ile devre dışı bırakabilir veya bir zaman aşımı süresi oluştururken bir sıklık süresi ayarlayabilirsiniz. Süre sonu zamanına ulaşıldığında, zamanlama devre dışı bırakılır.

### <a name="disable-a-schedule-from-the-azure-portal"></a>Azure portal bir zamanlamayı devre dışı bırakma

1. Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **zamanlamalar** ' ı seçin.
1. Ayrıntılar bölmesini açmak için bir zamanlamanın adını seçin.
1. **Etkin özelliği** **Hayır**olarak değiştirin.

> [!NOTE]
> Başlangıç zamanı geçmişte olan bir zamanlamayı devre dışı bırakmak istiyorsanız, kaydetmeden önce başlangıç tarihini gelecekte bir zaman olarak değiştirmeniz gerekir.

### <a name="disable-a-schedule-with-powershell"></a>PowerShell ile zamanlamayı devre dışı bırakma

Var olan bir zamanlamanın özelliklerini değiştirmek için [set-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationSchedule?view=azps-3.7.0) cmdlet 'ini kullanın. Zamanlamayı devre dışı bırakmak için, `IsEnabled` parametre için false belirtin.

Aşağıdaki örnek, bir Azure Resource Manager cmdlet 'i kullanarak bir runbook için zamanlamanın nasıl devre dışı bırakılacağını gösterir.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="remove-a-schedule"></a>Zamanlamayı kaldırma

Zamanlamalarınızı kaldırmaya hazırsanız Azure portal veya PowerShell kullanabilirsiniz. Önceki bölümde açıklandığı gibi, yalnızca devre dışı bırakılmış bir zamanlamayı kaldıracağınızdan emin olarak unutmayın.

### <a name="remove-a-schedule-using-the-azure-portal"></a>Azure portal kullanarak bir zamanlamayı kaldırma

1. Otomasyon hesabınızda, **paylaşılan kaynaklar**altında **zamanlamalar** ' ı seçin.
2. Ayrıntılar bölmesini açmak için bir zamanlamanın adına tıklayın.
3. **Sil**' e tıklayın.

### <a name="remove-a-schedule-with-powershell"></a>PowerShell ile zamanlama kaldırma

Varolan bir zamanlamayı silmek `Remove-AzAutomationSchedule` için cmdlet 'ini aşağıda gösterildiği gibi kullanabilirsiniz. 

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Remove-AzAutomationSchedule -AutomationAccountName $automationAccountName `
-Name $scheduleName -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Sonraki adımlar

* Zamanlamalara erişmek için kullanılan cmdlet 'ler hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'da modülleri yönetme](modules.md).
* Runbook 'lar hakkında genel bilgi için bkz. [Azure Otomasyonu 'Nda runbook yürütme](../automation-runbook-execution.md).
