---
title: Azure Otomasyonu’nda runbook yürütme
description: Azure Otomasyonu 'ndaki bir runbook 'un nasıl işlendiği hakkındaki ayrıntıları açıklar.
services: automation
ms.subservice: process-automation
ms.date: 04/14/2020
ms.topic: conceptual
ms.openlocfilehash: ace701a6c19f4fe3af1c9ae6f5e63097dd59d405
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871681"
---
# <a name="runbook-execution-in-azure-automation"></a>Azure Otomasyonu’nda runbook yürütme

Azure Otomasyonu 'nda işlem otomasyonu, PowerShell, PowerShell Iş akışı ve grafik runbook 'ları oluşturmanıza ve yönetmenize olanak sağlar. Ayrıntılar için bkz. [Azure Otomasyonu runbook 'ları](automation-runbook-types.md). 

Otomasyon, runbook 'larınızı, içinde tanımlanan mantığa göre yürütür. Bir runbook kesintiye uğrarsa, başlangıçta yeniden başlatılır. Bu davranış, geçici sorunlar oluşursa yeniden başlatılmasını destekleyen runbook 'ları yazmanızı gerektirir.

Azure Otomasyonu 'nda runbook 'u başlatmak, runbook 'un tek bir yürütme örneği olan bir iş oluşturur. Her iş Azure aboneliğinize bir bağlantı oluşturarak Azure kaynaklarına erişir. İş, yalnızca bu kaynaklara genel buluttan erişilebiliyorsa, veri merkezinizdeki kaynaklara erişebilir.

Azure Otomasyonu, runbook yürütmesi sırasında her bir işi çalıştırmak için bir çalışan atar. Çalışanlar birçok Azure hesabı tarafından paylaşıldığında, farklı Otomasyon hesaplarından gerçekleştirilen işler diğerinden yalıtılmıştır. İşinizin hangi çalışan hizmetlerinden hangilerinin isteklerinize yönelik olduğunu kontrol edebilirsiniz.

Azure portal runbook 'ların listesini görüntülediğinizde, her runbook için başlatılmış her bir işin durumunu gösterir. Azure Otomasyonu iş günlüklerini en fazla 30 gün boyunca depolar.

Aşağıdaki diyagramda [PowerShell runbook 'ları](automation-runbook-types.md#powershell-runbooks), [PowerShell iş akışı runbook 'ları](automation-runbook-types.md#powershell-workflow-runbooks)ve [grafik runbook](automation-runbook-types.md#graphical-runbooks)'lar için bir runbook işinin yaşam döngüsü gösterilmektedir.

![İş durumları-PowerShell Iş akışı](./media/automation-runbook-execution/job-statuses.png)

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="runbook-execution-environment"></a>Runbook yürütme ortamı

Azure Otomasyonu 'ndaki runbook 'lar, bir Azure korumalı alanı veya [karma runbook çalışanı](automation-hybrid-runbook-worker.md)üzerinde çalışabilir. 

Runbook 'lar kimlik doğrulaması yapmak ve Azure 'daki kaynaklara karşı çalıştırmak için tasarlandığında, birden çok işin kullanabileceği paylaşılan bir ortam olan bir Azure korumalı alanı çalışır. Aynı korumalı alanı kullanan işler, korumalı alanın kaynak sınırlamalarına göre bağlanır. Azure korumalı alan ortamı etkileşimli işlemleri desteklemez. Tüm işlem dışı COM sunucularına erişimi engeller. Ayrıca, Win32 çağrıları yapan runbook 'lar için yerel MOF dosyalarının kullanılmasını gerektirir.

Ayrıca, runbook 'u doğrudan rolü barındıran bilgisayarda ve ortamdaki yerel kaynaklara karşı çalıştırmak için [karma Runbook Worker](automation-hybrid-runbook-worker.md) 'ı kullanabilirsiniz. Azure Otomasyonu runbook 'ları depolar ve yönetir ve sonra bunları bir veya daha fazla atanmış bilgisayara gönderir.

>[!NOTE]
>Linux karma runbook çalışanı üzerinde çalıştırmak için betiklerinizin imzalı ve çalışan tarafından uygun şekilde yapılandırılmış olması gerekir. Alternatif olarak, [imza doğrulamasının](automation-linux-hrw-install.md#turn-off-signature-validation)kapalı olması gerekir. 

Aşağıdaki tabloda, her biri için önerilen yürütme ortamıyla bazı runbook yürütme görevleri listelenmiştir.

|Görev|Öneri|Notlar|
|---|---|---|
|Azure kaynaklarıyla tümleştirme|Azure korumalı alanı|Azure 'da barındırılan kimlik doğrulama daha basittir. Azure VM 'de karma runbook çalışanı kullanıyorsanız, [yönetilen kimliklerle runbook kimlik doğrulamasını kullanabilirsiniz](automation-hrw-run-runbooks.md#runbook-auth-managed-identities).|
|Azure kaynaklarını yönetmek için en iyi performansı alma|Azure korumalı alanı|Betiği aynı ortamda çalışır ve bu da daha az gecikme süresine sahiptir.|
|İşlem maliyetlerini en aza indir|Azure korumalı alanı|İşlem yükü yoktur ve VM 'ye gerek kalmaz.|
|Uzun süre çalışan betiği yürütme|Karma Runbook Çalışanı|Azure korumalı alanlar [kaynak limitlere](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)sahiptir.|
|Yerel hizmetlerle etkileşim kurma|Karma Runbook Çalışanı|Konak makinesine veya diğer bulut ortamlarınızdaki kaynaklara veya şirket içi ortama doğrudan erişin. |
|Üçüncü taraf yazılım ve yürütülebilir dosyaları gerektir|Karma Runbook Çalışanı|İşletim sistemini yönetirsiniz ve yazılım yükleyebilirsiniz.|
|Runbook ile bir dosyayı veya klasörü izleme|Karma Runbook Çalışanı|Karma Runbook Worker üzerinde bir [izleyici görevi](automation-watchers-tutorial.md) kullanın.|
|Kaynak yoğunluklu bir betiği çalıştırma|Karma Runbook Çalışanı| Azure korumalı alanlar [kaynak limitlere](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)sahiptir.|
|Belirli gereksinimlere sahip modüller kullanın| Karma Runbook Çalışanı|Bazı örnekler şunlardır:</br> WinSCP-WinSCP. exe ' de bağımlılık </br> IIS yönetimi-IIS etkinleştirme veya yönetme ile ilgili bağımlılık|
|Bir yükleyiciye modül yükleme|Karma Runbook Çalışanı|Korumalı alan için modüllerin kopyalamayı desteklemesi gerekir.|
|4.7.2 'ten farklı .NET Framework sürümü gerektiren runbook 'ları veya modülleri kullanın|Karma Runbook Çalışanı|Azure korumalı alanlar, 4.7.2 .NET Framework destekler ve farklı bir sürüme yükseltme desteklenmez.|
|Yükseltme gerektiren betikleri Çalıştır|Karma Runbook Çalışanı|Korumalı alanlar yükseltmeye izin vermez. Karma Runbook Worker ile UAC 'yi kapatabilir ve yükseltme gerektiren komutu çalıştırırken [Invoke-komutunu](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7) kullanabilirsiniz.|
|Windows Yönetim Araçları erişmesi gereken betikleri çalıştırın (WMI)|Karma Runbook Çalışanı|Bulutta korumalı alanlar üzerinde çalışan işler WMI sağlayıcısına erişemez. |

## <a name="resources"></a>Kaynaklar

Runbook 'larınız, [kaynaklarla ilgili kaynakları](https://docs.microsoft.com/rest/api/resources/resources), örneğin VM 'leri, ağı ve ağ kaynaklarını ele almak için mantık içermelidir. Kaynaklar bir Azure aboneliğine bağlanır ve Runbook 'lar herhangi bir kaynağa erişmek için uygun kimlik bilgileri gerektirir. Bir runbook 'taki kaynakları işlemeye ilişkin bir örnek için bkz. [tanıtıcı kaynakları](manage-runbooks.md#handle-resources). 

## <a name="security"></a>Güvenlik

Azure Otomasyonu, [Azure Güvenlik Merkezi 'ni (ASC)](https://docs.microsoft.com/azure/security-center/security-center-introAzure) kullanarak kaynaklarınız için güvenlik sağlar ve Linux sistemlerinde tehlikeye atabilir. Kaynaklar Azure 'da olup olmadığı için iş yükleriniz genelinde güvenlik sağlanır. Bkz. [Azure Otomasyonu 'nda kimlik doğrulamaya giriş](automation-security-overview.md).

ASC, bir VM 'de imzalanmış veya imzasız herhangi bir komut dosyasını çalıştırabilen kullanıcılara yönelik kısıtlamaları. VM 'ye kök erişimi olan bir kullanıcı değilseniz, makineyi bir dijital imza ile açıkça yapılandırmanız veya kapatmanız gerekir. Aksi takdirde, yalnızca bir Otomasyon hesabı oluşturduktan ve uygun özelliği etkinleştirerek işletim sistemi güncelleştirmelerini uygulamak için bir komut dosyası çalıştırabilirsiniz.

## <a name="subscriptions"></a>Abonelikler

Azure [aboneliği](https://docs.microsoft.com/office365/enterprise/subscriptions-licenses-accounts-and-tenants-for-microsoft-cloud-offerings) , Microsoft 'un ücretlendirildiği bir veya daha fazla bulut tabanlı hizmeti kullanması için bir anlaşmadır. Azure Otomasyonu için her abonelik bir Azure Otomasyonu hesabına bağlanır ve hesapta [birden çok abonelik oluşturabilirsiniz](manage-runbooks.md#work-with-multiple-subscriptions) .

## <a name="credentials"></a>Kimlik Bilgileri

Bir runbook, Azure veya üçüncü taraf sistemler için herhangi bir kaynağa erişmek üzere uygun [kimlik bilgileri](shared-resources/credentials.md) gerektirir. Bu kimlik bilgileri Azure Otomasyonu, Key Vault vb. içinde depolanır.  

## <a name="azure-monitor"></a>Azure İzleyici

Azure Otomasyonu, makine işlemlerini izlemek için [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) 'yi kullanır. İşlemler, bir Log Analytics çalışma alanı ve [Log Analytics Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)gerektirir.

### <a name="log-analytics-agent-for-windows"></a>Windows için Log Analytics Aracısı

[Windows için Log Analytics Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windowsmonitor) , Windows VM 'leri ve fiziksel bilgisayarları yönetmek Için Azure izleyici ile birlikte çalışmaktadır. Makineler Azure 'da veya yerel bir veri merkezi gibi Azure dışı bir ortamda çalıştırılabilir. Aracıyı bir veya daha fazla Log Analytics çalışma alanına rapor verecek şekilde yapılandırmanız gerekir. 

>[!NOTE]
>Windows için Log Analytics Aracısı daha önce Microsoft Monitoring Agent (MMA) olarak biliniyordu.

### <a name="log-analytics-agent-for-linux"></a>Linux için Log Analytics Aracısı

[Linux için Log Analytics Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux) Windows için aracıya benzer bir şekilde çalışır, ancak Linux bilgisayarlarını Azure izleyici 'ye bağlar. Aracı, örneğin bir karma runbook çalışanında kök izinleri gerektiren komutların yürütülmesine izin veren bir **nxautomation** Kullanıcı hesabı ile yüklenir. **Nxautomation** hesabı, parola gerektirmeyen bir sistem hesabıdır. 

[Bir Linux karma Runbook Worker yüklemesi](automation-linux-hrw-install.md)sırasında karşılık gelen sudo izinleri olan **nxautomation** hesabı bulunmalıdır. Çalışanı yüklemeye çalışırsanız ve hesap yoksa veya uygun izinlere sahip değilse, yükleme başarısız olur.

Log Analytics Aracısı ve **nxautomation** hesabı için kullanılabilir Günlükler şunlardır:

* /var/seçenek/Microsoft/omsagent/log/omsagent.log-Log Analytics aracı günlüğü 
* /var/seçenek/Microsoft/omsagent/Run/automationworker/Worker.log-Automation çalışan günlüğü

>[!NOTE]
>**Nxautomation** kullanıcısı eklendi güncelleştirme yönetimi bir parçası olarak yalnızca imzalı runbook 'ları yürütür.

## <a name="runbook-permissions"></a>Runbook izinleri

Bir runbook 'un kimlik bilgileriyle Azure 'da kimlik doğrulaması için izinleri olması gerekir. Bkz. [Azure Otomasyonu farklı çalıştır hesaplarını yönetme](manage-runas-account.md). 

## <a name="modules"></a>Modül

Azure Otomasyonu, bazı Azurerd modülleri (azurerm. Automation) ve birkaç iç cmdlet içeren bir modül dahil olmak üzere çeşitli varsayılan modülleri destekler. Desteklenen, az modüller (az. Automation) dahil olmak üzere, şu anda Azurermmodules için tercih edilen olarak kullanılan yüklenebilir modüllerdir. Runbook 'larınız ve DSC yapılandırmalarında kullanılabilen modüllerin ayrıntıları için bkz. [Azure Automation 'da modülleri yönetme](shared-resources/modules.md).

## <a name="certificates"></a>Sertifikalar

Azure Otomasyonu, Azure 'da kimlik doğrulaması için [sertifikaları](shared-resources/certificates.md) kullanır veya bunları Azure ya da üçüncü taraf kaynaklarına ekler. Sertifikalar runbook 'ların ve DSC yapılandırmalarının erişimi için güvenli bir şekilde depolanır. 

Runbook 'larınız, bir sertifika yetkilisi (CA) tarafından imzalanmamış otomatik olarak imzalanan sertifikalar kullanabilir. Bkz. [Yeni sertifika oluşturma](shared-resources/certificates.md#create-a-new-certificate).

## <a name="jobs"></a>İşler

Azure Otomasyonu, işleri aynı Otomasyon hesabından çalıştırmak için bir ortamı destekler. Tek bir runbook 'un tek seferde çalışan çok sayıda işi olabilir. Aynı anda çalıştırdığınız daha fazla iş, aynı korumalı alana daha fazla gönderilebilir. 

Aynı korumalı alan işleminde çalışan işler birbirini etkileyebilir. Bir örnek, [Disconnect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/disconnect-azaccount?view=azps-3.7.0) cmdlet 'ini çalıştırıyor. Bu cmdlet 'in yürütülmesi, paylaşılan korumalı alan işlemindeki her runbook işinin bağlantısını keser. Bu senaryoya ilişkin bir örnek için bkz. [eşzamanlı Işleri engelleme](manage-runbooks.md#prevent-concurrent-jobs).

>[!NOTE]
>Azure korumalı alanında çalışan bir runbook 'tan başlatılan PowerShell işleri tam [PowerShell dil modunda](/powershell/module/microsoft.powershell.core/about/about_language_modes)çalışmayabilir. 

### <a name="job-statuses"></a>İş durumları

Aşağıdaki tabloda, bir iş için olası durumlar açıklanmaktadır. Tüm runbook işlerinin durum özetini görüntüleyebilir veya Azure portal belirli bir runbook işinin ayrıntılarına gidebilirsiniz. Ayrıca, runbook iş durumu ve iş akışlarını iletmek için Log Analytics çalışma alanınız ile tümleştirmeyi yapılandırabilirsiniz. Azure Izleyici günlükleriyle tümleştirme hakkında daha fazla bilgi için bkz. [Otomasyon 'Dan Azure izleyici günlüklerine iş durumu ve iş akışları iletme](automation-manage-send-joblogs-log-analytics.md). Ayrıca bkz. bir runbook 'ta durumlar ile çalışma örneği için [iş durumları alma](manage-runbooks.md#obtain-job-statuses) .

| Durum | Açıklama |
|:--- |:--- |
| Tamamlandı |İş başarıyla tamamlandı. |
| Başarısız |Grafik veya PowerShell Iş akışı runbook 'u derlenemedi. PowerShell runbook 'u başlatılamadı veya iş bir özel duruma sahipti. Bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md).|
| Başarısız, kaynak bekleniyor |İş başarısız oldu çünkü bu, [dengeli](#fair-share) bir şekilde, her seferinde ve aynı denetim noktasından ya da runbook 'un başlangıcından her seferinde başlatılmış |
| Kuyruğa alındı |İş, bir Otomasyon çalışanındaki kaynakların başlatılabilmesini sağlamak için kullanılabilir hale gelmesini bekliyor. |
| Sürdürülüyor |Sistem askıya alındıktan sonra işi sürdürüyor. |
| Çalışıyor |İş çalışıyor. |
| Çalışıyor, kaynakları bekliyor |İş, dengeli bir paylaşımın sınırına ulaştığından kaldırıldı. Son denetim noktasından kısa bir süre içinde sürdürülecek. |
| Başlatılıyor |İş bir çalışana atandı ve sistem tarafından başlatılıyor. |
| Durduruldu |İş tamamlanmadan kullanıcı tarafından durduruldu. |
| Durduruluyor |Sistem işi durduruyor. |
| Askıya alındı |Yalnızca [grafik ve PowerShell Iş akışı runbook 'ları](automation-runbook-types.md) için geçerlidir. İş; kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Bir runbook 'ta bir denetim noktası yoksa, başlangıçtan başlar. Bir denetim noktası varsa, yeniden başlayabilir ve son denetim noktasından sürdürebilirsiniz. Sistem yalnızca bir özel durum oluştuğunda runbook 'u askıya alır. Varsayılan olarak, `ErrorActionPreference` değişken devam olarak ayarlanır ve bu, işin bir hata üzerinde çalışmaya devam kaldığını gösterir. Tercih değişkeni stop olarak ayarlandıysa, iş bir hata üzerinde askıya alınır.  |
| Askıya alınıyor |Yalnızca [grafik ve PowerShell Iş akışı runbook 'ları](automation-runbook-types.md) için geçerlidir. Sistem, kullanıcının isteği sırasında işi askıya almaya çalışıyor. Runbook, askıya alınmadan sonraki denetim noktasına erişmelidir. Son denetim noktasını zaten geçtiyse, askıya alınmadan önce tamamlanır. |

## <a name="activity-logging"></a>Etkinlik günlüğü

Azure Otomasyonu 'nda runbook 'ların yürütülmesi Otomasyon hesabı için bir etkinlik günlüğündeki ayrıntıları yazar. Günlüğü kullanma hakkında ayrıntılı bilgi için bkz. [etkinlik günlüğünden ayrıntıları alma](manage-runbooks.md#retrieve-details-from-activity-log). 

## <a name="exceptions"></a>Özel durumlar

Bu bölümde, runbook 'larınızda özel durumları veya aralıklı sorunları işlemenin bazı yolları açıklanmaktadır. Bir WebSocket özel durumu örnek olarak kullanılır. Doğru özel durum işleme, geçici ağ hatalarının runbook 'larınızın başarısız olmasına neden olmasını engeller. 

### <a name="erroractionpreference"></a>ErrorActionPreference

[ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) değişkeni, PowerShell 'in Sonlandırıcı olmayan bir hataya nasıl yanıt vereceğini belirler. Sonlandırma hataları her zaman sonlandırılır ve bundan etkilenmez `ErrorActionPreference`.

Runbook kullandığında `ErrorActionPreference`, `PathNotFound` [Get-ChildItem](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7) cmdlet 'inden olduğu gibi Normalde Sonlandırılmamış bir hata runbook 'un tamamlanmasını durduruyor. Aşağıdaki örnek öğesinin `ErrorActionPreference`kullanımını gösterir. Komut dosyası durdurulduğunda son [Write-output](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7) komutu hiçbir şekilde yürütülmez.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-ChildItem -path nofile.txt
Write-Output "This message will not show"
```

### <a name="try-catch-finally"></a>Son catch 'i deneyin

[Catch, son](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) hataları Işlemek için PowerShell betiklerinizde kullanılır. Betik belirli özel durumları veya genel özel durumları yakalamak için bu mekanizmayı kullanabilir. `catch` Deyimleri izlemek veya hataları işlemeye çalışmak için kullanılmalıdır. Aşağıdaki örnek varolmayan bir dosyayı indirmeye çalışır. `System.Net.WebException` Özel durumu yakalar ve diğer tüm özel durumlar için son değeri döndürür.

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

### <a name="throw"></a>Throw

[Throw](/powershell/module/microsoft.powershell.core/about/about_throw) , sonlandırma hatası oluşturmak için kullanılabilir. Bu mekanizma, bir runbook 'ta kendi mantığınızı tanımlarken yararlı olabilir. Betik tarafından durdurulması gereken bir ölçüt karşılıyorsa, durdurmak için `throw` ifadesini kullanabilir. Aşağıdaki örnek, gerekli bir işlev parametresini göstermek için bu ifadeyi kullanır.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

## <a name="errors"></a>Hatalar

Runbook 'larınızın hataları işlemesi gerekir. Azure Otomasyonu iki tür PowerShell hatasını destekler, sonlandırıp sonlandırmasız değildir. 

Hataları sonlandırmak, meydana geldiğinde runbook yürütmesini durdurur. Runbook, iş durumu başarısız olarak durduruluyor.

Sonlandırılmamış hatalar, bir betiğin gerçekleştikten sonra bile devam etmesine izin verir. Bir runbook `Get-ChildItem` cmdlet 'ini mevcut olmayan bir yol ile kullandığında, Sonlandırıcı olmayan hata bir örnek oluşur. PowerShell yolun mevcut olmadığını, bir hata olduğunu ve sonraki klasöre devam ettiğini görür. Bu durumda hata, runbook iş durumunu başarısız olarak ayarlanmamış ve iş da tamamlanmış olabilir. Bir runbook 'u Sonlandırıcı olmayan bir hata üzerinde durmaya zorlamak için cmdlet 'inde kullanabilirsiniz `ErrorAction Stop` .

## <a name="calling-processes"></a>İşlem çağırma

Azure korumalı alanında çalışan runbook 'lar, yürütülebilir dosyalar (**. exe** dosyaları) veya alt süreçler gibi çağrı süreçlerini desteklemez. Bunun nedeni, Azure korumalı alanının, tüm temel API 'Lere erişemeyebilir bir kapsayıcıda çalıştırılan paylaşılan bir işlemdir. Üçüncü taraf yazılım veya alt işlemlere yönelik çağrılar gerektiren senaryolarda, bir runbook 'u [karma Runbook Worker](automation-hybrid-runbook-worker.md)üzerinde yürütmelisiniz.

## <a name="device-and-application-characteristics"></a>Cihaz ve uygulama özellikleri

Azure korumalı alanındaki runbook işleri hiçbir cihaza veya uygulama özelliklerine erişemez. Windows üzerinde performans ölçümlerini sorgulamak için kullanılan en yaygın API, bazı yaygın ölçülerden bellek ve CPU kullanımı ile WMI 'dır. Ancak, bulutta çalışan işler web tabanlı kuruluş yönetimi 'nin (WBEM) Microsoft uygulamasına erişemediğinden, API 'nin hangi API 'nin kullanılması gerektiği konusunda değildir. Bu platform, cihaz ve uygulama özelliklerini tanımlamaya yönelik sektör standartları sağlayan Genel Bilgi Modeli (CıM) üzerine kurulmuştur.

## <a name="webhooks"></a>Web Kancaları

Dış hizmetler, örneğin Azure DevOps Services ve GitHub, Azure Otomasyonu 'nda bir runbook başlatabilir. Bu tür bir başlangıç yapmak için hizmet tek bir HTTP isteği aracılığıyla bir [Web kancası](automation-webhooks.md) kullanır. Web kancasının kullanımı, runbook 'ların tam bir Azure Otomasyonu çözümü uygulanmaksızın başlatılmasını sağlar. 

## <a name="shared-resources"></a><a name="fair-share"></a>Paylaşılan kaynaklar

Azure, buluttaki tüm runbook 'lar arasında kaynak paylaşmak için, dengeli bir Share adlı bir kavram kullanır. Azure, orta payı kullanarak, üç saatten uzun bir süreyle çalışan işleri geçici olarak kaldırır veya sonlandırır. [PowerShell runbook 'ları](automation-runbook-types.md#powershell-runbooks) ve [Python runbook 'ların](automation-runbook-types.md#python-runbooks) işleri durdurulur ve yeniden başlatılmaz ve iş durumu durdurulur.

Uzun süre çalışan Azure Otomasyonu görevlerinde karma Runbook Worker kullanılması önerilir. Karma runbook çalışanları, dengeli bir paylaşımdan sınırlı değildir ve bir runbook 'un ne kadar süreyle yürütülemediği konusunda bir sınırlama yoktur. Diğer iş sınırları hem Azure sanal [değerleri](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) hem de karma runbook çalışanları için geçerlidir. Karma runbook çalışanları 3 saatlik dengeli bir paylaşılan sınır ile sınırlı olmasa da, beklenmedik yerel altyapı sorunlarından yeniden başlatmaları destekleyen çalışanlar üzerinde çalıştırılacak runbook 'lar geliştirmeniz gerekir.

Diğer bir seçenek de runbook 'u alt runbook 'ları kullanarak iyileştirmenize olanak sağlar. Örneğin, runbook 'unuzu birkaç kaynak üzerinde aynı işlev aracılığıyla (örneğin, birkaç veritabanında bir veritabanı işlemiyle) döngüye alabilir. Bu işlevi bir [alt runbook](automation-child-runbooks.md) 'a taşıyabilir ve Runbook 'unuzu [Start-azautomationrunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0)kullanarak çağırabilmeniz gerekir. Alt runbook 'lar ayrı işlemlerde paralel olarak yürütülür.

Alt runbook 'ların kullanılması, üst runbook 'un tamamlanacağı toplam süreyi düşürür. Runbook 'niz, alt runbook için iş durumunu denetlemek üzere [Get-AzAutomationJob](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationjob?view=azps-3.7.0) cmdlet 'ini kullanarak alt öğe tamamlandıktan sonra hala daha fazla işlem varsa.

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook ile çalışmaya başlamak için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md).
* Dil başvurusu ve öğrenme modülleri dahil olmak üzere PowerShell hakkında daha fazla bilgi için bkz. [PowerShell belgeleri](https://docs.microsoft.com/powershell/scripting/overview).
* PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
