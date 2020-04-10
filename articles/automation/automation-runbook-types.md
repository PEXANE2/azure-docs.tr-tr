---
title: Azure Otomasyon runbook türleri
description: Azure Otomasyonu'nda kullanabileceğiniz farklı runbook türlerini ve hangi türün kullanılacağını belirlerken dikkate almanız gereken hususları açıklar.
services: automation
ms.subservice: process-automation
ms.date: 03/05/2019
ms.topic: conceptual
ms.openlocfilehash: 10f9c829207dc17fa39711e273ae4fbfab3ecbcd
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010180"
---
# <a name="azure-automation-runbook-types"></a>Azure Otomasyon runbook türleri

Azure Otomasyon proses otomasyon hizmeti, çeşitli runbook türlerini destekler. Türler aşağıdaki tabloda kısaca tanımlanır ve aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır. İşlem otomasyonu ortamı hakkında bilgi edinmek için [Azure Otomasyonu'nda Runbook yürütmesi'ne](automation-runbook-execution.md)bakın.

| Tür | Açıklama |
|:--- |:--- |
| [Grafik](#graphical-runbooks)|Windows PowerShell'i temel alan ve Azure portalındaki grafik düzenleyicide tamamen oluşturulan ve düzenlenen grafik çalışma kitabı. |
| [Grafiksel PowerShell İş Akışı](#graphical-runbooks)|Windows PowerShell İş Akışı'nı temel alan ve Azure portalındaki grafik düzenleyicisinde tamamen oluşturulan ve düzenlenen grafik çalışma kitabı. |
| [PowerShell](#powershell-runbooks) |Windows PowerShell komut dosyasına dayalı metin çalışma kitabı. |
| [PowerShell İş Akışı](#powershell-workflow-runbooks)|Windows PowerShell İş Akışı komut dosyası na dayalı metin runbook. |
| [Python](#python-runbooks) |Python komut dosyasına dayalı metin runbook. |

## <a name="graphical-runbooks"></a>Grafik çalışma kitapları

Azure portalındaki grafik düzenleyiciyi kullanarak grafiksel ve grafiksel PowerShell İş Akışı çalışma kitapları oluşturabilir ve edinebilirsiniz. Ancak, bu tür runbook'u başka bir araçla oluşturamaz veya düzenlemeyapamazsınız.

Bir grafik runbook aşağıdaki ana özelliklere sahiptir:

* Otomasyon hesabınızdaki bir dosyaya aktarılabilir ve sonra başka bir Otomasyon hesabına aktarılabilir. 
* PowerShell kodu oluşturur. 
* Alma sırasında grafiksel PowerShell İş Akışı runbook'una veya grafiksel PowerShell İş Akışı runbook'una dönüştürülebilir. 

### <a name="advantages"></a>Yararları

* Görsel ekle-bağlantı yapılandırma yazma modelini kullanır.
* Verilerin işlem boyunca nasıl aktığına odaklanır.
* Yönetim süreçlerini görsel olarak temsil eder.
* Üst düzey iş akışları oluşturmak için alt runbook'lar olarak diğer runbook'ları içerir.
* Modüler programlamayı teşvik eder.

### <a name="limitations"></a>Sınırlamalar

* Azure portalı dışında oluşturulamaz veya düzenlenemez.
* Karmaşık mantığı çalıştırmak için PowerShell kodu içeren bir kod etkinliği gerektirebilir.
* [Metin biçimlerinden](automation-runbook-types.md)birine dönüştürülemez ve metin runbook'u grafik biçimine dönüştürülemez. 
* Grafik iş akışının oluşturduğu PowerShell kodunu görüntülemenize veya doğrudan değiştirmenize izin vermez. Oluşturduğunuz kodu herhangi bir kod etkinliklerinde görüntüleyebilirsiniz.
* Linux Hybrid Runbook Worker'da çalışmıyor. [Bkz. Karma Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları otomatikleştirin.](automation-hybrid-runbook-worker.md)

## <a name="powershell-runbooks"></a>PowerShell runbook'lar

PowerShell runbook'ları Windows PowerShell'e dayanır. Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan siz düzenliyorsunuz.  Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook'u](manage-runbooks.md) Azure Otomasyonu'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* PowerShell İş Akışı'nın ek karmaşıklıkları olmadan PowerShell koduyla tüm karmaşık mantığı uygulayın.
* Runbook, çalıştırmadan önce derlenmesine gerek olmadığı için PowerShell İş Akışı runbook'lardan daha hızlı başlar.
* Azure'da veya hem Linux hem de Windows Karma Runbook Çalışanları'nda çalıştırılabilir

### <a name="limitations"></a>Sınırlamalar

* PowerShell komut dosyası ile aşina olmalıdır.
* Paralel olarak birden çok eylemi yürütmek için [paralel işleme](automation-powershell-workflow.md#parallel-processing) kullanamazsınız.
* Bir hata varsa, çalışma defterine devam etmek için [denetim noktalarını](automation-powershell-workflow.md#checkpoints) kullanamazsınız.
* PowerShell İş Akışı runbook'ları ve Grafik çalışma kitapları, yeni bir iş yaratan Start-AzureAutomationRunbook cmdlet'i kullanarak yalnızca alt runbook'lara eklenebilir.

### <a name="known-issues"></a>Bilinen Sorunlar

Aşağıda PowerShell runbook'larla ilgili güncel bilinen sorunlar yer alıyor.

* PowerShell runbooks, null değeri olan şifrelenmemiş [bir değişken kıymeti](automation-variables.md) geri alamaz.
* PowerShell runbook'lar, *~* adla değişken bir [kıymeti](automation-variables.md) geri alamaz.
* PowerShell runbook'taki bir döngüdeki Get-Process, yaklaşık 80 yinelemeden sonra çökebilir.
* PowerShell runbook, çıktı akışına aynı anda büyük miktarda veri yazmaya çalışırsa başarısız olabilir.   Genellikle büyük nesnelerle çalışırken yalnızca gereksinim duyduğunuz bilgileri çıkararak bu sorunu çözebilirsiniz.  Örneğin, *Get-Process*gibi bir şeyi çıkarmak yerine, *Alma İşlemi | ProcessName, CPU'yi seçin.*

## <a name="powershell-workflow-runbooks"></a>PowerShell İş Akışı runbook'ları

PowerShell İş Akışı runbook'ları [Windows PowerShell İş Akışı'nı](automation-powershell-workflow.md)temel alan metin runbook'larıdır.  Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan siz düzenliyorsunuz.  Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook'u](manage-runbooks.md) Azure Otomasyonu'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* PowerShell İş Akışı kodu ile tüm karmaşık mantığı uygulayın.
* Bir hata varsa runbook devam etmek için [denetim noktalarını](automation-powershell-workflow.md#checkpoints) kullanın.
* Paralel olarak birden çok eylem gerçekleştirmek için [paralel işleme](automation-powershell-workflow.md#parallel-processing) kullanın.
* Üst düzey iş akışları oluşturmak için diğer Grafik çalışma defterlerini ve PowerShell İş Akışı runbook'larını alt runbook'lar olarak içerebilir.

### <a name="limitations"></a>Sınırlamalar

* Yazar PowerShell İş Akışı aşina olmalıdır.
* [Runbook, deserialized nesneler](automation-powershell-workflow.md#code-changes)gibi PowerShell İş akışı ek karmaşıklığı ile uğraşmak zorundadır.
* Runbook'un çalıştırılmadan önce derlenmesine ihtiyaç olduğu için PowerShell runbook'lardan daha uzun zaman alır.
* PowerShell runbook'lar, yeni bir iş yaratan Start-AzureAutomationRunbook cmdlet'i kullanarak yalnızca alt çalışma kitapları olarak eklenebilir.
* Linux Hybrid Runbook Worker'da çalıştırılamıyorum.

## <a name="python-runbooks"></a>Python runbooks

Python runbooks Python 2 altında derler. Azure portalındaki metin düzenleyicisini kullanarak veya çevrimdışı bir metin düzenleyicisi ile runbook kodunu doğrudan düzenleyebilir ve runbook'u Azure Otomasyonu'na [aktarabilirsiniz.](manage-runbooks.md)

### <a name="advantages"></a>Yararları

* Sağlam Python kitaplıklarını kullan.
* Azure'da veya her iki Linux Karma Runbook Çalışanı'nda çalıştırılabilir. Windows Hybrid Runbook Workers [python2.7](https://www.python.org/downloads/release/latest/python2) yüklü olarak desteklenir.

### <a name="limitations"></a>Sınırlamalar

* Python komut dosyası na aşina olmalı.
* Şu anda yalnızca Python 2 desteklenir, bu da Python 3'ün belirli işlevlerinin başarısız olacağı anlamına gelir.
* Üçüncü taraf kitaplıklarını kullanmak için paketi kullanmak için Otomasyon hesabına [aktarmanız](python-packages.md) gerekir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Belirli bir runbook için hangi türde kullanılacağını belirlerken aşağıdaki ek hususları göz önünde bulundurun.

* Runbook'ları grafikten metin türüne veya başka bir şekilde dönüştüremezsiniz.
* Alt çalışma kitabı olarak farklı türde runbook'lar kullanılarak sınırlamalar vardır. Daha fazla bilgi için [Azure Otomasyonu'ndaki Alt runbook'lar'a](automation-child-runbooks.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik çalışma kitabı yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Grafik selyazma](automation-graphical-authoring-intro.md)
* Runbook'lar için PowerShell ve PowerShell iş akışları arasındaki farkları anlamak için [Windows PowerShell İş Akışını Öğrenme'ye](automation-powershell-workflow.md) bakın
* Runbook oluşturma veya alma hakkında daha fazla bilgi için [runbook oluşturma veya alma](manage-runbooks.md) hakkında bkz.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
