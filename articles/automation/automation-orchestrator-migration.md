---
title: Orchestrator 'dan Azure Otomasyonu 'na geçiş
description: Runbook 'ları ve Tümleştirme paketlerini System Center Orchestrator 'dan Azure Otomasyonu 'na nasıl geçirebileceğinizi açıklar.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 528b961ca07ec86ad502ee1b589772e354564a3d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75421692"
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Orchestrator 'dan Azure Otomasyonu 'na geçiş (Beta)
[System Center Orchestrator](https://technet.microsoft.com/library/hh237242.aspx) 'daki runbook 'Lar, Azure Otomasyonu 'ndaki runbook 'Lar Windows PowerShell 'i temel alırken Orchestrator için özel olarak yazılmış tümleştirme paketlerindeki etkinliklere dayanır.  Azure Automation 'daki [grafik runbook](automation-runbook-types.md#graphical-runbooks) 'Larında, Orchestrator Runbook 'Larına, PowerShell cmdlet 'lerini, alt runbook 'ları ve varlıkları temsil eden etkinlikleri içeren benzer bir görünüm vardır.

[System Center Orchestrator geçiş araç seti](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , runbook 'ları Orchestrator 'Dan Azure Otomasyonu 'na dönüştürmeye yardımcı olacak araçlar içerir.  Runbook 'ların kendilerini dönüştürmesinin yanı sıra, Tümleştirme paketlerini runbook 'ların Windows PowerShell cmdlet 'leriyle tümleştirmede kullandığı etkinliklerle dönüştürmeniz gerekir.  

Orchestrator Runbook 'larını Azure Otomasyonu 'na dönüştürmeye yönelik temel süreç aşağıda verilmiştir.  Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

1. Bu makalede ele alınan araçları ve modülleri içeren [System Center Orchestrator geçiş araç setini](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) indirin.
2. [Standart etkinlikler modülünü](#standard-activities-module) Azure Otomasyonu 'na aktarın.  Bu, dönüştürülmüş runbook 'lar tarafından kullanılabilen standart Orchestrator etkinliklerinin dönüştürülmüş sürümlerini içerir.
3. System Center 'a erişen runbook 'larınız tarafından kullanılan tümleştirme paketleri için [System Center Orchestrator tümleştirme modüllerini](#system-center-orchestrator-integration-modules) Azure Otomasyonu 'na aktarın.
4. [Tümleştirme paketi dönüştürücüsünü](#integration-pack-converter) kullanarak özel ve üçüncü taraf tümleştirme paketlerini dönüştürün ve Azure Otomasyonu ' na aktarın.
5. [Runbook dönüştürücüsünü](#runbook-converter) kullanarak Orchestrator Runbook 'larını dönüştürün ve Azure Otomasyonu 'nda uygulamasını yapın.
6. Runbook Dönüştürücüsü bu kaynakları dönüştürmediğinden, gerekli Orchestrator varlıklarını Azure Otomasyonu 'nda el ile oluşturun.
7. Yerel kaynaklara erişecek dönüştürülmüş runbook 'ları çalıştırmak için yerel veri merkezinizde [karma runbook çalışanı](#hybrid-runbook-worker) yapılandırın.

## <a name="service-management-automation"></a>Hizmet Yönetim Otomasyonu
[Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx) (SMA), runbook 'ları yerel veri merkezinizde Orchestrator gibi depolar ve çalıştırır ve Azure Otomasyonu ile aynı tümleştirme modüllerini kullanır. [Runbook Dönüştürücüsü](#runbook-converter) , Orchestrator RUNBOOK 'larını SMA içinde desteklenmeyen grafik runbook 'larına dönüştürür.  [Standart etkinlikler modülünü](#standard-activities-module) ve [System Center Orchestrator tümleştirme modüllerini](#system-center-orchestrator-integration-modules) SMA 'e yüklemeye devam edebilirsiniz, ancak [runbook 'larınızı el ile yeniden yazmanız](https://technet.microsoft.com/library/dn469262.aspx)gerekir.

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı
Orchestrator 'daki runbook 'lar bir veritabanı sunucusunda depolanır ve hem yerel veri merkezinizde hem de runbook sunucularında çalıştırılır.  Azure Otomasyonu 'ndaki runbook 'lar Azure bulutu 'nda depolanır ve [karma runbook çalışanı](automation-hybrid-runbook-worker.md)kullanarak yerel veri merkezinizde çalıştırılabilir.  Bu, genellikle yerel sunucularda çalışacak şekilde tasarlandıklarından, Orchestrator 'dan dönüştürülen runbook 'ları çalıştıracaksınız.

## <a name="integration-pack-converter"></a>Tümleştirme paketi Dönüştürücüsü
Tümleştirme paketi dönüştürücüsü [Orchestrator Integration Toolkit (OıT)](https://technet.microsoft.com/library/hh855853.aspx) kullanılarak oluşturulan Tümleştirme paketlerini, Azure otomasyonu veya Service Management Automation içeri aktarılabilecek Windows PowerShell tabanlı modüllerle tümleştirme için dönüştürür.  

Tümleştirme paketi dönüştürücüsünü çalıştırdığınızda, size bir tümleştirme paketi (. OIP) dosyası seçmenizi sağlayacak bir sihirbaz sunulur.  Sihirbaz daha sonra bu tümleştirme paketine dahil edilen etkinlikleri listeler ve hangilerinin geçirileceğini seçmenizi sağlar.  Sihirbazı tamamladığınızda, özgün Tümleştirme paketindeki etkinliklerin her biri için karşılık gelen bir cmdlet 'i içeren bir tümleştirme modülü oluşturur.

### <a name="parameters"></a>Parametreler
Tümleştirme paketindeki bir etkinliğin tüm özellikleri, tümleştirme modülündeki karşılık gelen cmdlet 'in parametrelerine dönüştürülür.  Windows PowerShell cmdlet 'leri, tüm cmdlet 'lerle kullanılabilecek bir dizi [ortak parametreye](https://technet.microsoft.com/library/hh847884.aspx) sahiptir.  Örneğin,-Verbose parametresi, bir cmdlet 'in işlem hakkındaki ayrıntılı bilgileri çıkışına neden olur.  Hiçbir cmdlet ortak parametre ile aynı ada sahip bir parametreye sahip olamaz.  Bir etkinliğin ortak parametre ile aynı ada sahip bir özelliği varsa, sihirbaz, parametre için başka bir ad sağlamanızı ister.

### <a name="monitor-activities"></a>Etkinlikleri izleme
Orchestrator 'daki runbook 'ları izleyerek [izleyici etkinliğini](https://technet.microsoft.com/library/hh403827.aspx) başlatın ve belirli bir olay tarafından çağrılması için sürekli olarak bekletmeyi çalıştırın.  Azure Otomasyonu runbook 'ları izlemeyi desteklemediğinden, tümleştirme paketindeki tüm izleme etkinlikleri dönüştürülmez.  Bunun yerine, izleme etkinliğinin tümleştirme modülünde bir yer tutucu cmdlet 'i oluşturulur.  Bu cmdlet 'in bir işlevi yoktur, ancak onu kullanan dönüştürülmüş runbook 'un yüklenmesini sağlar.  Bu runbook, Azure Otomasyonu 'nda çalıştırılamaz, ancak bunu değiştirebilmeniz için yüklenebilmeyecektir.

### <a name="integration-packs-that-cannot-be-converted"></a>Dönüştürülemeyen tümleştirme paketleri
OıT ile oluşturulmamış tümleştirme paketleri, tümleştirme paketi dönüştürücüsüyle dönüştürülemez. Microsoft tarafından sunulan ve şu anda bu araçla dönüştürülemediği bazı tümleştirme paketleri de mevcuttur.  Bu tümleştirme paketlerinin dönüştürülmüş sürümleri, Azure Otomasyonu 'nda veya Service Management Automation yüklenebilmeleri [için yüklenmek üzere sağlanmış](#system-center-orchestrator-integration-modules) .

## <a name="standard-activities-module"></a>Standart etkinlikler modülü
Orchestrator, bir tümleştirme paketine dahil olmayan ancak birçok runbook tarafından kullanılan bir dizi [Standart etkinlik](https://technet.microsoft.com/library/hh403832.aspx) içerir.  Standart etkinlikler modülü, bu etkinliklerin her biri için bir cmdlet eşdeğerini içeren bir tümleştirme modülüdür.  Standart bir etkinlik kullanan dönüştürülmüş runbook 'ları içeri aktarmadan önce Bu tümleştirme modülünü Azure Otomasyonu 'nda yüklemelisiniz.

Dönüştürülmüş runbook 'ların desteklenmesinin yanı sıra, standart etkinlikler modülündeki cmdlet 'ler, Azure Otomasyonu 'nda yeni runbook 'lar oluşturmak için Orchestrator 'ı bilen bir kişi tarafından kullanılabilir.  Tüm standart etkinliklerin işlevselliği cmdlet 'lerle gerçekleştirilese de, farklı şekilde çalışmayabilir.  Dönüştürülmüş standart etkinlikler modülündeki cmdlet 'ler ilgili etkinlikleriyle aynı şekilde çalışacaktır ve aynı parametreleri kullanacaktır.  Bu, mevcut Orchestrator Runbook Yazarı 'nın Azure Otomasyonu runbook 'larına geçişte yardımcı olabilir.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator tümleştirme modülleri
Microsoft, System Center bileşenlerini ve diğer ürünleri otomatikleştirmek için Runbook 'lar oluşturmak üzere [tümleştirme paketleri](https://technet.microsoft.com/library/hh295851.aspx) sağlar.  Bu tümleştirme paketlerinden bazıları şu anda OıT tabanlıdır ancak bilinen sorunlar nedeniyle şu anda tümleştirme modüllerine dönüştürülemez.  [System Center Orchestrator tümleştirme modülleri](https://www.microsoft.com/download/details.aspx?id=49555) , bu tümleştirme paketlerinin Azure otomasyonu ve Service Management Automation içeri aktarılabilecek dönüştürülmüş sürümlerini içerir.  

Bu aracın RTM sürümü ile, tümleştirme paketi dönüştürücüsüyle dönüştürülemeyen tümleştirme paketlerinin güncelleştirilmiş sürümleri yayımlanacak.  Ayrıca, BT 'yi temel alan tümleştirme paketlerindeki etkinlikleri kullanarak runbook 'ları dönüştürmeye yardımcı olacak yönergeler de sunulacaktır.

## <a name="runbook-converter"></a>Runbook Dönüştürücüsü
Runbook Dönüştürücüsü, Orchestrator Runbook 'larını Azure Otomasyonu 'na aktarılabilen [grafik runbook](automation-runbook-types.md#graphical-runbooks) 'larına dönüştürür.  

Runbook Dönüştürücüsü, dönüştürmeyi gerçekleştiren **ConvertFrom-SCORunbook** adlı bir cmdlet 'i Içeren bir PowerShell modülü olarak uygulanır.  Aracı yüklediğinizde, cmdlet 'i yükleyen bir PowerShell oturumu için bir kısayol oluşturulur.   

Aşağıda bir Orchestrator Runbook 'unu dönüştürmeye ve Azure Otomasyonu 'na aktarmaya yönelik temel işlem yer verilmiştir.  Aşağıdaki bölümlerde, aracı kullanımı ve dönüştürülmüş runbook 'larla çalışma hakkında daha fazla bilgi sağlanmaktadır.

1. Orchestrator 'dan bir veya daha fazla runbook dışarı aktarın.
2. Runbook 'taki tüm etkinlikler için tümleştirme modüllerini edinin.
3. İçe aktarılmış dosyadaki Orchestrator Runbook 'larını dönüştürün.
4. Değişiklikleri doğrulamak ve gerekli tüm el ile görevleri öğrenmek için günlüklerdeki bilgileri gözden geçirin.
5. Dönüştürülmüş runbook 'ları Azure Otomasyonu 'na aktarın.
6. Azure Otomasyonu 'nda gerekli varlıkları oluşturun.
7. Gerekli etkinlikleri değiştirmek için Azure Otomasyonu 'nda runbook 'u düzenleyin.

### <a name="using-runbook-converter"></a>Runbook dönüştürücüsünü kullanma
**ConvertFrom-SCORunbook** için sözdizimi aşağıdaki gibidir:

```powershell
ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>
```

* RunbookPath-dönüştürülecek runbook 'ları içeren dışa aktarma dosyasının yolu.
* Modül-runbook 'larda etkinlikleri içeren tümleştirme modüllerinin virgülle ayrılmış listesi.
* OutputFolder-dönüştürülen grafik runbook 'ları oluşturmak için klasörün yolu.

Aşağıdaki örnek komut, runbook 'ları **Myrunbook**'lar adlı bir dışarı aktarma dosyasında dönüştürür. ois_export.  Bu runbook 'lar Active Directory ve Data Protection Manager Tümleştirme paketlerini kullanır.

```powershell
ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"
```

### <a name="log-files"></a>Günlük dosyaları
Runbook Dönüştürücüsü, dönüştürülmüş runbook ile aynı konumda aşağıdaki günlük dosyalarını oluşturur.  Dosyalar zaten mevcutsa, son dönüşümdeki bilgilerle üzerine yazılır.

| Dosya | İçindekiler |
|:--- |:--- |
| Runbook Dönüştürücüsü-Ilerleme. log |Dönüştürmenin ayrıntılı adımları, her etkinlik için bilgiler ve Dönüştürülmeyen her etkinlik için uyarı olarak başarıyla dönüştürüldü. |
| Runbook Dönüştürücüsü-Özet. log |Tüm uyarılar dahil olmak üzere son dönüştürmenin Özeti ve dönüştürülen runbook için gereken bir değişken oluşturmak gibi gerçekleştirmeniz gereken görevleri takip edin. |

### <a name="exporting-runbooks-from-orchestrator"></a>Runbook 'ları Orchestrator 'dan dışarı aktarma
Runbook Dönüştürücüsü bir veya daha fazla runbook içeren bir Orchestrator 'dan dışarı aktarma dosyasıyla birlikte kullanılabilir.  Dışarı aktarma dosyasındaki her Orchestrator Runbook 'u için karşılık gelen bir Azure Otomasyonu runbook 'unu oluşturacaktır.  

Bir runbook 'u Orchestrator 'dan dışarı aktarmak için Runbook Designer 'de runbook 'un adına sağ tıklayın ve **dışarı aktar**' ı seçin.  Bir klasördeki tüm runbook 'ları dışarı aktarmak için klasörün adına sağ tıklayın ve **dışarı aktar**' ı seçin.

### <a name="runbook-activities"></a>Runbook etkinlikleri
Runbook Dönüştürücüsü, Orchestrator Runbook 'taki her etkinliği Azure Otomasyonu 'nda karşılık gelen bir etkinliğe dönüştürür.  Dönüştürülemeyen etkinlikler için, runbook 'ta uyarı metniyle birlikte bir yer tutucu etkinlik oluşturulur.  Dönüştürülmüş runbook 'u Azure Otomasyonu 'na aktardıktan sonra, bu etkinliklerin herhangi birini gerekli işlevselliği gerçekleştiren geçerli etkinliklerle değiştirmeniz gerekir.

[Standart etkinlikler modülündeki](#standard-activities-module) tüm Orchestrator etkinlikleri dönüştürülecek.  Bu modülde olmayan ve Dönüştürülmeyen bazı standart Orchestrator etkinlikleri vardır.  Örneğin, olay Orchestrator 'a özgü olduğundan, **Send platform olayında** hiçbir Azure Otomasyonu eşdeğeri yoktur.

Azure Otomasyonu 'nda bunlara eşdeğer olmadığından [izleyici etkinlikleri](https://technet.microsoft.com/library/hh403827.aspx) dönüştürülmez.  Özel durum, [dönüştürülmüş tümleştirme paketlerinde](#integration-pack-converter) yer tutucu etkinliğe dönüştürülecek olan etkinlikleri izler.

**Modüller** parametresiyle tümleştirme modülünün yolunu sağlarsanız, [dönüştürülmüş tümleştirme paketinden](#integration-pack-converter) herhangi bir etkinlik dönüştürülür.  System Center tümleştirme paketleri için [System Center Orchestrator tümleştirmesi modüllerini](#system-center-orchestrator-integration-modules)kullanabilirsiniz.

### <a name="orchestrator-resources"></a>Orchestrator kaynakları
Runbook dönüştürücüsü yalnızca runbook 'ları, sayaçlar, değişkenler veya bağlantılar gibi diğer Orchestrator kaynaklarını dönüştürür.  Sayaçlar Azure Otomasyonu 'nda desteklenmez.  Değişkenler ve bağlantılar desteklenir, ancak bunları el ile oluşturmanız gerekir.  Runbook 'un bu tür kaynaklar gerektirdiğini ve dönüştürülen runbook 'un düzgün çalışması için Azure Otomasyonu 'nda oluşturmanız gereken ilgili kaynakları belirtmesi durumunda günlük dosyaları size bildirir.

Örneğin, bir runbook bir etkinlikte belirli bir değeri doldurmak için bir değişken kullanabilir.  Dönüştürülen runbook, etkinliği dönüştürür ve Azure Otomasyonu 'nda Orchestrator değişkeniyle aynı ada sahip bir değişken varlığı belirler.  Bu, dönüştürmeden sonra oluşturulan **runbook Dönüştürücüsü-Summary. log** dosyasında not edilir.  Runbook 'u kullanmadan önce bu değişken varlığı Azure Otomasyonu 'nda el ile oluşturmanız gerekir.

### <a name="input-parameters"></a>Giriş parametreleri
Orchestrator 'daki runbook 'lar, **verileri Başlat** etkinliğiyle giriş parametrelerini kabul eder.  Dönüştürülen runbook bu etkinliği içeriyorsa, etkinliğin her bir parametresi için Azure Otomasyonu runbook 'unda bir [giriş parametresi](automation-graphical-authoring-intro.md#runbook-input-and-output) oluşturulur.  Dönüştürülmüş runbook 'ta her parametreyi alan ve döndüren bir [Iş akışı betik denetim](automation-graphical-authoring-intro.md#activities) etkinliği oluşturulur.  Runbook 'taki bir giriş parametresi kullanan etkinlikler, bu etkinliğin çıktısına başvurur.

Bu stratejinin kullanıldığı neden Orchestrator Runbook 'unda işlevselliği en iyi şekilde yansıtmasıdır.  Yeni grafik runbook 'larında etkinlik, runbook giriş veri kaynağı kullanarak doğrudan giriş parametrelerine başvurmalıdır.

### <a name="invoke-runbook-activity"></a>Runbook 'U çağır etkinliği
Orchestrator 'daki runbook 'lar **runbook 'U çağır** etkinliğine sahip diğer runbook 'ları başlatır. Dönüştürülen runbook bu etkinliği içeriyorsa ve **tamamlanmayı bekle** seçeneği ayarlandıysa, bu durumda, dönüştürülmüş runbook 'ta kendisi için bir runbook etkinliği oluşturulur.  **Tamamlanmasını bekle** seçeneği ayarlanmamışsa, runbook 'u başlatmak için **Start-AzureAutomationRunbook** kullanan bir iş akışı betiği etkinliği oluşturulur.  Dönüştürülmüş runbook 'u Azure Otomasyonu 'na aktardıktan sonra, bu etkinliği etkinlikte belirtilen bilgilerle değiştirmeniz gerekir.

## <a name="related-articles"></a>İlgili makaleler
* [System Center 2012-Orchestrator](https://technet.microsoft.com/library/hh237242.aspx)
* [Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
* [Karma Runbook Worker](automation-hybrid-runbook-worker.md)
* [Orchestrator Standart etkinlikleri](https://technet.microsoft.com/library/hh403832.aspx)
* [System Center Orchestrator geçiş araç setini indir](https://www.microsoft.com/en-us/download/details.aspx?id=47323)

