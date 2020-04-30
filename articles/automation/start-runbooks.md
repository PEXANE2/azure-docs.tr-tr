---
title: Azure Otomasyonu'nda Runbook başlatma
description: ", Azure Otomasyonu 'nda bir runbook başlatmak için kullanılabilecek farklı yöntemleri özetler ve hem Azure portal hem de Windows PowerShell kullanma hakkında ayrıntılı bilgi sağlar."
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 7f2c0dda952959db3bffba6016f48b986016c19e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679462"
---
# <a name="start-a-runbook-in-azure-automation"></a>Azure Otomasyonu'nda Runbook başlatma

Aşağıdaki tablo, belirli senaryonuz için en uygun olan Azure Otomasyonu 'nda bir runbook 'u başlatmak için yöntemi belirlemenize yardımcı olur. Bu makale, Azure portal ve Windows PowerShell ile bir runbook başlatma hakkındaki ayrıntıları içerir. Diğer yöntemler hakkındaki ayrıntılar, aşağıdaki bağlantılardan erişebileceğiniz diğer belgelerde sunulmaktadır.

| **Yöntem** | **Özellikler** |
| --- | --- |
| [Azure portal](#start-a-runbook-with-the-azure-portal) |<li>Etkileşimli kullanıcı arabirimiyle en basit yöntem.<br> <li>Basit parametre değerleri sağlamak için form.<br> <li>İş durumunu kolayca izleyin.<br> <li>Azure oturum açma ile erişim kimliği doğrulandı. |
| [Windows PowerShell](/powershell/module/azurerm.automation/start-azurermautomationrunbook) |<li>Windows PowerShell cmdlet 'leri ile komut satırından çağrı.<br> <li>, Birden çok adımla otomatikleştirilmiş çözüme eklenebilir.<br> <li>İstek sertifika veya OAuth Kullanıcı sorumlusu/hizmet sorumlusu ile doğrulandı.<br> <li>Basit ve karmaşık parametre değerlerini sağlayın.<br> <li>İş durumunu izleyin.<br> <li>PowerShell cmdlet 'lerini desteklemek için gereken istemci. |
| [Azure Otomasyonu API 'SI](/rest/api/automation/) |<li>En esnek yöntem, ancak çoğu karmaşıktır.<br> <li>HTTP istekleri yapan herhangi bir özel koddan çağırın.<br> <li>Sertifika veya OAuth Kullanıcı sorumlusu/hizmet sorumlusu ile kimlik doğrulaması iste.<br> <li>Basit ve karmaşık parametre değerlerini sağlayın. *API 'yi kullanarak bir Python runbook 'u arıyorsanız JSON yükünün serileştirilmesi gerekir.*<br> <li>İş durumunu izleyin. |
| [Web kancaları](automation-webhooks.md) |<li>Tek bir HTTP isteğinden runbook 'u başlatın.<br> <li>URL 'de güvenlik belirteci ile kimlik doğrulandı.<br> <li>İstemci, Web kancası oluşturulduğunda belirtilen parametre değerlerini geçersiz kılamaz. Runbook, HTTP istek ayrıntıları ile doldurulmuş tek bir parametre tanımlayabilir.<br> <li>Web kancası URL 'SI aracılığıyla iş durumunu izleyebilme yeteneği yoktur. |
| [Azure uyarısına yanıt verme](../log-analytics/log-analytics-alerts.md) |<li>Azure uyarısına yanıt olarak bir runbook başlatın.<br> <li>Runbook için Web kancasını yapılandırın ve uyarı bağlantısı yapın.<br> <li>URL 'de güvenlik belirteci ile kimlik doğrulandı. |
| [Zamanlama](automation-schedules.md) |<li>Runbook 'u saatlik, günlük, haftalık veya aylık zamanlamaya göre otomatik olarak başlatın.<br> <li>Azure portal, PowerShell cmdlet 'leri veya Azure API aracılığıyla zamanlamayı işleme.<br> <li>Zamanlamaya göre kullanılacak parametre değerlerini sağlayın. |
| [Başka bir runbook 'tan](automation-child-runbooks.md) |<li>Runbook 'u başka bir runbook 'ta etkinlik olarak kullanın.<br> <li>Birden çok runbook tarafından kullanılan işlevsellik için faydalıdır.<br> <li>Alt runbook 'a parametre değerleri sağlayın ve üst runbook 'ta çıktıyı kullanın. |

Aşağıdaki görüntüde, bir runbook 'un yaşam döngüsünde ayrıntılı adım adım işlem gösterilmektedir. Bu, bir runbook 'un Azure Otomasyonu 'nda başladığı farklı yollarını içerir. Bu, karma Runbook Worker için gereken bileşenleri Azure Otomasyonu runbook 'larını ve farklı bileşenler arasında etkileşimleri yürütmek için gereklidir. Veri merkezinizde Otomasyon Runbook 'larını yürütme hakkında bilgi edinmek için [karma runbook çalışanlarına](automation-hybrid-runbook-worker.md) bakın

![Runbook mimarisi](media/automation-starting-runbook/runbooks-architecture.png)

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="runbook-parameters"></a>Runbook parametreleri

Azure portal veya Windows PowerShell 'den bir runbook başlattığınızda, yönerge Azure Otomasyonu web hizmeti aracılığıyla gönderilir. Bu hizmet, karmaşık veri türleriyle parametreleri desteklemiyor. Karmaşık bir parametre için bir değer sağlamanız gerekiyorsa, bunu [Azure Otomasyonu 'Nda alt runbook 'lar](automation-child-runbooks.md)bölümünde açıklandığı gibi başka bir runbook 'tan satır içi olarak çağırmanız gerekir.

Azure Otomasyonu web hizmeti, aşağıdaki bölümlerde açıklandığı gibi belirli veri türlerini kullanarak parametreler için özel işlevler sağlar:

### <a name="named-values"></a>Adlandırılmış değerler

Parametre, [Object] veri türünde ise, adlandırılmış değerlerin bir listesini göndermek için şu JSON biçimini kullanabilirsiniz: *{name1: ' değer1 ', AD2: ' değer2 ', Name3: ' Value3 '}*. Bu değerler basit türler olmalıdır. Runbook, parametreyi her bir adlandırılmış değere karşılık gelen özelliklerle birlikte bir [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject) olarak alır.

kullanıcıadlı bir parametreyi kabul eden aşağıdaki sınama runbook’u göz önünde bulundurun.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

Aşağıdaki metin kullanıcı parametresi için kullanılabilir.

```json
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Bu, aşağıdaki çıkışa neden olur:

```output
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Diziler

Parametre, [dizi] veya [String []] gibi bir diziyse, değerlerin bir listesini göndermek için şu JSON biçimini kullanabilirsiniz: *[değer1, değer2, Value3]*. Bu değerler basit türler olmalıdır.

*kullanıcı*adlı bir parametreyi kabul eden aşağıdaki sınama runbook’u göz önünde bulundurun.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

Aşağıdaki metin kullanıcı parametresi için kullanılabilir.

```input
["Joe","Smith",2,true]
```

Bu, aşağıdaki çıkışa neden olur:

```output
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Kimlik Bilgileri

Parametre veri türü `PSCredential`ise, bir Azure Otomasyonu [kimlik bilgisi varlığının](automation-credentials.md)adını sağlayabilirsiniz. Runbook, kimlik bilgisini belirttiğiniz adla alır. Aşağıdaki test runbook 'u adlı `credential`bir parametreyi kabul eder.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

Aşağıdaki metin, adlı `My Credential`bir kimlik bilgisi varlığı olduğunu varsayarak Kullanıcı parametresi için kullanılabilir.

```input
My Credential
```

Kimlik bilgilerinde kullanıcı adının nerede olduğunu varsayarsak `jsmith`, aşağıdaki çıktı görüntülenir.

```output
jsmith
```

## <a name="start-a-runbook-with-the-azure-portal"></a>Azure portal bir runbook 'u başlatma

1. Azure portal, **Otomasyon** ' u seçin ve ardından bir Otomasyon hesabının adına tıklayın.
2. Hub menüsünde **runbook 'lar**' ı seçin.
3. Runbook 'Lar sayfasında, bir runbook seçin ve ardından **Başlat**' a tıklayın.
4. Runbook 'un parametreleri varsa her parametre için bir metin kutusuyla birlikte değerler sağlamanız istenir. Parametreler hakkında daha fazla bilgi için bkz. [runbook parametreleri](#runbook-parameters).
5. Iş bölmesinde, runbook işinin durumunu görüntüleyebilirsiniz.

## <a name="start-a-runbook-with-powershell"></a>PowerShell ile runbook başlatma

Windows PowerShell ile bir runbook başlatmak için [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) 'u kullanabilirsiniz. Aşağıdaki örnek kod, **Test-runbook**adlı bir runbook 'u başlatır.

```azurepowershell-interactive
Start-AzAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

`Start-AzAutomationRunbook`Runbook başlatıldıktan sonra durumu izlemek için kullanabileceğiniz bir iş nesnesi döndürür. Daha sonra bu iş nesnesini [Get-AzAutomationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJob?view=azps-3.7.0) ile birlikte kullanarak, işinin durumunu belirleyebilir ve çıktısını almak için [Get-Azautomationjoi put](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationjoboutput?view=azps-3.7.0) ' i kullanabilirsiniz. Aşağıdaki örnek, **Test-runbook**adlı bir runbook başlatır, tamamlanmasını bekler ve ardından çıktısını görüntüler.

```azurepowershell-interactive
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Runbook için parametreler gerekiyorsa, bunları bir [Hashtable](https://technet.microsoft.com/library/hh847780.aspx)olarak belirtmeniz gerekir. Hashtable 'ın anahtarı parametre adıyla ve değer parametre değeri olmalıdır. Aşağıdaki örnek; FirstName ve LastName adlı iki dize parametresi, RepeatCount adlı bir tamsayı ve Show adlı bir boolean parametresiyle bir runbook’u nasıl çalıştıracağınızı gösterir. Parametreler hakkında daha fazla bilgi için bkz. [runbook parametreleri](#runbook-parameters).

```azurepowershell-interactive
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="next-steps"></a>Sonraki adımlar

* Veri merkezinizde Otomasyon Runbook 'larını yürütme hakkında bilgi edinmek için [karma runbook çalışanlarına](automation-hybrid-runbook-worker.md)bakın.
* Belirli veya yaygın işlevler için diğer runbook 'lar tarafından kullanılacak modüler runbook 'lar oluşturma hakkında daha fazla bilgi edinmek için [alt runbook 'lara](automation-child-runbooks.md)bakın.
* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/overview)bakın.
