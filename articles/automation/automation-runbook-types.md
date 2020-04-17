---
title: Azure Otomasyon runbook türleri
description: Azure Otomasyonu'nda kullanabileceğiniz farklı runbook türlerini ve hangi türün kullanılacağını belirlemek için göz önünde bulundurulması gerekenleri açıklar.
services: automation
ms.subservice: process-automation
ms.date: 03/05/2019
ms.topic: conceptual
ms.openlocfilehash: 1ac6347bd8e723f356da4803da54a6ea45a4a71a
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535528"
---
# <a name="azure-automation-runbook-types"></a>Azure Otomasyon runbook türleri

Azure Otomasyon işlem otomasyon hizmeti, aşağıdaki tabloda tanımlandığı gibi çeşitli runbook türlerini destekler. İşlem otomasyonu ortamı hakkında bilgi edinmek için [Azure Otomasyonu'nda Runbook yürütmesi'ne](automation-runbook-execution.md)bakın.

| Tür | Açıklama |
|:--- |:--- |
| [Grafik](#graphical-runbooks)|Windows PowerShell'i temel alan ve Azure portalındaki grafik düzenleyicide tamamen oluşturulan ve düzenlenen grafik çalışma kitabı. |
| [Grafiksel PowerShell İş Akışı](#graphical-runbooks)|Windows PowerShell İş Akışı'nı temel alan ve Azure portalındaki grafik düzenleyicisinde tamamen oluşturulan ve düzenlenen grafik çalışma kitabı. |
| [PowerShell](#powershell-runbooks) |Windows PowerShell komut dosyasına dayalı metin çalışma kitabı. |
| [PowerShell İş Akışı](#powershell-workflow-runbooks)|Windows PowerShell İş Akışı komut dosyası na dayalı metin runbook. |
| [Python](#python-runbooks) |Python komut dosyasına dayalı metin runbook. |

Belirli bir runbook için hangi türde kullanılacağını belirlerken aşağıdaki hususları göz önünde bulundurun.

* Runbook'ları grafikten metin türüne veya başka bir şekilde dönüştüremezsiniz.
* Alt runbook'lar olarak farklı türlerde runbookkullanırken sınırlamalar vardır. Daha fazla bilgi için [Azure Otomasyonu'ndaki Alt runbook'lar'a](automation-child-runbooks.md)bakın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="graphical-runbooks"></a>Grafik çalışma kitapları

Azure portalındaki grafik düzenleyiciyi kullanarak grafiksel ve grafiksel PowerShell İş Akışı çalışma kitapları oluşturabilir ve edinebilirsiniz. Ancak, bu tür runbook'u başka bir araçla oluşturamaz veya düzenlemeyapamazsınız. Grafik çalışma kitaplarının temel özellikleri:

* Otomasyon hesabınızdaki dosyalara aktarılabilir ve daha sonra başka bir Otomasyon hesabına aktarılabilir. 
* PowerShell kodunu oluşturun. 
* Alma sırasında grafiksel PowerShell İş Akışı runbook'larına veya grafiksel PowerShell İş Akışı runbook'larına dönüştürülebilir. 

### <a name="advantages"></a>Yararları

* Görsel ekle-bağlantı-yapılandırma yazma modelini kullanın.
* Verilerin işlem boyunca nasıl aktığına odaklanın.
* Yönetim süreçlerini görsel olarak temsil edin.
* Üst düzey iş akışları oluşturmak için diğer runbook'ları alt runbook'lara ekleyin.
* Modüler programlamayı teşvik edin.

### <a name="limitations"></a>Sınırlamalar

* Azure portalı nın dışında oluşturamaz veya kaldıramaz.
* Karmaşık mantığı çalıştırmak için PowerShell kodu içeren bir kod etkinliği gerektirebilir.
* [Metin biçimlerinden](automation-runbook-types.md)birine dönüştüremezsiniz veya metin runbook'u grafik biçimine dönüştüremezsiniz. 
* Grafik iş akışının oluşturduğu PowerShell kodunu görüntüleyemez veya doğrudan kaldır. Oluşturduğunuz kodu herhangi bir kod etkinliklerinde görüntüleyebilirsiniz.
* Linux Hybrid Runbook Worker'da runbook çalıştıramaz. [Bkz. Karma Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları otomatikleştirin.](automation-hybrid-runbook-worker.md)

## <a name="powershell-runbooks"></a>PowerShell runbook'lar

PowerShell runbook'ları Windows PowerShell'e dayanır. Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan siz düzenliyorsunuz.  Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook'u](manage-runbooks.md) Azure Otomasyonu'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* PowerShell İş Akışı'nın ek karmaşıklıkları olmadan PowerShell koduyla tüm karmaşık mantığı uygulayın.
* Çalıştırmadan önce derlenmeleri gerekmediğinden PowerShell İş Akışı runbook'lardan daha hızlı başlayın.
* Hem Windows hem de Linux için Azure'da ve Karma Runbook Çalışanları'nda çalıştırın.

### <a name="limitations"></a>Sınırlamalar

* PowerShell komut dosyasına aşina olmalısınız.
* Runbook'lar, birden çok eylemi paralel olarak yürütmek için [paralel işleme](automation-powershell-workflow.md#parallel-processing) kullanamaz.
* [Runbook'lar,](automation-powershell-workflow.md#checkpoints) bir hata varsa runbook'u devam ettirmek için denetim noktalarını kullanamaz.
* Yeni bir iş yaratan [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) cmdlet'i kullanarak yalnızca PowerShell İş Akışı runbook'larını ve grafik runbook'ları alt runbook olarak ekleyebilirsiniz.

### <a name="known-issues"></a>Bilinen sorunlar

PowerShell runbook'ları ile ilgili güncel bilinen sorunlar şunlardır:

* PowerShell runbook'lar, null değeri olan şifrelenmemiş bir [değişken kıymeti](automation-variables.md) geri alamaz.
* PowerShell runbook'lar, `*~*` adla değişken bir kıymeti geri alamaz.
* PowerShell runbook'taki bir döngüdeki [Get-Process](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-process?view=powershell-7) işlemi, yaklaşık 80 yinelemeden sonra çökebilir.
* PowerShell runbook, çıktı akışına aynı anda büyük miktarda veri yazmaya çalışırsa başarısız olabilir. Genellikle runbook çıktısı yalnızca büyük nesnelerle çalışmak için gerekli bilgileri alarak bu sorunu çözmek. Örneğin, hiçbir sınırlama `Get-Process` olmaksızın kullanmak yerine, cmdlet çıktısını sadece gerekli `Get-Process | Select ProcessName, CPU`parametreleri .

## <a name="powershell-workflow-runbooks"></a>PowerShell İş Akışı runbook'ları

PowerShell İş Akışı runbook'ları [Windows PowerShell İş Akışı'nı](automation-powershell-workflow.md)temel alan metin runbook'larıdır. Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan siz düzenliyorsunuz. Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook'u](manage-runbooks.md) Azure Otomasyonu'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* PowerShell İş Akışı kodu ile tüm karmaşık mantığı uygulayın.
* Bir hata varsa çalışmaya devam etmek için [denetim noktalarını](automation-powershell-workflow.md#checkpoints) kullanın.
* Paralel olarak birden çok eylem yapmak için [paralel işleme](automation-powershell-workflow.md#parallel-processing) kullanın.
* Üst düzey iş akışları oluşturmak için diğer grafik runbook'ları ve PowerShell İş Akışı runbook'larını alt runbook'lar olarak içerebilir.

### <a name="limitations"></a>Sınırlamalar

* PowerShell İş Akışı'na aşina olmalısınız.
* Runbook'lar, [seri dışı nesneler](automation-powershell-workflow.md#code-changes)gibi PowerShell İş Akışının ek karmaşıklığıyla başa çıkmalıdır.
* Runbook'ların çalıştırılmadan önce derlenmeleri gerektiğinden, PowerShell runbook'lardan daha uzun süre ler.
* PowerShell runbook'larını yalnızca `Start-AzAutomationRunbook` cmdlet'i kullanarak alt runbook olarak ekleyebilirsiniz.
* Runbook'lar Linux Hybrid Runbook Worker'da çalıştırılamıyor.

## <a name="python-runbooks"></a>Python runbooks

Python runbooks Python 2 altında derler. Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan ayarlayabilirsiniz. Çevrimdışı metin düzenleyicisi de kullanabilir ve [runbook'u](manage-runbooks.md) Azure Otomasyonu'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* Sağlam Python kitaplıklarını kullanın.
* Azure'da veya Linux Hybrid Runbook Workers'da çalıştırılabilir. Windows Hybrid Runbook Workers [python2.7](https://www.python.org/downloads/release/latest/python2) yüklü olarak desteklenir.

### <a name="limitations"></a>Sınırlamalar

* Python komut dosyasına aşina olmalısınız.
* Şu anda yalnızca Python 2 desteklenir. Python 3'e özgü işlevler başarısız olursa.
* Üçüncü taraf kitaplıklarını kullanmak için paketleri Otomasyon hesabına [aktarmanız](python-packages.md) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik çalışma kitabı yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Grafik selyazma kitabına](automation-graphical-authoring-intro.md)bakın.
* Runbook'lar için PowerShell ve PowerShell iş akışları arasındaki farkları anlamak için [Windows PowerShell İş Akışını Öğrenme'ye](automation-powershell-workflow.md)bakın.
* Runbook oluşturma veya alma hakkında daha fazla bilgi için Azure [Otomasyonunda runbook'ları Yönet'e](manage-runbooks.md)bakın.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi edinmek için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
