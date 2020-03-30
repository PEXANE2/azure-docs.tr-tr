---
title: Azure Otomasyonu'nda Runbook başlatma
description: Azure Otomasyonu'nda bir runbook başlatmak için kullanılabilecek farklı yöntemleri özetler ve hem Azure portalını hem de Windows PowerShell'i kullanma hakkında ayrıntılı bilgi sağlar.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 71dd83db02537ed12dc2e711127e32d90603af6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252655"
---
# <a name="start-a-runbook-in-azure-automation"></a>Azure Otomasyonu'nda Runbook başlatma

Aşağıdaki tablo, Azure Otomasyonu'nda belirli senaryonuza en uygun bir runbook başlatma yöntemini belirlemenize yardımcı olur. Bu makalede, Azure portalı ve Windows PowerShell ile bir runbook başlatma ayrıntıları içerir. Diğer yöntemlerle ilgili ayrıntılar aşağıdaki bağlantılardan erişebileceğiniz diğer belgelerde verilmiştir.

| **Yöntem** | **Özellikler** |
| --- | --- |
| [Azure portalında](#start-a-runbook-with-the-azure-portal) |<li>Etkileşimli kullanıcı arabirimi ile en basit yöntem.<br> <li>Basit parametre değerleri sağlamak için form.<br> <li>İş durumunu kolayca takip edin.<br> <li>Azure oturum açma ile kimlik doğrulaması olan erişim. |
| [Windows PowerShell](/powershell/module/azurerm.automation/start-azurermautomationrunbook) |<li>Windows PowerShell cmdlets ile komut satırından arayın.<br> <li>Birden çok adımla otomatik çözüme dahil edilebilir.<br> <li>İstek, sertifika veya OAuth kullanıcı müdürü / hizmet sorumlusu ile doğrulanır.<br> <li>Basit ve karmaşık parametre değerleri sağlayın.<br> <li>İş durumunu takip et.<br> <li>PowerShell cmdlets desteklemek için istemci gereklidir. |
| [Azure Otomasyon API'si](/rest/api/automation/) |<li>En esnek yöntem ama aynı zamanda en karmaşık.<br> <li>HTTP isteklerini gerçekleştirebilecek herhangi bir özel koddan arama yapın.<br> <li>Sertifika veya Oauth kullanıcı müdürü / hizmet sorumlusu ile kimlik doğrulaması isteği.<br> <li>Basit ve karmaşık parametre değerleri sağlayın. *API'yi kullanarak Python runbook'u arıyorsanız, JSON yükü seri hale getirilmelidir.*<br> <li>İş durumunu takip et. |
| [Webhooks](automation-webhooks.md) |<li>Tek bir HTTP isteğinden runbook başlatın.<br> <li>URL'de güvenlik belirteciyle kimlik doğrulaması.<br> <li>İstemci, webhook oluşturulduğunda belirtilen parametre değerlerini geçersiz kAlamaz. Runbook, HTTP istek ayrıntılarıyla doldurulan tek bir parametre tanımlayabilir.<br> <li>Webhook URL üzerinden iş durumunu izleme olanağı yok. |
| [Azure Uyarısı'na yanıt verme](../log-analytics/log-analytics-alerts.md) |<li>Azure uyarısına yanıt olarak bir runbook başlatın.<br> <li>Runbook için webhook'u yapılandırın ve uyarı için bağlantı kurun.<br> <li>URL'de güvenlik belirteciyle kimlik doğrulaması. |
| [Zamanlama](automation-schedules.md) |<li>Çalışma kitabını saatlik, günlük, haftalık veya aylık zamanlamayla otomatik olarak başlatın.<br> <li>Azure portalı, PowerShell cmdlets veya Azure API'si aracılığıyla zamanlamayı değiştirin.<br> <li>Zamanlama ile kullanılacak parametre değerlerini sağlayın. |
| [Başka Bir Runbook Gönderen](automation-child-runbooks.md) |<li>Runbook'u başka bir runbook'ta etkinlik olarak kullanın.<br> <li>Birden çok runbook tarafından kullanılan işlevsellik için yararlıdır.<br> <li>Alt runbook'a parametre değerleri sağlayın ve üst çalışma kitabında çıktı kullanın. |

Aşağıdaki resim, bir runbook'un yaşam döngüsündeki ayrıntılı adım adım süreci göstermektedir. Bir runbook'un Azure Otomasyonu'nda başlatılmasının farklı yollarını içerir ve Karma Runbook Worker'ın Azure Otomasyon runbook'larını yürütmesi için gereken bileşenler ve farklı bileşenler arasındaki etkileşimleri içerir. Veri merkezinizde Otomasyon runbook'larını yürütme hakkında bilgi edinmek için [karma runbook çalışanlarına](automation-hybrid-runbook-worker.md) bakın

![Runbook Mimarisi](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="start-a-runbook-with-the-azure-portal"></a>Azure portalı ile bir runbook başlatma

1. Azure portalında **Otomasyon'u** seçin ve ardından bir otomasyon hesabının adını tıklatın.
2. Hub menüsünde **Runbook'ları**seçin.
3. **RunBook sayfasında** bir runbook seçin ve ardından **Başlat'ı**tıklatın.
4. Runbook parametreleri varsa, her parametre için bir metin kutusu ile değerler sağlamanız istenir. Parametreler hakkında daha fazla bilgi için [Runbook Parametreleri'ne](#runbook-parameters)bakın.
5. **İş** sayfasında, runbook işinin durumunu görüntüleyebilirsiniz.

## <a name="start-a-runbook-with-powershell"></a>PowerShell ile bir runbook başlatın

Windows PowerShell ile bir runbook başlatmak için [Start-AzureRmAutomationRunbook'u](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) kullanabilirsiniz. Aşağıdaki örnek kod, Test-Runbook adlı bir runbook'u başlatır.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Start-AzureRmAutomationRunbook, runbook başlatıldıktan sonra durumunu izlemek için kullanabileceğiniz bir iş nesnesini döndürür. Daha sonra bu iş nesnesini [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) ile işin durumunu belirlemek için ve çıktısını almak için [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) ile kullanabilirsiniz. Aşağıdaki örnek kod, Test-Runbook adlı bir runbook başlatır, tamamlanmasını bekler ve ardından çıktısını görüntüler.

```azurepowershell-interactive
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Runbook parametreleri gerektiriyorsa, bunları [karma tablo](https://technet.microsoft.com/library/hh847780.aspx)olarak sağlamanız gerekir. Karma tablonun anahtarı parametre adı ile eşleşmelidir ve değer parametre değeridir. Aşağıdaki örnek; FirstName ve LastName adlı iki dize parametresi, RepeatCount adlı bir tamsayı ve Show adlı bir boolean parametresiyle bir runbook’u nasıl çalıştıracağınızı gösterir. Parametreler hakkında daha fazla bilgi için aşağıdaki [Runbook Parametreleri'ne](#runbook-parameters) bakın.

```azurepowershell-interactive
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook parametreleri

Azure portalından veya Windows PowerShell'den bir runbook başlattığınızda, yönerge Azure Otomasyonu web hizmeti aracılığıyla gönderilir. Bu hizmet, karmaşık veri türleri ile parametreleri desteklemez. Karmaşık bir parametre için bir değer sağlamanız gerekiyorsa, azure otomasyonundaki Alt [çalışma kitaplarında](automation-child-runbooks.md)açıklandığı gibi başka bir runbook'tan satır satır aramanız gerekir.

Azure Otomasyonu web hizmeti, aşağıdaki bölümlerde açıklandığı gibi belirli veri türlerini kullanarak parametreler için özel işlevler sağlar:

### <a name="named-values"></a>Adlandırılmış değerler

Parametre veri türü [nesne] ise, aşağıdaki JSON biçimini kullanarak adlandırılmış değerlerin bir listesini gönderebilirsiniz: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*. Bu değerler basit türler olmalıdır. Runbook, adı geçen her değere karşılık gelen özelliklere sahip bir [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject) olarak parametre alır.

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

Bu, aşağıdaki çıktıyla sonuçlanır:

```output
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Diziler

Parametre [dizi] veya [string[]] gibi bir diziyse, aşağıdaki JSON biçimini kullanarak bir değer listesi gönderebilirsiniz: *[Value1, Value2, Value3]*. Bu değerler basit türler olmalıdır.

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

Bu, aşağıdaki çıktıyla sonuçlanır:

```output
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Kimlik Bilgileri

Parametre veri türü **PSCredential**ise, o zaman bir Azure Otomasyon [kimlik bilgisi varlığının](automation-credentials.md)adını sağlayabilirsiniz. Runbook, belirttiğiniz adla kimlik bilgisini alır.

Kullanıcı bilgisiadlı bir parametreyi kabul eden aşağıdaki sınama runbook’unu göz önünde bulundurun.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

Aşağıdaki metin, *Kimlik Bilgilerim*adlı bir kimlik bilgisi varlığı olduğunu varsayarak kullanıcı parametresi için kullanılabilir.

```input
My Credential
```

Kimlik bilgisindeki kullanıcı adının *jsmith*olduğunu varsayarsak, bu sonuç aşağıdaki çıktıyla sonuçlanır:

```output
jsmith
```

## <a name="next-steps"></a>Sonraki adımlar

* Geçerli makaledeki runbook mimarisi, Azure'da kaynakları yöneten runbook'ların üst düzey bir genel görünümünü ve Karma Runbook Worker ile şirket içi bir bakış sağlar. Veri merkezinizde Otomasyon runbook'larını yürütme hakkında bilgi edinmek için [Karma Runbook Çalışanları'na](automation-hybrid-runbook-worker.md)bakın.
* Belirli veya yaygın işlevler için diğer runbook'lar tarafından kullanılacak modüler runbook'lar oluşturma hakkında daha fazla bilgi edinmek için [Alt Runbook'lara](automation-child-runbooks.md)bakın.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
