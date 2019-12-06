---
title: Azure Otomasyonu 'nda grafik yazma
description: Grafik yazma, kodla çalışmaya gerek kalmadan Azure Otomasyonu için Runbook 'lar oluşturmanızı sağlar. Bu makalede grafik yazma konusuna ve grafik runbook 'u oluşturmaya başlamak için gereken tüm ayrıntılara giriş sağlanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 82a06510bd9d1e0de2b38260773cb4848156bf12
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850304"
---
# <a name="graphical-authoring-in-azure-automation"></a>Azure Otomasyonu 'nda grafik yazma

Grafik yazma, temel Windows PowerShell veya PowerShell Iş akışı kodunun karmaşıklıkları olmadan Azure Otomasyonu için Runbook 'lar oluşturmanızı sağlar. Bir cmdlet ve Runbook 'lardan etkinlikleri tuvale ekler, bunları birbirine bağlayabilir ve bir iş akışı oluşturacak şekilde yapılandırın. System Center Orchestrator veya Service Management Automation (SMA) ile çalıştıysanız, size tanıdık gelmelidir

Bu makalede grafik yazma ve grafik runbook 'u oluşturmaya başlamak için ihtiyacınız olan kavramlar hakkında bir giriş sunulmaktadır.

## <a name="graphical-runbooks"></a>Grafik runbook 'ları

Azure Otomasyonu 'ndaki tüm runbook 'lar Windows PowerShell Iş akışlarıdır. Grafik ve grafik PowerShell Iş akışı runbook 'ları Otomasyon çalışanları tarafından çalıştırılan PowerShell kodu oluşturur, ancak bunu görüntüleyemeyeceksiniz veya doğrudan değiştiremezsiniz. Grafik runbook 'u bir grafik PowerShell Iş akışı runbook 'una dönüştürülebilir, ancak bunun tersini yapabilir, ancak bir metin runbook 'una dönüştürülemez. Var olan bir metin runbook 'u grafik düzenleyiciye içeri aktarılamaz.

## <a name="overview-of-graphical-editor"></a>Grafik düzenleyiciye genel bakış

Grafik bir runbook oluşturarak veya düzenleyerek grafik düzenleyiciyi Azure portal açabilirsiniz.

![Grafik çalışma alanı](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

Aşağıdaki bölümlerde, grafik düzenleyicideki denetimler açıklanır.

### <a name="canvas"></a>Tuval

Tuvali, runbook 'unuzu tasarlayabileceğiniz yerdir. Kitaplık denetimindeki düğümlerdeki etkinlikleri runbook 'a eklersiniz ve Runbook 'un mantığını tanımlamak için bağlantıları bağlantılarla birlikte bağlayabilirsiniz.

Yakınlaştırmak ve uzaklaştırmak için tuvalin altındaki denetimleri kullanabilirsiniz.

### <a name="library-control"></a>Kitaplık denetimi

Kitaplık denetimi, runbook 'unuzu eklemek için [etkinlikleri](#activities) seçtiğiniz yerdir. Bunları diğer etkinliklere bağlandığınız tuvale eklersiniz. Aşağıdaki tabloda açıklanan dört bölüm içerir:

| Section | Açıklama |
|:--- |:--- |
| Cmdlet'ler |Runbook 'da kullanılabilen tüm cmdlet 'leri içerir. Cmdlet 'ler modüle göre düzenlenir. Otomasyon hesabınızda yüklediğiniz tüm modüller kullanılabilir. |
| Runbook'lar |Otomasyon hesabınızdaki runbook 'ları içerir. Bu runbook 'lar alt runbook 'lar olarak kullanılacak tuvale eklenebilir. Yalnızca düzenlenmekte olan runbook ile aynı çekirdek türündeki runbook 'lar gösterilir; Grafik runbook 'lar için yalnızca PowerShell tabanlı runbook 'lar gösterilir, ancak grafik PowerShell Iş akışı runbook 'ları yalnızca PowerShell-Iş akışı tabanlı runbook 'lar gösterilir. |
| Varlıklar |Otomasyon hesabınızda runbook 'larınızda kullanılabilecek [Otomasyon varlıklarını](/previous-versions/azure/dn939988(v=azure.100)) içerir. Bir runbook 'a varlık eklediğinizde, seçili varlığı alan bir iş akışı etkinliği ekler. Değişken varlıklar söz konusu olduğunda, değişkeni almak için bir etkinlik eklenip eklenmeyeceğini veya değişkeni ayarlamayı seçebilirsiniz. |
| Runbook denetimi |, Geçerli runbook 'lerinizde kullanılabilecek runbook denetim etkinliklerini içerir. Bir *birleşim* birden çok giriş alır ve iş akışına devam etmeden önce tüm tamamlanana kadar bekler. Bir *kod* etkinliği, grafik runbook türüne bağlı olarak bir veya daha fazla PowerShell veya PowerShell iş akışı kodu çalıştırır. Bu etkinliği, özel kod veya diğer etkinliklerle elde edilmesi zor olan işlevler için kullanabilirsiniz. |

### <a name="configuration-control"></a>Yapılandırma denetimi

Yapılandırma denetimi, tuvalde seçilmiş bir nesnenin ayrıntılarını sağladığınız yerdir. Bu denetimde kullanılabilen özellikler, seçilen nesne türüne bağlıdır. Yapılandırma denetiminde bir seçenek belirlediğinizde ek bilgiler sağlamak için ek dikey pencereler açılır.

### <a name="test-control"></a>Test denetimi

Grafik Düzenleyicisi ilk başlatıldığında test denetimi görüntülenmez. [Grafik runbook 'unu](#graphical-runbook-procedures)etkileşimli olarak test ettiğinizde açılır.

## <a name="graphical-runbook-procedures"></a>Grafik runbook yordamları

### <a name="exporting-and-importing-a-graphical-runbook"></a>Grafik runbook 'unu dışarı ve içeri aktarma

Yalnızca bir grafik runbook 'un yayımlanan sürümünü dışarı aktarabilirsiniz. Runbook henüz yayımlanmamışsa **dışarı aktarma** düğmesi devre dışı bırakılır. **Dışarı aktar** düğmesine tıkladığınızda, runbook yerel bilgisayarınıza indirilir. Dosyanın adı, bir *graphrunbook* Uzantısı ile Runbook adı ile eşleşir.

Bir runbook eklerken **Içeri aktar** seçeneğini belirleyerek bir grafik veya grafik PowerShell iş akışı runbook dosyasını içeri aktarabilirsiniz. İçeri aktarılacak dosyayı seçtiğinizde, aynı **adı** koruyabilir veya yeni bir tane sağlayabilirsiniz. Runbook türü alanı, seçili dosyayı değerlendirir sonra runbook türünü görüntüler ve doğru olmayan farklı bir tür seçip, olası çakışmalar olduğunu ve dönüştürme sırasında bir ileti gösterilir. hatası.

![Runbook'u içeri aktar](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Grafik runbook 'u test etme

Runbook 'un yayımlanan sürümünü değiştirmeden bırakarak Azure portal runbook 'un taslak sürümünü test edebilir veya yayımlanmadan önce yeni bir runbook 'u test edebilirsiniz. Bu, yayımlanan sürümü değiştirmeden önce runbook'un doğru çalıştığını doğrulamanızı sağlar. Bir runbook'u test ettiğinizde, Taslak runbook yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. İş geçmişi oluşturulmaz, ancak çıktı test çıkış bölmesinde görüntülenir.

Düzen için Runbook 'u açarak runbook için test denetimini açın ve ardından **Test bölmesi** düğmesine tıklayın.

Test denetimi herhangi bir giriş parametresi için istemde bulunur ve **Başlat** düğmesine tıklayarak runbook 'u başlatabilirsiniz.

### <a name="publishing-a-graphical-runbook"></a>Grafik runbook 'unu yayımlama

Azure Otomasyonu 'ndaki her runbook 'ta taslak ve yayımlanmış bir sürüm bulunur. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir olmaya hazır olduğunda, yayımlanan sürümün üzerine taslak sürümü üzerine yazan onu yayımlayın.

Bir grafik runbook 'unu düzenleyerek, düzenleyebilir ve ardından **Yayınla** düğmesine tıklayarak yayımlayabilirsiniz.

Bir runbook henüz yayımlanmamışsa, **Yeni**durumuna sahiptir. Yayımlandığında, durumu **yayımlanmış**olur. Yayımlandıktan sonra runbook 'u düzenlerseniz ve taslak ve yayımlanan sürümler farklıysa, runbook 'un durumu **düzenleme ' de**olur.

![Runbook durumları](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png)

Ayrıca, bir runbook 'un yayımlanan sürümüne dönmek için seçeneğiniz de vardır. Bu, runbook 'un en son yayımlanmasından bu yana yapılan tüm değişiklikleri atar ve Runbook 'un taslak sürümünü yayımlanan sürümle değiştirir.

## <a name="activities"></a>Olaylar

Etkinlikler bir runbook 'un yapı taşlarıdır. Etkinlik bir PowerShell cmdlet 'i, alt runbook veya bir iş akışı etkinliği olabilir. Kitaplık denetiminde sağ tıklayıp **tuvale Ekle**' yi seçerek runbook 'a bir etkinlik eklersiniz. Daha sonra istediğiniz tuvalde dilediğiniz yere yerleştirmek için aktiviteyi tıklatıp sürükleyebilirsiniz. Tuvaldeki etkinliğin konumu, runbook 'un çalışmasını herhangi bir şekilde etkilemez. Runbook 'unuzu düzenleyebilir, ancak işlemini görselleştirmek için en uygun olduğunu bulabilirsiniz.

![tuvale Ekle](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Yapılandırma dikey penceresinde özelliklerini ve parametrelerini yapılandırmak için tuvaldeki etkinliği seçin. Etkinliğin **etiketini** sizin için açıklayıcı bir şekilde değiştirebilirsiniz. Özgün cmdlet hala çalıştırılmakta, yalnızca grafik düzenleyicide kullanılan görünen adını değiştiriyorsunuz. Etiket, runbook içinde benzersiz olmalıdır.

### <a name="parameter-sets"></a>Parametre kümeleri

Bir parametre kümesi, belirli bir cmdlet için değerleri kabul eden zorunlu ve isteğe bağlı parametreleri tanımlar. Tüm cmdlet 'ler en az bir parametre kümesine sahiptir ve bazıları birden fazla. Bir cmdlet birden çok parametre kümesine sahipse, parametreleri yapılandırabilmeniz için önce hangisini kullanacağınızı seçmeniz gerekir. Yapılandırabileceğiniz parametreler, seçtiğiniz parametre kümesine bağlıdır. Bir etkinlik tarafından kullanılan parametre kümesini **parametre kümesi** seçip başka bir küme seçerek değiştirebilirsiniz. Bu durumda, yapılandırdığınız tüm parametre değerleri kaybolur.

Aşağıdaki örnekte, Get-AzureRmVM cmdlet 'inin üç parametre kümesi vardır. Parametre kümelerinden birini seçinceye kadar parametre değerlerini yapılandıramazsınız. Listvirtualmachineınresourcegroupparamset parametre kümesi, bir kaynak grubundaki tüm sanal makinelerin döndürülmesi için ve tek bir isteğe bağlı parametreye sahiptir. The **GetVirtualMachineInResourceGroupParamSet** is for specifying the virtual machine you want to return and has two mandatory and one optional parameter.

![Parametre kümesi](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parametre değeri

Bir parametre için bir değer belirttiğinizde, değerin nasıl belirtilceğini belirlemek için bir veri kaynağı seçersiniz. Belirli bir parametre için kullanılabilen veri kaynakları, bu parametre için geçerli değerlere bağlıdır. Örneğin, null değerlere izin verilmeyen bir parametre için null kullanılabilir bir seçenek değildir.

| Veri Kaynağı | Açıklama |
|:--- |:--- |
| Sabit değer |Parametre için bir değer yazın. Bu yalnızca şu veri türleri için kullanılabilir: Int32, Int64, String, Boolean, DateTime, anahtar. |
| Etkinlik çıkışı |İş akışındaki geçerli etkinlikten önce gelen bir etkinliğin çıktısı. Tüm geçerli etkinlikler listelenir. Kendi çıkışını parametre değeri için kullanmak üzere yalnızca etkinliği seçin. Etkinlik birden fazla özelliğe sahip bir nesne çıktısı alıyorsa, etkinliği seçtikten sonra özelliğin adını yazabilirsiniz. |
| Runbook girişi |Etkinlik parametresine giriş olarak bir runbook giriş parametresi seçin. |
| Değişken varlık |Giriş olarak bir Otomasyon değişkeni seçin. |
| Kimlik bilgisi varlığı |Giriş olarak bir Otomasyon kimlik bilgisi seçin. |
| Sertifika varlığı |Giriş olarak bir Otomasyon sertifikası seçin. |
| Bağlantı varlığı |Giriş olarak bir Otomasyon bağlantısı seçin. |
| PowerShell ifadesi |Basit [PowerShell ifadesi](#powershell-expressions)belirtin. İfade, etkinlik ve sonuç parametre değeri için kullanılmadan önce değerlendirilir. Bir etkinliğin veya Runbook giriş parametresinin çıktısına başvurmak için değişkenleri kullanabilirsiniz. |
| Yapılandırılmadı |Daha önce yapılandırılmış olan tüm değerleri temizler. |

#### <a name="optional-additional-parameters"></a>İsteğe bağlı ek parametreler

Tüm cmdlet 'ler ek parametreler sağlama seçeneğine sahiptir. Bunlar PowerShell ortak parametreleridir veya diğer özel parametrelerdir. PowerShell söz dizimini kullanarak parametreler sağlayabileceğiniz bir metin kutusuyla karşılaşırsınız. Örneğin, **ayrıntılı** ortak parametresini kullanmak için **"-verbose: $true"** belirtirsiniz.

### <a name="retry-activity"></a>Yeniden deneme etkinliği

**Yeniden deneme davranışı** , belirli bir koşul karşılanana kadar bir etkinliğin birden çok kez çalıştırılmasını sağlar. Bu özelliği, birden çok kez çalışması gereken, hataya açık olan ve bir başarı denemesine daha fazla gerekebilecek ve geçerli veriler için etkinliğin çıktı bilgilerini test eden etkinlikler için kullanabilirsiniz.

Bir etkinlik için yeniden denemeyi etkinleştirdiğinizde bir gecikme ve koşul belirleyebilirsiniz. Gecikme süresi, runbook 'un etkinliği yeniden çalıştırmadan önce bekleyeceği süredir (saniye veya dakika cinsinden ölçülür). Gecikme belirtilmemişse etkinlik tamamlandıktan hemen sonra yeniden çalışır.

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-delay.png)

Yeniden deneme koşulu, etkinlik her çalıştığında değerlendirilen bir PowerShell deyimidir. İfade true olarak çözümlenirse etkinlik yeniden çalıştırılır. İfade yanlış olarak çözümlenirse etkinlik yeniden çalıştırılmaz ve Runbook bir sonraki etkinliğe gider.

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-condition.png)

Yeniden deneme koşulu, etkinlik yeniden denemeleri hakkında bilgilere erişim sağlayan $RetryData adlı bir değişken kullanabilir. Bu değişken aşağıdaki tablodaki özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Numberofdenemeleri |Etkinliğin kaç kez çalıştırıldığı. |
| Çıktı |Etkinliğin son çalıştırağından çıktı. |
| Toplam süre |Etkinlik ilk kez başlatıldığından bu yana zaman geçti. |
| StartedAt |UTC biçimindeki zaman etkinliğin ilk başlatılmıştı. |

Etkinlik yeniden deneme koşullarının örnekleri aşağıda verilmiştir.

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

Bir etkinlik için yeniden deneme koşulu yapılandırdıktan sonra, etkinlik size anımsatacak iki görsel ipucu içerir. Etkinlik, etkinliğin yapılandırmasını gözden geçirdikten sonra etkinlikte, diğeri ise olur.

![Etkinlik yeniden deneme görsel göstergeleri](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>İş akışı betik denetimi

Kod denetimi, başka türlü kullanılamayan işlevselliği sağlamak için, yazılan grafik runbook 'un türüne bağlı olarak PowerShell veya PowerShell Iş akışı betiğini kabul eden özel bir etkinliktir. Parametreleri kabul edemez, ancak etkinlik çıktısı ve Runbook giriş parametreleri için değişkenleri kullanabilir. Herhangi bir giden bağlantısı olmadığından, etkinliğin herhangi bir çıkışı, runbook 'un çıktısına eklendiği durumda bir giden bağlantısı olmadığı müddetçe, verir 'e eklenir.

Örneğin, aşağıdaki kod $NumberOfDays adlı bir runbook giriş değişkenini kullanarak tarih hesaplamaları gerçekleştirir. Daha sonra, runbook 'taki izleyen etkinlikler tarafından kullanılacak çıktı olarak hesaplanmış bir tarih gönderir.

```powershell-interactive
$DateTimeNow = (Get-Date).ToUniversalTime()
$DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
$DateTimeStart
```

## <a name="links-and-workflow"></a>Bağlantılar ve iş akışı

Grafik runbook 'taki bir **bağlantı** iki etkinliği birbirine bağlar. Kaynak etkinlikten hedef etkinliğe işaret eden bir ok olarak tuvalde görüntülenir. Etkinlikler, kaynak etkinlik tamamlandıktan sonra başlangıç hedef etkinliği ile ok yönünde çalışır.

### <a name="create-a-link"></a>Bağlantı oluştur

Kaynak etkinliğini seçip şeklin altındaki daireye tıklayarak iki etkinlik arasında bir bağlantı oluşturun. Oku hedef etkinlik ve yayına sürükleyin.

![Bağlantı oluştur](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Yapılandırma dikey penceresinde özelliklerini yapılandırmak için bağlantıyı seçin. Bu, aşağıdaki tabloda açıklanan bağlantı türünü içerir:

| Bağlantı türü | Açıklama |
|:--- |:--- |
| İşlem hattı |Hedef etkinlik, kaynak etkinlikten her nesne çıktısı için bir kez çalıştırılır. Kaynak etkinlik hiçbir çıkış içermiyorsa, hedef etkinlik çalışmaz. Kaynak etkinlikten alınan çıkış bir nesne olarak kullanılabilir. |
| Dizisi |Hedef etkinlik yalnızca bir kez çalışır. Kaynak etkinlikten bir nesne dizisi alır. Kaynak etkinlikten alınan çıkış, nesne dizisi olarak kullanılabilir. |

### <a name="starting-activity"></a>Etkinlik başlatılıyor

Grafik runbook, gelen bağlantısı olmayan etkinliklerle başlar. Bu genellikle yalnızca tek bir etkinliktir ve bu, runbook 'un başlangıç etkinliği olarak davranır. Birden çok etkinliğin gelen bağlantısı yoksa, runbook bunları paralel olarak çalıştırarak başlatılır. Her tamamlandıkça diğer etkinlikleri çalıştırmak için bağlantıları izler.

### <a name="conditions"></a>Koşullar

Bir bağlantı üzerinde bir koşul belirttiğinizde, hedef etkinlik yalnızca koşul true olarak çözümlenirse çalıştırılır. Kaynak etkinlikten çıktıyı almak için genellikle bir koşulda $ActivityOutput değişkenini kullanırsınız

İşlem hattı bağlantısı için, tek bir nesne için bir koşul belirtirsiniz ve koşul, kaynak etkinliğin her bir nesne çıktısı için değerlendirilir. Hedef etkinlik, koşulu karşılayan her nesne için çalıştırılır. Örneğin, Get-AzureRmVm ' nin kaynak etkinliğiyle, aşağıdaki sözdizimi yalnızca *grup1*adlı kaynak grubundaki sanal makineleri almak üzere koşullu bir işlem hattı bağlantısı için kullanılabilir.

```powershell-interactive
$ActivityOutput['Get Azure VMs'].Name -match "Group1"
```

Bir sıra bağlantısı için koşul yalnızca, kaynak etkinlikten alınan tüm nesneleri kapsayan tek bir dizi döndürüldüğünden bir kez değerlendirilir. Bu nedenle, bir sıra bağlantısı, bir işlem hattı bağlantısı gibi filtrelemede kullanılamaz, ancak bir sonraki etkinliğin çalıştırılıp çalıştırılmadığını saptacaktır. Örnek olarak, başlangıç VM runbook 'ımızda aşağıdaki etkinlik kümesini yapın.

![Dizileri olan koşullu bağlantı](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)

Tek bir VM 'yi başlatmak için uygun eylem olduğunu belirleyebilmek için, VM adını ve kaynak grubu adını temsil eden iki Runbook giriş parametresine değer doğrulayan üç farklı dizi bağlantısı verilmiştir ve kaynaktaki tüm VM 'Leri başlatın Grup veya bir abonelikteki tüm VM 'Ler. Azure 'a Bağlan ve tek VM al arasındaki sıra bağlantısı için aşağıdaki koşul mantığı verilmiştir:

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

Koşullu bir bağlantı kullandığınızda, kaynak etkinlikten söz konusu daldaki diğer etkinliklere sunulan veriler koşula göre filtrelenir. Bir etkinlik, kaynak birden çok bağlantıya ise, her dalda etkinlik için kullanılabilen veriler, bu dala bağlanan bağlantıdaki koşula bağlıdır.

Örneğin, aşağıdaki runbook 'taki **Start-AzureRmVm** etkinliği tüm sanal makineleri başlatır. İki koşullu bağlantıya sahiptir. İlk koşullu bağlantı *$ActivityOutput [' Start-AzureRmVM '] ifadesini kullanır. IsSuccessStatusCode-* start-AzureRmVm etkinliğinin başarıyla tamamlandığını filtrelemek için eq $true. İkincisi *$ActivityOutput [' Start-AzureRmVM '] ifadesini kullanır. IsSuccessStatusCode-* start-AzureRmVm etkinliği sanal makineyi başlatamadıysanız süzülecek $true.

![Koşullu bağlantı örneği](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

İlk bağlantıyı izleyen ve Get-AzureVM ' den gelen etkinlik çıktısını kullanan herhangi bir etkinlik yalnızca, Get-AzureVM çalıştırıldığı sırada başlatılan sanal makineleri alır. İkinci bağlantıyı izleyen herhangi bir etkinlik yalnızca Get-AzureVM çalıştırıldığı zaman durdurulmuş olan sanal makineleri alır. Üçüncü bağlantıyı izleyen herhangi bir etkinlik, çalışma durumlarına bakılmaksızın tüm sanal makineleri alır.

### <a name="junctions"></a>Merkezleriyle

Birleşim, tüm gelen dallar tamamlanana kadar bekleyen özel bir etkinliktir. Bu, paralel olarak birden çok etkinlik çalıştırmanızı ve taşınmadan önce tümünün tamamlandığından emin olmanızı sağlar.

Bir birleşimin sınırsız sayıda gelen bağlantısı olabilir, ancak bu bağlantılardan birden fazlası bir işlem hattı olabilir. Gelen dizi bağlantılarının sayısı sınırlı değil. Birden çok gelen işlem hattı bağlantısı ile birleşim oluşturmanıza ve Runbook 'u kaydetmeye izin verilir, ancak çalıştırıldığında başarısız olur.

Aşağıdaki örnek, bir sanal makine kümesi başlatan bir runbook 'un parçası olan bir runbook 'un aynı anda bu makinelere uygulanacak düzeltme eklerini de indiriyor. Bağlantı, runbook devam etmeden önce her iki işlemin de tamamlandığından emin olmak için kullanılır.

![Bağlantısı](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Döngüler

Bir döngüyle, hedef etkinliğin kaynak etkinliğine veya sonunda kaynağına geri bağlanan başka bir etkinliğe bağlantısı olur. Grafik yazarken döngülerine Şu anda izin verilmiyor. Runbook 'üzde bir döngüyle karşılaşırsanız, doğru şekilde kaydedilir ancak çalıştırıldığında bir hata alır.

![Aralıklı](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Etkinlikler arasında veri paylaşma

Giden bağlantı ile bir etkinlik tarafından çıkış yapan tüm veriler runbook 'un *verir* 'una yazılır. Runbook 'taki tüm etkinlikler parametre değerlerini doldurmak veya betik koduna eklemek için verir üzerindeki verileri kullanabilir. Etkinlik, iş akışındaki önceki etkinliklerin çıktısına erişebilir.

Verilerin verir 'a nasıl yazıldığı, etkinliğin bağlantı türüne bağlıdır. Bir işlem **hattı**için, veriler nesnelerin katları olarak çıktı olur. **Sıralı** bir bağlantı için, veriler dizi olarak çıktı olur. Yalnızca bir değer varsa, tek bir öğe dizisi olarak çıktı olur.

Verir üzerindeki verilere iki yöntemden birini kullanarak erişebilirsiniz. İlk olarak, başka bir etkinliğin parametresini doldurmak için bir **etkinlik çıkış** veri kaynağı kullanıyor. Çıktı bir nesnedir, tek bir özellik belirtebilirsiniz.

![Etkinlik çıkışı](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

Ayrıca, bir etkinliğin çıkışını bir **PowerShell ifadesi** veri kaynağında veya bir **iş akışı betiği** aktivitesinden bir activitızput değişkeniyle elde edebilirsiniz. Çıktı bir nesnedir, tek bir özellik belirtebilirsiniz. Activitızput değişkenleri aşağıdaki sözdizimini kullanır.

```powershell-interactive
$ActivityOutput['Activity Label']
$ActivityOutput['Activity Label'].PropertyName
```

### <a name="checkpoints"></a>Kontrol noktaları

Herhangi bir etkinlikte *denetim noktası runbook* 'u seçerek bir grafik PowerShell iş akışı runbook 'unda [denetim noktaları](automation-powershell-workflow.md#checkpoints) belirleyebilirsiniz. Bu, etkinlik çalıştıktan sonra bir denetim noktasının ayarlanmış olmasına neden olur.

![Checkpoint](media/automation-graphical-authoring-intro/set-checkpoint.png)

Kontrol noktaları yalnızca grafik PowerShell Iş akışı runbook 'larında etkinleştirilir, grafik runbook 'larda kullanılamaz. Runbook 'un Azure cmdlet 'lerini kullanması durumunda, runbook 'un askıya alınması ve farklı bir çalışan üzerinde bu denetim noktasından yeniden başlatılması durumunda bir Connect-AzureRmAccount ile tüm CHECKPOINT etkinlikleri izlemeniz gerekir.

## <a name="authenticating-to-azure-resources"></a>Azure kaynaklarında kimlik doğrulaması

Azure Otomasyonu 'ndaki runbook 'lar Azure kaynaklarını yöneten Azure 'da kimlik doğrulaması gerektirir. [Farklı Çalıştır hesabı](automation-create-runas-account.md) (hizmet sorumlusu olarak da anılır), aboneliğinizdeki Azure Resource Manager kaynaklarına Otomasyon Runbook 'ları ile erişmek için varsayılan yöntemdir. PowerShell [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) cmdlet 'ini ve tuvale [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet 'Ini kullanarak **azurerunasconnection** bağlantı varlığını ekleyerek bu işlevselliği bir grafik runbook 'a ekleyebilirsiniz. Bu, aşağıdaki örnekte gösterilmiştir:

![Farklı çalıştır kimlik doğrulama etkinlikleri](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)

Farklı Çalıştır bağlantısını Al etkinliği (yani, Get-AutomationConnection), AzureRunAsConnection adlı bir sabit değer veri kaynağı ile yapılandırılır.

![Farklı Çalıştır bağlantı yapılandırması](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)

Connect-AzureRmAccount adlı sonraki etkinlik, runbook 'ta kullanılmak üzere kimliği doğrulanmış farklı çalıştır hesabını ekler.

![Connect-AzureRmAccount parametre kümesi](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)

> [!IMPORTANT]
> **Add-azurermaccount** artık **Connect-azurermaccount**için bir diğer addır. Kitaplık öğelerinizi ararken, **Connect-AzureRMAccount**' u görmüyorsanız **Add-azurermaccount**komutunu kullanabilir veya bir Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

**ApplicationId**, **CERTIFICATETHUMBPRINT**ve **tenantıd** parametreleri için, etkinlik birden fazla özelliğe sahip bir nesne çıktıdığı için alan yolu için özelliğin adını belirtmeniz gerekir. Aksi takdirde, runbook 'u yürüttüğünüzde kimlik doğrulaması gerçekleştirmeye çalışmaz. Bu, runbook 'unuzu farklı çalıştır hesabıyla doğrulamak için en azından ihtiyacınız olan şeydir.

Azure klasik dağıtımını veya Azure Resource Manager kaynaklarını yönetmek için bir [Azure AD Kullanıcı hesabı](automation-create-aduser-account.md) kullanan bir Otomasyon hesabı oluşturan aboneler için geriye dönük uyumluluk sağlamak için, kimlik doğrulama yöntemi, Azure hesabına erişimi olan Active Directory bir kullanıcıyı temsil eden bir [kimlik bilgisi](automation-credentials.md) varlığına sahip Add-AzureAccount cmdlet 'larıdır.

Tuvale bir kimlik bilgisi varlığı ekleyerek ve sonra Add-AzureAccount etkinliğinin ardından bu işlevselliği bir grafik runbook 'una ekleyebilirsiniz. Add-AzureAccount, girişinin kimlik bilgisi etkinliğini kullanır. Bu, aşağıdaki örnekte gösterilmiştir:

![Kimlik doğrulama etkinlikleri](media/automation-graphical-authoring-intro/authentication-activities.png)

Runbook 'un başlangıcında ve her denetim noktasından sonra kimlik doğrulaması yapmanız gerekir. Bu, herhangi bir kontrol noktası etkinliğinden sonra ek Add-AzureAccount etkinliği eklemenin anlamına gelir. Aynı şekilde kullanabilmeniz için bir ek kimlik bilgisi etkinliğine ihtiyacınız yoktur

![Etkinlik çıkışı](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Runbook giriş ve çıkış

### <a name="runbook-input"></a>Runbook girişi

Runbook, runbook 'u Azure portal aracılığıyla başlatırken bir kullanıcıdan veya geçerli bir alt öğe olarak kullanılıyorsa başka bir runbook 'tan giriş gerektirebilir.
Örneğin, bir sanal makine oluşturan bir runbook varsa, runbook 'u her başlattığınızda sanal makine adı ve diğer özellikler gibi bilgileri sağlamanız gerekebilir.

Bir veya daha fazla giriş parametresi tanımlayarak bir runbook için girişi kabul etmiş olursunuz. Runbook her başlatıldığında bu parametrelerin değerlerini sağlarsınız. Azure portal bir runbook 'u başlattığınızda, runbook 'un giriş parametrelerinin her biri için değerler sağlamanızı ister.

Runbook araç çubuğundaki **giriş ve çıkış** düğmesine tıklayarak bir runbook için giriş parametrelerine erişebilirsiniz.

Bu, var olan bir giriş parametresini düzenleyebileceğiniz veya **giriş Ekle**' ye tıklayarak yeni bir tane oluşturabileceğiniz **giriş ve çıkış** denetimini açar.

![Girdi ekleme](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Her giriş parametresi aşağıdaki tablodaki Özellikler tarafından tanımlanır:

| Özellik | Açıklama |
|:--- |:--- |
| Adı |Parametrenin benzersiz adı. Bu yalnızca alfa sayısal karakterler içerebilir ve bir boşluk içeremez. |
| Açıklama |Giriş parametresi için isteğe bağlı bir açıklama. |
| Tür |Parametre değeri için beklenen veri türü. Azure portal, giriş isterken her bir parametre için veri türü için uygun bir denetim sağlar. |
| Zorunlu |Parametresi için bir değer sağlanması gerekip gerekmediğini belirtir. Varsayılan değeri tanımlanmış olmayan her zorunlu parametre için bir değer sağlamazsanız runbook başlatılamaz. |
| Default Value |Bir parametre sağlanmazsa parametresi için hangi değerin kullanıldığını belirtir. Bu, null ya da belirli bir değer olabilir. |

### <a name="runbook-output"></a>Runbook çıkışı

Giden bağlantısı olmayan herhangi bir etkinlik tarafından oluşturulan veriler [runbook 'un çıktısına](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages)kaydedilir. Çıktı runbook işiyle birlikte kaydedilir ve Runbook bir alt öğe olarak kullanıldığında üst runbook 'ta kullanılabilir.

## <a name="powershell-expressions"></a>PowerShell ifadeleri

Grafik yazmanın avantajlarından biri, PowerShell hakkında en az bilgiye sahip bir runbook oluşturmanıza olanak sağlar. Şu anda, bazı [parametre değerlerini](#activities) doldurarak ve [bağlantı koşullarını](#links-and-workflow)ayarlarken, PowerShell 'in bir bitini bilmeniz gerekir. Bu bölümde, bu kullanıcılara tanıdık olmayan kullanıcılar için PowerShell ifadelerine hızlı bir giriş sunulmaktadır. PowerShell 'in tüm ayrıntıları, [Windows PowerShell Ile komut dosyası oluşturma](https://technet.microsoft.com/library/bb978526.aspx)sırasında kullanılabilir.

### <a name="powershell-expression-data-source"></a>PowerShell ifadesi veri kaynağı
Bir [etkinlik parametresinin](#activities) değerini bazı PowerShell kodu sonuçlarıyla doldurmak için bir veri kaynağı olarak bir PowerShell ifadesi kullanabilirsiniz. Bu, bazı basit işlevleri veya bazı karmaşık mantık gerçekleştiren birden çok satırı gerçekleştiren tek bir kod satırı olabilir. Bir değişkene atanmamış bir komuttan gelen tüm çıktılar parametre değerine çıkış olur.

Örneğin, aşağıdaki komut geçerli tarihin çıktısını verebilir.

Örneğin, aşağıdaki komut geçerli tarihin çıktısını verebilir.

```powershell-interactive
Get-Date
```

Aşağıdaki komutlar geçerli tarihten bir dize oluşturur ve bir değişkene atar. Değişkenin içeriği daha sonra çıkışa gönderilir

```powershell-interactive
$string = "The current date is " + (Get-Date)
$string
```

Aşağıdaki komutlar geçerli tarihi değerlendirir ve geçerli günün hafta sonu veya hafta içi olduğunu gösteren bir dize döndürür.

```powershell-interactive
$date = Get-Date
if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
else { "Weekday" }
```

### <a name="activity-output"></a>Etkinlik çıkışı

Runbook 'taki önceki bir etkinliğin çıkışını kullanmak için, $ActivityOutput değişkenini aşağıdaki sözdizimiyle kullanın.

```powershell-interactive
$ActivityOutput['Activity Label'].PropertyName
```

Örneğin, bir sanal makine adı gerektiren bir özelliği olan bir etkinliğiniz olabilir, bu durumda aşağıdaki ifadeyi kullanabilirsiniz:

```powershell-interactive
$ActivityOutput['Get-AzureVm'].Name
```

Yalnızca bir özellik yerine sanal makine nesnesini gerektiren özellik varsa, aşağıdaki söz dizimini kullanarak tüm nesneyi döndürebilirsiniz.

```powershell-interactive
$ActivityOutput['Get-AzureVm']
```

Ayrıca, bir etkinliğin çıkışını, metni sanal makine adına birleştiren aşağıdakiler gibi daha karmaşık bir ifadede de kullanabilirsiniz.

```powershell-interactive
"The computer name is " + $ActivityOutput['Get-AzureVm'].Name
```

### <a name="conditions"></a>Koşullar

Değerleri karşılaştırmak veya bir değerin belirtilen bir Düzenle eşleşip eşleşmediğini anlamak için [karşılaştırma işleçlerini](https://technet.microsoft.com/library/hh847759.aspx) kullanın. Karşılaştırma, $true veya $false bir değer döndürür.

Örneğin, aşağıdaki koşul *Get-AzureVM* adlı bir etkinliğin sanal makinenin Şu anda *durdurulmuş*olup olmadığını belirler.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"
```

Aşağıdaki koşul, aynı sanal makinenin *durdurulmuş*dışında bir durumda olup olmadığını denetler.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"
```

**-Ve** veya gibi bir [mantıksal işleç](https://technet.microsoft.com/library/hh847789.aspx) kullanarak birden çok koşulu **birleştirebilirsiniz.** Örneğin, aşağıdaki koşul, önceki örnekteki aynı sanal makinenin *durdurulmuş* veya *durduruluyor*durumunda olup olmadığını denetler.

```powershell-interactive
($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping")
```

### <a name="hashtables"></a>Tablolularıyla

[Hashtables](https://technet.microsoft.com/library/hh847780.aspx) bir değer kümesi döndürmek için yararlı olan ad/değer çiftleridir. Belirli etkinliklerin özellikleri basit bir değer yerine bir karma değer bekleyebilir. Ayrıca, bir sözlük olarak adlandırılan karma tablosu olarak da görebilirsiniz.

Aşağıdaki sözdizimiyle bir karma tablosu oluşturursunuz. Bir Hashtable herhangi bir sayıda girdi içerebilir, ancak her biri bir ad ve değer tarafından tanımlanır.

```powershell-interactive
@{ <name> = <value>; [<name> = <value> ] ...}
```

Örneğin, aşağıdaki ifade bir veri kaynağında, bir Hashtable 'ı bir internet araması için değerlerle beklenen bir etkinlik parametresi için kullanılmak üzere bir Hashtable oluşturur.

```powershell-interactive
$query = "Azure Automation"
$count = 10
$h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
$h
```

Aşağıdaki örnek, bir Hashtable 'ı doldurmak için *Twitter bağlantısını al* adlı bir etkinliğin çıktısını kullanır.

```powershell-interactive
@{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
    'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
    'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
    'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}
```

## <a name="next-steps"></a>Sonraki Adımlar

* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* Otomasyon farklı çalıştır hesabını kullanarak kimlik doğrulaması yapılacağını anlamak için bkz. [Azure farklı çalıştır hesabını yapılandırma](automation-sec-configure-azure-runas-account.md)
