---
title: Azure Otomasyonu 'nda runbook yürütmesi
description: Azure Otomasyonu 'ndaki bir runbook 'un nasıl işlendiği hakkındaki ayrıntıları açıklar.
services: automation
ms.subservice: process-automation
ms.date: 04/04/2019
ms.topic: conceptual
ms.openlocfilehash: 6a51e57bd2411c19dfd5e7740f9e918d0bd09e27
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79278655"
---
# <a name="runbook-execution-in-azure-automation"></a>Azure Otomasyonu 'nda runbook yürütmesi

Runbook 'lar, içinde tanımlanan mantığa göre yürütülür. Bir runbook kesintiye uğrarsa, runbook başlangıcında yeniden başlatılır. Bu davranış, geçici sorunlar oluşursa yeniden başlatılmasını destekleyen runbook 'ları yazmanızı gerektirir.

Azure Otomasyonu 'nda runbook başlatıldığında bir iş oluşturulur. Bir iş, runbook 'un tek bir yürütme örneğidir. Her iş Azure aboneliğinize bir bağlantı oluşturarak Azure kaynaklarına erişebilir. İş, yalnızca bu kaynaklara genel buluttan erişilebiliyorsa, veri merkezinizdeki kaynaklara erişebilir.

Azure Otomasyonu, runbook yürütmesi sırasında her bir işi çalıştırmak için bir çalışan atar. Çalışanlar birçok Azure hesabı tarafından paylaşıldığında, farklı Otomasyon hesaplarından gerçekleştirilen işler diğerinden yalıtılmıştır. İşiniz için gereken çalışan hizmetleri üzerinde denetiminiz yok.

Azure portal runbook 'ların listesini görüntülediğinizde, her runbook için başlatılmış her bir işin durumunu gösterir. Azure Otomasyonu iş günlüklerini en fazla 30 gün boyunca depolar. 

Aşağıdaki diyagramda [PowerShell runbook](automation-runbook-types.md#powershell-runbooks)'ları, [grafik runbook](automation-runbook-types.md#graphical-runbooks)'Lar ve [PowerShell iş akışı runbook 'ları](automation-runbook-types.md#powershell-workflow-runbooks)için bir runbook işinin yaşam döngüsü gösterilmektedir.

![İş durumları-PowerShell Iş akışı](./media/automation-runbook-execution/job-statuses.png)

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="where-to-run-your-runbooks"></a>Runbook 'larınızın çalıştırılacağı konum

Azure Otomasyonu 'ndaki runbook 'lar, bir Azure korumalı alanı veya [karma runbook çalışanı](automation-hybrid-runbook-worker.md)üzerinde çalışabilir. Birçok runbook, birden çok işin kullanabileceği paylaşılan bir ortam olan bir Azure korumalı alanı içinde kolayca çalıştırılabilir. Aynı korumalı alanı kullanan işler, korumalı alanın kaynak sınırlamalarına göre bağlanır.

Runbook 'ları doğrudan rolü barındıran bilgisayarda ve ortamdaki yerel kaynaklara karşı çalıştırmak için karma Runbook Worker 'ı kullanabilirsiniz. Azure Otomasyonu runbook 'ları depolar ve yönetir ve sonra bunları bir veya daha fazla atanmış bilgisayara gönderir.

Aşağıdaki tabloda, her biri için önerilen yürütme ortamıyla bazı runbook yürütme görevleri listelenmiştir.

|Görev|En iyi seçim|Notlar|
|---|---|---|
|Azure kaynaklarıyla tümleştirme|Azure korumalı alanı|Azure 'da barındırılan kimlik doğrulama daha basittir. Azure VM 'de karma Runbook Worker kullanıyorsanız [Azure kaynakları için Yönetilen kimlikler](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)kullanabilirsiniz.|
|Azure kaynaklarını yönetmek için en iyi performansı alma|Azure korumalı alanı|Betiği aynı ortamda çalışır ve bu da daha az gecikme süresine sahiptir.|
|İşlem maliyetlerini en aza indir|Azure korumalı alanı|İşlem yükü yoktur ve VM 'ye gerek kalmaz.|
|Uzun süre çalışan betiği yürütme|Karma Runbook Çalışanı|Azure korumalı [kaynakları, kaynaklarla ilgili sınırlamalara](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)sahiptir.|
|Yerel hizmetlerle etkileşim kurma|Karma Runbook Çalışanı|, Doğrudan konak makinesine erişebilir.|
|Üçüncü taraf yazılım ve yürütülebilir dosyaları gerektir|Karma Runbook Çalışanı|İşletim sistemini yönetirsiniz ve yazılım yükleyebilirsiniz.|
|Runbook ile bir dosyayı veya klasörü izleme|Karma Runbook Çalışanı|Karma Runbook Worker üzerinde bir [izleyici görevi](automation-watchers-tutorial.md) kullanın.|
|Kaynak yoğunluklu bir betiği çalıştırma|Karma Runbook Çalışanı| Azure korumalı [kaynakları, kaynaklarla ilgili sınırlamalara](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)sahiptir.|
|Belirli gereksinimlere sahip modüller kullanın| Karma Runbook Çalışanı|Bazı örnekler şunlardır:</br> WinSCP-WinSCP. exe ' de bağımlılık </br> Iısadministration-IIS etkinleştirme bağımlılığı.|
|Bir yükleyiciye modül yükleme|Karma Runbook Çalışanı|Korumalı alan için modüllerin kopyalamayı desteklemesi gerekir.|
|4\.7.2 'ten farklı .NET Framework sürümü gerektiren runbook 'ları veya modülleri kullanın|Karma Runbook Çalışanı|Automation korumalı alanlar .NET Framework 4.7.2 sahiptir ve bunu yükseltmenin bir yolu yoktur.|
|Yükseltme gerektiren betikleri Çalıştır|Karma Runbook Çalışanı|Korumalı alanlar yükseltmeye izin vermez. Karma Runbook Worker ile UAC 'yi kapatabilir ve yükseltme gerektiren komutu çalıştırırken **Invoke-komutunu** kullanabilirsiniz.|
|WMI erişimi gerektiren betikleri Çalıştır|Karma Runbook Çalışanı|Bulutta korumalı alanlar üzerinde çalışan işlerin WMI erişimi yoktur. |

## <a name="runbook-behavior"></a>Runbook davranışı

### <a name="creating-resources"></a>Kaynak oluşturma

Runbook 'ağınız bir kaynak oluşturursa, betiği oluşturmayı denemeden önce kaynağın zaten mevcut olup olmadığını kontrol etmelidir. Temel bir örnek aşağıda verilmiştir.

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

### <a name="supporting-time-dependent-scripts"></a>Zamana bağlı betikleri destekleme

Runbook 'larınız sağlam ve yeniden başlatılmasına veya başarısız olmasına neden olabilecek geçici hataları işleyebilme yeteneğine sahip olmalıdır. Bir runbook başarısız olursa, Azure Otomasyonu bunu yeniden dener.

Runbook 'larınız normalde bir zaman kısıtlaması içinde çalışıyorsa, yürütme süresini denetlemek için komut dosyasının Logic uygular. Bu denetim, yalnızca belirli zamanlarda başlatma, kapatma veya genişleme gibi işlemlerin çalıştırılmasını sağlar.

> [!NOTE]
> Azure korumalı alan işlemindeki yerel saat UTC olarak ayarlanır. Runbook 'larınızda tarih ve saat hesaplamaları bu olguyu göz önünde bulundurmanız gerekir.

### <a name="tracking-progress"></a>İlerlemeyi izleme

Runbook 'larınızın daha kolay yeniden kullanılabilmesi ve yeniden başlatılması için Runbook 'ların yapısını yapılandırırken, runbook 'ların modüler olmasını sağlamak iyi bir uygulamadır. Bir runbook 'ta ilerlemeyi izlemek, sorunlar varsa runbook mantığının doğru şekilde yürütüldüğünü sağlamak için iyi bir yoldur. Bir runbook 'un ilerlemesini depolama hesabı, veritabanı veya paylaşılan dosyalar gibi bir dış kaynak kullanarak izlemek mümkündür. Runbook 'inizdeki mantığı, ilk olarak gerçekleştirilen son eylemin durumunu denetlemek için oluşturabilirsiniz. Ardından, Denetim sonucuna bağlı olarak, mantıksal runbook 'taki belirli görevleri atlayabilir veya devam edebilir.

### <a name="preventing-concurrent-jobs"></a>Eş zamanlı işlerin önlenmesi

Bazı runbook 'lar aynı anda birden çok iş üzerinde çalışıyorsa Strangely davranır. Bu durumda, bir runbook 'un zaten çalışmakta olan bir işin olup olmadığını belirlemede Logic uygulaması için önem taşır. Temel bir örnek aşağıda verilmiştir.

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

Birden çok abonelikle başa çıkmak için Runbook 'un, kimlik doğrulama bağlamının aynı korumalı alanda çalışan başka bir runbook 'tan alınmadığından emin olmak için [Disable-AzContextAutosave](https://docs.microsoft.com/powershell/module/Az.Accounts/Disable-AzContextAutosave?view=azps-3.5.0) cmdlet 'ini kullanması gerekir. Runbook, az Module cmdlet 'lerinde *azcontext* parametresini de kullanır ve uygun bağlamı geçirir.

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

Bu bölümde, runbook 'larınızda özel durumları veya aralıklı sorunları işlemenin bazı yolları açıklanmaktadır.

#### <a name="erroractionpreference"></a>ErrorActionPreference

[ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) değişkeni, PowerShell 'in Sonlandırıcı olmayan bir hataya nasıl yanıt vereceğini belirler. Hataların sonlandırılması her zaman sonlandırılıyor ve *ErrorActionPreference*tarafından etkilenmiyor.

Runbook *ErrorActionPreference*kullandığında, **Get-ChildItem** cmdlet 'inden **PathNotFound** gibi Normalde Sonlandırılmamış bir hata runbook 'un tamamlanmasını durduruyor. Aşağıdaki örnek *ErrorActionPreference*kullanımını gösterir. Komut dosyası durdurulduğunda son **Write-output** komutu hiçbir şekilde yürütülmez.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-Childitem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Son catch 'i deneyin

[Catch, son](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) hataları Işlemek için PowerShell betiklerinizde kullanılır. Betik belirli özel durumları veya genel özel durumları yakalamak için bu mekanizmayı kullanabilir. **Catch** ifadesinin hataları izlemek veya işlemek için kullanılması gerekir. Aşağıdaki örnek varolmayan bir dosyayı indirmeye çalışır. System .net. WebException özel durumunu yakalar ve diğer tüm özel durumlar için son değeri döndürür.

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

#### <a name="throw"></a>Yaratır

[Throw](/powershell/module/microsoft.powershell.core/about/about_throw) , sonlandırma hatası oluşturmak için kullanılabilir. Bu mekanizma, bir runbook 'ta kendi mantığınızı tanımlarken yararlı olabilir. Betik tarafından durdurulması gereken bir ölçüt karşılıyorsa, durdurmak için **throw** ifadesini kullanabilir. Aşağıdaki örnek, gerekli bir işlev parametresini göstermek için bu ifadeyi kullanır.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>Yürütülebilir dosyaları veya çağırma süreçlerini kullanma

Azure korumalı alanları 'nda çalışan runbook 'lar, yürütülebilir dosyalar ( **. exe** dosyaları) veya alt süreçler gibi işlemlerin çağrılmasını desteklemez.  Bunun nedeni, Azure korumalı alanının tüm temel API 'lere erişemeyebilir bir kapsayıcıda çalıştırılan paylaşılan bir işlemdir. Üçüncü taraf yazılım veya alt işlemlere yönelik çağrılar gerektiren senaryolar için, bir runbook 'u [karma Runbook Worker](automation-hybrid-runbook-worker.md)üzerinde yürütmeniz önerilir.

### <a name="accessing-device-and-application-characteristics"></a>Cihaza ve uygulama özelliklerine erişme

Azure korumalı alanında çalışan runbook işlerinin hiçbir cihaza veya uygulama özelliklerine erişimi yoktur. Windows üzerinde performans ölçümlerini sorgulamak için kullanılan en yaygın API, bazı yaygın ölçülerden bellek ve CPU kullanımı ile WMI 'dır. Ancak, bulutta çalışan işlerin Web tabanlı kuruluş yönetimi 'nin (WBEM) Microsoft uygulamasına erişimi olmadığı için API 'nin kullanılması önemlidir. Bu platform, cihaz ve uygulama özelliklerini tanımlamaya yönelik sektör standartları sağlayan Genel Bilgi Modeli (CıM) üzerine kurulmuştur.

## <a name="handling-errors"></a>Hataları işleme

Runbook 'larınızın hata işleme yeteneğine sahip olması gerekir. PowerShell 'de iki tür hata vardır, sonlandırılıyor ve Sonlandırılmamış. Hataları sonlandırmak, meydana geldiğinde runbook yürütmesini durdurur. Runbook, iş durumu **başarısız**olarak durduruluyor.

Sonlandırılmamış hatalar, bir betiğin gerçekleştikten sonra bile devam etmesine izin verir. Bir runbook, **Get-ChildItem** cmdlet 'ini mevcut olmayan bir yol ile kullandığında, Sonlandırıcı olmayan bir hata örneği oluşur. PowerShell yolun mevcut olmadığını, bir hata olduğunu ve sonraki klasöre devam ettiğini görür. Bu durumda hata runbook iş durumu durumunu **başarısız**olarak ayarlanmamış ve iş da tamamlanmış olabilir. Bir runbook 'u Sonlandırıcı olmayan bir hata üzerinde durdurmaya zorlamak için cmdlet üzerinde `-ErrorAction Stop` kullanabilirsiniz.

## <a name="handling-jobs"></a>İşleri işleme

Aynı Otomasyon hesabındaki işlerin yürütme ortamını yeniden kullanabilirsiniz. Tek bir runbook 'un tek seferde çalışan çok sayıda işi olabilir. Aynı anda çalıştırdığınız daha fazla iş, aynı korumalı alana daha fazla gönderilebilir.

Aynı korumalı alan işleminde çalışan işler birbirini etkileyebilir. Bir örnek, **Disconnect-AzAccount** cmdlet 'ini çalıştırıyor. Bu cmdlet 'in yürütülmesi, paylaşılan korumalı alan işlemindeki her runbook işinin bağlantısını keser.

Azure korumalı alanında çalışan bir runbook 'tan başlatılan PowerShell işleri, tam dil modunda çalışmayabilir. PowerShell dil modları hakkında daha fazla bilgi için bkz. [PowerShell dil modları](/powershell/module/microsoft.powershell.core/about/about_language_modes). Azure Otomasyonu 'nda işlerle etkileşim kurma hakkında daha fazla bilgi için bkz. [PowerShell ile iş durumunu alma](#retrieving-job-status-using-powershell).

### <a name="job-statuses"></a>İş durumları

Aşağıdaki tabloda, bir iş için olası durumlar açıklanmaktadır.

| Durum | Açıklama |
|:--- |:--- |
| Tamamlandı |İş başarıyla tamamlandı. |
| Başarısız |Grafik veya PowerShell Iş akışı runbook 'u derlenemedi. PowerShell betiği runbook 'u başlatılamadı veya iş bir özel duruma sahipti. Bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md).|
| Başarısız, kaynak bekleniyor |İş başarısız oldu çünkü bu, [dengeli](#fair-share) bir şekilde, her seferinde ve aynı denetim noktasından ya da runbook 'un başlangıcından her seferinde başlatılmış |
| Kuyruğa alındı |İş, bir Otomasyon çalışanındaki kaynakların başlatılabilmesini sağlamak için kullanılabilir hale gelmesini bekliyor. |
| Başlatılıyor |İş bir çalışana atandı ve sistem tarafından başlatılıyor. |
| Sürdürülüyor |Sistem askıya alındıktan sonra işi sürdürüyor. |
| Çalışıyor |İş çalışıyor. |
| Çalışıyor, kaynakları bekliyor |İş, dengeli bir paylaşımın sınırına ulaştığından kaldırıldı. Son denetim noktasından kısa bir süre içinde sürdürülecek. |
| Durduruldu |İş tamamlanmadan kullanıcı tarafından durduruldu. |
| Durduruluyor |Sistem işi durduruyor. |
| Askıya alındı |Yalnızca [grafik ve PowerShell Iş akışı runbook 'ları](automation-runbook-types.md) için geçerlidir. İş; kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Bir runbook 'ta bir denetim noktası yoksa, başlangıçtan başlar. Bir denetim noktası varsa, yeniden başlayabilir ve son denetim noktasından sürdürebilirsiniz. Sistem yalnızca bir özel durum oluştuğunda runbook 'u askıya alır. Varsayılan olarak, *ErrorActionPreference* değişkeni **devam**olarak ayarlanır ve bu, işin bir hata üzerinde çalışmaya devam kaldığını gösterir. Tercih değişkeni **stop**olarak ayarlandıysa, iş bir hata üzerinde askıya alınır.  |
| Askıya alınıyor |Yalnızca [grafik ve PowerShell Iş akışı runbook 'ları](automation-runbook-types.md) için geçerlidir. Sistem, kullanıcının isteği sırasında işi askıya almaya çalışıyor. Runbook, askıya alınmadan önce bir sonraki denetim noktasına ulaşmalıdır. Son denetim noktasını zaten geçtiyse, askıya alınmadan önce tamamlanır. |

### <a name="viewing-job-status-from-the-azure-portal"></a>Azure portal iş durumunu görüntüleme

Tüm runbook işlerinin özetlenen durumunu görüntüleyebilir veya Azure portal belirli bir runbook işinin ayrıntılarına gidebilirsiniz. Ayrıca, runbook iş durumu ve iş akışlarını iletmek için Log Analytics çalışma alanınız ile tümleştirmeyi yapılandırabilirsiniz. Azure Izleyici günlükleriyle tümleştirme hakkında daha fazla bilgi için bkz. [Otomasyon 'Dan Azure izleyici günlüklerine iş durumu ve iş akışları iletme](automation-manage-send-joblogs-log-analytics.md).

Seçtiğiniz Otomasyon hesabınızın sağında, **Iş istatistikleri** kutucuğu altında tüm runbook işlerinin özetini görebilirsiniz.

![İş Istatistikleri kutucuğu](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Bu kutucuk, yürütülen her iş için iş durumunun bir sayısını ve grafik temsilini görüntüler.

Kutucuğa tıkladığınızda, yürütülen tüm işlerin özetlenen listesini içeren **işler** sayfası sunulmaktadır. Bu sayfada her iş için durum, Runbook adı, başlangıç saati ve tamamlanma zamanı gösterilir.

![Otomasyon hesabı Işleri sayfası](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

İş listesini **filtre işleri**' ni seçerek filtreleyebilirsiniz. Belirli bir runbook 'u, iş durumunu veya açılan listeden bir seçimi filtreleyin ve arama için zaman aralığını belirtin.

![İş durumunu filtrele](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternatif olarak, Otomasyon hesabınızdaki runbook **'lar** sayfasından söz konusu runbook 'u seçerek ve sonra **işler** kutucuğunu seçerek belirli bir runbook için iş Özeti ayrıntılarını görüntüleyebilirsiniz. Bu eylem **işler** sayfasını gösterir. Buradan, ayrıntılarını ve çıktısını görüntülemek için iş kaydına tıklayabilirsiniz.

![Otomasyon hesabı Işleri sayfası](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="viewing-the-job-summary"></a>İş özetini görüntüleme

Yukarıda açıklanan iş Özeti, belirli bir runbook için oluşturulmuş tüm işlerin ve en son durumunun bir listesini görmenizi sağlar. Bir işin ayrıntılı bilgilerini ve çıktısını görmek için listedeki adına tıklayın. İşin ayrıntılı görünümü, bu iş için sağlanmış olan runbook parametrelerinin değerlerini içerir.

Bir runbook işlerini görüntülemek için aşağıdaki adımları kullanabilirsiniz.

1. Azure portal **Otomasyon** ' u seçin ve ardından bir Otomasyon hesabının adını seçin.
2. Hub 'dan, **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.
3. **Runbook 'lar** sayfasında, listeden bir runbook seçin.
3. Seçili runbook 'un sayfasında **işler** kutucuğuna tıklayın.
4. Listedeki işlerden birine tıklayın ve runbook iş ayrıntıları sayfasında ayrıntılarını ve çıktısını görüntüleyin.

### <a name="retrieving-job-status-using-powershell"></a>PowerShell kullanarak iş durumunu alma

Bir runbook için oluşturulan işleri ve belirli bir işin ayrıntılarını almak için **Get-AzAutomationJob** cmdlet 'ini kullanın. **Start-AzAutomationRunbook**kullanarak PowerShell ile bir runbook başlatırsanız, sonuçta elde edilen işi döndürür. İş çıktısını almak için [Get-Azautomationjoi put](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0) komutunu kullanın.

Aşağıdaki örnek, örnek bir runbook 'un son işini alır ve durumunu, runbook parametreleri için belirtilen değerleri ve iş çıktısını görüntüler.

```azurepowershell-interactive
$job = (Get-AzAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Aşağıdaki örnek, belirli bir işin çıkışını alır ve her bir kaydı döndürür. Kayıtlardan biri için bir özel durum varsa, komut dosyası değeri yerine özel durumu yazar. Bu davranış yararlı olur, çünkü özel durumlar çıkış sırasında normalde günlüğe kaydedilmiş olabilecek ek bilgiler sağlayabilir.

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

## <a name="getting-details-from-the-activity-log"></a>Etkinlik günlüğünden Ayrıntılar alma

Otomasyon hesabı için etkinlik günlüğünden runbook 'u başlatan kişi veya hesap gibi runbook ayrıntılarını alabilirsiniz. Aşağıdaki PowerShell örneği, son kullanıcının belirtilen runbook 'u çalıştırmasını sağlar.

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

## <a name="fair-share"></a>Runbook 'lar arasında kaynakları paylaşma

Azure Otomasyonu, bulutta bulunan tüm runbook 'lar arasında kaynak paylaşmak için, üç saatten uzun bir süreyle çalışan işleri geçici olarak kaldırır veya sonlandırır. [PowerShell runbook 'ları](automation-runbook-types.md#powershell-runbooks) ve [Python runbook 'ların](automation-runbook-types.md#python-runbooks) işleri durdurulur ve yeniden başlatılmaz ve iş durumu **durdurulur**.

Uzun süre çalışan görevler için karma Runbook Worker kullanılması önerilir. Karma runbook çalışanları, dengeli bir paylaşımdan sınırlı değildir ve bir runbook 'un ne kadar süreyle yürütülemediği konusunda bir sınırlama yoktur. Diğer iş sınırları hem Azure sanal [değerleri](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) hem de karma runbook çalışanları için geçerlidir. Karma runbook çalışanları 3 saatlik dengeli bir paylaşılan sınır ile sınırlı olmasa da, beklenmedik yerel altyapı sorunlarından yeniden başlatmaları destekleyen çalışanlar üzerinde çalıştırılacak runbook 'lar geliştirmeniz gerekir.

Diğer bir seçenek de runbook 'u alt runbook 'ları kullanarak iyileştirmenize olanak sağlar. Örneğin, runbook 'unuzu birkaç veritabanında bir veritabanı işlemi gibi birkaç kaynak üzerinde aynı işlev aracılığıyla döngüye alabilir. Bu işlevi bir [alt runbook](automation-child-runbooks.md) 'a taşıyabilir ve Runbook 'unuzu **Start-azautomationrunbook**kullanarak çağırabilmeniz gerekir. Alt runbook 'lar ayrı işlemlerde paralel olarak yürütülür.

Alt runbook 'ların kullanılması, üst runbook 'un tamamlanacağı toplam süreyi düşürür. Runbook 'seniz, alt runbook için iş durumunu denetlemek üzere **Get-AzAutomationJob** cmdlet 'ini kullanarak alt runbook 'un devam ettiği işlemler varsa, bu işlem için iş durumu kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu 'nda bir runbook başlatmak için kullanılabilecek yöntemler hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'nda runbook başlatma](automation-starting-a-runbook.md).
* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/overview)bakın.
