---
title: Orkestratörden Azure Otomasyonuna Geçiş
description: Runbook'ların ve tümleştirme paketlerinin System Center Orchestrator'dan Azure Otomasyonuna nasıl geçirilen leri açıklar.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: c7df6e31cd021fc61129131f9bd02acc7b96e2ad
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457561"
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Orkestratörden Azure Otomasyonuna Geçiş (Beta)

[System Center Orchestrator'daki](https://technet.microsoft.com/library/hh237242.aspx) runbook'lar, Azure Otomasyonu'ndaki runbook'lar Windows PowerShell'e dayanırken, Orkestratör için özel olarak yazılmış tümleştirme paketlerinden gelen etkinliklere dayanır.  Azure Otomasyonu'ndaki [grafik runbook'lar,](automation-runbook-types.md#graphical-runbooks) PowerShell cmdlets'i, alt runbook'ları ve varlıkları temsil eden etkinlikleriyle Orchestrator runbook'larına benzer bir görünüme sahiptir.

[System Center Orchestrator Migration Toolkit,](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) runbook'ları Orchestrator'dan Azure Otomasyonuna dönüştürmenize yardımcı olacak araçlar içerir.  Runbook'ları dönüştürmenin yanı sıra, tümleştirme paketlerini runbook'ların windows powershell cmdlets ile tümleştirme modüllerine kullandığı etkinliklerle dönüştürmeniz gerekir.  

Orchestrator runbook'ları Azure Otomasyonuna dönüştürmek için temel işlem aşağıda veda edilebilmektedir.  Bu adımların her biri aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

1. Bu makalede tartışılan araçları ve modülleri içeren [System Center Orchestrator Migration Toolkit'i](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) indirin.
2. [Standart Etkinlikler Modül'u](#standard-activities-module) Azure Otomasyonuna Aktarın.  Bu, dönüştürülmüş runbook'lar tarafından kullanılabilecek standart Orchestrator etkinliklerinin dönüştürülmüş sürümlerini içerir.
3. Sistem Merkezi'ne erişen runbook'larınız tarafından kullanılan tümleştirme paketleri için [Sistem Merkezi Orkestrator Entegrasyon Modüllerini](#system-center-orchestrator-integration-modules) Azure Otomasyonuna Aktarın.
4. [Tümleştirme Paketi Dönüştürücü'nü](#integration-pack-converter) kullanarak özel ve üçüncü taraf tümleştirme paketlerini dönüştürün ve Azure Otomasyonuna aktarın.
5. [Runbook Converter'ı](#runbook-converter) kullanarak Orchestrator runbook'ları dönüştürün ve Azure Otomasyonu'na yükleyin.
6. Runbook Dönüştürücü bu kaynakları dönüştürmediğinden Azure Otomasyonu'nda gerekli Orchestrator varlıklarını el ile oluşturun.
7. Yerel kaynaklara erişecek dönüştürülmüş runbook'ları çalıştırmak için yerel veri merkezinizde bir [Karma Runbook Çalışanı](#hybrid-runbook-worker) yapılandırın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="service-management-automation"></a>Hizmet Yönetimi Otomasyonu

[Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx) (SMA), Orchestrator gibi yerel veri merkezinizde runbook'ları depolar ve çalıştırAr ve Azure Otomasyonu ile aynı tümleştirme modüllerini kullanır. [Runbook Converter,](#runbook-converter) Orchestrator runbook'ları SMA'da desteklenmeyen grafik runbook'lara dönüştürür.  [Standart Etkinlikler Modülü](#standard-activities-module) ve Sistem Merkezi [Orkestrasyon Entegrasyon Modüllerini](#system-center-orchestrator-integration-modules) SMA'ya yüklemeye devam edebilirsiniz, ancak [runbook'larınızı](https://technet.microsoft.com/library/dn469262.aspx)el ile yeniden yazmanız gerekir.

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı

Orchestrator'daki runbook'lar bir veritabanı sunucusunda depolanır ve her ikisi de yerel veri merkezinizde runbook sunucularında çalıştırılır.  Azure Otomasyonundaki runbook'lar Azure bulutunda depolanır ve [Karma Runbook Çalışanı](automation-hybrid-runbook-worker.md)kullanarak yerel veri merkezinizde çalıştırılabilir.  Yerel sunucularda çalışacak şekilde tasarlandıkları için, genellikle Orchestrator'dan dönüştürülmüş runbook'ları bu şekilde çalıştırabilirsiniz.

## <a name="integration-pack-converter"></a>Entegrasyon Paketi Dönüştürücü

Tümleştirme Paketi Dönüştürücü, [Orchestrator Integration Toolkit (OIT)](https://technet.microsoft.com/library/hh855853.aspx) kullanılarak oluşturulan tümleştirme paketlerini, Azure Otomasyonu veya Hizmet Yönetimi Otomasyonuna aktarılabilen Windows PowerShell tabanlı tümleştirme modüllerine dönüştürür.  

Tümleştirme Paketi Dönüştürücü'nü çalıştırdığınızda, bir tümleştirme paketi (.oip) dosyası seçmenize olanak tanıyan bir sihirbaz sunulur.  Sihirbaz daha sonra bu tümleştirme paketinde yer alan etkinlikleri listeler ve hangilerinin geçirileceğini seçmenize olanak tanır.  Sihirbazı tamamladığınızda, özgün tümleştirme paketindeki her etkinlik için karşılık gelen bir cmdlet içeren bir tümleştirme modülü oluşturur.

### <a name="parameters"></a>Parametreler

Tümleştirme paketindeki bir etkinliğin tüm özellikleri, entegrasyon modülündeki ilgili cmdlet parametrelerine dönüştürülür.  Windows PowerShell cmdlets tüm cmdlets ile kullanılabilecek [ortak parametreler](https://technet.microsoft.com/library/hh847884.aspx) kümesi vardır. Örneğin, -Verbose parametresi, bir cmdletin çalışması hakkında ayrıntılı bilgi çıkışına neden olur.  Hiçbir cmdletin ortak bir parametreyle aynı ada sahip bir parametresi olabilir. Bir etkinlik ortak bir parametre yle aynı ada sahip bir özelliğe sahipse, sihirbaz parametre için başka bir ad vermenizi ister.

### <a name="monitor-activities"></a>Etkinlikleri izleme

Orchestrator'daki runbook'ları izleyin bir [monitör etkinliğiyle](https://technet.microsoft.com/library/hh403827.aspx) başlar ve belirli bir olay tarafından çağrılmayı bekleyen sürekli olarak çalıştırın.  Azure Otomasyonu izleme runbook'larını desteklemez, bu nedenle tümleştirme paketindeki tüm monitör etkinlikleri dönüştürülmez.  Bunun yerine, monitör etkinliği için entegrasyon modülünde bir yer tutucu cmdlet oluşturulur.  Bu cmdlet hiçbir işlevselliği vardır, ancak yüklü olması için kullanan herhangi bir dönüştürülmüş runbook sağlar.  Bu runbook Azure Otomasyonu'nda çalıştırılamaz, ancak değiştirebilmeniz için yüklenebilir.

### <a name="integration-packs-that-cannot-be-converted"></a>Dönüştürülemeyen tümleştirme paketleri

OIT ile oluşturulmayan tümleştirme paketleri Tümleştirme Paketi Dönüştürücüile dönüştürülemez. Microsoft tarafından sağlanan ve şu anda bu araçla dönüştürülemeyen bazı tümleştirme paketleri de vardır.  Azure Otomasyonu veya Hizmet Yönetimi Otomasyonu'na yüklenebilmeleri için bu tümleştirme paketlerinin dönüştürülmüş sürümleri [indirilmeye hazırdır.](#system-center-orchestrator-integration-modules)

## <a name="standard-activities-module"></a>Standart Faaliyetler Modülü

Orchestrator, tümleştirme paketine dahil olmayan ancak birçok runbook tarafından kullanılan bir dizi [standart etkinlik](https://technet.microsoft.com/library/hh403832.aspx) içerir.  Standart Etkinlikler modülü, bu faaliyetlerin her biri için bir cmdlet eşdeğeri içeren bir entegrasyon modülüdür.  Standart bir etkinlik kullanan dönüştürülmüş runbook'ları içe aktarmadan önce bu tümleştirme modüllerini Azure Otomasyonu'na yüklemeniz gerekir.

Dönüştürülmüş runbook'ları desteklemenin yanı sıra, standart etkinlikler modülündeki cmdletler, Azure Otomasyonu'nda yeni runbook'lar oluşturmak için Orchestrator'u tanıyan biri tarafından kullanılabilir.  Tüm standart etkinliklerin işlevselliği cmdlets ile yapılabilse de, farklı çalışabilir.  Dönüştürülmüş standart faaliyetler modülündeki cmdletler, ilgili faaliyetleriyle aynı şekilde çalışacak ve aynı parametreleri kullanacaktır.  Bu, mevcut Orchestrator runbook yazarının Azure Otomasyon runbook'larına geçişinde yardımcı olabilir.

## <a name="system-center-orchestrator-integration-modules"></a>Sistem Merkezi Orkestrasyon Entegrasyon Modülleri

Microsoft, Sistem Merkezi bileşenlerini ve diğer ürünleri otomatikleştirmek için çalışma kitapları oluşturmak için [tümleştirme paketleri](https://technet.microsoft.com/library/hh295851.aspx) sağlar.  Bu tümleştirme paketlerinin bazıları şu anda OIT'ye dayalıdır, ancak bilinen sorunlar nedeniyle şu anda tümleştirme modüllerine dönüştürülemez.  [System Center Orchestrator Integration Modules,](https://www.microsoft.com/download/details.aspx?id=49555) Azure Otomasyonu ve Hizmet Yönetimi Otomasyonu'na aktarılabilen bu tümleştirme paketlerinin dönüştürülmüş sürümlerini içerir.  

Bu aracın RTM sürümünde, Tümleştirme Paketi Dönüştürücüile dönüştürülebilen OIT'ye dayalı tümleştirme paketlerinin güncelleştirilmiş sürümleri yayınlanacaktır.  Ayrıca, OIT'ye dayanmayan tümleştirme paketlerinden gelen etkinlikleri kullanarak runbook'ları dönüştürmenize yardımcı olmak için rehberlik de sağlanacaktır.

## <a name="runbook-converter"></a>Runbook Dönüştürücü

Runbook Converter, Orchestrator runbook'ları Azure Otomasyonuna aktarılabilen [grafik runbook'lara](automation-runbook-types.md#graphical-runbooks) dönüştürür.  

Runbook Converter, dönüştürmeyi gerçekleştiren bir cmdlet `ConvertFrom-SCORunbook` ile PowerShell modülü olarak uygulanır.  Aracı yüklediğinizde, cmdlet'i yükleyen bir PowerShell oturumuna kısayol oluşturur.   

Bir Orchestrator runbook'u dönüştürüp Azure Otomasyonuna aktarmanın temel işlemi aşağıda veda edilebilmektedir.  Aşağıdaki bölümlerde aracı kullanma ve dönüştürülmüş runbook'larla çalışma hakkında daha fazla bilgi verilmiştir.

1. Orchestrator'dan bir veya daha fazla runbook dışa aktarın.
2. Runbook'taki tüm etkinlikler için tümleştirme modülleri edinin.
3. Dışa aktarılan dosyadaki Orchestrator runbook'ları dönüştürün.
4. Dönüşümü doğrulamak ve gerekli el ile görevleri belirlemek için günlüklerde bilgileri gözden geçirin.
5. Dönüştürülmüş runbook'ları Azure Otomasyonu'na aktarın.
6. Azure Otomasyonu'nda gerekli varlıkları oluşturun.
7. Gerekli etkinlikleri değiştirmek için Azure Automation'da runbook'u edin.

### <a name="using-runbook-converter"></a>Runbook Dönüştürücü kullanma

**ConvertFrom-SCORunbook** için sözdizimi aşağıdaki gibidir:

```powershell
ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>
```

* RunbookPath - Dönüştürülecek runbookbook'ları içeren dışa aktarma dosyasına giden yol.
* Modül - Runbook'larda etkinlikleri içeren entegrasyon modüllerinin virgüllü listesi.
* OutputFolder - Dönüştürülmüş grafik runbook'lar oluşturmak için klasöre yol.

Aşağıdaki örnek komut, **MyRunbooks.ois_export**adlı bir dışa aktarma dosyasında runbook'ları dönüştürür.  Bu runbook'lar Active Directory ve Veri Koruma Yöneticisi tümleştirme paketlerini kullanır.

```powershell
ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"
```

### <a name="log-files"></a>Günlük dosyaları

Runbook Converter, dönüştürülen çalışma kitabıyla aynı konumda aşağıdaki günlük dosyalarını oluşturur.  Dosyalar zaten varsa, son dönüşümden gelen bilgilerle birlikte üzerine yazılır.

| Dosya | İçindekiler |
|:--- |:--- |
| Runbook Converter - Progress.log |Dönüşüm, başarılı bir şekilde dönüştürülen her etkinlik için bilgiler ve dönüştürülmemiş her etkinlik için uyarı içeren dönüştürmenin ayrıntılı adımlarıdır. |
| Runbook Converter - Summary.log |Dönüştürülmüş çalışma kitabı için gerekli olan değişkeni oluşturma gibi gerçekleştirmeniz gereken uyarılar ve izleme görevleri de dahil olmak üzere son dönüşümün özeti. |

### <a name="exporting-runbooks-from-orchestrator"></a>Orchestrator'dan runbook'ların dışa aktarımı

Runbook Converter, Orchestrator'un bir veya daha fazla runbook içeren bir dışa aktarma dosyasıyla çalışır.  Dışa aktarma dosyasındaki her Orchestrator runbook için karşılık gelen bir Azure Otomasyonu runbook'u oluşturur.  

Orchestrator'dan bir runbook dışa aktarmak için Runbook Designer'da runbook'un adını sağ tıklatın ve **Dışa Aktarma'yı**seçin.  Tüm runbook'ları bir klasöre aktarmak için klasörün adını sağ tıklatın ve **Dışa Aktar'ı**seçin.

### <a name="runbook-activities"></a>Runbook etkinlikleri

Runbook Dönüştürücü, Orchestrator runbook'taki her etkinliği Azure Otomasyonu'ndaki ilgili bir faaliyete dönüştürür.  Dönüştürülemeyen etkinlikler için, çalışma kitabında uyarı metniiçeren bir yer tutucu etkinliği oluşturulur.  Dönüştürülmüş runbook'u Azure Otomasyonu'na aktardıktan sonra, bu etkinliklerden herhangi birini gerekli işlevselliği gerçekleştiren geçerli etkinliklerle değiştirmeniz gerekir.

[Standart Faaliyetler Modülündeki](#standard-activities-module) tüm Orchestrator faaliyetleri dönüştürülecektir.  Bu modülde olmayan ve dönüştürülmeyen bazı standart Orchestrator etkinlikleri vardır.  Örneğin, `Send Platform Event` etkinlik Orchestrator'a özgü olduğundan Azure Otomasyon eşdeğeri yoktur.

Azure Otomasyonu'nda bunların eşdeğeri olmadığından [monitör etkinlikleri](https://technet.microsoft.com/library/hh403827.aspx) dönüştürülmez.  Özel durum, yer tutucu faaliyetine dönüştürülecek [dönüştürülmüş tümleştirme paketlerindeki](#integration-pack-converter) izleme etkinlikleridir.

Dönüştürülmüş bir [tümleştirme paketinden](#integration-pack-converter) herhangi bir `modules` etkinlik, parametre ile entegrasyon modülüne giden yolu sağlarsanız dönüştürülür. Sistem Merkezi Entegrasyon Paketleri [için, Sistem Merkezi Orkestrasyon Entegrasyon Modüllerini](#system-center-orchestrator-integration-modules)kullanabilirsiniz.

### <a name="orchestrator-resources"></a>Orkestratör kaynakları

Runbook Dönüştürücü, sayaçlar, değişkenler veya bağlantılar gibi diğer Orchestrator kaynaklarını değil, yalnızca runbook'ları dönüştürür.  Sayaçlar Azure Otomasyonu'nda desteklenmez.  Değişkenler ve bağlantılar desteklenir, ancak bunları el ile oluşturmanız gerekir.  Kayıt defteri, bu tür kaynaklar gerektiriyorsa sizi bilgilendirir ve dönüştürülmüş runbook'un düzgün çalışması için Azure Otomasyonu'nda oluşturmanız gereken ilgili kaynakları belirtir.

Örneğin, bir runbook bir etkinlikte belirli bir değeri doldurmak için bir değişken kullanabilir.  Dönüştürülen runbook etkinliği dönüştürür ve Azure Otomasyon'da Orchestrator değişkeniyle aynı ada sahip değişken bir varlık belirtir.  Bu, dönüşümden sonra oluşturulan **Runbook Converter - Summary.log** dosyasında belirtilecektir.  Runbook'u kullanmadan önce bu değişken varlığı Azure Automation'da el ile oluşturmanız gerekir.

### <a name="input-parameters"></a>Giriş parametreleri

Orchestrator'daki runbook'lar giriş `Initialize Data` parametrelerini etkinlikle birlikte kabul eder.  Dönüştürülen runbook bu etkinliği içeriyorsa, etkinlikteki her parametre için Azure Otomasyon uyrayında bir [giriş parametresi](automation-graphical-authoring-intro.md#runbook-input-and-output) oluşturulur.  Dönüştürülmüş çalışma kitabında her parametreyi alan ve döndüren bir [İş Akışı Komut Dosyası denetimi](automation-graphical-authoring-intro.md#activities) etkinliği oluşturulur.  Runbook'ta giriş parametresi kullanan tüm etkinlikler, bu etkinlikten çıkan çıktıya başvurur.

Bu stratejinin kullanılmasının nedeni, Orchestrator runbook'taki işlevselliği en iyi şekilde yansıtmaktır.  Yeni grafik runbook'taki etkinlikler, runbook giriş veri kaynağını kullanarak doğrudan giriş parametrelerine başvurmalıdır.

### <a name="invoke-runbook-activity"></a>Runbook etkinliğini çağırma

Orchestrator'daki runbook'lar diğer `Invoke Runbook` runbook'ları etkinlikle başlatır. Dönüştürülen runbook bu etkinliği içeriyorsa ve `Wait for completion` seçenek ayarlanmışsa, dönüştürülmüş runbook'ta bunun için bir runbook etkinliği oluşturulur.  `Wait for completion` Seçenek ayarlı değilse, runbook'u başlatmak için [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) kullanan bir İş Akışı Komut Dosyası etkinliği oluşturulur. Dönüştürülmüş runbook'u Azure Otomasyonu'na aktardıktan sonra, bu etkinliği etkinlikte belirtilen bilgilerle değiştirmeniz gerekir.

## <a name="related-articles"></a>İlgili makaleler:

* [System Center 2012 - Orchestrator](https://technet.microsoft.com/library/hh237242.aspx)
* [Hizmet Yönetimi Otomasyonu](https://technet.microsoft.com/library/dn469260.aspx)
* [Karma Runbook Çalışanı](automation-hybrid-runbook-worker.md)
* [Orkestratör Standart Aktiviteler](https://technet.microsoft.com/library/hh403832.aspx)
* [Download System Center Orchestrator Geçiş Araç Seti](https://www.microsoft.com/en-us/download/details.aspx?id=47323)

