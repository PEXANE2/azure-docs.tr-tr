---
title: Azure Otomasyonunda Grafik yazma
description: Grafik yazma, kodla çalışmadan Azure Otomasyonu için runbook'lar oluşturmanıza olanak tanır. Bu makalede, grafik yazarlık ve grafik runbook oluşturmaya başlamak için gerekli tüm ayrıntıları bir giriş sağlar.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: cf8ced05066923c94e80628651d8983560601d69
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406042"
---
# <a name="graphical-authoring-in-azure-automation"></a>Azure Otomasyonunda Grafik yazma

Grafik yazma, temel Windows PowerShell veya PowerShell İş Akışı kodunun karmaşıklığı olmadan Azure Otomasyonu için runbook'lar oluşturmanıza olanak tanır. Cmdlets ve runbooks bir kütüphaneden tuvale etkinlikler ekleyebilirsiniz, bunları birbirine bağlayabilir ve bir iş akışı oluşturacak şekilde yapılandırabilirsiniz. System Center Orchestrator veya Service Management Automation (SMA) ile çalıştıysanız, grafiksel yazarlık tanıdık görünmelidir. Bu makalede, grafik çalışma kitabı oluşturmaya başlamak için gereken kavramlara giriş sağlar.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="graphical-runbooks"></a>Grafik çalışma kitapları

Azure Otomasyonu'ndaki tüm runbook'lar Windows PowerShell iş akışlarıdır. Grafik çalışma kitapları ve grafikpowerShell İş akışı runbook'ları, Otomasyon çalışanlarının çalıştırdığı ancak görüntüleyemediğiniz veya değiştiremediğiniz PowerShell kodu oluşturur. Grafik çalışma kitabını grafiksel PowerShell İş Akışı runbook'una dönüştürebilirsiniz ve bunun tersi de olabilir. Ancak, bu runbook'ları metin seli runbook'a dönüştüremezsiniz. Ayrıca, Otomasyon grafik düzenleyicisi metinsel bir runbook içe aktaramaz.

## <a name="overview-of-graphical-editor"></a>Grafik düzenleyiciye genel bakış

Azure portalındaki grafik düzenleyicisini grafik çalışma kitabı oluşturarak veya düzenleyerek açabilirsiniz.

![Grafik çalışma alanı](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

Aşağıdaki bölümlerde grafik düzenleyicisindeki denetimler açıklayınız.

### <a name="canvas-control"></a>Tuval kontrolü

Tuval denetimi, çalışma kitabınızı tasarlamanızı sağlar. Kitaplık denetimindeki düğümlerden runbook'a etkinlikler ekleyebilir ve bunları runbook mantığını tanımlamak için bağlantılara bağlayabilirsiniz. Tuvalin alt kısmında, yakınlaştırmanızı ve uzaklaştırmanızı sağlayan denetimler vardır.

### <a name="library-control"></a>Kitaplık denetimi

Kitaplık denetimi, runbook'unuza eklemek için [etkinlikleri](#activities) seçmenize olanak tanır. Bunları tuvale eklersiniz ve bunları diğer etkinliklere bağlayabilirsiniz. Kitaplık denetimi, aşağıdaki tabloda tanımlanan bölümleri içerir.

| Section | Açıklama |
|:--- |:--- |
| Cmdlet’ler |Runbook'unuzda kullanılabilecek tüm cmdletler. Cmdletler modüle göre düzenlenir. Otomasyon hesabınıza yüklediğiniz tüm modüller mevcuttur. |
| Runbook'lar |Otomasyon hesabınızdaki runbook'lar. Bu runbook'ları alt çalışma kitabı olarak kullanılmak üzere tuvale ekleyebilirsiniz. Yalnızca düzenlenen runbook ile aynı çekirdek türündeki runbook'lar gösterilir. Grafik çalışma kitapları için yalnızca PowerShell tabanlı runbook'lar gösterilir. Grafiksel PowerShell İş Akışı runbook'ları için yalnızca PowerShell İş Akışı tabanlı runbook'lar gösterilir. |
| Varlıklar |Runbook'unuzda kullanabileceğiniz Otomasyon hesabınızdaki [otomasyon varlıkları.](/previous-versions/azure/dn939988(v=azure.100)) Runbook'a kıymet eklemek, seçili varlığı alan bir iş akışı etkinliği ekler. Değişken varlıklar söz konusu olduğunda, değişkeni almak için bir etkinlik ekleyip eklemeyin veya değişkeni ayarlayıp ayarlamamayı seçebilirsiniz. |
| Runbook Kontrolü |Geçerli çalışma defterinizde kullanılabilecek denetim etkinlikleri. Bir Bağlantı Etkinliği birden çok giriş alır ve iş akışına devam etmeden önce tüm tamamlanana kadar bekler. Kod etkinliği, grafik çalışma kitabı türüne bağlı olarak powershell veya PowerShell İş akışı kodunun bir veya daha fazla satırını çalıştırAr. Bu etkinliği özel kod veya diğer etkinliklerle elde etmesi zor işlevsellik için kullanabilirsiniz. |

### <a name="configuration-control"></a>Yapılandırma denetimi

Yapılandırma denetimi, tuvalde seçilen bir nesne için ayrıntılar sağlamanızı sağlar. Bu denetimde kullanılabilen özellikler, seçilen nesnenin türüne bağlıdır. Yapılandırma denetiminde bir seçenek seçtiğinizde, daha fazla bilgi sağlamak için ek bıçaklar açılır.

### <a name="test-control"></a>Test kontrolü

Grafik düzenleyici ilk başlatıldığında Test denetimi görüntülenmez. Bir grafik çalışma kitabını etkileşimli olarak sınadığınızda açılır.

## <a name="activities"></a>Etkinlikler

Etkinlikler bir runbook yapı taşlarıdır. Etkinlik PowerShell cmdlet, alt runbook veya iş akışı olabilir. Kitaplık denetiminde sağ tıklayarak ve **tuvale Ekle'yi**seçerek runbook'a bir etkinlik ekleyebilirsiniz. Daha sonra, istediğiniz tuvalin üzerine yerleştirmek için etkinliği tıklatıp sürükleyebilirsiniz. Tuval üzerindeki etkinliğin konumu runbook çalışmasını etkilemez. Runbook'unuzu, çalışmasını görselleştirmek için en uygun bulduğunuz şekilde ortaya koyabilirsiniz.

![Tuvale ekle](media/automation-graphical-authoring-intro/add-to-canvas-cmdlet.png)

Yapılandırma bıçağındaki özelliklerini ve parametrelerini yapılandırmak için tuval üzerinde bir etkinlik seçin. Etkinliğin etiketini açıklayıcı bulduğunuz bir adla değiştirebilirsiniz. Runbook hala orijinal cmdlet çalışır. Grafik düzenleyicinin kullandığı görüntü adını değiştirmeniz yeterlidir. Etiketin runbook içinde benzersiz olması gerektiğini unutmayın.

### <a name="parameter-sets"></a>Parametre kümeleri

Parametre kümesi, belirli bir cmdlet için değerleri kabul eden zorunlu ve isteğe bağlı parametreleri tanımlar. Tüm cmdlets en az bir parametre kümesi var ve bazı çeşitli setleri var. Bir cmdlette birden çok parametre kümesi varsa, parametreleri yapılandırmadan önce kullanılacak olanını seçmeniz gerekir. **Parametre Kümesi'ni** seçip başka bir küme seçerek bir etkinlik tarafından kullanılan parametre kümesini değiştirebilirsiniz. Bu durumda, önceden yapılandırdığınız parametre değerleri kaybolur.

Aşağıdaki örnekte, [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm?view=azps-3.5.0) cmdlet üç parametre kümesi vardır. Örnek, bir kaynak grubundaki tüm sanal makineleri döndürmek için tek bir isteğe bağlı parametreile **ListVirtualMachineInResourceGroupParamSet**adlı bir set kullanır. Örnek ayrıca, sanal makinenin geri döneceğini belirtmek için **GetVirtualMachineInResourceGroupParamSet** parametre kümesini de kullanır. Bu kümenin iki zorunlu parametresi ve bir isteğe bağlı parametre vardır.

![Parametre kümesi](media/automation-graphical-authoring-intro/get-azvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parametre değerleri

Bir parametre için değer belirttiğiniz zaman, değerin nasıl belirtilmesini belirlemek için bir veri kaynağı seçersiniz. Belirli bir parametre için kullanılabilen veri kaynakları, bu parametrenin geçerli değerlerine bağlıdır. Örneğin, Null, null değerlerine izin vermeyen bir parametre için kullanılabilir bir seçenek değildir.

| veri kaynağı | Açıklama |
|:--- |:--- |
| Sabit Değer |Parametre için bir değer yazın. Bu veri kaynağı yalnızca aşağıdaki veri türleri için kullanılabilir: Int32, Int64, String, Boolean, DateTime, Switch. |
| Faaliyet Çıktısı |İş akışındaki geçerli etkinlikten önce gelen bir etkinlikten çıktı kullanın. Tüm geçerli etkinlikler listelenir. Parametre değeri için yalnızca çıktıyı üreten etkinliği kullanın. Etkinlik birden çok özelliği olan bir nesne çıktıları, etkinliği seçtikten sonra belirli bir özelliğin adını yazabilirsiniz. |
| Runbook Girişi |Etkinlik parametresi için giriş olarak bir runbook girişi seçin. |
| Değişken Varlık |Giriş olarak bir Otomasyon değişkeni seçin. |
| Kimlik Kıymeti |Giriş olarak bir Otomasyon kimlik bilgisi seçin. |
| Sertifika Varlığı |Giriş olarak bir Otomasyon sertifikası seçin. |
| Bağlantı Varlığı |Giriş olarak bir Otomasyon bağlantısı seçin. |
| PowerShell ifadesi |Basit bir [PowerShell ifadesi belirtin.](#powershell-expressions) İfade etkinlikten önce değerlendirilir ve sonuç parametre değeri için kullanılır. Bir etkinliğin çıktısına veya runbook giriş parametresine başvurmak için değişkenler kullanabilirsiniz. |
| Yapılandırılmadı |Daha önce yapılandırılan herhangi bir değeri temizleyin. |

#### <a name="optional-additional-parameters"></a>İsteğe bağlı ek parametreler

Tüm cmdletler ek parametreler sağlama seçeneğine sahiptir. Bunlar PowerShell yaygın parametreleri veya diğer özel parametrelerdir. Grafik düzenleyici, PowerShell sözdizimini kullanarak parametreleri sağlayabileceğiniz bir metin kutusu sunar. Örneğin, ortak parametreyi `Verbose` kullanmak `-Verbose:$True`için.

### <a name="retry-activity"></a>Etkinliği yeniden deneyin

Bir etkinlik için işlevselliği yeniden deneyin, döngü gibi belirli bir koşul karşılanana kadar birden çok kez çalıştırılmasına olanak tanır. Bu özelliği, birden çok kez çalışması gereken, hataya yatkın olan, başarı için birden fazla deneme gerektirebilecek veya geçerli veriler için etkinliğin çıktı bilgilerini sınayan etkinlikler için kullanabilirsiniz.

Bir etkinlik için yeniden denemeyi etkinleştirdiğinizde, bir gecikme ve bir koşul ayarlayabilirsiniz. Gecikme, runbook'un etkinliği yeniden çalıştırmadan önce beklediği saattir (saniye veya dakika cinsinden ölçülür). Bir gecikme belirtmezseniz, etkinlik tamamlandıktan hemen sonra yeniden çalışır.

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-delay.png)

Yeniden deneme koşulu, etkinliğin her çalıştığından sonra değerlendirilen bir PowerShell ifadesidir. İfade True'ya çözülürse, etkinlik yeniden çalışır. İfade False olarak çözülürse, etkinlik yeniden çalışmaz ve runbook sonraki aktiviteye taşınır.

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-condition.png)

Yeniden deneme koşulu, etkinlik `RetryData` yeniden denemeleri hakkındaki bilgilere erişim sağlayan adlandırılmış bir değişken kullanabilir. Bu değişkenaşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| `NumberOfAttempts` |Etkinliğin kaç kez çalıştırıldığı. |
| `Output` |Etkinliğin son çalışmasından çıktı. |
| `TotalDuration` |Etkinlik ilk kez başlatıldığından beri zaman aşımına uğradı. |
| `StartedAt` |Etkinlik ilk başlatıldığında (UTC biçiminde) saat. |

Aşağıda etkinlik yeniden deneme koşullarına örnekler verilmiştir.

```powershell-interactive
# Run the activity exactly 10 times.
$RetryData.NumberOfAttempts -ge 10
```

```powershell-interactive
# Run the activity repeatedly until it produces any output.
$RetryData.Output.Count -ge 1
```

```powershell-interactive
# Run the activity repeatedly until 2 minutes has elapsed.
$RetryData.TotalDuration.TotalMinutes -ge 2
```

Bir etkinlik için yeniden deneme koşulunu yapılandırıldıktan sonra, etkinlik size hatırlatacak iki görsel ipucu içerir. Biri etkinlikte sunulur ve diğeri etkinliğin yapılandırmasını gözden geçirdiğinizde gösterilir.

![Etkinlik Görsel Göstergeleri Yeniden Deneyin](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>İş Akışı Komut Dosyası denetimi

İş akışı Komut Dosyası denetimi, yazılan grafik çalışma kitabının türüne bağlı olarak PowerShell veya PowerShell İş Akışı komut dosyasını kabul eden özel bir etkinliktir. Bu denetim, başka yollarla kullanılamayabilecek işlevsellik sağlar. Parametreleri kabul edemez, ancak etkinlik çıktısı ve runbook giriş parametreleri için değişkenleri kullanabilirsiniz. Etkinliğin herhangi bir çıktısı databus eklenir. Bir istisna, giden bağlantısı olmayan çıktıdır ve bu durumda çıktı runbook çıktısına eklenir.

Örneğin, aşağıdaki kod, runbook giriş değişkeni adlı `NumberOfDays`bir tarih hesaplamaları gerçekleştirir. Daha sonra, runbook'taki sonraki etkinlikler tarafından kullanılmak üzere hesaplanan bir DateTime değeri gönderir.

```powershell-interactive
$DateTimeNow = (Get-Date).ToUniversalTime()
$DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
$DateTimeStart
```

## <a name="links-and-workflow"></a>Bağlantılar ve iş akışı

Grafik çalışma kitabındaki bir bağlantı iki aktiviteyi birbirine bağlar. Tuvalde kaynak etkinlikten hedef aktiviteye işaret eden bir ok olarak görüntülenir. Etkinlikler, kaynak etkinlik tamamlandıktan sonra başlayan hedef etkinliğiyle okun yönünde çalışır.

### <a name="link-creation"></a>Bağlantı oluşturma

Kaynak etkinliği seçerek ve şeklin altındaki daireyi tıklatarak iki etkinlik arasında bir bağlantı oluşturabilirsiniz. Oku hedef aktiviteye sürükleyin ve bırakın.

![Bağlantı oluşturma](media/automation-graphical-authoring-intro/create-link-options.png)

Yapılandırma bıçağındaki özelliklerini yapılandırmak için bağlantıyı seçin. Özellikler, aşağıdaki tabloda açıklanan bağlantı türünü içerir.

| Bağlantı Türü | Açıklama |
|:--- |:--- |
| İşlem hattı |Hedef etkinlik, kaynak etkinlikten her nesne çıktısı için bir kez çalışır. Kaynak etkinlik çıktı yla sonuçlanmazsa hedef etkinlik çalışmaz. Kaynak etkinlikten çıktı bir nesne olarak kullanılabilir. |
| Sequence |Hedef etkinlik, kaynak etkinlikten çıktı aldığında yalnızca bir kez çalışır. Kaynak etkinlikten çıktı bir nesne dizisi olarak kullanılabilir. |

### <a name="start-of-activity"></a>Faaliyetin başlangıcı

Grafik çalışma kitabı, gelen bağlantısı olmayan tüm etkinliklerle başlar. Genellikle runbook için başlangıç etkinliği gibi davranan tek bir etkinlik vardır. Birden çok etkinlik gelen bir bağlantı yoksa, runbook bunları paralel olarak çalıştırarak başlar. Her biri tamamlarken diğer etkinlikleri çalıştırmak için bağlantıları izler.

### <a name="link-conditions"></a>Bağlantı koşulları

Bir bağlantıda bir koşul belirttiğiniz zaman, hedef etkinlik yalnızca koşul True'ya çözülürse çalışır. Genellikle kaynak etkinlikten çıktı almak için bir durumda bir `ActivityOutput` değişken kullanın.

Bir ardışık hat lar bağlantısı için, tek bir nesne için bir koşul belirtmeniz gerekir. Runbook kaynak etkinliği tarafından her nesne çıktısı için durumu değerlendirir. Daha sonra durumu tatmin eden her nesne için hedef etkinliği çalıştırır. Örneğin, bir kaynak etkinliği `Get-AzVM`ile, koşullu bir ardışık bağlantı için aşağıdaki sözdizimini kullanabilirsiniz grup1 adlı kaynak grubunda yalnızca sanal makineleri almak için.

```powershell-interactive
$ActivityOutput['Get Azure VMs'].Name -match "Group1"
```

Bir dizi bağlantısı için runbook, kaynak etkinlikteki tüm nesneleri içeren tek bir dizi döndürüldenedeniyle durumu yalnızca bir kez değerlendirir. Bu nedenle, runbook bir ardışık bağlantı ile olduğu gibi, filtreleme için bir dizi bağlantısı kullanamazsınız. Dizi bağlantısı yalnızca bir sonraki etkinliğin çalıştırılıp çalıştırılmayacağını belirleyebilir.

Örneğin, **Başlangıç VM** çalışma kitabımızda aşağıdaki etkinlik kümesini ele alalım:

![Dizili Koşullu Bağlantı](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)

Runbook, giriş parametrelerinin `VMName` değerlerini doğrulayan ve `ResourceGroupName` yapılacak uygun eylemi belirlemek için üç farklı sıra bağlantısı kullanır. Olası eylemler tek bir VM başlatmak, kaynak grubundaki tüm VM'leri başlatmak veya bir abonelikteki tüm VM'leri başlatmaktır. Arasındaki `Connect to Azure` sıra bağlantısı `Get single VM`için ve , burada durum mantığı:

```powershell-interactive
<#
Both VMName and ResourceGroupName runbook input parameters have values
#>
(
(($VMName -ne $null) -and ($VMName.Length -gt 0))
) -and (
(($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
)
```

Koşullu bir bağlantı kullandığınızda, kaynak etkinlikten o daldaki diğer etkinliklere kadar kullanılabilen veriler duruma göre filtrelenir. Bir etkinlik birden çok bağlantının kaynağıysa, her daldaki etkinlikleriçin kullanılabilen veriler, bağlantının o şubeye bağlanan durumuna bağlıdır.

Örneğin, aşağıdaki `Start-AzVM` runbook'taki etkinlik tüm sanal makineleri başlatır. İki koşullu bağlantısı vardır. İlk koşullu bağlantı, `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -eq $true` etkinlik başarıyla `Start-AzVM` tamamlanırsa filtrelemek için ifadeyi kullanır. İkinci koşullu bağlantı, `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -ne $true` `Start-AzVm` etkinlik sanal makineyi başlatamazsa filtrelemek için ifadeyi kullanır.

![Koşullu bağlantı örneği](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

İlk bağlantıyı izleyen ve etkinlik çıktısını `Get-AzureVM` kullanan herhangi bir etkinlik yalnızca çalıştırıldığı sırada `Get-AzureVM` başlatılan sanal makineleri alır. İkinci bağlantıyı izleyen herhangi bir etkinlik yalnızca çalıştırıldığı sırada `Get-AzureVM` durdurulan sanal makineleri alır. Üçüncü bağlantıyı izleyen herhangi bir etkinlik, çalışma durumuna bakılmaksızın tüm sanal makineleri alır.

### <a name="junctions"></a>Kavşak

Kavşak, gelen tüm dallar tamamlanana kadar bekleyen özel bir etkinliktir. Bu, runbook'un birden çok faaliyeti paralel olarak çalıştırmasına ve devam etmeden önce tüm bunların tamamlandığına emin olmasını sağlar.

Bir kavşak sınırsız sayıda gelen bağlantıya sahip olsa da, bu bağlantılardan yalnızca biri bir boru hattı olabilir. Gelen sıra bağlantılarının sayısı kısıtlanmaz. Birden çok gelen ardışık bağlantıyla bağlantı oluşturabilir ve runbook'u kaydedebilirsiniz, ancak çalıştırıldığında başarısız olur.

Aşağıdaki örnek, aynı anda bu makinelere uygulanacak düzeltme emüllerini indirirken bir dizi sanal makine başlatan runbook'un bir parçasıdır. Çalışma kitabı devam etmeden önce her iki işlemin de tamamlandığından emin olmak için bir bağlantı kullanır.

![Kavşak](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Döngüler

Bir hedef etkinliği kaynağı etkinliğine veya sonunda kaynağına geri bağlanan başka bir aktiviteye bağlandığında bir döngü oluşur. Grafik sel yazma şu anda döngüleri desteklemez. Runbook'unuzun bir döngüsü varsa, düzgün kaydeder, ancak çalıştığında bir hata alır.

![Aralıklı](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="data-sharing-between-activities"></a>Etkinlikler arasında veri paylaşımı

Bir etkinliğin giden bir bağlantıyla çıktıverdiği tüm veriler runbook'un veri veri busilere yazılır. Runbook'taki herhangi bir etkinlik, parametre değerlerini doldurmak veya komut dosyası koduna eklemek için veri veribus'taki verileri kullanabilir. Bir etkinlik, iş akışındaki önceki herhangi bir etkinliğin çıktısı erişebilir.

Verilerin databus'a nasıl yazıldığı, etkinlikteki bağlantının türüne bağlıdır. Bir ardışık hat lar bağlantısı için, veriler birden çok nesne olarak çıktıdır. Bir dizi bağlantısı için, veriler bir dizi olarak çıktıdır. Yalnızca bir değer varsa, tek öğeli bir dizi olarak çıktıdır.

Runbook'unuzun veri veri sûresindeki verilere erişmenin iki yolu vardır: 
* Etkinlik çıktısı veri kaynağını kullanın.
* PowerShell ifade veri kaynağı kullanın.

İlk mekanizma, başka bir etkinliğin parametresini doldurmak için bir etkinlik çıktısı veri kaynağı kullanır. Çıktı bir nesneyse, runbook tek bir özellik belirtebilir.

![etkinlik çıktısı](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

İkinci veri erişim mekanizması, aşağıda gösterilen sözdizimi kullanarak PowerShell ifade veri kaynağındaki bir etkinliğin çıktısını veya değişkenli bir `ActivityOutput` iş akışı komut dosyası etkinliğini alır. Çıktı bir nesneyse, runbook'unuzun tek bir özelliği belirtebilir.

```powershell-interactive
$ActivityOutput['Activity Label']
$ActivityOutput['Activity Label'].PropertyName
```

### <a name="checkpoints"></a>Kontrol noktaları

Herhangi bir etkinlikte **Checkpoint runbook'u** seçerek denetim [noktalarını](automation-powershell-workflow.md#checkpoints) grafiksel PowerShell İş Akışı çalışma kitabında ayarlayabilirsiniz. Bu, etkinlik çalıştırDıktan sonra bir denetim noktasının ayarlanılmasına neden olur.

![Checkpoint](media/automation-graphical-authoring-intro/set-checkpoint.png)

Denetim noktaları yalnızca grafiksel PowerShell İş Akışı çalışma kitaplarında etkinleştirilir ve grafik çalışma kitaplarında kullanılamaz. Runbook'ta Azure cmdlets kullanıyorsa, bir etkinlikle birlikte herhangi bir `Connect-AzAccount` denetim noktası etkinliği izlemelidir. Bağlama işlemi, runbook'un askıya alınması ve bu denetim noktasından farklı bir alt çalışma üzerinde yeniden başlatılması durumunda kullanılır.

## <a name="runbook-input-and-output"></a>Runbook giriş ve çıktı

### <a name="runbook-input"></a>Runbook girişi<a name="runbook-input"></a>

Runbook, geçerli kitap çocuk olarak kullanılıyorsa, çalışma kitabını başlatan bir kullanıcıdan veya başka bir runbook'tan giriş gerektirir. Örneğin, sanal bir makine oluşturan bir runbook için, kullanıcının her runbook başlatında sanal makinenin adı ve diğer özellikler gibi bilgileri sağlaması gerekebilir.

Runbook, bir veya daha fazla giriş parametresini tanımlayarak girişi kabul eder. Kullanıcı, runbook her başlatında bu parametreler için değerler sağlar. Kullanıcı, Azure portalını kullanarak runbook'u başlattığında, kullanıcıdan runbook tarafından desteklenen her giriş parametresi için değerler sağlaması istenir.

Runbook'unuzu yazarken, runbook araç çubuğundaki **Giriş ve çıktıyı** tıklatarak giriş parametrelerine erişebilirsiniz. Bu, varolan bir giriş parametresini düzenlemeyapabileceğiniz veya giriş ekle'yi tıklatarak yeni bir giriş oluşturabileceğiniz Giriş ve Çıktı **denetimini**açar.

![Girdi ekleme](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Her giriş parametresi aşağıdaki tablodaki özelliklerle tanımlanır:

| Özellik | Açıklama |
|:--- |:--- |
| Adı | Gereklidir. Parametrenin adı. Ad runbook içinde benzersiz olmalıdır. Bir harfle başlamalı ve yalnızca harfler, sayılar ve alt çizerler içerebilir. Ad boşluk içeremez. |
| Açıklama |İsteğe bağlı. Giriş parametresi amacının açıklaması. |
| Tür | İsteğe bağlı. Parametre değeri için beklenen veri türü. Azure portalı, giriş istenirken her parametre için veri türü için uygun bir denetim sağlar. Desteklenen parametre türleri String, Int32, Int64, Ondalık, Boolean, DateTime ve Nesne'dir. Bir veri türü seçili değilse, varsayılan olarak String'e aktarılır.|
| Zorunlu | İsteğe bağlı. Parametre için bir değer sağlanmalı mı belirten ayar. İsterseniz, `yes`runbook başlatıldığında bir değer sağlanmalıdır. Seçerseniz, `no`runbook başlatıldığında bir değer gerekmez ve varsayılan bir değer kullanılabilir. Varsayılan değeri tanımlanmamış her zorunlu parametre için bir değer sağlamazsanız runbook başlatılamaz. |
| Varsayılan Değer | İsteğe bağlı. Runbook başlatıldığında bir parametre için kullanılan değer. Varsayılan değer ayarlamak için `Custom`. Varsayılan `None` değer sağlamak istemiyorsanız seçin. |

### <a name="runbook-output"></a>Runbook çıkışı

Grafik [yazma, runbook](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages)çıktısına giden bir bağlantı yok herhangi bir etkinlik tarafından oluşturulan verileri kaydeder. Çıktı runbook işiyle kaydedilir ve çalışma defteri çocuk olarak kullanıldığında bir üst çalışma kitabı tarafından kullanılabilir.

## <a name="powershell-expressions"></a>PowerShell ifadeleri

Grafik sel yazmanın avantajlarından biri, PowerShell'in en az bilgisine sahip bir runbook oluşturmanıza olanak sağlamasıdır. Şu anda, ancak, belirli [parametre değerleri](#activities) doldurmak ve [bağlantı koşulları](#links-and-workflow)ayarlamak için PowerShell biraz bilmeniz gerekir. Bu bölümde PowerShell ifadeleri hızlı bir giriş sağlar. PowerShell'in tüm [detaylarıwindows powershell ile Scripting'de](https://technet.microsoft.com/library/bb978526.aspx)mevcuttur.

### <a name="powershell-expression-data-source"></a>PowerShell ifade veri kaynağı

PowerShell kodunun sonuçlarıyla etkinlik [parametresinin](#activities) değerini doldurmak için veri kaynağı olarak PowerShell ifadesini kullanabilirsiniz. İfade, basit bir işlev veya bazı karmaşık mantık gerçekleştiren birden çok satır gerçekleştiren tek bir kod satırı olabilir. Bir değişkene atanmamış bir komutun çıktısı parametre değerine çıktıdır.

Örneğin, aşağıdaki komut geçerli tarihi çıkar.

```powershell-interactive
Get-Date
```

Sonraki kod snippet geçerli tarihten bir dize oluşturur ve bir değişkene atar. Kod, değişkenin içeriğini çıktıya gönderir.

```powershell-interactive
$string = "The current date is " + (Get-Date)
$string
```

Aşağıdaki komutlar geçerli tarihi değerlendirir ve geçerli günün hafta sonu mu yoksa hafta sonu mu olduğunu belirten bir dize döndürer.

```powershell-interactive
$date = Get-Date
if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
else { "Weekday" }
```

### <a name="activity-output"></a>Etkinlik çıktısı

Runbook'unuzdaki önceki bir etkinlikten çıktıyı `ActivityOutput` kullanmak için aşağıdaki sözdizimi içeren değişkeni kullanın.

```powershell-interactive
$ActivityOutput['Activity Label'].PropertyName
```

Örneğin, sanal bir makinenin adını gerektiren bir özelliği olan bir etkinlik olabilir. Bu durumda, runbook'unuzun kullanımı aşağıdaki ifadeyi kullanabilir.

```powershell-interactive
$ActivityOutput['Get-AzureVM'].Name
```

Özellik yalnızca bir ad yerine sanal makine nesnesi gerektiriyorsa, runbook aşağıdaki sözdizimini kullanarak nesnenin tamamını döndürür.

```powershell-interactive
$ActivityOutput['Get-AzureVM']
```

Runbook aşağıdaki gibi daha karmaşık bir ifade, bir etkinliğin çıktısı kullanabilirsiniz. Bu ifade, metni sanal makine adına birleştirir.

```powershell-interactive
"The computer name is " + $ActivityOutput['Get-AzureVM'].Name
```

### <a name="conditions"></a>Koşullar

Değerleri karşılaştırmak veya bir değerin belirli bir desenle eşleşip eşleşmeyemeyişsini belirlemek için [karşılaştırma işleçlerini](https://technet.microsoft.com/library/hh847759.aspx) kullanın. Karşılaştırma, True veya False değerini döndürür.

Örneğin, aşağıdaki koşul, adlı `Get-AzureVM` bir etkinlikten gelen sanal makinenin şu anda durdurulup durdurulmamayı belirler.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"
```

Aşağıdaki koşul, aynı sanal makinenin durduruldu dışında herhangi bir durumda olup olmadığını belirler.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"
```

Runbook'unuzda ki birden çok koşula mantıksal `-and` bir `-or` [işleç](https://technet.microsoft.com/library/hh847789.aspx)(örneğin) veya . Örneğin, önceki örnekteki sanal makinenin Durdurulma veya Durdurma durumunda olup olmadığını görmek için aşağıdaki koşul denetler.

```powershell-interactive
($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping")
```

### <a name="hashtables"></a>Hashtables

[Karma tablolar,](https://technet.microsoft.com/library/hh847780.aspx) bir değer kümesini döndürmek için yararlı olan ad değeri çiftleridir. Sözlük olarak adlandırılan bir karma tablo da görebilirsiniz. Belirli etkinlikleriçin özellikler basit bir değer yerine karma tablo bekliyor.

Aşağıdaki sözdizimini kullanarak karma tablo oluşturun. Herhangi bir sayıda giriş içerebilir, ancak her biri bir ad ve değerle tanımlanır.

```powershell-interactive
@{ <name> = <value>; [<name> = <value> ] ...}
```

Örneğin, aşağıdaki ifade, internet araması için bir değer karmasını bekleyen bir etkinlik parametresi için veri kaynağı olarak kullanılmak üzere bir karma tablo oluşturur.

```powershell-interactive
$query = "Azure Automation"
$count = 10
$h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
$h
```

Aşağıdaki örnek, karma tabloyu `Get Twitter Connection` doldurmak için çağrılan bir etkinlikten çıktı kullanır.

```powershell-interactive
@{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
    'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
    'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
    'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}
```

## <a name="authenticating-to-azure-resources"></a>Azure kaynaklarına kimlik doğrulama

Azure kaynaklarını yöneten Azure Otomasyonu'ndaki runbook'lar azure için kimlik doğrulaması gerektirir. Hizmet sorumlusu olarak da adlandırılan [Çalıştır Hesabı,](automation-create-runas-account.md)bir Otomasyon runbook'unun aboneliğinizdeki Azure Kaynak Yöneticisi kaynaklarına erişmek için kullandığı varsayılan mekanizmadır. Bu işlevselliği, PowerShell [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) `AzureRunAsConnection` cmdlet'ini kullanan bağlantı varlığını tuvale ekleyerek grafik çalışma kitabına ekleyebilirsiniz. [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet'i de ekleyebilirsiniz. Bu senaryo aşağıdaki örnekte gösterilmiştir.

![Kimlik Doğrulama Etkinlikleri Olarak Çalıştır](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)

Etkinlik, `Get Run As Connection` ya `Get-AutomationConnection`da , adlı `AzureRunAsConnection`sabit bir değer veri kaynağı ile yapılandırılır.

![Bağlantı Yapılandırması Olarak Çalıştır](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)

Sonraki etkinlik, `Connect-AzAccount`runbook'ta kullanılmak üzere doğrulanmış Çalıştır Hesabı'nı ekler.

![Connect-AzAccount Parametre Seti](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)

>[!NOTE]
>PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` `Connect-AzAccount`diğer adlar . Bu diğer adların grafik çalışma kitaplarınız için kullanılamadığını unutmayın. Grafik çalışma kitabı yalnızca `Connect-AzAccount` kendisini kullanabilir.

Parametre alanları **APPLICATIONID**, **CERTIFICATETHUMBPRINT**ve **TENANTID**için, etkinlik birden çok özelliğe sahip bir nesne çıktığından, alan yolu için özelliğin adını belirtin. Aksi takdirde, runbook yürütüldüğünde, kimlik doğrulaması yapmaya çalışırken başarısız olur. Run As hesabıyla runbook'unuzun kimliğini doğrulamak için en az ihtiyacınız olan budur.

Bazı aboneler, Azure klasik dağıtımını veya Azure Kaynak Yöneticisi kaynaklarını yönetmek için bir [Azure AD kullanıcı hesabı](automation-create-aduser-account.md) kullanarak bir Otomasyon hesabı oluşturur. Bu aboneler için geriye dönük uyumluluğu korumak için, runbook'unuzda kullanılacak kimlik doğrulama mekanizması, kimlik bilgisi `Add-AzureAccount` [kıymetine](automation-credentials.md)sahip cmdlettir. Varlık, Azure hesabına erişimi olan bir Active Directory kullanıcısını temsil eder.

Bu işlevselliği, grafiğe bir kimlik bilgisi varlığı ekleyerek grafik çalışma kitabınız `Add-AzureAccount` için etkinleştirebilirsiniz ve ardından giriş için kimlik bilgisi varlığını kullanan bir etkinlik. Aşağıdaki örneğe bakın.

![Kimlik doğrulama etkinlikleri](media/automation-graphical-authoring-intro/authentication-activities.png)

Runbook'un her denetim noktasında ve sonrasında kimlik doğrulaması gerekir. Bu nedenle herhangi `Add-AzureAccount` `Checkpoint-Workflow` bir etkinlikten sonra bir etkinlik kullanmanız gerekir. Ek bir kimlik bilgisi etkinliği kullanmanız gerekmez.

![Etkinlik çıktısı](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="exporting-and-importing-a-graphical-runbook"></a>Grafik çalışma defterini dışa aktarma ve alma

Yalnızca grafik çalışma kitabının yayımlanmış sürümünü dışa aktarabilirsiniz. Runbook henüz yayımlanmadıysa, **Dışa Aktarma** düğmesi devre dışı bırakılır. **Dışa Aktarma** düğmesini tıklattığınızda, runbook yerel bilgisayarınıza indirilir. Dosyanın adı, **.graphrunbook** uzantılı runbook'un adıyla eşleşir.

Runbook eklerken **Alma** seçeneğini seçerek grafiksel veya grafiksel PowerShell İş Akışı runbook dosyasını içe aktarabilirsiniz. İçe aktarılmayı seçdiğinizde, aynı adı tutabilir veya yeni bir dosya sağlayabilirsiniz. **Runbook Türü** alanı, seçili dosyayı değerlendirdikten sonra runbook türünü görüntüler. Doğru olmayan farklı bir tür seçmeye çalışırsanız, grafik düzenleyicisi olası çakışmalar olduğunu ve dönüştürme sırasında sözdizimi hataları olabileceğini belirten bir ileti sunar.

![Runbook'u içe aktarma](media/automation-graphical-authoring-intro/runbook-import.png)

## <a name="testing-a-graphical-runbook"></a>Grafik çalışma kitabını test etme

Azure Otomasyonu'ndaki her grafik çalışma kitabının bir Taslak sürümü ve Yayımlanmış sürümü vardır. Yalnızca Yayımlanmış sürümü çalıştırabilirsiniz, ancak yalnızca Taslak sürümünü edebilirsiniz. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanıma hazır olduğunda, geçerli Yayımlanmış sürümün üzerine taslak sürümünün üzerine yazan sürümü yayımlarsınız.

Yayımlanmış sürümü değiştirmeden bırakırken azure portalında bir runbook'un Taslak sürümünü sınayabilirsiniz. Alternatif olarak, yeni bir runbook'u yayımlanmadan önce sınayabilirsiniz, böylece herhangi bir sürüm değiştirmeden önce runbook'un doğru çalıştığını doğrulayabilirsiniz. Runbook'un sınaması Taslak sürümünü yürütür ve gerçekleştirdiği eylemlerin tamamlanmasını sağlar. Hiçbir iş geçmişi oluşturulur, ancak Test Çıktısı bölmesi çıktıyı görüntüler.

Düzenleme için runbook'u açıp **Test bölmesini**tıklatarak grafik çalışma kitabınız için Test denetimini açın. Giriş parametreleri için Test denetimi ister ve **Başlat'ı**tıklatarak runbook'u başlatabilirsiniz.

## <a name="publishing-a-graphical-runbook"></a>Grafik çalışma kitabı yayımlama

Düzenleme için runbook'u açıp **Yayımla'yı**tıklatarak grafik çalışma kitabı yayımlayın. Runbook için olası durumlar şunlardır:

* Yeni -- runbook henüz yayınlanmadı. 
* Yayınlandı -- runbook yayınlandı.
* Düzenlemede -- runbook yayımlandıktan sonra düzenlendi ve Taslak ve Yayımlanmış sürümler farklı.

![Runbook durumları](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png)

Runbook'un Yayımlanmış sürümüne geri döndürme seçeneğiniz var. Bu işlem, runbook'un en son yayımlandığından beri yapılan değişiklikleri ortadan atar. Runbook'un Taslak sürümünü Yayımlanmış sürümle değiştirir.

## <a name="next-steps"></a>Sonraki Adımlar

* PowerShell İş Akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell İş Akışı runbook uygulamam](automation-first-runbook-textual.md).
* Grafik çalışma kitaplarıyla başlamak için [ilk grafik çalışma kitabıma](automation-first-runbook-graphical.md)bakın.
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi için [Azure Otomasyon runbook türlerine](automation-runbook-types.md)bakın.
* Otomasyon Çalıştır'ı Kullanarak kimlik doğrulaması yapmak için Azure [Çalıştır'ı Hesap Olarak Yapılandır'a](automation-sec-configure-azure-runas-account.md)bakın.
* PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
