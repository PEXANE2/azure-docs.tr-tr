---
title: Azure Windows sanal makinelerinde yüksek CPU sorunlarını giderme
description: .
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: mnanda
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: 9971cc26-916f-4e49-83cd-71eca74804f0
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 9/24/2020
ms.author: mnanda
ms.openlocfilehash: 3bd19f301b1afd7dd1c35f03f6f6131a26b00708
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91596832"
---
# <a name="troubleshoot-high-cpu-issues-for-azure-windows-virtual-machines"></a>Azure Windows sanal makinelerinde yüksek CPU sorunlarını giderme

## <a name="summary"></a>Özet

Performans sorunları farklı işletim sistemlerinde veya uygulamalarda oluşur ve her sorun gidermek için benzersiz bir yaklaşım gerektirir. Bu sorunların çoğu CPU, bellek, ağ ve giriş/çıkış (g/ç) etrafında, sorunun gerçekleştiği anahtar konumları olarak döner. Bu alanların her biri farklı belirtiler (bazen eşzamanlı olarak) oluşturur ve farklı bir tanılama ve çözüm gerektirir.

Bu makalede, Windows işletim sistemini çalıştıran Azure sanal makinelerinde (VM) gerçekleşen yüksek CPU kullanımı sorunları ele alınmaktadır.

### <a name="high-cpu-issues-on-azure-windows-vms"></a>Azure Windows VM 'lerinde yüksek CPU sorunları

G/ç ve ağ gecikme sorunlarından itibaren, CPU ve bellek sorunlarını giderme, şirket içi sunucularla aynı araçları ve adımları gerektirir. Microsoft 'un genellikle desteklediği araçlardan biri şunlardır (hem Windows hem de Linux için kullanılabilir). Perfinsıghts, Kullanıcı dostu bir raporda Azure VM en iyi yöntemler tanılaması sağlayabilir. Perfinsıghts, araç içinde seçilen bayraklara bağlı olarak Perfmon, XPerf ve NetMon verilerini toplamaya yardımcı olabilecek bir sarmalayıcı araçtır.

Perfmon veya procmon gibi mevcut performans sorunlarını giderme araçlarınızın çoğu, şirket içi sunucular için kullanılan Azure Windows VM 'lerinde çalışacaktır. Ancak, Perfinsıghts, Azure 'un En Iyi uygulamaları, SQL En Iyi uygulamaları, yüksek çözünürlüklü g/ç gecikme grafikleri, CPU ve bellek sekmeleri gibi daha fazla öngörü sağlamak üzere Azure VM 'Leri için özel olarak tasarlanmıştır.

User-Mode veya çekirdek modu olarak çalışıp çalışmadığını, etkin bir işlemin herhangi bir iş parçacığının oluşturulduğu kodu çalıştırmak için CPU döngüleri gerekir. Birçok sorun doğrudan iş yüküyle ilgilidir. Sunucu, CPU da dahil olmak üzere kaynak tüketimine sahip olan iş yükünün türü.

#### <a name="common-factors"></a>Ortak etmenler

Aşağıdaki faktörler yüksek CPU durumunda ortaktır:

- Genellikle Internet Information Services (IIS), Microsoft SharePoint, Microsoft SQL Server veya üçüncü taraf uygulamalar gibi uygulamalar için geçerli olan son kod değişikliği veya dağıtımı.

- Bir işletim sistemi düzeyi güncelleştirmesiyle veya uygulama düzeyi toplu güncelleştirmeler ve düzeltmeleriyle ilgili olabilecek son güncelleştirme.

- Sorgu değişikliği veya güncel olmayan dizinler. SQL Server ve Oracle veri katmanı uygulamaları, başka bir faktör olarak sorgu planı iyileştirmesi de vardır. Veri değişiklikleri veya uygun dizinlerin olmaması, birkaç sorgunun daha fazla işlem yoğunluğu olmasına neden olabilir.

- Azure VM 'ye özgü. Rdadgent gibi bazı süreçler ve Izleme Aracısı, MMA Aracısı veya güvenlik istemcisi gibi uzantıya özgü süreçler, yüksek CPU kullanımına neden olabilir. Bu işlemlerin bir yapılandırma ya da bilinen sorunlar perspektifinden görüntülenmesi gerekir.

## <a name="troubleshoot-the-issue"></a>Sorunu giderin

Bu makalede sorunlu işlem yalıulmaya odaklanılır. Daha fazla analiz, yüksek CPU tüketimi sunan işleme özgüdür.

Örneğin, işlem SQL Server (sqlservr.exe) ise, sonraki adımlar belirli bir dönemde en fazla CPU döngüsünü hangi sorgunun kullandığını analiz etmek olacaktır.

### <a name="scope-the-issue"></a>Sorunu kapsama

Sorunu giderirken sorun gidermeye yönelik birkaç soru aşağıda verilmiştir:

- Sorun için bir model var mı? Örneğin, yüksek CPU sorunu her gün, hafta veya ayda belirli bir zamanda gerçekleşsin mi? Bu durumda, bu sorunu bir iş, rapor veya Kullanıcı oturumu açmayla ilişkilendirebilirler?

- Son bir kod değişikliğinden sonra yüksek CPU sorunu başladı mı? Windows veya bir uygulamada bir güncelleştirme uyguladınız mı?

- Kullanıcı sayısında bir artış, daha yüksek bir veri etkileyen x veya daha fazla rapor sayısı gibi iş yükünde bir değişiklikten sonra yüksek CPU sorunu başladı mı?

- Azure için, yüksek CPU sorunu aşağıdaki koşullardan herhangi birinde başlayacak mı?

  - Son yeniden dağıtım veya yeniden başlatma işleminden sonra
  - Bir SKU veya VM türü değiştirildiğinde
  - Yeni bir uzantı eklendiğinde
  - Yük dengeleyici değişiklikleri yapıldıktan sonra

### <a name="azure-caveats"></a>Azure uyarıları

İş yükünüzü anlayın. Bir VM seçtiğinizde, genel aylık barındırma maliyetine baktığınızda sanal CPU (vCPU) sayılarını tahmin edebilirsiniz. İş yükünüz işlem yoğunluğu kullanıyorsa, bir veya iki vCPU içeren daha küçük bir VM SKU 'SU seçilmesi iş yükü sorunlarına neden olabilir. Gerekli en iyi bilgi işlem yeteneğini öğrenmek için iş yükünüz için farklı yapılandırmalara test edin.

Kalite güvencesi (QA) ve test için önerilen B (patlama modu) serisi gibi belirli VM serileri vardır. Üretim ortamında bu serinin kullanılması, CPU kredileri tükendikten sonra bilgi işlem yeteneğini sınırlandırır.

SQL Server, Oracle, RDS (Uzak Masaüstü Hizmetleri), Windows sanal masaüstü, IIS veya SharePoint gibi bilinen uygulamalarda, bu iş yükleri için en az yapılandırma önerilerini içeren Azure En Iyi uygulama makaleleri bulunmaktadır.

### <a name="ongoing-high-cpu-issues"></a>Devam eden yüksek CPU sorunları

Sorun şu anda gerçekleşirse, bu sorunun ne olduğunu belirlemek için işlem izlemeyi yakalamak için en iyi fırsattır. İşlemi bulmak için şirket içi Windows Server 'lar için kullanmakta olduğunuz mevcut araçları kullanabilirsiniz. Azure VM 'Leri için Azure desteği tarafından aşağıdaki araçlar önerilir.

### <a name="perfinsights"></a>PerfInsights

Perfinsıghts, VM performans sorunları için Azure desteği 'nden önerilen araçtır. CPU, bellek ve yüksek çözünürlüklü g/ç grafikleri için en iyi uygulamaları ve adanmış analiz sekmelerini kapsayacak şekilde tasarlanmıştır. Azure portal veya VM içinden bu adımları çalıştırabilirsiniz. Verileri Azure destek ekibi ile paylaşabilirsiniz.

#### <a name="run-perfinsights"></a>Çalışan Perfinsıghts

Perfinsıghts, hem [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) hem de [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) işletim sistemi için kullanılabilir. Windows için seçenekler şunlardır.

#### <a name="run-and-analyze-reports-through-azure-portal"></a>Azure portal aracılığıyla raporları çalıştırma ve çözümleme

[Azure Portal aracılığıyla yüklendiğinde](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/performance-diagnostics), aslında VM 'ye bir uzantı yüklenir. Kullanıcılar ayrıca, [VM dikey penceresindeki uzantılara](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/performance-diagnostics-vm-extension)doğrudan gidip bir performans tanılama seçeneği belirleyerek bir uzantı olarak Perfinsıghts 'i de yükleyebilir.

#### <a name="azure-portal-option-1"></a>Azure portal seçeneği 1

VM dikey penceresine gidip **Performans Tanılama** seçeneğini belirleyin. Seçtiğiniz VM 'ye (uzantıları kullanır) seçeneğini yüklemek isteyip istemediğiniz sorulur.

  ![Performans tanılamayı yükler](./media/troubleshoot-high-cpu-issues-azure-windows-vm/1-install-performance-diagnostics.png)

#### <a name="azure-portal-option-2"></a>Azure portal seçenek 2

VM dikey penceresinde **sorunları tanılamak ve çözmek** için göz atın ve **VM performans sorunlarını**bulun.

  ![VM performans sorunlarını giderme](./media/troubleshoot-high-cpu-issues-azure-windows-vm/2-troubleshoot-vm-performance-issues.png)

**Sorun giderme**' yi seçerseniz, Perfinsıghts yükleme ekranı yüklenir.

**Yükleme ' yi**seçerseniz, yükleme farklı koleksiyon seçenekleri sağlar.

  ![Performans Analizi](./media/troubleshoot-high-cpu-issues-azure-windows-vm/3-performance-analysis.png)

Ekran görüntüsündeki numaralandırılmış seçenekler aşağıdaki yorumlarla ilgilidir:

1. **Yüksek CPU**seçeneği Için **Performans Analizi** veya **Gelişmiş**' i seçin.

2. Buraya belirti eklediğinizde, bu bilgiler rapora eklenir ve bu da Azure desteğiyle bilgi paylaşmanıza yardımcı olur.

3. Veri toplama süresini seçin. Yüksek CPU seçeneği için en az 15 dakika veya daha fazlasını seçin. Azure portal modunda, en fazla 15 dakikalık verileri toplayabilirsiniz. Daha uzun süre içinde, programı VM içinde yürütülebilir olarak çalıştırmanız gerekir.

4. Bu verileri toplamak için Azure desteği ile sorulursa, Bilet numarasını buraya ekleyebilirsiniz. Bu alan isteğe bağlıdır.

5. Son Kullanıcı Lisans Sözleşmesi 'Ni (EULA) kabul etmek için bu alanı seçin.

6. Bu raporun Azure destek ekibi için kullanılabilir olmasını istiyorsanız bu alanı seçin.

Rapor, aboneliğinizdeki depolama hesaplarından birinde depolanır. Daha sonra görüntülemek ve indirmek için kullanılabilir.

#### <a name="run-perfinsights-from-within-the-vm"></a>VM içinden Perfinsıghts çalıştırma

Bu yöntem, daha uzun süreler için Perfinsıghts 'i çalıştırmak istiyorsanız kullanılabilir. [Perfinsıghts makalesi](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights#how-do-i-run-perfinsights) , yürütülebilir dosya olarak perfinsıghts çalıştırmak için gereken farklı komutlara ve bayraklara yönelik ayrıntılı bir yol sunar. Yüksek CPU kullanımı amacına yönelik olarak, aşağıdaki modlardan birini kullanmanız gerekir:

- Gelişmiş senaryo

  - `PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics`

- VM yavaş (performans) senaryosu

  - `PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>`

Komut çıktısı, Perfinsıghts yürütülebilirini kaydettiğiniz klasörde olacaktır.

#### <a name="what-to-look-for-in-the-report"></a>Raporda aramanız gerekenler

Raporu çalıştırdıktan sonra, içeriğin konumu Azure portal veya yürütülebilir dosya aracılığıyla çalıştırılmış olmasına bağlıdır. Her iki seçenek için, oluşturulan günlük klasörüne erişin veya analiz için yerel olarak (Azure portal) indirin.

### <a name="run-through-the-azure-portal"></a>Azure portal üzerinden Çalıştır

  ![Yüksek etki performansı tanılaması](./media/troubleshoot-high-cpu-issues-azure-windows-vm/4-high-impact-performance-diagnostics.png)

  ![Raporu indir](./media/troubleshoot-high-cpu-issues-azure-windows-vm/5-download-report.png)

#### <a name="run-from-within-vm"></a>VM içinden Çalıştır

Klasör yapınız aşağıdaki görüntülere benzer olmalıdır:

  ![Çıkış seçin](./media/troubleshoot-high-cpu-issues-azure-windows-vm/6-select-output.png)

  ![Klasör yapısı](./media/troubleshoot-high-cpu-issues-azure-windows-vm/7-folder-structure.png)

1. Perfmon, XPerf, NetMon, SMB günlükleri, olay günlükleri vb. gibi ek koleksiyonlar çıkış klasöründe bulunabilir.

1. Analiz ve önerilerle birlikte gerçek rapor.

1. Hem performans (VMslow) hem de gelişmiş için rapor, Perfinsıghts çalışma süresi boyunca **Perfmon** bilgilerini toplar.

1. Olay günlükleri, yararlı sistem düzeyi veya işlem kilitlenme ayrıntılarının hızlı bir görünümünü gösterir.

#### <a name="where-to-start"></a>Başlangıç noktası

Perfinsıghts raporunu açın. **Bulgular** sekmesi, tüm aykırı değerleri kaynak tüketimi bakımından günlüğe kaydeder. Yüksek CPU kullanımının örnekleri varsa, **bulguları** sekmesi yüksek etki veya orta etki olarak sınıflandıracaktır.

  ![Etki düzeyi kaynakları](./media/troubleshoot-high-cpu-issues-azure-windows-vm/8-impact-level-resources.png)

Önceki örneğe benzer şekilde, Perfinsıghts 30 dakika boyunca çalıştırıldı. Bu süre boyunca, vurgulanan işlem CPU 'nun daha yüksek bir tarafında tüketildi. Aynı işlem toplama süresi boyunca çalışıyorsa, etki düzeyi **yüksek**olarak değişir.

**Bulguları** olayını genişletirseniz, bazı önemli ayrıntıları görürsünüz. Bu sekmede işlemler, **Ortalama CPU** tüketimine göre azalan sırada listelenir ve işlemin sistemle, Microsoft 'a ait bir uygulamayla (SQL, IIS) veya üçüncü taraf bir işlemle ilişkili olup olmadığını gösterir.

#### <a name="more-details"></a>Daha fazla ayrıntı’yı seçin

**CPU** altında, ayrıntılı model analizi, çekirdek başına veya işlem başına kullanılabilecek bir adanmış alt sekme vardır.

**En ÜSTTEKI CPU tüketicileri** sekmesinin iki ayrı ilgi bölümü vardır ve burada işlemci istatistiklerini buradan görüntüleyebilirsiniz. Uygulama tasarımı genellikle Single-Threaded veya kendisini tek bir işlemciye sabitler. Bu senaryoda, bir veya birkaç çekirdek yüzde 100 ' de çalışır, diğer çekirdekler beklenen düzeylerde çalışır. Sunucu üzerindeki ortalama CPU beklendiği gibi çalıştığı için bu senaryolar daha karmaşıktır, ancak yüksek kullanıma sahip çekirdekler üzerinde sabitlenmiş süreçler beklenenden daha yavaş olacaktır.

  ![yüksek CPU kullanımı](./media/troubleshoot-high-cpu-issues-azure-windows-vm/9-high-cpu-usage.png)

İkinci bölüm (eşit ölçüde önemli), **en uzun çalışan CPU tüketicileri**. Bu bölümde, işlem ayrıntıları ve bunların CPU kullanım deseninin her ikisi de gösterilmektedir. Liste, en üst düzeyde ortalama CPU tüketicileri olacak şekilde sıralanır.

  ![Tom Long çalışan CPU tüketicileri](./media/troubleshoot-high-cpu-issues-azure-windows-vm/10-top-long-running-cpu-consumers.png)

Bu iki sekme, sonraki sorun giderme adımlarının yolunu ayarlamak için yeterli olacaktır. Yüksek CPU koşulunu yönlendiren işleme bağlı olarak, daha önce istenen soruları ele almanız gerekir. SQL Server (sqlservr.exe) veya IIS (w3wp.exe) gibi işlemlere, bu koşula neden olan sorgu veya kod değişiklikleri için belirli bir ayrıntıya gitme gerekir. WMI veya Lsass.exe gibi sistem işlemlerinde, farklı bir yolu izlemeniz gerekir.

Rdadgent, OMS ve izleme uzantısı yürütülebilir dosyaları gibi Azure VM ile ilgili süreçler için, Azure destek ekibinden yardım alarak yeni bir derlemeyi veya sürümü çözmeniz gerekebilir.

### <a name="perfmon"></a>Sin

**Perfmon** , Windows Server 'da bir kaynak sorununu gidermeye yönelik en eski araçlardan biridir. Öneriler veya bulguları olan açık bir rapor vermez. Bunun yerine, kullanıcının toplanan verileri araştırması ve farklı sayaç kategorilerinin altında belirli bir filtre kullanması gerekir.

Perfinsıghts, VMSlow ve gelişmiş senaryolar için bir ek günlük olarak Perfmon toplar. Ancak, Perfmon bağımsız olarak toplanabilir ve bu ek avantajlara sahiptir:

- Uzaktan toplanabilir.

- **Görevler**aracılığıyla zamanlanabilir.

- Daha uzun süreler veya sürekli modda toplama özelliği kullanılarak toplanabilir.

Performans Izleyicisi 'nin bu verileri nasıl gösterdiğini görmek için, Perfinsıghts içinde gösterilen aynı örneği göz önünde bulundurun. Gerekli sayaç kategorileri aşağıdaki gibidir:

- İşlemci bilgileri >% Işlemci zamanı > _Total

- İşlem >% ProcessorTime > tüm örnekler

#### <a name="where-to-start"></a>Başlangıç noktası

Perfmon 'un çıkış dosyası adları bir uzantıya sahiptir `.blg` . Bu dosyaları bağımsız bir şekilde veya Perfinsıghts kullanarak toplayabilirsiniz. Bu tartışma için, `.blg` Perfinsıghts verilerinde bulunan ve önceki örnek başına toplanan Perfmon 'u kullanacaksınız.

Perfmon 'da kullanılabilir varsayılan kullanıcıya hazır rapor yok. Grafik türünü değiştiren farklı görünümler vardır, ancak işlem filmi (veya bu işlemleri tanımlamak için gereken iş) el ile yapılır.

> [!NOTE]
> [PAL aracı](https://github.com/clinthuffman/PAL) `.blg` dosyaları kullanabilir ve ayrıntılı raporlar oluşturabilir.

Başlamak için, **Sayaç Ekle** kategorisini seçin.

1. **Kullanılabilir sayaçlar**' ın altında, **işlemci bilgileri** kategorisinde **% processortime** sayacını seçin.

1. Tüm Birleşik çekirdekler için istatistikleri sağlayan **_Total**seçin.

1. **Ekle**’yi seçin. Pencere, **eklenen sayaçlar**altında **% processortime süresini** gösterir.

  ![İşlemci zamanı Ekle](./media/troubleshoot-high-cpu-issues-azure-windows-vm/11-add-processor-time.png)

Sayaçlar yüklendikten sonra, toplama zaman çerçevesinde çizgi eğilimi grafiklerini görürsünüz. Sayaçları seçebilir veya temizleyebilirsiniz. Şimdiye kadar yalnızca bir sayaç eklediniz.

  ![Performans Izleyicisi ayarları](./media/troubleshoot-high-cpu-issues-azure-windows-vm/12-performance-monitor-1.png)

Her sayacın **Ortalama**, **En düşük**ve **en yüksek** değerleri olacaktır. Ortalama değer, veri toplama süresine bağlı olarak değişebildiğinden, hem **Ortalama** hem de **en fazla** değere odaklanın. Genel koleksiyon 40 dakika iken yüksek CPU etkinliği 10 dakika boyunca görülense, ortalama değerler çok daha düşüktür.

Önceki eğilim grafiğinde, **Toplam işlemcinin** yaklaşık 15 dakika boyunca yüzde 80 ' ün yakınında olduğunu gösterir.

#### <a name="identify-the-process"></a>İşlemi tanımla

Sunucuda belirli bir süre için yüksek CPU tüketimi olduğunu belirledik, ancak henüz sürücüyü belirlemedik. Perfinsıghts kullanmaktan farklı olarak, bu durumda söz konusu süreci el ile aramanız gerekir.

Bu görev için önceden eklenen **% Processortime** sayaçlarını temizlemeniz veya kaldırmanız ve ardından yeni bir kategori eklemeniz gerekir:

- İşlem >% ProcessorTime > tüm örnekler

Bu kategori, bu sırada çalışan tüm işlemlerin sayaçlarını yükler.

  ![Sayaç ekleme](./media/troubleshoot-high-cpu-issues-azure-windows-vm/13-add-counters.png)

Tipik bir üretim bilgisayarında yüzlerce veya işlem çalışıyor olabilir. Bu nedenle, düşük veya düz bir eğilim grafiğine sahip olan her bir sayacı temizlemek biraz zaman alabilir.

Bu işlemi hızlandırmak için **histogram** görünümünü kullanın ve görünüm türünü **çizgi** **iken çubuk grafik olarak değiştirin**. Bu, size bir çubuk grafik verecektir. Toplama sırasında yüksek CPU kullanımı deneyimi sunan işlemlerin kolayca görülmesini daha kolay bulacaksınız.

Her zaman **Toplam**için bir çubuk olacağı için, yüksek bir tükenme oranını gösteren çubuklara odaklanın. Görünümü temizlemek için diğer çubukları silebilirsiniz. Şimdi **satır** görünümüne geri kaydır.

  ![Performans İzleyicisi göstergeleri](./media/troubleshoot-high-cpu-issues-azure-windows-vm/14-performance-monitor-2.png)

Şimdi de bu süreci yakalamak daha kolay. Varsayılan olarak, **Max** ve **Min** değerleri, sunucudaki çekirdeklerin veya işlemin iş parçacıklarının katsayısıdır.

  ![Performans İzleyicisi sonuçları](./media/troubleshoot-high-cpu-issues-azure-windows-vm/15-performance-monitor-3.png)

Kullanılabilir araçların listesi, Perfmon için performans açısından bitmez. **ProcessMonitor** (ProcMon) veya **XPerf**gibi diğer araçlara erişiminiz vardır. Gerektiğinde kullanabileceğiniz birçok üçüncü taraf araç mevcuttur.

### <a name="azure-monitoring-tools"></a>Azure izleme araçları

Azure VM 'Ler, CPU, ağ g/ç ve g/ç bayt gibi temel bilgileri içeren güvenilir ölçümlere sahiptir. Azure Izleyici gibi gelişmiş ölçümler için, belirttiğiniz bir depolama hesabını yapılandırmak ve kullanmak üzere yalnızca birkaç seçim yapmanız gerekir.

#### <a name="basic-default-counters"></a>Temel (varsayılan) sayaçlar

  ![Grafik ölçümleri](./media/troubleshoot-high-cpu-issues-azure-windows-vm/16-chart-metrics.png)

#### <a name="enabling-azure-monitor"></a>Azure Izleyicisini etkinleştirme

Azure Izleyici ölçümlerini etkinleştirdikten sonra, yazılım VM 'ye bir uzantı yükleyerek, sonra da Perfmon sayaçlarını içeren ayrıntılı ölçümleri toplamaya başlar.

  ![Tanılama depolama hesabı](./media/troubleshoot-high-cpu-issues-azure-windows-vm/17-diagnostics-storage-account.png)

**Temel** sayaç kategorileri **varsayılan**olarak ayarlanır. Ancak, **özel** bir koleksiyon da ayarlayabilirsiniz.

  ![Performans sayaçları](./media/troubleshoot-high-cpu-issues-azure-windows-vm/18-performance-counters.png)

Ayarlar etkinleştirildikten sonra, bu **Konuk** sayaçlarını **ölçümler** bölümünde görüntüleyebilirsiniz. Ölçümler belirli bir eşiğe ulaştığında da **Uyarılar** (e-posta iletileri dahil) ayarlayabilirsiniz.

  ![Ölçüm ad alanı](./media/troubleshoot-high-cpu-issues-azure-windows-vm/19-metrics-namespace.png)

Azure izleyici 'yi Azure sanal makinelerini yönetmek üzere kullanma hakkında daha fazla bilgi için bkz. Azure [sanal makinelerini Azure izleyici Ile izleme](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-vm-azure).

### <a name="reactive-troubleshooting"></a>Reaktif sorun giderme

Sorun zaten oluştuysa, ilk yerde yüksek CPU sorununa neden olan sorunu bulmanız gerekir. Reakdesteklemeyle karmaşık olabilir. Sorun zaten oluştuğu için veri toplama modu yararlı olmayacak.

Bu sorun tek seferlik bir oluşumsa, hangi uygulamanın neden olduğunu belirlemek zor olabilir. Azure VM, OMS veya diğer tanılama izlemesini kullanacak şekilde yapılandırıldıysa, sorunun nedeni hakkında öngörüler elde edebilirsiniz.

Yinelenen bir model ile uğraşıyorsanız, sorunun oluşma süresini bir sonraki gerçekleşirken toplayın.

Perfinsıghts henüz **Zamanlanmış bir çalıştırma** özelliğine sahip değil. Ancak, Perfmon çalıştırılabilir ve komut satırı aracılığıyla zamanlanabilir.

### <a name="logman-command"></a>Logman komutu

**Logman Create Counter** komutu, komut satırı aracılığıyla Perfmon koleksiyonunu çalıştırmak, **Görev Yöneticisi**aracılığıyla zamanlamak veya uzaktan çalıştırmak için kullanılır.

**Örnek** (uzak koleksiyon modunu içerir)

`Logman create counter LOGNAME -u DOMAIN\USERNAME * -f bincirc -v mmddhhmm -max 300 -c "\\SERVERNAME\LogicalDisk(*)\*" "\\SERVERNAME\Memory\*" "\\SERVERNAME\Network Interface(*)\*" "\\SERVERNAME\Paging File(*)\*" "\\SERVERNAME\PhysicalDisk(*)\*" "\\SERVERNAME\Process(*)\*" "\\SERVERNAME\Redirector\*" "\\SERVERNAME\Server\*" "\\SERVERNAME\System\*" "\\SERVERNAME\Terminal Services\*" "\\SERVERNAME\Processor(*)\*" "\\SERVERNAME\Cache\*" -si 00:01:00`

Aynı sanal ağdaki eş bir Azure VM bilgisayarından Logman.exe de başlatılabilir.

Bu parametreler hakkında daha fazla bilgi edinmek için bkz. [Logman Create Counter](https://docs.microsoft.com/windows-server/administration/windows-commands/logman-create-counter).

Sorun oluştuğu sırada Perfmon verileri toplandıktan sonra, verileri çözümlemek için geri kalan adımlar daha önce ele alınmıştır.

## <a name="conclusion"></a>Sonuç

Herhangi bir performans sorunu için iş yükünüzü anlamak, sorunu çözme anahtarıdır. Farklı VM SKU 'Larında ve farklı disk depolama seçeneklerinde bulunan seçenekler, odak üretim iş yüküne göre değerlendirilmelidir. Farklı VM 'lerde çözümlerin test edilmesi, en iyi kararı vermenize yardımcı olabilir.

Kullanıcı işlemleri ve veri miktarı değiştiğinden, sanal makinenin bilgi işlem, ağ ve g/ç özellikleri ' nde her zaman bir arabellek saklayın. Artık iş yükünde ani değişiklikler harika bir etkiye sahip değildir.

İş yükünü yakında artırdıysanız daha fazla bilgi işlem gücü olan daha yüksek bir SKU 'ya geçin. İş yükü işlem açısından yoğun olarak kullanılıyorsa VM SKU 'Larını akıllıca seçin.
