---
title: PerfInsights Microsoft Azure'da nasıl kullanılır? Microsoft Dokümanlar
description: Windows VM performans sorunlarını gidermek için PerfInsights'ı nasıl kullanacağınızı öğrenir.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: dcscontentpm
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 783b479dd3e5f429516799d7d3ea82f363cac2ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250133"
---
# <a name="how-to-use-perfinsights"></a>PerfInsights’ı kullanma

[PerfInsights,](https://aka.ms/perfinsightsdownload) tanılama verilerini toplayan ve analiz eden ve Azure'daki Windows sanal makine performansı sorunlarını gidermeye yardımcı olacak bir rapor sağlayan bir kendi kendine yardım tanılama aracıdır. PerfInsights, sanal makinelerde bağımsız bir araç olarak, doğrudan [Azure sanal makineler için Performans Tanılama'yı](performance-diagnostics.md)kullanarak veya Azure Performans [Tanılama VM Uzantısı](performance-diagnostics-vm-extension.md)yükleyerek çalıştırılabilir.

Destekle iletişime geçmeden önce sanal makinelerle ilgili performans sorunları yaşıyorsanız, bu aracı çalıştırın.

## <a name="supported-troubleshooting-scenarios"></a>Desteklenen sorun giderme senaryoları

PerfInsights çeşitli türde bilgileri toplayabilir ve analiz edebilir. Aşağıdaki bölümler sık karşılaşılan senaryoları kapsar.

### <a name="quick-performance-analysis"></a>Hızlı performans analizi

Bu senaryo, disk yapılandırmasını ve aşağıdakiler dahil olmak üzere diğer önemli bilgileri toplar:

-   Olay günlükleri

-   Tüm gelen ve giden bağlantılar için ağ durumu

-   Ağ ve güvenlik duvarı yapılandırma ayarları

-   Şu anda sistemde çalışan tüm uygulamalar için görev listesi

-   Microsoft SQL Server veritabanı yapılandırma ayarları (VM SQL Server çalıştıran bir sunucu olarak tanımlanırsa)

-   Depolama güvenilirlik sayaçları

-   Önemli Windows düzeltmeleri

-   Yüklü filtre sürücüleri

Bu, sistemi etkilememesi gereken pasif bir bilgi koleksiyonudur. 

>[!Note]
>Bu senaryo otomatik olarak aşağıdaki senaryoların her birine dahil edilir:

### <a name="benchmarking"></a>Kıyaslama

Bu senaryo, VM'ye bağlı tüm sürücüler için [Diskspd](https://github.com/Microsoft/diskspd) kıyaslama testini (IOPS ve MBPS) çalıştırır. 

> [!Note]
> Bu senaryo sistemi etkileyebilir ve canlı üretim sisteminde çalıştırılmamalıdır. Gerekirse, herhangi bir sorun önlemek için özel bir bakım penceresinde bu senaryoyu çalıştırın. İzleme veya kıyaslama testinin neden olduğu artan iş yükü, VM'nizin performansını olumsuz etkileyebilir.
>

### <a name="performance-analysis"></a>Performans analizi

Bu senaryo RuleEngineConfig.json dosyasında belirtilen sayaçları kullanarak bir [performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) izleme çalışır. VM, SQL Server çalıştıran bir sunucu olarak tanımlanırsa, bir performans sayacı izleme çalıştırılır. Bunu RuleEngineConfig.json dosyasında bulunan sayaçları kullanarak yapar. Bu senaryo da performans tanılama verileri içerir.

### <a name="azure-files-analysis"></a>Azure Dosyaları analizi

Bu senaryo, ağ izlemesiyle birlikte özel bir performans sayacı yakalama yı çalıştırZ. Yakalama, tüm Sunucu İleti Bloğu (SMB) istemci paylaşım sayaçlarını içerir. Yakalamanın bir parçası olan bazı önemli Kobİ istemci payı performans sayaçları şunlardır:

| **Tür**     | **SMB istemci hisseleri sayacı** |
|--------------|-------------------------------|
| IOPS         | Veri İstekleri/sn             |
|              | İstekleri Okuma/sn             |
|              | İstek/sn yazma            |
| Gecikme süresi      | Avg. sn/Veri İsteği         |
|              | Avg. sn/Oku                 |
|              | Avg. sn/Yaz                |
| IO Boyutu      | Avg. Bayt/Veri İsteği       |
|              | Avg. Bayt/Oku               |
|              | Avg. Bayt /Yazma              |
| Aktarım hızı   | Veri Baytları/sn                |
|              | Okuma Bayt/sn                |
|              | Yazma Bayt/sn               |
| Sırası Uzunluğu | Avg. Okuma Sıra Uzunluğu        |
|              | Avg. Sıra Uzunluğu Yaz       |
|              | Avg. Veri Sıra Uzunluğu        |

### <a name="advanced-performance-analysis"></a>Gelişmiş performans analizi

Gelişmiş bir performans çözümlemesi çalıştırdığınızda, paralel çalışacak izleri seçersiniz. İsterseniz, hepsini çalıştırabilirsiniz (Performance Counter, Xperf, Network ve StorPort).  

> [!Note]
> Bu senaryo sistemi etkileyebilir ve canlı üretim sisteminde çalıştırılmamalıdır. Gerekirse, herhangi bir sorun önlemek için özel bir bakım penceresinde bu senaryoyu çalıştırın. İzleme veya kıyaslama testinin neden olduğu artan iş yükü, VM'nizin performansını olumsuz etkileyebilir.
>

## <a name="what-kind-of-information-is-collected-by-perfinsights"></a>PerfInsights tarafından ne tür bilgiler toplanır?

Windows VM, diskler veya depolama havuzları yapılandırması, performans sayaçları, günlükler ve çeşitli izlemeler hakkında bilgiler toplanır. Kullandığınız performans senaryosuna bağlıdır. Ayrıntılar aşağıdaki tabloda verilmiştir:

|Toplanan veriler                              |  |  | Performans senaryoları |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                               | Hızlı performans analizi | Kıyaslama | Performans analizi | Azure Dosyaları analizi | Gelişmiş performans analizi |
| Olay günlüklerinden bilgiler       | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Sistem bilgileri                | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Birim haritası                        | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Disk haritası                          | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Görevleri çalıştırma                     | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Depolama güvenilirlik sayaçları      | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Depolama bilgileri               | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Fsutil çıkışı                     | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Sürücü bilgilerini filtrele                | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Netstat çıktısı                    | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Ağ yapılandırması             | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Güvenlik duvarı yapılandırması            | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| SQL Server yapılandırması          | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Performans tanılama izleri *  | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Performans sayacı izleme **      |                            |                                    | Evet                      |                      | Evet                  |
| Kobİ sayaç izleme **              |                            |                                    |                          | Evet                  |                      |
| SQL Server sayaç takibi **       |                            |                                    | Evet                      |                      | Evet                  |
| Xperf izi                       |                            |                                    |                          |                      | Evet                  |
| StorPort izleme                    |                            |                                    |                          |                      | Evet                  |
| Ağ izleme                     |                            |                                    |                          | Evet                  | Evet                  |
| Diskspd kıyaslama izleme ***       |                            | Evet                                |                          |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Performans tanılama izleme (*)

Veri toplamak ve devam eden performans sorunlarını tanılamak için arka planda kural tabanlı bir altyapı çalıştırın. Aşağıdaki kurallar şu anda desteklenir:

- HighCpuKullanım kuralı: Yüksek CPU kullanım dönemlerini algılar ve bu dönemlerde en iyi CPU kullanım tüketicilerini gösterir.
- HighDiskKullanım kuralı: Fiziksel disklerde yüksek disk kullanım sürelerini algılar ve bu dönemlerde üst disk kullanım tüketicilerini gösterir.
- HighResolutionDiskMetric kuralı: Her fiziksel disk için 50 milisaniye başına IOPS, iş ve G gecikme ölçümlerini gösterir. Disk azaltma dönemlerini hızla belirlemeye yardımcı olur.
- HighMemoryUsage kuralı: Yüksek bellek kullanım sürelerini algılar ve bu dönemlerde en iyi bellek kullanım tüketicilerini gösterir.

> [!NOTE] 
> Şu anda,.NET Framework 4.5 veya sonraki sürümlerini içeren Windows sürümleri desteklenir.

### <a name="performance-counter-trace-"></a>Performans sayacı izleme (**)

Aşağıdaki performans sayaçlarını toplar:

- \İşlem, \İşlemci, \Bellek, \İş Parçacığı, \PhysicalDisk ve \LogicalDisk
- \Önbellek\Kirli Sayfalar, \Önbellek\Tembel Yazma Floşlar/sn, \Sunucu\Havuz Sayfasız, Hatalar ve \Sunucu\Havuz Sayfalı Hatalar
- \Ağ Arabirimi, \IPv4\Datagramlar, \IPv6\Datagramlar, \TCPv4\Segmentler, \TCPv6\Segmentler, \Ağ Bağdaştırıcısı, \WFPv4\Paketler, \WFPv6\Paketler, \UDPv4\Datagramlar, \UDPv6\Datagramlar, \TCPv4\Bağlantı, \TCPv6\Bağlantı, \ Ağ QoS İlkesi\Paketleri, \İşlemci Başına Ağ Arabirimi Kartı Etkinliği ve \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>SQL Server örnekleri için
- \SQL Server:Buffer Manager, \SQLServer:Kaynak Havuzu İstatistikleri ve \SQLServer:SQL İstatistikleri\
- \SQLServer:Kilitler, \SQLServer:Genel, İstatistik
- \SQLServer:Erişim Yöntemleri

#### <a name="for-azure-files"></a>Azure Dosyaları için
\SMB Müşteri Paylaşımları

### <a name="diskspd-benchmark-trace-"></a>Diskspd kıyaslama izleme (***)
Diskspd G/Ç iş yükü testleri (OS Disk [yazma] ve havuz sürücüleri [okuma/yazma])

## <a name="run-the-perfinsights-tool-on-your-vm"></a>VM'nizde PerfInsights aracını çalıştırın

### <a name="what-do-i-have-to-know-before-i-run-the-tool"></a>Aracı çalıştırmadan önce bilmem gerekenler nelerdir? 

#### <a name="tool-requirements"></a>Araç gereksinimleri

-  Bu araç, performans sorunu olan VM üzerinde çalıştırılmalıdır. 

-  Aşağıdaki işletim sistemleri desteklenir: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016; Windows 8.1 ve Windows 10.

#### <a name="possible-problems-when-you-run-the-tool-on-production-vms"></a>Aracı üretim VM'lerinde çalıştırdığınızda olası sorunlar

-  Kıyaslama senaryosu veya Xperf veya Diskspd kullanmak üzere yapılandırılan "Gelişmiş performans çözümlemesi" senaryosu için araç VM'nin performansını olumsuz etkileyebilir. Bu senaryolar canlı üretim ortamında çalıştırılmamalıdır.

-  Kıyaslama senaryosu veya Diskspd'i kullanacak şekilde yapılandırılan "Gelişmiş performans çözümlemesi" senaryosu için, g/Ç iş yükünü başka hiçbir arka plan etkinliğinin karışmadığından emin olun.

-  Varsayılan olarak, araç veri toplamak için geçici depolama sürücüsükullanır. İzleme daha uzun süre etkin kalırsa, toplanan veri miktarı alakalı olabilir. Bu, geçici diskteki alanın kullanılabilirliğini azaltabilir ve bu nedenle bu sürücüye dayanan tüm uygulamaları etkileyebilir.

### <a name="how-do-i-run-perfinsights"></a>PerfInsights'ı nasıl çalıştırıyorum? 

[Azure Performans Tanılama VM Uzantısı'nı](performance-diagnostics-vm-extension.md)yükleyerek PerfInsights'ı sanal bir makinede çalıştırabilirsiniz. Ayrıca bağımsız bir araç olarak çalıştırabilirsiniz. 

**Azure portalından PerfInsights'ı yükleyin ve çalıştırın**

Bu seçenek hakkında daha fazla bilgi için [Azure Performans Tanılama VM Uzantısı yükle'ye](performance-diagnostics-vm-extension.md#install-the-extension)bakın.  

**PerfInsights'ı bağımsız modda çalıştırın**

PerfInsights aracını çalıştırmak için aşağıdaki adımları izleyin:


1. Karşıdan yükleme [PerfInsights.zip](https://aka.ms/perfinsightsdownload).

2. PerfInsights.zip dosyasının engelini kaldırın. Bunu yapmak için PerfInsights.zip dosyasına sağ tıklayın ve **Özellikler'i**seçin. **Genel** sekmesinde, **Engeli Kaldır'ı**ve ardından **Tamam'ı**seçin. Bu, aracın ek güvenlik istemleri olmadan çalışan olmasını sağlar.  

    ![Engelsiz vurgulanmış PerfInsights Özellikleri ekran görüntüsü](media/how-to-use-perfInsights/pi-unlock-file.png)

3.  Sıkıştırılmış PerfInsights.zip dosyasını geçici sürücünüze genişletin (varsayılan olarak bu genellikle D sürücüsüdür). 

4.  Windows komut istemini yönetici olarak açın ve kullanılabilir komut satırı parametrelerini görüntülemek için PerfInsights.exe'yi çalıştırın.

    ```
    cd <the path of PerfInsights folder>
    PerfInsights
    ```
    ![PerfInsights komut satırı çıkışının ekran görüntüsü](media/how-to-use-perfInsights/pi-commandline.png)
    
    PerfInsights senaryolarını çalıştırmak için temel sözdizimi:
    
    ```
    PerfInsights /run <ScenarioName> [AdditionalOptions]
    ```

    Aşağıdaki örneği kullanarak performans analizi senaryosunu 5 dakika boyunca çalıştırabilirsiniz:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Gelişmiş senaryoyu Xperf ve Performans sayacı izleriyle 5 dakika boyunca çalıştırmak için aşağıdaki örneği kullanabilirsiniz:
    
    ```
    PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Aşağıdaki örneği kullanarak 5 dakika boyunca performans analizi senaryosunu çalıştırabilir ve sonuç zip dosyasını depolama hesabına yükleyebilirsiniz:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>
    ```

    **/list** komutunu kullanarak tüm kullanılabilir senaryoları ve seçenekleri inceleyebilirsiniz:
    
    ```
    PerfInsights /list
    ```

    >[!Note]
    >Bir senaryoyu çalıştırmadan önce, PerfInsights kullanıcıdan tanılama bilgilerini paylaşmayı ve EULA'yı kabul etmeyi kabul etmesini ister. Bu istemleri atlamak için **/AcceptDisclaimerAndShareDiagnostics** seçeneğini kullanın. 
    >
    >Microsoft ile etkin bir destek biletiniz varsa ve birlikte çalıştığınız destek mühendisinin isteği ne olursa olsun PerfInsights çalıştırıyorsanız, **/sr** seçeneğini kullanarak destek bilet numarasını verdiğinizden emin olun.
    >
    >Varsayılan olarak, PerfInsights varsa kendisini en son sürüme güncellemeyi deneyecektir. Otomatik güncelleştirmeyi atlamak için **/SkipAutoUpdate** veya **/sau** parametresini kullanın.  
    >
    >Süre anahtarı **/d** belirtilmemişse, PerfInsights vmslow, azurefilesve gelişmiş senaryoları çalıştırırken sorunu yeniden düzenlemenizi ister. 

İzlemeler veya işlemler tamamlandığında, PerfInsights ile aynı klasörde yeni bir dosya görüntülenir. Dosyanın adı **PerformanceDiagnostics\_yyyy-MM-dd\_hh-mm-ss-fff.zip'tir.** Bu dosyayı çözümleme için destek aracısına gönderebilir veya bulguları ve önerileri gözden geçirmek için zip dosyası içindeki raporu açabilirsiniz.

## <a name="review-the-diagnostics-report"></a>Tanılama raporunu gözden geçirme

**PerformanceDiagnostics\_\_yyyy-MM-dd hh-mm-ss-fff.zip** dosyasında, PerfInsights'ın bulgularını ayrıntılarıyla gösteren bir HTML raporu bulabilirsiniz. Raporu gözden geçirmek için **PerformanceDiagnostics\_yyyy-MM-dd\_hh-mm-ss-fff.zip** dosyasını genişletin ve ardından **PerfInsights Report.html** dosyasını açın.

**Bulgular** sekmesini seçin.

![PerfInsights Raporu](media/how-to-use-perfInsights/pi-finding-tab.png)
![Ekran Görüntüsü PerfInsights Raporu](media/how-to-use-perfInsights/pi-findings.png)

> [!NOTE] 
> Yüksek olarak sınıflandırılan bulgular, performans sorunlarına neden olabilecek bilinen sorunlardır. Orta olarak sınıflandırılan bulgular, performans sorunlarına neden olmayan en iyi olmayan yapılandırmaları temsil eder. Düşük olarak kategorize edilen bulgular yalnızca bilgilendirici ifadelerdir.

Tüm yüksek ve orta bulgular için önerileri ve bağlantıları gözden geçirin. Performansı nasıl etkileyebilecekleri ve performans için en iyi yapılandırmalar hakkında bilgi edinin.

### <a name="storage-tab"></a>Depolama sekmesi

**Bulgular** bölümünde depolamayla ilgili çeşitli bulgular ve öneriler görüntülenir.

**Disk Eşlemi** ve **Birim Eşlemi** bölümleri, mantıksal birimlerin ve fiziksel disklerin birbiriyle nasıl ilişkili olduğunu açıklar.

Fiziksel disk perspektifinde (Disk Eşlemi) tablo, diskte çalışan tüm mantıksal birimleri gösterir. Aşağıdaki örnekte, **PhysicalDrive2** birden çok bölüm (J ve H) üzerinde oluşturulan iki mantıksal birim çalışır:

![Disk sekmesinin ekran görüntüsü](media/how-to-use-perfInsights/pi-disk-tab.png)

Birim perspektifinde (Birim Eşlemi) tablolar, her mantıksal birimin altındaki tüm fiziksel diskleri gösterir. RAID/Dinamik diskler için birden çok fiziksel diskte mantıksal bir birim çalıştırabileceğinize dikkat edin. Aşağıdaki örnekte, *\\C: mount* fiziksel diskler 2 ve 3 *SpannedDisk* olarak yapılandırılan bir montaj noktasıdır:

![Ses sekmesinin ekran görüntüsü](media/how-to-use-perfInsights/pi-volume-tab.png)

### <a name="sql-tab"></a>SQL sekmesi

Hedef VM herhangi bir SQL Server örneğini barındırıyorsa, raporda **SQL**adlı ek bir sekme görürsünüz:

![SQL sekmesinin ekran görüntüsü](media/how-to-use-perfInsights/pi-sql-tab.png)

Bu bölümde, VM'de barındırılan SQL Server örneklerinin her biri için bir **Bulgular** sekmesi ve ek sekmeler bulunur.

**Bulgular** sekmesi, bulunan tüm SQL ile ilgili performans sorunlarının ve önerilerin bir listesini içerir.

Aşağıdaki örnekte, **PhysicalDrive0** (C sürücüsünü çalıştıran) görüntülenir. Bunun nedeni, hem **modeldev** hem de **modelgünlüğü** dosyalarının C sürücüsünde bulunması ve farklı türde olmalarıdır (sırasıyla veri dosyası ve işlem günlüğü gibi).

![Günlük bilgilerinin ekran görüntüsü](media/how-to-use-perfInsights/pi-log-info.png)

SQL Server'ın belirli örneklerine yönelik sekmeler, seçili örnekle ilgili temel bilgileri görüntüleyen genel bir bölüm içerir. Sekmeler, ayarlar, yapılandırmalar ve kullanıcı seçenekleri de dahil olmak üzere gelişmiş bilgiler için ek bölümler de içerir.

### <a name="diagnostic-tab"></a>Tanılama sekmesi
**Tanılama** sekmesi, PerfInsights'ın çalıştırılış süresi boyunca bilgisayardaki en iyi IŞLEMCI, disk ve bellek tüketicileri hakkında bilgi içerir. Ayrıca, sistemin eksik olabileceği kritik düzeltme emaları, görev listesi ve önemli sistem olayları hakkında da bilgi bulabilirsiniz. 

## <a name="references-to-the-external-tools-used"></a>Kullanılan dış araçlara başvurular

### <a name="diskspd"></a>Diskspd

Diskspd, Microsoft'un depolama yük jeneratörü ve performans test aracıdır. Daha fazla bilgi için Bkz. [Diskspd.](https://github.com/Microsoft/diskspd)

### <a name="xperf"></a>Xperf

Xperf, Windows Performans Araç Kiti'nden izlemeler yakalamak için bir komut satırı aracıdır. Daha fazla bilgi için [Windows Performance Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/)' a bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla inceleme için tanılama günlüklerini ve raporlarını Microsoft Destek'e yükleyebilirsiniz. Destek, sorun giderme işlemine yardımcı olmak için PerfInsights tarafından oluşturulan çıktıyı iletmenizi isteyebilir.

Aşağıdaki ekran görüntüsü, alabileceklerinize benzer bir ileti gösterir:

![Microsoft Desteği'nden örnek iletinin ekran görüntüsü](media/how-to-use-perfInsights/pi-support-email.png)

Dosya aktarım çalışma alanına erişmek için iletideki yönergeleri izleyin. Ek güvenlik için parolanızı ilk kullanımda değiştirmeniz gerekir.

Oturum açtıktan sonra, PerfInsights tarafından toplanan **PerformanceDiagnostics\_\_yyyy-MM-dd hh-mm-ss-fff.zip** dosyasını yüklemek için bir iletişim kutusu bulacaksınız.

