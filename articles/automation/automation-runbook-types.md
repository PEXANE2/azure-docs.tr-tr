---
title: Azure Otomasyon Runbook Türleri
description: "Azure Otomasyonu'nda kullanabileceğiniz farklı runbook türlerini ve hangi türün kullanılacağını belirlerken dikkate almanız gereken hususları açıklar. "
services: automation
ms.subservice: process-automation
ms.date: 03/05/2019
ms.topic: conceptual
ms.openlocfilehash: 6346c29210b6390f11c884ff51e0b60af89bbbb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278616"
---
# <a name="azure-automation-runbook-types"></a>Azure Otomasyon runbook türleri

Azure Otomasyonu, aşağıdaki tabloda kısaca açıklanan çeşitli runbook türlerini destekler.  Aşağıdaki bölümlerde her bir türü kullanmak için ne zaman hususlar da dahil olmak üzere her türü hakkında daha fazla bilgi sağlar.

| Tür | Açıklama |
|:--- |:--- |
| [Grafik](#graphical-runbooks)|Windows PowerShell'e dayalı olarak Azure portalında tamamen grafik düzenleyicide oluşturulmuş ve düzenlenmiş. |
| [Grafiksel PowerShell İş Akışı](#graphical-runbooks)|Windows PowerShell İş Akışı'na dayalı olarak, Azure portalındaki grafik düzenleyicide tamamen oluşturuldu ve düzenlendi. |
| [Powershell](#powershell-runbooks) |Windows PowerShell komut dosyasına dayalı metin runbook. |
| [PowerShell İş Akışı](#powershell-workflow-runbooks)|Windows PowerShell İş Akışı'nı temel alan metin çalışma kitabı. |
| [Python](#python-runbooks) |Python'u temel alan metin runbook' u. |

## <a name="graphical-runbooks"></a>Grafik çalışma kitapları

[Grafik ve Grafiksel](automation-runbook-types.md#graphical-runbooks) PowerShell İş Akışı runbook'ları oluşturulur ve Azure portalındaki grafik düzenleyicisi ile düzenlenir.  Bunları bir dosyaya dışa aktarıp başka bir otomasyon hesabına aktarabilirsiniz. Ancak bunları başka bir araçla oluşturamaz veya düzenlemeyapamazsınız. Grafik çalışma kitapları PowerShell kodu oluşturur, ancak kodu doğrudan görüntüleyebilir veya değiştiremezsiniz. Grafik çalışma kitapları [metin biçimlerinden](automation-runbook-types.md)birine dönüştürülemez ve metin runbook'u grafik biçimine dönüştürülemez. Grafik çalışma kitapları, içe aktarma sırasında ve diğer yolla Grafik PowerShell İş Akışı runbook'larına dönüştürülebilir.

### <a name="advantages"></a>Yararları

* Görsel ekle-bağlantı-yapılandırma yazma modeli
* Verilerin işlem boyunca nasıl aktığına odaklanın
* Yönetim süreçlerini görsel olarak temsil etmek
* Üst düzey iş akışları oluşturmak için diğer runbook'ları alt çalışma kitapları olarak ekleme
* Modüler programlamayı teşvik eder

### <a name="limitations"></a>Sınırlamalar

* Çalışma kitabını Azure portalı dışında da ayarlayamıyorum.
* Karmaşık mantığı çalıştırmak için PowerShell kodu içeren bir Kod etkinliği gerektirebilir.
* Grafik iş akışı tarafından oluşturulan PowerShell kodunu görüntüleyemez veya doğrudan düzenlemeyin. Oluşturduğunuz kodu herhangi bir Kod etkinliklerinde görüntüleyebilirsiniz.
* Linux Hybrid Runbook Worker'da çalıştırılamaz

## <a name="powershell-runbooks"></a>PowerShell runbook'lar

PowerShell runbook'ları Windows PowerShell'e dayanır.  Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan siz düzenliyorsunuz.  Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook'u](manage-runbooks.md) Azure Otomasyonu'na aktarabilirsiniz.

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
* Linux Hybrid Runbook Worker'da çalıştırılamaz

## <a name="python-runbooks"></a>Python runbooks

Python runbooks Python 2 altında derler.  Azure portalındaki metin düzenleyicisini kullanarak veya çevrimdışı bir metin düzenleyicisi ile runbook kodunu doğrudan düzenleyebilir ve runbook'u Azure Otomasyonu'na [aktarabilirsiniz.](manage-runbooks.md)

### <a name="advantages"></a>Yararları

* Sağlam Python kitaplıklarını kullan.
* Azure'da veya her iki Linux Karma Runbook Çalışanı'nda çalıştırılabilir. Windows Hybrid Runbook Workers [python2.7](https://www.python.org/downloads/release/latest/python2) yüklü olarak desteklenir.

### <a name="limitations"></a>Sınırlamalar

* Python komut dosyası na aşina olmalı.
* Şu anda yalnızca Python 2 desteklenir, bu da Python 3'ün belirli işlevlerinin başarısız olacağı anlamına gelir.
* Üçüncü taraf kitaplıklarını kullanmak için paketi kullanmak için Otomasyon Hesabına [aktarmanız](python-packages.md) gerekir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Belirli bir runbook için hangi türde kullanılacağını belirlerken aşağıdaki ek hususları göz önünde bulundurun.

* Runbook'ları grafikten metin türüne veya başka bir şekilde dönüştüremezsiniz.
* Alt çalışma kitabı olarak farklı türde runbook'lar kullanılarak sınırlamalar vardır. Daha fazla bilgi için [Azure Otomasyonu'ndaki Alt runbook'lar'a](automation-child-runbooks.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik çalışma kitabı yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Grafik selyazma](automation-graphical-authoring-intro.md)
* Runbook'lar için PowerShell ve PowerShell iş akışları arasındaki farkları anlamak için [Windows PowerShell İş Akışını Öğrenme'ye](automation-powershell-workflow.md) bakın
* Runbook oluşturma veya alma hakkında daha fazla bilgi için [runbook oluşturma veya alma](manage-runbooks.md) hakkında bkz.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
