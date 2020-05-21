---
title: Azure Otomasyonu runbook türleri
description: Azure Otomasyonu 'nda kullanabileceğiniz farklı runbook türlerini ve hangi türün kullanılacağını belirlemek için önemli noktaları açıklar.
services: automation
ms.subservice: process-automation
ms.date: 03/05/2019
ms.topic: conceptual
ms.openlocfilehash: 390f14e8369f206b2f5ffce74f0775b33e313021
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714993"
---
# <a name="azure-automation-runbook-types"></a>Azure Otomasyonu runbook türleri

Azure Otomasyonu işlem Otomasyonu hizmeti, aşağıdaki tabloda tanımlandığı gibi çeşitli runbook türlerini destekler. İşlem otomasyonu ortamı hakkında bilgi edinmek için bkz. [Azure Otomasyonu 'Nda runbook yürütme](automation-runbook-execution.md).

| Tür | Açıklama |
|:--- |:--- |
| [Grafik](#graphical-runbooks)|Windows PowerShell 'i temel alan ve Azure portal grafik düzenleyicisinde tamamen oluşturulan ve düzenlenen grafik runbook 'u. |
| [Grafik PowerShell Iş akışı](#graphical-runbooks)|Windows PowerShell Iş akışına dayalı grafik runbook 'u ve Azure portal içindeki grafik düzenleyicide tamamen oluşturulmuş ve düzenlenmiş. |
| [PowerShell](#powershell-runbooks) |Windows PowerShell komut dosyası tabanlı metin runbook 'u. |
| [PowerShell İş Akışı](#powershell-workflow-runbooks)|Windows PowerShell Iş akışı komut dosyası tabanlı metin runbook 'u. |
| [Python](#python-runbooks) |Python betik temelli metin runbook 'u. |

Belirli bir runbook için kullanılacak türü belirlerken aşağıdaki noktaları dikkate alın.

* Runbook 'ları grafik 'ten metin türüne veya başka bir şekilde dönüştüremezsiniz.
* Farklı türlerde runbook 'ların alt runbook 'lar olarak kullanılması için sınırlamalar vardır. Daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda alt runbook 'lar](automation-child-runbooks.md).

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="graphical-runbooks"></a>Grafik runbook 'ları

Azure portal grafik düzenleyicisini kullanarak grafik ve grafik PowerShell Iş akışı runbook 'ları oluşturup düzenleyebilirsiniz. Ancak, başka bir araçla bu tür bir runbook oluşturamaz veya düzenleyemezsiniz. Grafik runbook 'ların ana özellikleri:

* Otomasyon hesabınızdaki dosyalara aktarılabilir ve daha sonra başka bir Otomasyon hesabına aktarılabilir. 
* PowerShell kodu oluştur. 
* İçeri aktarma sırasında grafik PowerShell Iş akışı runbook 'larına veya öğesinden dönüştürülebilir. 

### <a name="advantages"></a>Yararları

* Visual Insert-Link-configure Authoring model kullanın.
* Verilerin süreç boyunca nasıl akacağını odaklayın.
* Yönetim süreçlerini görsel olarak temsil eder.
* Üst düzey iş akışları oluşturmak için diğer runbook 'ları alt runbook 'lar olarak ekleyin.
* Modüler programlamayı teşvik edin.

### <a name="limitations"></a>Sınırlamalar

* Azure portal dışında oluşturulamıyor veya düzenlenemiyor.
* Karmaşık mantık yürütmek için PowerShell kodu içeren bir kod etkinliği gerekebilir.
* [Metin biçimlerinden](automation-runbook-types.md)birine dönüştürülemez, ya da bir metin runbook 'unu grafik biçimine dönüştürebilirsiniz. 
* Grafik iş akışının oluşturduğu PowerShell kodunu görüntüleyemez veya doğrudan düzenleyemezsiniz. Oluşturduğunuz kodu herhangi bir kod etkinliği içinde görüntüleyebilirsiniz.
* Bir Linux karma Runbook Worker üzerinde runbook 'lar çalıştırılamıyor. Bkz. [karma runbook çalışanı kullanarak veri merkezinizdeki veya buluttaki kaynakları otomatikleştirme](automation-hybrid-runbook-worker.md).

## <a name="powershell-runbooks"></a>PowerShell runbook 'ları

PowerShell runbook 'ları Windows PowerShell 'i temel alır. Azure portal metin düzenleyicisini kullanarak runbook 'un kodunu doğrudan düzenleyebilirsiniz.  Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook 'U](manage-runbooks.md) Azure Otomasyonu 'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* PowerShell Iş akışı ek karmaşıklıklarını olmadan tüm karmaşık mantığı PowerShell koduyla birlikte uygulayın.
* Çalıştırmadan önce derlenmesi gerekmediğinden, PowerShell Iş akışı runbook 'lardan daha hızlı bir başlangıç yapın.
* Hem Windows hem de Linux için Azure 'da ve karma runbook çalışanlarında çalıştırın.

### <a name="limitations"></a>Sınırlamalar

* PowerShell betiği hakkında bilgi sahibi olmanız gerekir.
* Runbook 'lar paralel olarak birden çok eylem yürütmek için [paralel işleme](automation-powershell-workflow.md#use-parallel-processing) kullanamaz.
* Runbook 'lar bir hata oluşursa runbook 'u yeniden başlatmak için [denetim noktaları](automation-powershell-workflow.md#use-checkpoints-in-a-workflow) kullanamaz.
* Yeni bir iş oluşturan [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) cmdlet 'ini kullanarak yalnızca PowerShell iş akışı runbook 'larını ve grafik runbook 'larını alt runbook 'lar olarak ekleyebilirsiniz.

### <a name="known-issues"></a>Bilinen sorunlar

PowerShell runbook 'larında şu andaki bilinen sorunlar şunlardır:

* PowerShell runbook 'ları null değeri olan şifrelenmemiş bir [değişken varlığı](automation-variables.md) alamıyor.
* PowerShell runbook 'ları, adında bir değişken varlık alamıyor `*~*` .
* Bir PowerShell runbook 'unda döngüdeki bir [Get-Process](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-process?view=powershell-7) işlemi, 80 yinelemeden sonra çökebilir.
* Bir PowerShell runbook 'u aynı anda çıkış akışına büyük miktarda veri yazmayı denediğinde başarısız olabilir. Genellikle bu soruna geçici bir çözüm olarak, runbook 'un büyük nesnelerle çalışması için gereken bilgileri vermesini sağlayabilirsiniz. Örneğin, hiçbir kısıtlama olmadan kullanmak yerine `Get-Process` cmdlet 'in yalnızca ' de olduğu gibi gerekli parametreleri çıkışına sahip olabilirsiniz `Get-Process | Select ProcessName, CPU` .

## <a name="powershell-workflow-runbooks"></a>PowerShell Iş akışı runbook 'ları

PowerShell Iş akışı runbook 'ları [Windows PowerShell Iş akışını](automation-powershell-workflow.md)temel alan metin runbook 'larıdır. Azure portal metin düzenleyicisini kullanarak runbook 'un kodunu doğrudan düzenleyebilirsiniz. Ayrıca herhangi bir çevrimdışı metin düzenleyicisini kullanabilir ve [runbook 'U](manage-runbooks.md) Azure Otomasyonu 'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* PowerShell Iş akışı kodu ile tüm karmaşık mantığı uygulayın.
* Bir hata oluşursa, işlemi sürdürmeye yönelik [denetim noktaları](automation-powershell-workflow.md#use-checkpoints-in-a-workflow) kullanın.
* Paralel olarak birden çok eylem yapmak için [paralel işleme](automation-powershell-workflow.md#use-parallel-processing) kullanın.
* , Üst düzey iş akışları oluşturmak için alt runbook 'lar olarak diğer grafik runbook 'larını ve PowerShell Iş akışı runbook 'larını içerebilir.

### <a name="limitations"></a>Sınırlamalar

* PowerShell Iş akışı hakkında bilgi sahibi olmanız gerekir.
* Runbook 'lar, [Serisi kaldırılan nesneler](automation-powershell-workflow.md#deserialized-objects)gibi PowerShell iş akışının ek karmaşıklığı ile uğraşmalıdır.
* Runbook 'lar çalıştırılmadan önce derlenmesi gerektiğinden PowerShell runbook 'lardan daha uzun sürer.
* PowerShell runbook 'larını yalnızca cmdlet 'ini kullanarak alt runbook 'lar olarak ekleyebilirsiniz `Start-AzAutomationRunbook` .
* Runbook 'lar Linux karma Runbook Worker üzerinde çalıştırılamaz.

## <a name="python-runbooks"></a>Python runbook 'ları

Python runbook 'ları Python 2 altında derlenir. Azure portal metin düzenleyicisini kullanarak runbook 'un kodunu doğrudan düzenleyebilirsiniz. Ayrıca, çevrimdışı bir metin Düzenleyicisi kullanabilir ve [runbook 'U](manage-runbooks.md) Azure Otomasyonu 'na aktarabilirsiniz.

### <a name="advantages"></a>Yararları

* Sağlam Python kitaplıklarını kullanın.
* , Azure 'da veya Linux karma runbook çalışanları üzerinde çalışabilir. Windows karma runbook çalışanları, [Python 2.7](https://www.python.org/downloads/release/latest/python2) yüklü olarak desteklenir.

### <a name="limitations"></a>Sınırlamalar

* Python betiği hakkında bilgi sahibi olmanız gerekir.
* Şu anda yalnızca Python 2 desteklenir. Python 3 ' ü özel işlevler başarısız olur.
* Üçüncü taraf kitaplıklarını kullanmak için paketleri Otomasyon hesabına [aktarmanız](python-packages.md) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik runbook 'u yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda grafik yazma](automation-graphical-authoring-intro.md).
* Runbook 'lar için PowerShell ve PowerShell iş akışları arasındaki farkları anlamak için bkz. [Windows PowerShell Iş akışını öğrenme](automation-powershell-workflow.md).
* Runbook oluşturma veya içeri aktarma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md).
* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi edinmek için [PowerShell docs](https://docs.microsoft.com/powershell/scripting/overview)bölümüne bakın.
