---
title: Azure Otomasyonu 'nda runbook çıkışını izleme
description: Bu makalede, runbook çıkışının ve iletilerinin nasıl izleneceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 12/04/2018
ms.topic: conceptual
ms.openlocfilehash: 387e100a05cb51eb034f737b259bad4e5812465c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85557886"
---
# <a name="monitor-runbook-output"></a>Runbook çıkışını izleme

Çoğu Azure Otomasyonu runbook 'un bir çıktı biçimi vardır. Bu çıktı kullanıcıya veya başka bir runbook ile kullanılması amaçlanan karmaşık bir nesneye yönelik bir hata iletisi olabilir. Windows PowerShell, bir betikten veya iş akışından çıkış göndermek için [birden çok akış](/powershell/module/microsoft.powershell.core/about/about_redirection) sağlar. Azure Otomasyonu, bu akışların her biriyle farklı şekilde çalışır. Runbook oluştururken akışları kullanmak için en iyi uygulamaları izlemeniz gerekir.

Aşağıdaki tabloda, yayımlanan runbook 'lar için Azure portal ve [bir runbook 'u test etme](automation-testing-runbook.md)sırasında davranışını içeren her bir akış kısaca açıklanmaktadır. Çıkış akışı, runbook 'lar arasındaki iletişim için kullanılan ana akışdır. Diğer akışlar, bilgileri kullanıcıya iletmek amaçlanan ileti akışları olarak sınıflandırılır. 

| Akış | Açıklama | Yayımlanmış | Test etme |
|:--- |:--- |:--- |:--- |
| Hata |Kullanıcıya yönelik hata iletisi. Bir özel durumla farklı olarak, runbook varsayılan olarak bir hata iletisinden sonra devam eder. |İş geçmişine yazıldı |Test çıkış bölmesinde gösterilir |
| Hata ayıklama |Etkileşimli bir kullanıcıya yönelik iletiler. Runbook 'larda kullanılmamalıdır. |İş geçmişine yazılmadı |Test çıkış bölmesinde görüntülenmiyor |
| Çıktı |Diğer runbook'lar tarafından kullanılması amaçlanan nesneler. |İş geçmişine yazıldı |Test çıkış bölmesinde gösterilir |
| İlerleme Durumu |Runbook'taki her etkinlikten önce ve sonra otomatik oluşturulan kayıtlar. Runbook, etkileşimli bir kullanıcı için tasarlandıklarından kendi ilerleme kayıtlarını oluşturmayı denememelidir. |Yalnızca runbook için ilerleme günlüğü açık olduğunda iş geçmişine yazılır |Test çıkış bölmesinde görüntülenmiyor |
| Ayrıntılı |Genel veya hata ayıklama bilgileri veren mesajlar. |Yalnızca runbook için ayrıntılı günlük kaydı açıksa iş geçmişine yazılır |Yalnızca `VerbosePreference` değişken runbook 'Ta devam olarak ayarlandıysa test çıkış bölmesi 'nde görüntülenir |
| Uyarı |Kullanıcıya yönelik uyarı iletisi. |İş geçmişine yazıldı |Test çıkış bölmesinde gösterilir |

## <a name="use-the-output-stream"></a>Çıkış akışını kullanma

Çıkış akışı, doğru çalıştığı zaman bir betik veya iş akışı tarafından oluşturulan nesnelerin çıkışı için kullanılır. Azure Otomasyonu öncelikle bu akışı, [Geçerli runbook 'u](automation-child-runbooks.md)çağıran üst runbook 'lar tarafından tüketilen nesneler için kullanır. Bir üst öğe [bir runbook 'u satır içi olarak çağırdığında](automation-child-runbooks.md#invoke-a-child-runbook-using-inline-execution), alt öğe çıkış akışından üst öğeye veri döndürür. 

Runbook 'larınız, genel bilgileri yalnızca başka bir runbook tarafından çağrılmadıysa istemciyle iletişim kurmak için çıktı akışını kullanır. Ancak, en iyi uygulama olarak, runbook 'larınız genellikle genel bilgileri kullanıcıyla iletişim kurmak için [ayrıntılı akışı](#monitor-verbose-stream) kullanmalıdır.

Runbook 'larınızın [yazma çıkışı](https://technet.microsoft.com/library/hh849921.aspx)kullanarak çıkış akışına veri yazmasını sağlayabilirsiniz. Alternatif olarak, nesneyi betiğe kendi satırına koyabilirsiniz.

```powershell
#The following lines both write an object to the output stream.
Write-Output –InputObject $object
$object
```

### <a name="handle-output-from-a-function"></a>Bir işlevden çıkış işleme

Bir runbook işlevi çıkış akışına yazdığında, çıkış runbook 'a geri geçirilir. Runbook bu çıktıyı bir değişkene atarsa, çıktı çıkış akışına yazılmaz. İşlevin içinden diğer akışlara yazma, runbook için karşılık gelen akışa yazar. Aşağıdaki örnek PowerShell Iş akışı runbook 'unu göz önünde bulundurun.

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

Runbook işi için çıkış akışı:

```output
Output inside of function
Output outside of function
```

Runbook işi için ayrıntılı akış:

```output
Verbose outside of function
Verbose inside of function
```

Runbook 'u yayımladıktan ve başlamadan önce, ayrıntılı akış çıkışını almak için Runbook ayarlarında ayrıntılı günlüğü de açmalısınız.

### <a name="declare-output-data-type"></a>Çıkış verisi türünü bildir

Çıktı veri türleri örnekleri aşağıda verilmiştir:

* `System.String`
* `System.Int32`
* `System.Collections.Hashtable`
* `Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine`

#### <a name="declare-output-data-type-in-a-workflow"></a>Bir iş akışında çıkış verisi türünü bildirme

Bir iş akışı [OutputType özniteliğini](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute)kullanarak çıkışının veri türünü belirtir. Bu özniteliğin çalışma zamanı sırasında bir etkisi yoktur, ancak size runbook 'un beklenen çıktısının tasarım zamanında bir gösterge sağlar. Runbook 'lar için ayarlanan araç sürekli gelişmeye devam ediyorsa, çıkış veri türlerinin tasarım zamanında bildirilmesinin önemi artar. Bu nedenle, bu bildirimi oluşturduğunuz tüm runbook 'lara eklemek en iyi uygulamadır.

Aşağıdaki örnek runbook bir dize nesnesi çıkışı yapar ve çıkış türü bildirimini içerir. Runbook'unuz belirli bir türde dizi çıkışı yapıyorsa, yine de o türdeki dizi yerine, türü belirtmelisiniz.

```powershell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

#### <a name="declare-output-data-type-in-a-graphical-runbook"></a>Grafik runbook 'unda çıkış verisi türünü bildirme

Grafik veya grafik PowerShell Iş akışı runbook 'unda bir çıkış türü bildirmek için **giriş ve çıkış** menü seçeneğini seçip çıkış türünü girebilirsiniz. Bir üst runbook buna başvurduğunda türü kolayca tanımlanabilir hale getirmek için tam .NET sınıf adının kullanılması önerilir. Tam adı kullanmak, sınıfın tüm özelliklerini runbook 'taki verir ' e gösterir ve koşullu mantık, günlüğe kaydetme ve diğer runbook etkinlikleri için değer olarak başvurmak üzere Özellikler kullanıldığında esnekliği artırır.<br> ![Runbook giriş ve çıkış seçeneği](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

>[!NOTE]
>Giriş ve çıkış özellikleri bölmesindeki **Çıkış türü** alanına bir değer girdikten sonra, girdinizi tanıması için denetimin dışına tıkladığınızdan emin olun.

Aşağıdaki örnekte, giriş ve çıkış özelliğini göstermek için iki grafik runbook gösterilmektedir. Modüler runbook tasarım modelini uygulayarak, farklı çalıştır hesabını kullanarak Azure ile kimlik doğrulamasını yöneten runbook şablonu kimlik doğrulaması adlı bir runbook 'a sahip olursunuz. Normalde belirli bir senaryoyu otomatikleştirmek için çekirdek mantığı gerçekleştiren ikinci runbook, bu durumda runbook 'U doğrula şablonunu yürütür. Sonuçları test çıkış bölmenizi görüntüler. Normal koşullarda, bu runbook 'un alt runbook 'tan gelen çıktıyı kullanan bir kaynağa karşı bir işlem yapması gerekir.

Aşağıda, **Authenticateto-Azure** runbook 'un temel mantığı verilmiştir.<br> ![Runbook şablonu örneği kimliğini doğrulayın ](media/automation-runbook-output-and-messages/runbook-authentication-template.png) .

Runbook, `Microsoft.Azure.Commands.Profile.Models.PSAzureContext` kimlik doğrulama profili özelliklerini döndüren çıkış türünü içerir.<br> ![Runbook çıkış türü örneği](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png)

Bu runbook basit olsa da, buradan çağırmak için bir yapılandırma öğesi vardır. Son etkinlik, `Write-Output` parametre için bir PowerShell ifadesi kullanarak bir değişkene profil verileri yazmak için cmdlet 'ini yürütür `Inputobject` . İçin bu parametre gereklidir `Write-Output` .

Bu örnekteki **Test-ChildOutputType**adlı ikinci runbook, yalnızca iki etkinliği tanımlar.<br> ![Örnek alt çıkış türü runbook 'U](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png)

İlk etkinlik, **Authenticateto-Azure** runbook 'u çağırır. İkinci etkinlik, `Write-Verbose` cmdlet 'Ini **etkinlik çıktısı**olarak ayarlanan **veri kaynağı** ile çalıştırır. Ayrıca, **alan yolu** **Context. Subscription. subscriptionName**, **authenticateto-Azure** runbook 'daki bağlam çıktısı olarak ayarlanır.<br> ![Write-Verbose cmdlet parametresi veri kaynağı](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)

Elde edilen çıktı, aboneliğin adıdır.<br> ![Test-ChildOutputType runbook sonuçları](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

## <a name="monitor-message-streams"></a>İleti akışlarını izleme

Çıkış akışının aksine ileti akışları kullanıcıya bilgi iletidir. Farklı bilgi türleri için birden çok ileti akışı vardır ve Azure Otomasyonu her bir akışı farklı şekilde işler.

### <a name="monitor-warning-and-error-streams"></a>İzleme uyarısı ve hata akışları

Uyarı ve hata akışı, bir runbook 'ta oluşan günlük sorunlarını günlüğe kaydeder. Azure Otomasyonu, runbook yürütürken bu akışları iş geçmişine yazar. Otomasyon, bir runbook test edildiğinde Azure portal test çıkış bölmesindeki akışları içerir. 

Varsayılan olarak, bir runbook bir uyarı veya hatadan sonra yürütülmeye devam eder. Runbook 'un iletiyi oluşturmadan önce bir [tercih değişkeni](#work-with-preference-variables) ayarlayarak runbook 'un bir uyarı veya hata üzerinde askıya alınması gerektiğini belirtebilirsiniz. Örneğin, bir özel durum üzerinde olduğu gibi runbook 'un hata üzerinde askıda kalmasına neden olmak için, `ErrorActionPreference` değişkenini durdur olarak ayarlayın.

[Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) veya [Write-Error](https://technet.microsoft.com/library/hh849962.aspx) cmdlet'ini kullanarak bir uyarı veya hata iletisi oluşturun. Etkinlikler uyarı ve hata akışlarına da yazabilir.

```powershell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning –Message "This is a warning message."
Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="monitor-debug-stream"></a>Hata ayıklama akışını izle

Azure Otomasyonu etkileşimli kullanıcılar için hata ayıklama iletisi akışını kullanır. Runbook 'larda kullanılmamalıdır.

### <a name="monitor-verbose-stream"></a>Ayrıntılı akışı izle

Ayrıntılı ileti akışı, runbook işlemi hakkında genel bilgileri destekler. Hata ayıklama akışı bir runbook için kullanılamadığından, runbook 'un hata ayıklama bilgileri için ayrıntılı iletileri kullanması gerekir. 

Varsayılan olarak, iş geçmişi, performans nedenleriyle, yayımlanan runbook 'lardan ayrıntılı iletileri depolamaz. Ayrıntılı iletileri depolamak için, yayımlanan runbook 'larınızı ayrıntılı iletileri günlüğe kaydetmek üzere yapılandırmak için **ayrıntılı kayıtları günlüğe kaydet** ayarıyla Azure Portal **Yapılandır** sekmesini kullanın. Bu seçeneği yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için açın. Çoğu durumda, ayrıntılı kayıtları günlüğe kaydetme varsayılan ayarını tutmanız gerekir.

[Bir runbook 'u test](automation-testing-runbook.md)ederken, runbook ayrıntılı kayıtları günlüğe kaydetmek üzere yapılandırılmış bile olsa ayrıntılı iletiler görüntülenmez. [Bir runbook 'u test](automation-testing-runbook.md)ederken ayrıntılı iletileri göstermek için, `VerbosePreference` değişkeni devam edecek şekilde ayarlamanız gerekir. Bu değişken kümesiyle, ayrıntılı iletiler Azure portal test çıkış bölmesinde görüntülenir.

Aşağıdaki kod, [Write-Verbose](https://technet.microsoft.com/library/hh849951.aspx) cmdlet 'ini kullanarak ayrıntılı bir ileti oluşturur.

```powershell
#The following line creates a verbose message.

Write-Verbose –Message "This is a verbose message."
```

## <a name="handle-progress-records"></a>İlerleme kayıtlarını işle

İlerleme kayıtlarını günlüğe kaydetmek üzere bir runbook 'u yapılandırmak için Azure portal **Yapılandır** sekmesini kullanabilirsiniz. Varsayılan ayar, performansı en üst düzeye çıkarmak için kayıtları günlüğe içermemelidir. Çoğu durumda, varsayılan ayarı tutmanız gerekir. Bu seçeneği yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için açın. 

İlerleme kaydı kaydını etkinleştirirseniz, runbook uygulamanız her etkinlik çalıştırılmadan önce ve sonra iş geçmişine bir kayıt yazar. Runbook 'u test etmek, runbook ilerleme kaydı kayıtlarını günlüğe kaydetmek üzere yapılandırılmış olsa bile ilerleme iletilerini görüntülemez.

>[!NOTE]
>Bu cmdlet etkileşimli bir kullanıcıyla kullanılmak üzere tasarlandığından, [Write-Progress](https://technet.microsoft.com/library/hh849902.aspx) cmdlet 'i runbook 'ta geçerli değildir.

## <a name="work-with-preference-variables"></a>Tercih değişkenleriyle çalışma

Farklı çıkış akışlarına gönderilen verilerin yanıtını denetlemek için, runbook 'larınızda belirli Windows PowerShell [tercih değişkenlerini](https://technet.microsoft.com/library/hh847796.aspx) ayarlayabilirsiniz. Aşağıdaki tabloda, varsayılan ve geçerli değerleriyle runbook 'larda kullanılabilen tercih değişkenleri listelenmektedir. Windows PowerShell 'de Azure Otomasyonu dışında kullanıldığında tercih değişkenleri için ek değerler mevcuttur.

| Değişken | Varsayılan değer | Geçerli Değerler |
|:--- |:--- |:--- |
| `WarningPreference` |Devam et |Durdur<br>Devam et<br>SilentlyContinue |
| `ErrorActionPreference` |Devam et |Durdur<br>Devam et<br>SilentlyContinue |
| `VerbosePreference` |SilentlyContinue |Durdur<br>Devam et<br>SilentlyContinue |

Sonraki tabloda, runbook 'larda geçerli olan tercih değişkeni değerlerinin davranışı listelenmektedir.

| Değer | Davranış |
|:--- |:--- |
| Devam et |İletiyi günlüğe kaydeder ve runbook'u yürütmeye devam eder. |
| SilentlyContinue |İletiyi günlüğe kaydetmeden runbook'u yürütmeye devam eder. Bu değer iletiyi yok saymakla aynı etkiye sahiptir. |
| Durdur |İletiyi günlüğe kaydeder ve runbook'u askıya alır. |

## <a name="retrieve-runbook-output-and-messages"></a><a name="runbook-output"></a>Runbook çıkışı ve iletilerini alma

### <a name="retrieve-runbook-output-and-messages-in-azure-portal"></a>Azure portal runbook çıkışını ve iletilerini alma

Runbook için **işler** sekmesini kullanarak Azure Portal runbook işinin ayrıntılarını görüntüleyebilirsiniz. İş Özeti, giriş parametrelerini ve [Çıkış akışını](#use-the-output-stream), işle ilgili genel bilgilerin yanı sıra gerçekleşen tüm özel durumları görüntüler. İş geçmişi, çıkış akışından ve [uyarı ve hata akışlarından](#monitor-warning-and-error-streams)iletiler içerir. Ayrıca, runbook ayrıntılı ve ilerleme kayıtlarını günlüğe kaydetmek üzere yapılandırıldıysa, [ayrıntılı akış](#monitor-verbose-stream) ve [ilerleme kaydından](#handle-progress-records) iletiler de dahildir.

### <a name="retrieve-runbook-output-and-messages-in-windows-powershell"></a>Windows PowerShell 'de Runbook çıkışı ve iletileri alma

Windows PowerShell 'de [Get-Azautomationjoi put](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0) cmdlet 'ini kullanarak bir runbook 'tan çıkışı ve iletileri alabilirsiniz. Bu cmdlet iş KIMLIĞINI gerektirir ve alınacak akışın alınacağı bir parametreye sahip olur `Stream` . İş için tüm akışları almak üzere bu parametre için herhangi bir değer belirtebilirsiniz.

Aşağıdaki örnek, örnek bir runbook başlatır ve tamamlanmasını bekler. Runbook yürütmeyi tamamladıktan sonra, betik runbook çıkış akışını işten toplar.

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

### <a name="retrieve-runbook-output-and-messages-in-graphical-runbooks"></a>Grafik runbook 'larında Runbook çıkışı ve iletileri alma

Grafik runbook 'lar için, çıkış ve iletilerin fazladan günlüğe kaydedilmesi etkinlik düzeyi izleme biçiminde kullanılabilir. İki izleme düzeyi vardır: temel ve ayrıntılı. Temel izleme runbook 'taki her etkinlik için başlangıç ve bitiş saatini ve etkinlik yeniden denemeleri ile ilgili bilgileri görüntüler. Etkinliğin deneme sayısı ve başlangıç saati örnekleri aşağıda verilmiştir. Ayrıntılı izleme temel izleme özelliklerini ve her etkinlik için giriş ve çıkış verilerinin günlüğe kaydedilmesini içerir. 

Şu anda etkinlik düzeyindeki izleme, ayrıntılı akışı kullanarak kayıtları yazar. Bu nedenle, izlemeyi etkinleştirdiğinizde ayrıntılı günlüğü etkinleştirmeniz gerekir. İzleme özelliği etkinken grafik runbook 'lar için ilerleme kayıtlarının günlüğe kaydını yapmanız gerekmez. Temel izleme aynı amaca hizmet eder ve daha bilgilendirici olur.

![Grafik yazma iş akışları görünümü](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

Grafik runbook 'lar için ayrıntılı günlük kaydı ve izlemeyi etkinleştiren görüntüden, üretim **Iş akışları** görünümünde daha fazla bilgi daha fazla bilgi bulabilirsiniz. Bu ek bilgiler, bir runbook ile ilgili üretim sorunlarını gidermek için gerekli olabilir. 

Ancak, sorun giderme için bir runbook 'un ilerlemesini izlemek üzere bu bilgilere gerek duymadığınız sürece izlemeyi genel bir uygulama olarak devre dışı bırakmak isteyebilirsiniz. İzleme kayıtları özellikle çok fazla olabilir. Grafik runbook izlemeyi kullanarak, temel veya ayrıntılı izlemenin yapılandırmasına bağlı olarak, etkinlik başına iki ile dört arasında kayıt alabilirsiniz.

**Etkinlik düzeyinde izlemeyi etkinleştirmek için:**

1. Azure portalında, Otomasyon hesabınızı açın.
2. Runbook 'ların listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. Runbook 'Lar sayfasında, runbook listenizden bir grafik runbook seçin.
4. **Ayarlar**altında **günlüğe kaydetme ve izleme**' ye tıklayın.
5. Günlüğe kaydetme ve Izleme sayfasında, ayrıntılı günlük kaydını etkinleştirmek için, **ayrıntılı kayıtları günlüğe kaydet**altında **Açık** ' a tıklayın.
6. **Etkinlik düzeyi izleme**altında, ihtiyacınız olan izleme düzeyine bağlı olarak Izleme düzeyini **temel** veya **ayrıntılı**olarak değiştirin.<br>

   ![Grafik yazma günlüğe kaydetme ve Izleme sayfası](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="retrieve-runbook-output-and-messages-in-microsoft-azure-monitor-logs"></a>Microsoft Azure Izleme günlüklerinde Runbook çıkışı ve iletileri alma

Azure Otomasyonu, Log Analytics çalışma alanınıza runbook iş durumu ve iş akışları gönderebilir. Azure Izleyici şunları yapmanıza olanak sağlayan günlükleri destekler:

* Otomasyon işleriniz hakkında öngörüler elde edin.
* Runbook iş durumunuz temelinde e-posta veya uyarı tetikleyin, örneğin, başarısız veya askıya alındı.
* İş akışları arasında gelişmiş sorgular yazın.
* Otomasyon hesaplarında işlerin ilişkilendirilmesi.
* İş geçmişini görselleştirin.

İş verilerini toplamak, ilişkilendirmek ve üzerinde işlem yapmak için Azure Izleyici günlükleriyle tümleştirmeyi yapılandırma hakkında daha fazla bilgi için bkz. [Otomasyon 'Dan Azure izleyici günlüklerine iş durumu ve iş akışları iletme](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larla çalışmak için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md).
* PowerShell ayrıntıları için bkz. [PowerShell belgeleri](https://docs.microsoft.com/powershell/scripting/overview).
* * PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
