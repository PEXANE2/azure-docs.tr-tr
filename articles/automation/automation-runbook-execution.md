---
title: Azure Otomasyon'da runbook yürütme
description: Azure Otomasyonu'ndaki bir runbook'un nasıl işlendiğine ilişkin ayrıntıları açıklar.
services: automation
ms.subservice: process-automation
ms.date: 04/04/2019
ms.topic: conceptual
ms.openlocfilehash: de01a7a76a5d225770c273c67f864c83226ecd07
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261321"
---
# <a name="runbook-execution-in-azure-automation"></a>Azure Otomasyon'da runbook yürütme

Azure Otomasyonu'nda süreç otomasyonu PowerShell, PowerShell İş Akışı ve grafik runbook'lar oluşturmanıza ve yönetmenize olanak tanır. Ayrıntılar için Azure [Otomasyon runbook'ları'na](automation-runbook-types.md)bakın. 

Otomasyon, runbook'larınızı içlerinde tanımlanan mantığa göre yürütür. Bir runbook kesilirse, başlangıçta yeniden başlatılır. Bu davranış, geçici sorunlar oluşursa yeniden başlatılmasını destekleyen runbook'lar yazmanızı gerektirir.

Azure Otomasyonu'nda bir runbook başlatmak, runbook'un tek bir yürütme örneği olan bir iş oluşturur. Her iş, Azure aboneliğinize bağlantı kurarak Azure kaynaklarına erişiyor. İş, yalnızca bu kaynaklara genel buluttan erişilebiliyorsa, veri merkezinizdeki kaynaklara erişebilir.

Azure Otomasyon, runbook yürütme sırasında her işi çalıştıracak bir işçi atar. Çalışanlar birçok Azure hesabı tarafından paylaşılırken, farklı Otomasyon hesaplarındaki işler birbirinden izole edilir. İş isteklerinizi hangi işçinin hizmet eve sunabileceğini denetemezsiniz.

Azure portalında runbook listesini görüntülediğinizde, her runbook için başlatılan her işin durumunu gösterir. Azure Otomasyon, iş günlüklerini en fazla 30 gün boyunca saklar. 

Aşağıdaki diyagram [PowerShell runbook'lar, PowerShell](automation-runbook-types.md#powershell-runbooks)Çalışma [Akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks)ve [grafik çalışma kitapları](automation-runbook-types.md#graphical-runbooks)için bir runbook işinin yaşam döngüsünü gösterir.

![İş Durumları - PowerShell İş Akışı](./media/automation-runbook-execution/job-statuses.png)

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="where-to-run-your-runbooks"></a>Runbook'larınızı nerede çalıştıracağınız

Azure Otomasyonundaki runbook'lar Azure kum havuzunda veya [Karma Runbook Çalışanı'nda](automation-hybrid-runbook-worker.md)çalıştırılabilir. Çoğu runbook'u, birden çok işin kullanabileceği paylaşılan bir ortam olan Azure kum havuzunda kolayca çalıştırabilirsiniz. Aynı kum havuzunu kullanan işler, kum havuzunun kaynak sınırlamalarına bağlıdır.

>[!NOTE]
>Azure kum havuzu ortamı etkileşimli işlemleri desteklemez. Ayrıca Win32 aramaları yapmak runbooks için yerel MOF dosyalarının kullanımını gerektirir.

Çalışma kitaplarını doğrudan rolü barındıran bilgisayarda çalıştırmak ve ortamdaki yerel kaynaklara karşı çalıştırmak için Karma Runbook Çalışanı kullanabilirsiniz. Azure Otomasyon, runbook'ları depolar ve yönetir ve bunları bir veya daha fazla atanmış bilgisayara teslim eder.

Aşağıdaki tabloda, her biri için önerilen yürütme ortamı listelenen bazı runbook yürütme görevleri listelenir.

|Görev|En İyi Seçim|Notlar|
|---|---|---|
|Azure kaynaklarıyla tümleştirme|Azure Kum Havuzu|Azure'da barındırılan kimlik doğrulama sı daha basittir. Azure VM'de Karma Runbook İşçisi kullanıyorsanız, [Azure kaynakları için yönetilen kimlikleri](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)kullanabilirsiniz.|
|Azure kaynaklarını yönetmek için en iyi performansı elde edin|Azure Kum Havuzu|Komut dosyası, daha az gecikme sebebleri olan aynı ortamda çalıştırılır.|
|Operasyonel maliyetleri en aza indirin|Azure Kum Havuzu|Hiçbir bilgi işlem yükü ve vm gerek yoktur.|
|Uzun süreli komut dosyalarını yürütme|Karma Runbook Çalışanı|Azure sandboxes [kaynakları üzerinde sınırlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)vardır.|
|Yerel Hizmetlerle etkileşim|Karma Runbook Çalışanı|Ana makineye doğrudan erişim olabilir.|
|Üçüncü taraf yazılım ları ve çalıştırılabilir yazılımları gerektirme|Karma Runbook Çalışanı|İşletim sistemini yönetirsiniz ve yazılım yükleyebilirsiniz.|
|Bir dosyayı veya klasörü runbook ile izleme|Karma Runbook Çalışanı|Bir Karma Runbook Worker bir [Watcher görev](automation-watchers-tutorial.md) kullanın.|
|Kaynak yoğun bir komut dosyası çalıştırma|Karma Runbook Çalışanı| Azure sandboxes [kaynakları üzerinde sınırlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)vardır.|
|Özel gereksinimlere sahip modülleri kullanma| Karma Runbook Çalışanı|Bazı örnekler şunlardır:</br> WinSCP - winscp.exe bağımlılığı </br> IISAdministration - IIS'nin etkinleştirilmesine bağımlılık.|
|Yükleyicili bir modül yükleme|Karma Runbook Çalışanı|Kum havuzu modülleri kopyalamayı desteklemelidir.|
|4.7.2'den farklı .NET Framework sürümü gerektiren runbook'ları veya modülleri kullanın|Karma Runbook Çalışanı|Otomasyon ve kasalarında .NET Framework 4.7.2 vardır ve sürümü yükseltmenin bir yolu yoktur.|
|Yükseklik gerektiren komut dosyalarını çalıştırma|Karma Runbook Çalışanı|Kum kutuları yüksekliğe izin vermez. Bir Karma Runbook Worker ile UAC'yi kapatabilir ve yükseklik gerektiren komutu çalıştırırken [Invoke Komutu'nu](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7) kullanabilirsiniz.|
|Windows Yönetim Araçları 'na (WMI) erişim gerektiren komut dosyaları çalıştırma|Karma Runbook Çalışanı|Buluttaki kum kutularında çalışan işler WMI'ye erişemez. |

## <a name="runbook-behavior"></a>Runbook davranışı

### <a name="creating-resources"></a>Kaynak oluşturma

Runbook'unuz bir kaynak oluşturuyorsa, komut dosyası, kaynağı oluşturmaya çalışmadan önce kaynağın zaten var olup olmadığını denetlemelidir. İşte temel bir örnek.

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exist, creating"
    New-AzureRMVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

### <a name="supporting-time-dependent-scripts"></a>Zamana bağlı komut dosyalarını destekleme

Runbook'larınız sağlam ve yeniden başlatmalarına veya başarısız olmalarına neden olabilecek geçici hataları işleyebilme yeteneğine sahip olmalıdır. Bir runbook başarısız olursa, Azure Automation yeniden çalışır.

Runbook'unuzun normalde bir zaman kısıtlaması içinde çalışması durumunda, yürütme süresini denetlemek için komut dosyası uygulama mantığına sahip olmasını edin. Bu denetim, yalnızca belirli zamanlarda başlatma, kapatma veya ölçeklendirme gibi işlemlerin çalışmasını sağlar.

> [!NOTE]
> Azure kum havuzu işleminde yerel saat UTC olarak ayarlanır. Runbook'larınızdaki tarih ve saat hesaplamaları bu gerçeği göz önünde bulundurmalıdır.

### <a name="tracking-progress"></a>İlerlemeyi izleme

Runbook'larınızı doğada modüler olarak yazar, mantığınızı kolayca yeniden kullanıp yeniden başlatabilirsiniz iyi bir uygulamadır. Runbook'taki ilerlemeyi izleme, sorun varsa runbook mantığının doğru şekilde yürütülmesini sağlamanın iyi bir yoludur. Bir runbook'un ilerlemesini, depolama hesabı, veritabanı veya paylaşılan dosyalar gibi harici bir kaynak kullanarak izlemek mümkündür. İlk olarak alınan son eylemin durumunu denetlemek için çalışma kitabınızda mantık oluşturabilirsiniz. Daha sonra, denetimin sonucuna bağlı olarak, mantık runbook'taki belirli görevleri atlayabilir veya devam edebilir.

### <a name="preventing-concurrent-jobs"></a>Eşzamanlı işlerin önlenmesi

Bazı runbook'lar aynı anda birden fazla işte çalıştıklarında garip bir şekilde çalışırlar. Bu durumda, çalışan bir iş olup olmadığını belirlemek için bir runbook'un mantık uygulaması önemlidir. İşte temel bir örnek.

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationId $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

$AzContext = Select-AzSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzContext $AzureContext

# Check to see if it is already running
$runningCount = ($jobs | ? {$_.Status -eq "Running"}).count

If (($jobs.status -contains "Running" -And $runningCount -gt 1 ) -Or ($jobs.Status -eq "New")) {
    # Exit code
    Write-Output "Runbook is already running"
    Exit 1
} else {
    # Insert Your code here
}
```

### <a name="working-with-multiple-subscriptions"></a>Birden fazla abonelik ile çalışma

Birden çok abonelikle başa çıkmak için runbook'unuzun [Devre Dışı Bırak-AzContextAutosave](https://docs.microsoft.com/powershell/module/Az.Accounts/Disable-AzContextAutosave?view=azps-3.5.0) cmdlet'ini kullanması gerekir. Bu cmdlet, kimlik doğrulama bağlamının aynı kum havuzunda çalışan başka bir runbook'tan alınmamasını sağlar. Runbook ayrıca Az`AzContext` modülü cmdlets üzerinde parametre kullanır ve uygun bağlam geçer.

```powershell
# Ensures that you do not inherit an AzContext in your runbook
Disable-AzContextAutosave –Scope Process

$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal `
-Tenant $Conn.TenantID `
-ApplicationId $Conn.ApplicationID `
-CertificateThumbprint $Conn.CertificateThumbprint

$context = Get-AzContext

$ChildRunbookName = 'ChildRunbookDemo'
$AutomationAccountName = 'myAutomationAccount'
$ResourceGroupName = 'myResourceGroup'

Start-AzAutomationRunbook `
    -ResourceGroupName $ResourceGroupName `
    -AutomationAccountName $AutomationAccountName `
    -Name $ChildRunbookName `
    -DefaultProfile $context
```

### <a name="handling-exceptions"></a>Özel durum işleme

Bu bölümde, runbook'larınızdaki özel durumları veya aralıklı sorunları işlemek için bazı yollar açıklanmaktadır.

#### <a name="erroractionpreference"></a>ErrorActionPreference

[ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) değişkeni PowerShell'in sonlandırıcı olmayan bir hataya nasıl yanıt veriş olduğunu belirler. Sonlandırma hataları her zaman sona erer `ErrorActionPreference`ve 'den etkilenmez.

Runbook kullandığında, `ErrorActionPreference` `PathNotFound` [Get-ChildItem](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7) cmdlet gibi normalde sonlandırıcı olmayan bir hata runbook'un tamamlanmasını durdurur. Aşağıdaki örnekte `ErrorActionPreference`. Komut dosyası durdukça son [Yazma-Çıktı](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7) komutu hiçbir zaman yürütülmez.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-ChildItem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Catch Finally deneyin

[Catch Finally'u deneyin,](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) powershell komut dosyalarında sonlandırma hatalarını işlemek için kullanılır. Komut dosyası, belirli özel durumları veya genel özel durumları yakalamak için bu mekanizmayı kullanabilir. İfade, `catch` hataları izlemek veya işlemeye çalışmak için kullanılmalıdır. Aşağıdaki örnek, var olmayan bir dosyayı karşıdan yüklemeye çalışır. `System.Net.WebException` Özel durumu yakalar ve diğer istisnalar için son değeri döndürür.

```powershell-interactive
try
{
   $wc = new-object System.Net.WebClient
   $wc.DownloadFile("http://www.contoso.com/MyDoc.doc")
}
catch [System.Net.WebException]
{
    "Unable to download MyDoc.doc from http://www.contoso.com."
}
catch
{
    "An error occurred that could not be resolved."
}
```

#### <a name="throw"></a>Throw

[Throw](/powershell/module/microsoft.powershell.core/about/about_throw) bir sonlandırma hatası oluşturmak için kullanılabilir. Bu mekanizma, bir runbook kendi mantığı tanımlarken yararlı olabilir. Komut dosyası, onu durdurması gereken bir ölçütle `throw` karşılaşırsa, deyimi durdurmak için kullanabilir. Aşağıdaki örnek, gerekli işlev parametresini göstermek için bu deyimi kullanır.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>Yürütülebilir leri veya arama işlemlerini kullanma

Azure ve zum kutularında çalışan runbook'lar, yürütülebilir ler **(.exe** dosyaları) veya alt işlemler gibi arama işlemlerini desteklemez. Bunun nedeni, Azure kum havuzunun, temel deki tüm API'lere erişemeyebilen bir kapsayıcıda paylaşılan bir işlem çalışması olmasıdır. Üçüncü taraf yazılım ları veya alt işlemlere çağrı gerektiren senaryolar için, [Bir Karma Runbook Çalışanı](automation-hybrid-runbook-worker.md)üzerinde bir runbook yürütmeniz gerekir.

### <a name="accessing-device-and-application-characteristics"></a>Cihaza ve uygulama özelliklerine erişim

Azure ve zum kutularında çalışan runbook işleri hiçbir aygıta veya uygulama özelliğine erişemez. Windows'da performans ölçümlerini sorgulamak için kullanılan en yaygın API, bazı yaygın ölçümlerbellek ve CPU kullanımı olmak üzere WMI'dir. Ancak, bulutta çalışan işler Web Tabanlı Kurumsal Yönetim'in (WBEM) Microsoft uygulamasına erişilemedığından, API'nin hangi şekilde kullanıldığı önemli değildir. Bu platform, cihaz ve uygulama özelliklerini tanımlamak için endüstri standartlarını sağlayan Ortak Bilgi Modeli (CIM) üzerine kurulmuştur.

## <a name="handling-errors"></a>Hataları işleme

Runbook'larınız hataları işleyebilme yeteneğine sahip olmalıdır. PowerShell'de sonlandırma ve sonlandırma olmayan iki tür hata vardır. Sonlandırma hataları oluştuğunda runbook yürütmeyi durdurur. Runbook Başarısız bir iş durumu ile durur.

Sonlandırma yanlışhatalar, bir komut dosyasın oluştuktan sonra bile sürecüsüne izin verir. Sonlandırma hatası olmayan bir örnek, bir runbook var olmayan bir `Get-ChildItem` yol ile cmdlet kullandığında ortaya çıkan bir örnektir. PowerShell yolun var olmadığını görür, hata atar ve bir sonraki klasöre devam eder. Bu durumda hata runbook iş durumunu Başarısız olarak ayarlamaz ve iş tamamlanmış bile olabilir. Bir runbook'u sonlandırma hatası namına durdurmaya zorlamak `ErrorAction Stop` için cmdlet'te kullanabilirsiniz.

## <a name="handling-jobs"></a>İşleri işleme

Aynı Otomasyon hesabından işler için yürütme ortamını yeniden kullanabilirsiniz. Tek bir runbook aynı anda çalışan birçok iş olabilir. Aynı anda ne kadar çok iş çalıştırarsanız, aynı kum havuzuna o kadar sık gönderilebilir.

Aynı kum havuzu işleminde çalışan işler birbirini etkileyebilir. Bir [örnek, Disconnect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/disconnect-azaccount?view=azps-3.7.0) cmdlet'i çalıştırmaktır. Bu cmdlet'in yürütülmesi, paylaşılan kum havuzu işlemindeki her runbook işini keser.

PowerShell işleri, Azure kum havuzunda çalışan bir runbook'tan başladı ve tam [PowerShell dil modunda](/powershell/module/microsoft.powershell.core/about/about_language_modes)çalışmayabilir. Azure Otomasyonu'ndaki işlerle etkileşim hakkında daha fazla bilgi için [PowerShell ile iş durumu alma](#retrieving-job-status-using-powershell)bilgisine bakın.

### <a name="job-statuses"></a>İş durumları

Aşağıdaki tabloda bir iş için mümkün olan durumları açıklanır.

| Durum | Açıklama |
|:--- |:--- |
| Tamamlandı |İş başarıyla tamamlandı. |
| Başarısız |Grafik veya PowerShell İş Akışı çalışma kitabı derlemede başarısız oldu. PowerShell komut dosyası runbook başlatmak için başarısız oldu veya iş bir özel durum vardı. Bkz. [Azure Otomasyon runbook türleri.](automation-runbook-types.md)|
| Başarısız oldu, kaynak bekleniyor |İş, [adil paylaşım](#fair-share) sınırına üç kez ulaştığı ve aynı kontrol noktasından veya her seferinde runbook'un başından başladığı için başarısız oldu. |
| Kuyruğa alındı |İş, başlatılabilmek için bir Otomasyon çalışanının kullanılabilir hale gelmesini bekliyor. |
| Başlatılıyor |İş bir işçiye atandı ve sistem bunu başlatıyor. |
| Sürdürülüyor |Sistem askıya alındıktan sonra işe devam ediyor. |
| Çalışıyor |İş çalışıyor. |
| Çalışan, kaynak bekleyen |Adil hisse sınırına ulaştığı için iş boşaltıldı. Son kontrol noktasından kısa bir süre sonra devam edecek. |
| Durduruldu |İş tamamlanmadan kullanıcı tarafından durduruldu. |
| Durduruluyor |Sistem işi durduruyor. |
| Askıya alındı |Yalnızca [grafik ve PowerShell İş Akışı runbook'ları](automation-runbook-types.md) için geçerlidir. İş; kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Runbook'ta denetim noktası yoksa, en baştan başlar. Bir denetim noktası varsa, yeniden başlatılabilir ve son denetim noktasından devam edebilir. Sistem yalnızca bir özel durum oluştuğunda runbook askıya. Varsayılan olarak, `ErrorActionPreference` değişken, işin bir hata üzerinde çalışmaya devam ettiğini belirten Devam olarak ayarlanır. Tercih değişkeni Dur olarak ayarlanmışsa, iş bir hata üzerine askıya alınır.  |
| Askıya alınıyor |Yalnızca [grafik ve PowerShell İş Akışı runbook'ları](automation-runbook-types.md) için geçerlidir. Sistem, kullanıcının isteği üzerine işi askıya almaya çalışıyor. Runbook, askıya alınmadan sonraki denetim noktasına erişmelidir. Son kontrol noktasını çoktan geçtiyse, askıya alınmadan önce tamamlar. |

### <a name="viewing-job-status-from-the-azure-portal"></a>Azure portalından iş durumunu görüntüleme

Tüm runbook işleri için durum özetini görüntüleyebilir veya Azure portalında belirli bir runbook işinin ayrıntılarını inceleyebilirsiniz. Log Analytics çalışma alanınızla tümleştirmeyi runbook iş durumunu ve iş akışlarını iletmek için de yapılandırabilirsiniz. Azure Monitor günlükleriyle tümleştirme hakkında daha fazla bilgi için, [Otomasyon'dan Azure Monitor günlüklerine İleri iş durumu ve iş akışları](automation-manage-send-joblogs-log-analytics.md)na bakın.

Seçili Otomasyon hesabınızın sağında, **İş İstatistikleri** döşemesi altındaki tüm runbook işlerinin bir özetini görebilirsiniz.

![İş İstatistikleri döşemesi](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Bu döşeme, yürütülen her iş için iş durumunun bir sayı ve grafik gösterimi görüntüler.

Döşemeyi tıklattığınızda, yürütülen tüm işlerin özetlistesini içeren İşler sayfası yer alıyor. Bu sayfa, her iş için durum, runbook adı, başlangıç saati ve tamamlama saatini gösterir.

![Otomasyon hesabı İşler sayfası](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

İş yerlerini Filtrele'yi **Filter jobs**seçerek iş listesini filtreleyebilirsiniz. Belirli bir runbook'a, iş durumuna veya açılır bırakma listesinden bir seçime filtre uygulayın ve arama için zaman aralığını sağlayın.

![Filtre iş durumu](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternatif olarak, Otomasyon hesabınızdaki RunBook sayfasından o runbook'u seçip sonra **İşler** döşemesini seçerek belirli bir runbook'un iş özeti ayrıntılarını görüntüleyebilirsiniz. Bu eylem İşler sayfasını sunar. Buradan, ayrıntılarını ve çıktısını görüntülemek için iş kaydını tıklatabilirsiniz.

![Otomasyon hesabı İşler sayfası](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="viewing-the-job-summary"></a>İş özetini görüntüleme

Yukarıda açıklanan iş özeti, belirli bir runbook için oluşturulan tüm işlerin listesine ve bunların en son durumlarına bakmanızı sağlar. Bir iş için ayrıntılı bilgi ve çıktı görmek için, listedeki adını tıklatın. İşin ayrıntılı görünümü, bu işe sağlanan runbook parametreleri için değerleri içerir.

Bir runbook işlerini görüntülemek için aşağıdaki adımları kullanabilirsiniz.

1. Azure portalında **Otomasyon'u** seçin ve ardından bir Otomasyon hesabının adını seçin.
2. Merkezden, **Proses Otomasyonu**altında **Runbook'ları** seçin.
3. RunBook'lar sayfasında, listeden bir runbook seçin.
3. Seçili runbook'un **sayfasında, İşler** döşemesini tıklatın.
4. Listedeki işlerden birini tıklatın ve runbook iş ayrıntıları sayfasında ayrıntılarını ve çıktısını görüntüleyin.

### <a name="retrieving-job-status-using-powershell"></a>PowerShell kullanarak iş durumunu alma

Bir `Get-AzAutomationJob` runbook için oluşturulan işleri ve belirli bir işin ayrıntılarını almak için cmdlet'i kullanın. PowerShell kullanarak `Start-AzAutomationRunbook`bir runbook başlarsanız, ortaya çıkan işi döndürür. İş çıktısını almak için [Get-AzAutomationJobOutput'u](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0) kullanın.

Aşağıdaki örnek, örnek bir runbook için son işi alır ve durumunu, runbook parametreleri için sağlanan değerleri ve iş çıktısını görüntüler.

```azurepowershell-interactive
$job = (Get-AzAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Aşağıdaki örnek, belirli bir iş için çıktı alır ve her kaydı döndürür. Kayıtlardan biri için bir özel durum varsa, komut dosyası değer yerine özel durumu yazar. Özel durumlar çıktı sırasında normal olarak günlüğe kaydedilemeyecek ek bilgiler sağlayabildiği için bu davranış yararlıdır.

```azurepowershell-interactive
$output = Get-AzAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="getting-details-from-the-activity-log"></a>Etkinlik günlüğünden ayrıntıları alma

Runbook'u başlatan kişi veya hesap gibi runbook ayrıntılarını Otomasyon hesabının Etkinlik günlüğünden alabilirsiniz. Aşağıdaki PowerShell örneği, belirtilen runbook'u çalıştıran son kullanıcıyı sağlar.

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$AutomationResourceGroupName = "MyResourceGroup"
$AutomationAccountName = "MyAutomationAccount"
$RunbookName = "MyRunbook"
$StartTime = (Get-Date).AddDays(-1)
$JobActivityLogs = Get-AzLog -ResourceGroupName $AutomationResourceGroupName -StartTime $StartTime `
                                | Where-Object {$_.Authorization.Action -eq "Microsoft.Automation/automationAccounts/jobs/write"}

$JobInfo = @{}
foreach ($log in $JobActivityLogs)
{
    # Get job resource
    $JobResource = Get-AzResource -ResourceId $log.ResourceId

    if ($JobInfo[$log.SubmissionTimestamp] -eq $null -and $JobResource.Properties.runbook.name -eq $RunbookName)
    {
        # Get runbook
        $Runbook = Get-AzAutomationJob -ResourceGroupName $AutomationResourceGroupName -AutomationAccountName $AutomationAccountName `
                                            -Id $JobResource.Properties.jobId | ? {$_.RunbookName -eq $RunbookName}

        # Add job information to hashtable
        $JobInfo.Add($log.SubmissionTimestamp, @($Runbook.RunbookName,$Log.Caller, $JobResource.Properties.jobId))
    }
}
$JobInfo.GetEnumerator() | sort key -Descending | Select-Object -First 1
```

## <a name="sharing-resources-among-runbooks"></a><a name="fair-share"></a>Kaynakları runbook'lar arasında paylaşma

Buluttaki tüm runbook'lar arasında kaynakları paylaşmak için Azure Otomasyon, üç saatten uzun süredir çalışan herhangi bir işi geçici olarak kaldırır veya durdurur. [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks) ve [Python runbook'ları](automation-runbook-types.md#python-runbooks) için işler durdurulur ve yeniden başlatılmez ve iş durumu Durdurulur olur.

Uzun süren görevler için Karma Runbook Worker kullanılması önerilir. Karma Runbook Çalışanları adil payla sınırlı değildir ve bir runbook'un ne kadar süreyle çalıştırabileceğiyle ilgili bir sınırlamayoktur. Diğer iş [sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) hem Azure ve Karma Runbook Çalışanları için geçerlidir. Karma Runbook Çalışanları 3 saatlik adil paylaşım sınırıyla sınırlı olmasa da, beklenmeyen yerel altyapı sorunlarından yeniden başlatılmasını destekleyen işçiler üzerinde çalışacak runbook'lar geliştirmeniz gerekir.

Başka bir seçenek, alt runbook'ları kullanarak bir runbook'u optimize etmektir. Örneğin, runbook'unuz, çeşitli veritabanlarındaki veritabanı işlemi gibi çeşitli kaynaklarda aynı işlev arasında döngü kurabilir. Bu işlevi bir [alt çalışma kitabına](automation-child-runbooks.md) taşıyabilir ve runbook'unuzun [Başlat-AzAutomationRunbook'u](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0)kullanarak aramasını sağlayabilirsiniz. Alt runbooks ayrı işlemlerde paralel olarak yürütülür.

Alt runbook'ların kullanılması, üst çalışma kitabının tamamlanması için gereken toplam süreyi azaltır. Runbook'unuz, çocuk tamamlandıktan sonra daha fazla işlem varsa, bir çocuk runbook'un iş durumunu kontrol etmek için [Get-AzAutomationJob](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationjob?view=azps-3.7.0) cmdlet'ini kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook ile nasıl çalışacağımı öğrenmek için [Azure Otomasyonunda runbook'ları Yönet'e](manage-runbooks.md)bakın.
* Azure Otomasyonu'nda bir runbook başlatmak için kullanılabilecek yöntemler hakkında daha fazla bilgi edinmek için azure [otomasyonunda runbook başlatma](automation-starting-a-runbook.md)'ya bakın.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
