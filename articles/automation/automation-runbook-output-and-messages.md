---
title: Azure Otomasyonu'nda runbook çıktısı ve iletileri
description: Azure Otomasyonu'nda runbook'lardan çıktı ve hata iletilerinin nasıl oluşturulup alınır, bunları açıklar.
services: automation
ms.subservice: process-automation
ms.date: 12/04/2018
ms.topic: conceptual
ms.openlocfilehash: 92b6378b00e12f618d07798b5ce789cbd9971544
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535545"
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Azure Otomasyonu'nda runbook çıktısı ve iletileri

Çoğu Azure Otomasyon runbook'unun bir tür çıktısı var. Bu çıktı, kullanıcıya gönderilen bir hata iletisi veya başka bir runbook ile kullanılması amaçlanan karmaşık bir nesne olabilir. Windows PowerShell, komut dosyasından veya iş akışından çıktı göndermek için [birden çok akış](/powershell/module/microsoft.powershell.core/about/about_redirection) sağlar. Azure Otomasyonu bu akışların her biriyle farklı şekilde çalışır. Bir runbook oluştururken akışları kullanmak için en iyi uygulamaları izlemeniz gerekir.

Aşağıdaki tablo, yayınlanan runbook'lar için Azure portalındaki ve [bir runbook'un sınanma](automation-testing-runbook.md)sırasındaki davranışlarıyla her akışı kısaca açıklar. Çıktı akışı, runbook'lar arasındaki iletişim için kullanılan ana akıştır. Diğer akışlar, kullanıcıya bilgi iletmek amacıyla ileti akışları olarak sınıflandırılır. 

| Akış | Açıklama | Yayımlanmış | Test etme |
|:--- |:--- |:--- |:--- |
| Hata |Kullanıcıya yönelik hata iletisi. Bir özel durum aksine, runbook varsayılan olarak bir hata iletisi sonra devam eder. |İş geçmişine yazılmış |Test çıktı bölmesinde görüntülenir |
| Hata ayıklama |Etkileşimli bir kullanıcıya yönelik iletiler. Runbooks kullanılmamalıdır. |İş geçmişine yazılmadı |Test çıktı bölmesinde görüntülenmez |
| Çıktı |Diğer runbook'lar tarafından kullanılması amaçlanan nesneler. |İş geçmişine yazılmış |Test çıktı bölmesinde görüntülenir |
| İlerleme durumu |Runbook'taki her etkinlikten önce ve sonra otomatik oluşturulan kayıtlar. Runbook, etkileşimli bir kullanıcı için tasarlandığından, kendi ilerleme kayıtlarını oluşturmaya çalışmamalıdır. |Çalışma defteri için ilerleme günlüğe kaydetme açıksa iş geçmişine yazılır |Test çıktı bölmesinde görüntülenmez |
| Ayrıntılı |Genel veya hata ayıklama bilgileri veren iletiler. |Yalnızca tam günlüğe kaydetme runbook için açıksa iş geçmişine yazılır |`VerbosePreference` Yalnızca değişken runbook'ta Devam edecek şekilde ayarlanmışsa Test çıktı bölmesinde görüntülenir |
| Uyarı |Kullanıcıya yönelik uyarı iletisi. |İş geçmişine yazılmış |Test çıktı bölmesinde görüntülenir |

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="output-stream"></a>Çıkış akışı

Çıktı akışı, bir komut dosyası veya iş akışı tarafından doğru çalıştığında oluşturulan nesnelerin çıktısı için kullanılır. Azure Otomasyonu öncelikle [geçerli runbook'u](automation-child-runbooks.md)arayan üst çalışma kitapları tarafından tüketilen nesneler için bu akışı kullanır. Bir üst öğe [çalışan defteri satır satır olarak aradığında,](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution)alt öğe Çıktı akışındaki verileri üst öğeye döndürür. 

Runbook'unuzun genel bilgileri istemciye iletmek için çıktı akışını kullanır, ancak başka bir runbook tarafından çağrılmazsa. Ancak en iyi yöntem olarak, siz runbook'lar genellikle genel bilgileri kullanıcıya iletmek için [Verbose akışını](#verbose-stream) kullanmalıdır.

Runbook'unuzun [Yazma-Çıktı'yı](https://technet.microsoft.com/library/hh849921.aspx)kullanarak Çıktı akışına veri yazmasını sağlar. Alternatif olarak, nesneyi komut dosyasına kendi satırına koyabilirsiniz.

```powershell
#The following lines both write an object to the output stream.
Write-Output –InputObject $object
$object
```

### <a name="handling-output-from-a-function"></a>Bir işlevden çıktı işleme

Bir runbook işlevi Çıktı akışına yazdığında, çıktı runbook'a geri aktarılır. Runbook bu çıktıyı bir değişkene atatırsa, çıktı çıktı akışına yazılmaz. İşlev içinden diğer akışlara yazma runbook için ilgili akış akışın asılarına yazar. Aşağıdaki örnek PowerShell İş Akışı runbook düşünün.

```powershell
Workflow Test-Runbook
{
  Write-Verbose "Verbose outside of function" -Verbose
  Write-Output "Output outside of function"
  $functionOutput = Test-Function
  $functionOutput

  Function Test-Function
  {
    Write-Verbose "Verbose inside of function" -Verbose
    Write-Output "Output inside of function"
  }
}
```

Runbook işi için Çıktı akışı:

```output
Output inside of function
Output outside of function
```

Runbook iş için Verbose akışı:

```output
Verbose outside of function
Verbose inside of function
```

Runbook'u yayımladıktan sonra ve başlatmadan önce, Verbose akışı çıktısını almak için runbook ayarlarında ayrıntılı günlüğe kaydetmeyi de açmanız gerekir.

### <a name="declaring-output-data-type"></a>Çıktı veri türünü bildirme

Çıktı veri türlerine örnekler aşağıda verilmiştir:

* `System.String`
* `System.Int32`
* `System.Collections.Hashtable`
* `Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine`

#### <a name="declare-output-data-type-in-a-workflow"></a>Çıktı veri türünü iş akışında bildirme

İş akışı, [OutputType özniteliğini](https://technet.microsoft.com/library/hh847785.aspx)kullanarak çıktısının veri türünü belirtir. Bu öznitelik çalışma süresi sırasında hiçbir etkisi yoktur, ancak runbook beklenen çıktıtasarım zamanında bir gösterge sağlar. Runbook'lar için araç kümesi gelişmeye devam ettikçe, tasarım zamanında çıktı veri türlerini bildirmenin önemi artar. Bu nedenle, oluşturduğunuz runbook'lara bu bildirimi eklemek en iyi yöntemdir.

Aşağıdaki örnek runbook bir dize nesnesi çıkışı yapar ve çıkış türü bildirimini içerir. Runbook'unuz belirli bir türde dizi çıkışı yapıyorsa, yine de o türdeki dizi yerine, türü belirtmelisiniz.

```powershell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

#### <a name="declare-output-data-type-in-a-graphical-runbook"></a>Çıktı veri türünü grafik çalışma kitabında bildirme

Grafik veya grafiksel PowerShell İş Akışı runbook'ta bir çıktı türünü bildirmek için **Giriş ve Çıkış** menüsü seçeneğini belirleyebilir ve çıktı türünü girebilirsiniz. Bir üst çalışma kitabı başvurulduğunda türü kolayca tanımlanabilir hale getirmek için tam .NET sınıf adının kullanılması önerilir. Tam adıkullanmak, sınıfın tüm özelliklerini runbook'taki veri veri busilere gösterir ve özellikler koşullu mantık, günlüğe kaydetme ve diğer runbook etkinlikleri için değer olarak başvuru için kullanıldığında esnekliği artırır.<br> ![Runbook Giriş ve Çıktı seçeneği](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

>[!NOTE]
>Giriş ve Çıktı özellikleri bölmesine **Çıktı Türü** alanına bir değer girdikten sonra, girişinizi tanıması için denetimin dışına tıkladığınızdan emin olun.

Aşağıdaki örnekte, Giriş ve Çıktı özelliğini göstermek için iki grafik çalışma kitabı gösterilmektedir. Modüler runbook tasarım modelini uygulayarak, Run As hesabını kullanarak Azure ile kimlik doğrulamasını yöneten Authenticate Runbook şablonu olarak bir runbook'unuz vardır. Normalde belirli bir senaryoyu otomatikleştirmek için temel mantığı gerçekleştiren ikinci runbook, bu durumda Authenticate Runbook şablonu yürütür. Sonuçları Test çıktı bölmenize gösterir. Normal koşullar altında, bu runbook çocuk runbook çıktı yararlanarak bir kaynağa karşı bir şey yapmak olurdu.

Burada **AuthenticateTo-Azure** runbook temel mantığı.<br> ![Runbook ŞablonU Örneğini](media/automation-runbook-output-and-messages/runbook-authentication-template.png)Doğrula.

Runbook, kimlik doğrulama `Microsoft.Azure.Commands.Profile.Models.PSAzureContext`profili özelliklerini döndüren çıktı türünü içerir.<br> ![Runbook Çıktı Türü Örneği](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png)

Bu runbook basit olsa da, burada aramak için bir yapılandırma öğesi vardır. Son etkinlik, parametre için PowerShell ifadesini kullanarak profil verilerini bir değişkene yazmak için `Write-Output` cmdlet'i `Inputobject` yürütür. Bu parametre için `Write-Output`gereklidir.

Bu örnekte **Test-ChildOutputType**adlı ikinci runbook, sadece iki etkinlik tanımlar.<br> ![Örnek Çocuk Çıktı Türü Runbook](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png)

İlk etkinlik **AuthenticateTo-Azure** runbook çağırır. İkinci etkinlik, `Write-Verbose` **Etkinlik çıktısına** **Veri kaynağı** kümesi ile cmdlet çalışır. Ayrıca, **Alan yolu** **Context.Subscription.SubscriptionName**, **AuthenticateTo-Azure** runbook bağlam çıktısı olarak ayarlanır.<br> ![Write-Verbose Cmdlet Parametre Veri Kaynağı](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)

Ortaya çıkan çıktı aboneliğin adıdır.<br> ![Test-ChildOutputType Runbook Sonuçları](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

## <a name="message-streams"></a>İleti akışları

Çıktı akışından farklı olarak, ileti akışları bilgileri kullanıcıya ileter. Farklı türde bilgiler için birden çok ileti akışı vardır ve Azure Otomasyonu her akışı farklı şekilde işler.

### <a name="warning-and-error-streams"></a>Uyarı ve hata akışları

Uyarı ve Hata, bir runbook'ta oluşan sorunları günlüğe aktarıyor. Azure Otomasyon, bir runbook çalıştırırken bu akışları iş geçmişine yazar. Otomasyon, bir runbook sınandığında Azure portalındaki Test çıktı bölmesindeki akışları içerir. 

Varsayılan olarak, bir runbook bir uyarı veya hata dan sonra yürütmeye devam eder. Runbook'unuzun iletiyi oluşturmadan önce bir [tercih değişkeni](#preference-variables) ayarlamasını sağlayarak bir uyarı veya hata üzerine askıya alması gerektiğini belirtebilirsiniz. Örneğin, runbook'un bir özel durum üzerinde olduğu gibi bir `ErrorActionPreference` hatayı askıya almasını sağlamak için değişkeni Durdur'a ayarlayın.

[Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) veya [Write-Error](https://technet.microsoft.com/library/hh849962.aspx) cmdlet'ini kullanarak bir uyarı veya hata iletisi oluşturun. Etkinlikler uyarı ve hata akışlarına da yazabilir.

```powershell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning –Message "This is a warning message."
Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="debug-stream"></a>Hata ayıklama akışı

Azure Otomasyonu, etkileşimli kullanıcılar için Hata Ayıklama ileti akışını kullanır. Runbook'larda kullanılmamalıdır.

### <a name="verbose-stream"></a>Verbose akışı

Verbose ileti akışı runbook işlemi hakkında genel bilgileri destekler. Hata Ayıklama akışı bir runbook için kullanılamadığından, runbook'unuzun hata ayıklama bilgileri için ayrıntılı iletiler kullanması gerekir. 

Varsayılan olarak, iş geçmişi, performans nedenleriyle yayımlanmış runbook'lardan ayrıntılı iletiler depolamaz. Ayrıntılı iletileri depolamak için, yayımlanmış runbook'larınızı ayrıntılı iletileri günlüğe kaydetmek üzere yapılandırmak için **Günlük Verbose Records** ayarı ile Azure portal **Yapılandırma** sekmesini kullanın. Bu seçeneği yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için açın. Çoğu durumda, ayrıntılı kayıt günlüğe kaydetmeme varsayılan ayarını tutmanız gerekir.

[Bir runbook sınanırken,](automation-testing-runbook.md)runbook ayrıntılı kayıtları günlüğe kaydacak şekilde yapılandırıldığı halde ayrıntılı iletiler görüntülenmez. [Bir runbook test](automation-testing-runbook.md)ederken ayrıntılı iletileri görüntülemek için, değişkeni `VerbosePreference` Devam olarak ayarlamanız gerekir. Bu değişken kümesiyle, Azure portalının Test çıktı bölmesinde ayrıntılı iletiler görüntülenir.

Aşağıdaki [kod, Write-Verbose](https://technet.microsoft.com/library/hh849951.aspx) cmdlet kullanarak ayrıntılı bir ileti oluşturur.

```powershell
#The following line creates a verbose message.

Write-Verbose –Message "This is a verbose message."
```

## <a name="progress-records"></a>İlerleme kayıtları

İlerleme kayıtlarını günlüğe kaydetmek için bir runbook yapılandırmak için Azure portalının **Yapılandırma** sekmesini kullanabilirsiniz. Varsayılan ayar, performansı en üst düzeye çıkarmak için kayıtları günlüğe kaydetmemektir. Çoğu durumda, varsayılan ayarı tutmanız gerekir. Bu seçeneği yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için açın. 

İlerleme kaydı günlüğe kaydetmeyi etkinleştiriyorsanız, runbook'unuz her etkinlik çalışmadan önce ve sonra iş geçmişine bir kayıt yazar. Runbook'u sınamak, çalışma kitabı ilerleme kayıtlarını günlüğe kaydetmek üzere yapılsa bile ilerleme iletilerini görüntülemez.

>[!NOTE]
>Bu cmdlet etkileşimli bir kullanıcı ile kullanılmak üzere tasarlandığından, [Write-Progress](https://technet.microsoft.com/library/hh849902.aspx) cmdlet bir runbook'ta geçerli değildir.

## <a name="preference-variables"></a>Tercih değişkenleri

Farklı çıktı akışlarına gönderilen verilere yanıtı denetlemek için runbook'larınızda belirli Windows PowerShell [tercih değişkenlerini](https://technet.microsoft.com/library/hh847796.aspx) ayarlayabilirsiniz. Aşağıdaki tablo, varsayılan ve geçerli değerleriyle runbook'larda kullanılabilecek tercih değişkenlerini listeler. Azure Otomasyonu dışında Windows PowerShell'de kullanıldığında tercih değişkenleri için ek değerler kullanılabilir.

| Değişken | Varsayılan Değer | Geçerli Değerler |
|:--- |:--- |:--- |
| `WarningPreference` |Devam et |Durdur<br>Devam et<br>SilentlyContinue |
| `ErrorActionPreference` |Devam et |Durdur<br>Devam et<br>SilentlyContinue |
| `VerbosePreference` |SilentlyContinue |Durdur<br>Devam et<br>SilentlyContinue |

Sonraki tabloda, çalışma kitaplarında geçerli olan tercih değişkeni değerlerinin davranışı listelenir.

| Değer | Davranış |
|:--- |:--- |
| Devam et |İletiyi günlüğe kaydeder ve runbook'u yürütmeye devam eder. |
| SilentlyContinue |İletiyi günlüğe kaydetmeden runbook'u yürütmeye devam eder. Bu değer, iletiyi yoksayma etkisine sahiptir. |
| Durdur |İletiyi günlüğe kaydeder ve runbook'u askıya alır. |

## <a name="retrieving-runbook-output-and-messages"></a><a name="runbook-output"></a>Runbook çıktısını ve iletilerini alma

### <a name="retrieve-runbook-output-and-messages-in-azure-portal"></a>Azure portalında runbook çıktısını ve iletilerini alma

Runbook'un **İşler** sekmesini kullanarak Azure portalındaki runbook işinin ayrıntılarını görüntüleyebilirsiniz. İş özeti, iş ve oluşan özel durumlar hakkında genel bilgilere ek olarak giriş parametrelerini ve [Çıktı akışını](#output-stream)görüntüler. İş geçmişi, Çıktı akışıve Uyarı ve [Hata akışlarından](#warning-and-error-streams)gelen iletileri içerir. Ayrıca, runbook ayrıntılı ve ilerleme kayıtları günlüğe kaydacak şekilde yapılandırılırsa, [Verbose akışı](#verbose-stream) ve [ilerleme kayıtlarından](#progress-records) gelen iletileri de içerir.

### <a name="retrieve-runbook-output-and-messages-in-windows-powershell"></a>Windows PowerShell'de runbook çıktısını ve iletilerini alma

Windows PowerShell'de [Get-AzAutomationJobOutput](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0) cmdlet'i kullanarak bir runbook'tan çıktı ve ileti leri alabilirsiniz. Bu cmdlet işin kimliğini gerektirir ve almak `Stream` için akışı belirtmek için çağrılan bir parametre vardır. Bu parametrenin iş için tüm akışları almak için Herhangi bir değerini belirtebilirsiniz.

Aşağıdaki örnek, örnek bir runbook başlatır ve tamamlanmasını bekler. Runbook yürütmeyi tamamladıktan sonra, komut dosyası çalışma defteri çıktı akışını iş yerinden toplar.

```powershell
$job = Start-AzAutomationRunbook -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

$doLoop = $true
While ($doLoop) {
  $job = Get-AzAutomationJob -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
  $status = $job.Status
  $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

# For more detailed job output, pipe the output of Get-AzAutomationJobOutput to Get-AzAutomationJobOutputRecord
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Any | Get-AzAutomationJobOutputRecord
```

### <a name="retrieve-runbook-output-and-messages-in-graphical-runbooks"></a>Grafik çalışma kitaplarında runbook çıktısını ve iletileri alma

Grafik çalışma kitapları için, çıktı ve iletilerin ek günlüğe kaydedilmesini etkinlik düzeyinde izleme şeklinde kullanılabilir. İki izleme düzeyi vardır: Temel ve Ayrıntılı. Temel izleme, runbook'taki her etkinliğin başlangıç ve bitiş saatini ve herhangi bir etkinlik yeniden denemesiyle ilgili bilgileri görüntüler. Bazı örnekler, deneme sayısı ve etkinliğin başlangıç saatidir. Ayrıntılı izleme, temel izleme özelliklerinin yanı sıra her etkinlik için giriş ve çıktı verilerinin günlüğe kaydedilmesini içerir. 

Şu anda etkinlik düzeyinde izleme Verbose akışını kullanarak kayıtları yazar. Bu nedenle, izleme etkinleştirdiğinizde ayrıntılı günlüğe kaydetmeyi etkinleştirmeniz gerekir. İzleme etkinleştirilmiş grafik çalışma kitapları için ilerleme kayıtlarını günlüğe kaydetmeye gerek yoktur. Temel izleme aynı amaca hizmet eder ve daha bilgilendiricidir.

![Grafik Yazma İş Akışları Görünümü](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

Grafik çalışma kitapları için ayrıntılı günlüğe kaydetmeyi ve izlemeyi etkinleştirmenin, üretim **İş Akışları** görünümünde çok daha fazla bilgi kullanılabilir hale geldiğini görüntüden görebilirsiniz. Bu ek bilgiler, bir runbook ile üretim sorunları giderme için gerekli olabilir. 

Ancak, sorun giderme için bir runbook'un ilerlemesini izlemek için bu bilgileri gerektirmedikçe, genel bir uygulama olarak izlemeyi kapalı tutmak isteyebilirsiniz. İzleme kayıtları özellikle çok sayıda olabilir. Grafik çalışma kitabı izleme ile, Temel veya Ayrıntılı izleme yapılandırmanıza bağlı olarak etkinlik başına iki ila dört kayıt alabilirsiniz.

**Etkinlik düzeyinde izleme sağlamak için:**

1. Azure portalında, Otomasyon hesabınızı açın.
2. Runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.
3. Runbook'lar sayfasında, runbook'lar listenizden bir grafik runbook seçin.
4. **Ayarlar**altında, **Günlüğe Kaydetme ve İzlemeyi**tıklatın.
5. Günlük ve İzleme sayfasında, **Günlük verbose kayıtları**altında, ayrıntılı günlük etkinleştirmek için **Üzerine** tıklayın.
6. **Etkinlik düzeyinde izleme**altında, izleme düzeyini, gereksinim duyduğunuz izleme düzeyine bağlı olarak **Temel** veya **Ayrıntılı**olarak değiştirin.<br>

   ![Grafik yazma Günlük ve İzleme sayfası](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="retrieve-runbook-output-and-messages-in-microsoft-azure-monitor-logs"></a>Microsoft Azure Monitor günlüklerinde runbook çıktısını ve iletilerini alma

Azure Otomasyonu, Log Analytics çalışma alanınıza runbook iş durumu ve iş akışları gönderebilir. Azure Monitor, şunları yapmanıza olanak tanıyan günlükleri destekler:

* Otomasyon işleriniz hakkında bilgi edinin.
* Runbook iş durumunuza bağlı olarak bir e-posta veya uyarı tetikleme (örneğin, Başarısız veya Askıya Alındı).
* İş akışları arasında gelişmiş sorgular yazın.
* Otomasyon hesapları daki işleri ilişkilendirin.
* İş geçmişini görselleştirin.

İş verilerini toplamak, ilişkilendirmek ve hareket etmek için Azure Monitor günlükleriyle tümleştirmeyi yapılandırma hakkında daha fazla bilgi için, [Otomasyon'dan Azure Monitor günlüklerine İleri iş durumu ve iş akışları](automation-manage-send-joblogs-log-analytics.md)na bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook yürütme, runbook işlerinin izlenmesi ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için [bkz.](automation-runbook-execution.md)
* Alt runbook'ların nasıl tasarlanıp kullanılacağını anlamak için [Azure Otomasyonunda Alt runbook'lara](automation-child-runbooks.md)bakın.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](/powershell/scripting/overview)bakın.
