---
title: Azure 'da SQL Server için performans yönergeleri | Microsoft Docs
description: Microsoft Azure Sanal Makineler SQL Server performansını iyileştirmek için yönergeler sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/18/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 3ce829a9fd58fb2940ee3265a66717af3dc9c0b5
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289055"
---
# <a name="performance-guidelines-for-sql-server-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server için performans yönergeleri
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede Microsoft Azure Sanal Makineler SQL Server performansını iyileştirmeye yönelik rehberlik sunulmaktadır.

## <a name="overview"></a>Genel Bakış

 Azure sanal makinelerinde SQL Server çalıştırılırken, şirket içi sunucu ortamlarında SQL Server için geçerli olan veritabanı performansı ayarlama seçeneklerini kullanmaya devam etmenizi öneririz. Bununla birlikte genel buluttaki bir ilişkisel veritabanının performansı, sanal makinenin boyutu ve veri disklerinin yapılandırması gibi birçok faktöre bağlıdır.

[Azure Portal sağlanan görüntüleri SQL Server](sql-vm-create-portal-quickstart.md) genel depolama yapılandırması en iyi yöntemlerini izleyin (depolamanın nasıl yapılandırıldığı hakkında daha fazla bilgi için bkz. [SQL Server sanal makineler (VM) için depolama yapılandırması](storage-configuration.md)). Sağlamaktan sonra, bu makalede ele alınan diğer iyileştirmeleri uygulamayı düşünün. Seçimlerinizi iş yükünüze dayandırın ve test aracılığıyla doğrulayın.

> [!TIP]
> Genellikle maliyetleri iyileştirmek ve performansı iyileştirmek arasında bir denge vardır. Bu makale, Azure sanal makinelerinde SQL Server için *en iyi* performansı almaya odaklanır. İş yükünüz daha az güç alıyorsa, aşağıda listelenen her iyileştirme gerektirmeyebilir. Bu önerileri değerlendirdiğiniz için performans ihtiyaçlarınızı, maliyetlerinizi ve iş yükü desenlerini göz önünde bulundurun.

## <a name="quick-checklist"></a>Hızlı denetim listesi

Aşağıda Azure sanal makinelerinde SQL Server en iyi performansı için hızlı bir denetim listesi verilmiştir:

| Alan | İyileştirmeler |
| --- | --- |
| [VM boyutu](#vm-size-guidance) | -VM boyutlarını [E4S_v3](../../../virtual-machines/ev3-esv3-series.md) veya üzeri ya da [DS12_v2](../../../virtual-machines/dv2-dsv2-series-memory.md) ya da daha yüksek bir şekilde 4 veya daha fazla vCPU kullanın.<br/><br/> - [Es, EAS, DS ve DAS serisi](../../../virtual-machines/sizes-general.md) , OLTP iş yükü performansı için gereken En Iyi belleği vCPU oranına sunmaktadır. <br/><br/> - [E serisi](../../../virtual-machines/m-series.md) , görev açısından kritik performans için gereken en yüksek bellek oranını ve veri ambarı iş yükleri için ideal olduğunu sunar. <br/><br/> - [Uygulama performansı gereksinimleri denetim listesini](../../../virtual-machines/windows/premium-storage-performance.md#application-performance-requirements-checklist) izleyerek hedef Iş yükünün [IOPS](../../../virtual-machines/windows/premium-storage-performance.md#iops)'sini, [aktarım hızını](../../../virtual-machines/windows/premium-storage-performance.md#throughput) ve [gecikme süresini](../../../virtual-machines/windows/premium-storage-performance.md#latency) yoğun saatlerde toplayın ve Iş yükünüzün performans gereksinimlerine ölçeklenebilen [VM boyutunu](../../../virtual-machines/sizes-general.md) seçin.|
| [Depolama](#storage-guidance) | -TPC-E ve TPC_C kıyaslamalar ile Azure sanal makinelerinde SQL Server performansının ayrıntılı testi için, [OLTP performansını iyileştirme](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794)başlıklı bloga başvurun. <br/><br/> -En iyi fiyat/performans avantajları için [Premium SSD 'ler](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794) kullanın. Veri dosyaları için [salt okunur önbelleği](../../../virtual-machines/windows/premium-storage-performance.md#disk-caching) yapılandırma ve günlük dosyası için önbellek yok. <br/><br/> -İş yükü için 1 MS 'den az depolama gecikme süresi gerekliyse [Ultra diskleri](../../../virtual-machines/windows/disks-types.md#ultra-disk) kullanın. Daha fazla bilgi edinmek için bkz. [Ultra diske geçiş](storage-migrate-to-ultradisk.md) . <br/><br/> -Disk türünü seçmeden önce [uygulamayı izleyerek](../../../virtual-machines/windows/premium-storage-performance.md#application-performance-requirements-checklist) SQL Server verileri, günlüğü ve Temp DB dosyaları için depolama gecikmesi gereksinimlerini toplayın. <1 MS depolama gecikmeleri gerekliyse, Ultra diskler ' i kullanın, aksi takdirde Premium SSD kullanın. Düşük gecikme süreleri yalnızca günlük dosyası için gerekliyse ve veri dosyaları için gerekli değilse, yalnızca günlük dosyası için gerekli ıOPS ve üretilen iş düzeylerinde [Ultra disk sağlayın](../../../virtual-machines/windows/disks-enable-ultra-ssd.md) . <br/><br/> -  [Premium dosya paylaşımları](failover-cluster-instance-premium-file-share-manually-configure.md) , bir SQL Server yük devretme kümesi örneği için paylaşılan depolama alanı olarak önerilir. Premium dosya paylaşımları, önbelleğe almayı desteklemez ve Premium SSD disklerine kıyasla sınırlı performansı sunar. Tek başına SQL örnekleri için Premium dosya paylaşımları üzerinden Premium SSD tarafından yönetilen diskler seçin; Ancak, bakım kolaylığı ve esnek ölçeklenebilirlik için yük devretme kümesi örneği paylaşılan depolama için Premium dosya paylaşımlarından yararlanın. <br/><br/> -Standart depolama yalnızca geliştirme ve test amaçları için ya da yedekleme dosyaları için önerilir ve üretim iş yükleri için kullanılmamalıdır. <br/><br/> - [Depolama hesabını](../../../storage/common/storage-create-storage-account.md) ve SQL Server VM aynı bölgede saklayın.<br/><br/> -Depolama hesabında Azure [coğrafi olarak yedekli depolamayı](../../../storage/common/storage-redundancy.md) (coğrafi çoğaltma) devre dışı bırakın.  |
| [Disklerinden](#disks-guidance) | -En az 2 [PREMIUM SSD disk](../../../virtual-machines/windows/disks-types.md#premium-ssd) kullanın (günlük dosyası için 1 ve veri dosyaları için 1). <br/><br/> -<1 MS GÇ gecikme süreleri gerektiren iş yükleri için, M serisi için yazma hızlandırıcıyı etkinleştirin ve es ve DS serisi için Ultra SSD diskleri kullanmayı düşünün. <br/><br/> -Veri dosyalarını barındıran diskler üzerinde [salt okuma işlemini](../../../virtual-machines/windows/premium-storage-performance.md#disk-caching) etkinleştirin.<br/><br/> - [SQL Server verileri, günlüğü ve tempdb dosyaları için depolamayı yapılandırırken](storage-configuration.md) iş yükünüze göre %20 ' ye AIT ek IOPS/verimlilik kapasitesi ekleyin <br/><br/> -Veritabanı depolama veya günlük kaydı için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br/><br/> -Günlük dosyasını barındıran disk (ler) i önbelleğe almayı etkinleştirmeyin.  **Önemli**: bir Azure sanal makineler diskinin önbellek ayarlarını değiştirirken SQL Server hizmetini durdurun.<br/><br/> -Daha fazla depolama verimi sağlamak için birden çok Azure veri diski dizili.<br/><br/> -Belgelenmiş ayırma boyutlarıyla biçimlendirin. <br/><br/> - `D:\` Görev açısından kritik SQL Server iş yükleri için yerel SSD sürücüsüne tempdb yerleştirin (doğru VM boyutunu seçtikten sonra). VM 'yi Azure portal veya Azure hızlı başlangıç şablonlarından oluşturup [GEÇICI veritabanını yerel diske yerleştirirseniz](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583) , başka bir işlem yapmanız gerekmez; Tüm diğer durumlar için, başlatmalardaki hatalardan kaçınmak üzere [tempdb 'yi depolamak üzere SSD 'Yi kullanmak](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/) için blogdaki adımları izleyin. Yerel sürücünün kapasitesi, geçici DB boyutunuz için yeterli değilse, geçici DB 'yi [salt okuma önbelleği](../../../virtual-machines/windows/premium-storage-performance.md#disk-caching)olan Premium SSD disklerinde [dizili](../../../virtual-machines/windows/premium-storage-performance.md) bir depolama havuzuna yerleştirin. |
| [G/Ç](#io-guidance) |-Veritabanı sayfa sıkıştırmasını etkinleştirin.<br/><br/> -Veri dosyaları için anlık dosya başlatmayı etkinleştirin.<br/><br/> -Veritabanının otomatik büyümesini sınırlayın.<br/><br/> -Veritabanının bir daha küçültmeyi devre dışı bırakın.<br/><br/> -Sistem veritabanları dahil olmak üzere tüm veritabanlarını veri disklerine taşıyın.<br/><br/> -SQL Server hata günlüğü ve izleme dosyası dizinlerini veri disklerine taşıyın.<br/><br/> -Varsayılan yedekleme ve veritabanı dosya konumlarını yapılandırın.<br/><br/> - [Bellekte kilitli sayfaları etkinleştirin](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows?view=sql-server-2017).<br/><br/> -SQL Server performans düzeltmeleri uygulayın. |
| [Özelliğe özgü](#feature-specific-guidance) | -Doğrudan Azure Blob depolamaya yedekleme.<br/><br/>-12 TB 'den büyük veritabanları için [dosya anlık görüntüsü yedeklemeleri](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) kullanın. <br/><br/>-Birden çok Temp DB dosyası, çekirdek başına 1 dosya, en fazla 8 dosya kullanın.<br/><br/>-Işletim sistemi için en fazla sunucu belleğini %90 veya en fazla 50 GB olacak şekilde ayarlayın. <br/><br/>-Geçici NUMA 'yı etkinleştirin. |

Bu iyileştirmelerin *nasıl* ve *neden* yapılacağı hakkında daha fazla bilgi için lütfen aşağıdaki bölümlerde sunulan ayrıntıları ve Kılavuzu gözden geçirin.

## <a name="vm-size-guidance"></a>VM boyut Kılavuzu

En yoğun zamanlarda iş yükünün CPU, bellek ve depolama verimlilik gereksinimlerini toplayarak başlayın. \LogicalDisk\Disk Okuma/sn ve \LogicalDisk\Disk yazma/sn performans sayaçları, okuma ve yazma ıOPS gereksinimlerini toplamak için kullanılabilir ve \LogicalDisk\Disk baytları/sn sayacı veri, günlük ve Geçici VERITABANı dosyaları için [depolama aktarım hızı gereksinimlerini](../../../virtual-machines/windows/premium-storage-performance.md#disk-caching) toplamak üzere kullanılabilir. IOPS ve verimlilik gereksinimleri yoğun olarak tanımlandıktan sonra VM boyutlarını değerlendir bu kapasiteyi sağlar. Örneğin, iş yükünüz 20 K okuma ıOPS ve 10.000 yazma ıOPS 'yi gerektiriyorsa, 2 P30 disk ile E16s_v3 (en fazla 32 K önbelleğe alınmış ve 25600 önbelleğe alınmış ıOPS ile) ya da M16_s (en fazla 20 KB önbelleğe alınmış ve 10.000, önbelleğe alınmış ıOPS ile) seçeneğini belirleyebilirsiniz. VM 'Ler, ıOPS ve aktarım hızı için farklı ölçek sınırlarına sahip olduğu için iş yükünün hem aktarım hızını hem de ıOPS gereksinimlerini anladığınızdan emin olun.<br/><br/>[DSv_3](../../../virtual-machines/dv3-dsv3-series.md) ve [Es_v3 serisi](../../../virtual-machines/ev3-esv3-series.md) , Intel Haswell veya çok iyi işlemciler içeren genel amaçlı donanımlarda barındırılır. [M serisi](../../../virtual-machines/m-series.md) en büyük SQL Server iş yükleri için en yüksek vCPU sayısını ve belleğini sunar ve ufuk Gölü işlemci ailesiyle bellek için iyileştirilmiş donanımda barındırılır. Bu VM Serisi, ana bilgisayar düzeyinde okuma önbelleğiyle en iyi performansı elde etmek için önerilen Premium depolamayı destekler. Hem Es_v3 hem de M serisi, daha düşük işlem ve yüksek depolama kapasitesi taleplerine sahip iş yükleri için para tasarrufu sağlayan [kısıtlı çekirdek boyutlarında](../../../virtual-machines/windows/constrained-vcpu.md)de mevcuttur. 

## <a name="storage-guidance"></a>Depolama yönergeleri

TPC-E ve TPC_C kıyaslamalar ile Azure sanal makinelerinde SQL Server performansının ayrıntılı testi için, [OLTP performansını iyileştirme](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794)başlıklı bloga başvurun. 

Tüm üretim iş yükleri için Premium SSD 'Ler içeren Azure Blob önbelleği önerilir. 

> [!WARNING]
> Standart HDD 'Ler ve SSD 'Ler, değişen gecikme süreleri ve bant genişliğine sahiptir ve yalnızca geliştirme ve test iş yükleri için önerilir. Üretim iş yükleri Premium SSD 'Ler kullanmalıdır.

Ayrıca, aktarım gecikmelerini azaltmak için Azure depolama hesabınızı SQL Server sanal makinelerinize göre aynı veri merkezinde oluşturmanızı öneririz. Bir depolama hesabı oluştururken, Coğrafi çoğaltmayı devre dışı bırakın, birden fazla diskte tutarlı bir yazma sırası garanti edilmez. Bunun yerine, iki Azure veri merkezi arasında SQL Server olağanüstü durum kurtarma teknolojisini yapılandırmayı düşünün. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](business-continuity-high-availability-disaster-recovery-hadr-overview.md).

## <a name="disks-guidance"></a>Diskler Kılavuzu

Azure sanal makinelerinde üç ana disk türü vardır:

* **İşletim sistemi diski**: bir Azure sanal makinesi oluşturduğunuzda platform, işletim SISTEMI diskinizin VM 'sine en az bir disk ( **C** sürücüsü olarak etiketlenir) ekler. Bu disk, depolamada bir Sayfa Blobu olarak depolanan bir VHD 'dir.
* **Geçici disk**: Azure sanal makineleri, geçici disk adlı başka bir disk ( **D**: sürücüsü olarak etiketlenir) içerir. Bu, düğüm üzerinde karalama alanı için kullanılabilecek bir disktir.
* **Veri diskleri**: sanal makinenize veri diski olarak ek diskler de ekleyebilirsiniz ve bunlar, sayfa Blobları olarak depolama alanında depolanır.

Aşağıdaki bölümlerde bu farklı diskleri kullanmaya yönelik öneriler açıklanır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

İşletim sistemi diski, işletim sisteminin çalışan bir sürümü olarak önyükleme yapabilir ve takabilirsiniz ve **C** sürücüsü olarak ETIKETLEDIĞINIZ bir VHD 'dir.

İşletim sistemi diskinde varsayılan önbelleğe alma ilkesi **okuma/yazma**. Performans duyarlı uygulamalar için, işletim sistemi diski yerine veri disklerini kullanmanızı öneririz. Aşağıdaki veri disklerinin bölümüne bakın.

### <a name="temporary-disk"></a>Geçici disk

**D** sürücüsü olarak etiketlenen geçici depolama sürücüsü Azure Blob depolama alanına kalıcı değildir. Kullanıcı veritabanı dosyalarınızı veya Kullanıcı işlem günlüğü dosyalarını **D**: sürücüsünde saklamayın.

`D:\`Görev açısından kritik SQL Server iş yükleri için (doğru VM boyutunu seçtikten sonra) tempdb 'yi yerel SSD sürücüsüne yerleştirin. VM 'yi Azure portal veya Azure hızlı başlangıç şablonlarından oluşturup [GEÇICI veritabanını yerel diske yerleştirirseniz](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583), başka bir işlem yapmanız gerekmez; Tüm diğer durumlar için, başlatmalardaki hatalardan kaçınmak üzere [tempdb 'yi depolamak üzere SSD 'Yi kullanmak](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/) için blogdaki adımları izleyin. Yerel sürücünün kapasitesi, geçici DB boyutunuz için yeterli değilse, geçici DB 'yi [salt okuma önbelleği](../../../virtual-machines/windows/premium-storage-performance.md#disk-caching)olan Premium SSD disklerinde [dizili](../../../virtual-machines/windows/premium-storage-performance.md) bir depolama havuzuna yerleştirin.

Premium SSD 'Leri destekleyen VM 'Ler için TempDB 'yi okuma önbelleği etkinken Premium SSD 'leri destekleyen bir diskte da saklayabilirsiniz.


### <a name="data-disks"></a>Veri diskleri

* **Veri ve günlük dosyaları için PREMIUM SSD disklerini kullanın**: disk şeritleme kullanmıyorsanız, bir diskin günlük dosyasını içerdiği ve diğeri verileri içerdiği ıkı Premium SSD diski kullanın. Her Premium SSD [,, makalesinde](../../../virtual-machines/windows/disks-types.md)gösterildiği gibi, boyutuna bağlı olarak BIR dizi IOPS ve bant GENIŞLIĞI (MB/s) sağlar. Depolama alanları gibi bir disk şeritleme tekniği kullanıyorsanız, biri günlük dosyası (ler) ve diğer veri dosyaları için olmak üzere iki havuza sahip olacak şekilde en iyi performansa ulaşmanız gerekir. Ancak, SQL Server yük devretme kümesi örnekleri (FCı) kullanmayı planlıyorsanız, bir havuz yapılandırmanız veya bunun yerine [Premium dosya paylaşımlarını](failover-cluster-instance-premium-file-share-manually-configure.md) kullanmanız gerekir.

   > [!TIP]
   > - Çeşitli disk ve iş yükü yapılandırmalarında test sonuçları için aşağıdaki blog gönderisine bakın: [Azure sanal makinelerinde SQL Server Için depolama yapılandırma yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/).
   > - ~ 50.000 ıOPS gerektiren SQL Server 'Lar için görev açısından kritik performans için, 10-P30 disklerini bir Ultra SSD değiştirmeyi düşünün. Daha fazla bilgi için şu blog gönderisine bakın: [Ultra SSD Ile görev açısından kritik performans](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/).

   > [!NOTE]
   > Portalda bir SQL Server VM sağladığınızda, depolama yapılandırmanızı Düzenle seçeneğiniz vardır. Azure, yapılandırmanıza bağlı olarak bir veya daha fazla disk yapılandırır. Birden çok disk, dizme ile tek bir depolama havuzunda birleştirilir. Hem veri hem de günlük dosyaları, bu yapılandırmada birlikte bulunur. Daha fazla bilgi için bkz. [SQL Server VM 'ler Için depolama yapılandırması](storage-configuration.md).

* **Disk şeridi**: daha fazla verimlilik için, ek veri diskleri ekleyebilir ve disk şeritleme kullanabilirsiniz. Veri disklerinin sayısını öğrenmek için, günlük dosyanız için gereken ıOPS ve bant genişliği sayısını ve verileriniz ile TempDB dosyalarını analiz etmeniz gerekir. Farklı VM boyutlarının, IOPS sayısı ve desteklenen bant genişliği için farklı sınırlara sahip olduğuna dikkat edin. [VM boyutu](../../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)başına IOPS üzerindeki tablolara bakın. Aşağıdaki yönergeleri kullanın:

  * Windows 8/Windows Server 2012 veya üzeri için, [depolama alanlarını](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergelerle kullanın:

      1. Bölüm hatalı hizalaması nedeniyle performans etkisini önlemek için, veri ambarı iş yükleri için Interleave (Stripe boyutu) 64 KB (65.536 262.144 256 bayt) olarak ayarlayın. Bunun PowerShell ile ayarlanması gerekir.
      2. Sütun sayısını ayarla = fiziksel disk sayısı. 8 ' den fazla disk yapılandırırken PowerShell kullanın (Sunucu Yöneticisi Kullanıcı arabirimi). 

    Örneğin, aşağıdaki PowerShell, ayırma boyutu 64 KB ile yeni bir depolama havuzu ve depolama havuzundaki fiziksel disk miktarına eşit olan sütun sayısını oluşturur:

    ```powershell
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}
    
    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" `
        -PhysicalDisks $PhysicalDisks | New- VirtualDisk -FriendlyName "DataFiles" `
        -Interleave 65536 -NumberOfColumns $PhysicalDisks .Count -ResiliencySettingName simple `
        –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter `
        -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" `
        -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 veya önceki sürümlerde, dinamik diskleri (işletim sistemi şeritli birimler) kullanabilir ve şerit boyutu her zaman 64 KB 'tır. Bu seçenek, Windows 8/Windows Server 2012 itibarıyla kullanım dışıdır. Daha fazla bilgi için bkz. [sanal disk hizmetindeki Destek bildirisi Windows Storage YÖNETIM API 'sine geçiyor](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * [SQL Server yük devretme kümesi örnekleriyle](failover-cluster-instance-storage-spaces-direct-manually-configure.md) [depolama alanları doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) kullanıyorsanız, tek bir havuz yapılandırmanız gerekir. Bu tek havuzda farklı birimler oluşturulabilse de, hepsi aynı önbelleğe alma ilkesi gibi aynı özellikleri paylaşır.

  * Yük beklentilerinize göre depolama havuzunuzun ilişkili disk sayısını belirleme. Farklı VM boyutlarının farklı sayıda ekli veri diskine izin verdiğini aklınızda bulundurun. Daha fazla bilgi için bkz. [sanal makineler Için boyutlar](../../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Premium SSD 'leri (geliştirme/test senaryoları) kullanmıyorsanız, [sanal makine boyutunuzu](../../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) desteklediği en fazla veri diski sayısını eklemek ve disk şeritleme kullanılması önerilir.

* **Önbelleğe alma ilkesi**: depolama yapılandırmanıza bağlı olarak önbelleğe alma ilkesi için aşağıdaki önerilere göz önünde edin.

  * Veri ve günlük dosyaları için ayrı diskler kullanıyorsanız, veri dosyalarınızı ve TempDB veri dosyalarını barındıran veri disklerinde okuma önbelleğini etkinleştirin. Bu, önemli bir performans avantajı oluşmasına neden olabilir. Günlük dosyasını tutan diskte önbelleğe alma özelliğini etkinleştirmeyin çünkü bu, performansın küçük bir düşüşine neden olur.

  * Tek bir depolama havuzunda disk şeridi kullanıyorsanız, çoğu iş yükü okuma önbelleklemesi avantajına sahip olur. Günlük ve veri dosyaları için ayrı depolama havuzlarınız varsa, yalnızca veri dosyaları için depolama havuzunda okuma önbelleğini etkinleştirin. Bazı ağır yazma iş yükleri için, önbelleğe alma olmadan daha iyi performans elde edilebilir. Bu, yalnızca test aracılığıyla belirlenebilir.

  * Önceki öneriler Premium SSD 'Ler için geçerlidir. Premium SSD kullanmıyorsanız, herhangi bir veri diskinde önbelleğe alma etkinleştirmeyin.

  * Disk önbelleğe almayı yapılandırma yönergeleri için aşağıdaki makalelere bakın. Klasik (ASM) dağıtım modeli için bkz: [set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ve [set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Azure Resource Manager dağıtım modeli için bkz: [set-AzOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) ve [set-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdatadisk).

     > [!WARNING]
     > Herhangi bir veritabanı bozulması olasılığını ortadan kaldırmak için Azure sanal makineler disklerinin önbellek ayarını değiştirirken SQL Server hizmetini durdurun.

* **NTFS ayırma birimi boyutu**: veri diski biçimlendirilirken veri ve günlük dosyaları için BIR 64 KB ayırma birimi boyutu ve tempdb de kullanmanız önerilir. TempDB geçici diske yerleştirilmişse (D:\ sürücü) bu sürücüden yararlanarak elde edilen performans, 64K ayırma birimi boyutu gereksinimini ortadan kaldırarak ortaya çıktı. 

* **Disk yönetimi en iyi uygulamaları**: bir veri diski kaldırılırken veya önbellek türünü değiştirirken, değişiklik sırasında SQL Server hizmetini durdurun. Önbelleğe alma ayarları işletim sistemi diskinde değiştirildiğinde, Azure VM 'yi sonlandırır, önbellek türünü değiştirir ve VM 'yi yeniden başlatır. Bir veri diskinin önbellek ayarları değiştirildiğinde, VM durdurulmaz, ancak veri diski değişiklik sırasında VM 'den ayrılır ve yeniden eklenir.

  > [!WARNING]
  > Bu işlemler sırasında SQL Server hizmeti durdurulamaması veritabanının bozulmasına neden olabilir.


## <a name="io-guidance"></a>G/ç Kılavuzu

* Uygulama ve isteklerinizi paralel hale getirmek yaparken Premium SSD 'Lerin en iyi sonuçları elde edilir. Premium SSD 'ler, GÇ sırası derinliğinin 1 ' den büyük olduğu senaryolar için tasarlanmıştır, bu nedenle tek iş parçacıklı seri istekler için çok az veya hiç performans kazancı (depolama alanı yoğun olsalar bile) görürsünüz. Örneğin, bu, SQBIR gibi performans analizi araçlarının tek iş parçacıklı test sonuçlarını etkileyebilir.

* G/ç yoğunluklu iş yüklerinin performansını artırmaya yardımcı olması için [veritabanı sayfa sıkıştırmasını](https://msdn.microsoft.com/library/cc280449.aspx) kullanmayı göz önünde bulundurun. Ancak, veri sıkıştırması, veritabanı sunucusundaki CPU tüketimini artırabilir.

* İlk dosya ayırma için gereken süreyi azaltmak üzere anlık dosya başlatmayı etkinleştirmeyi düşünün. Anlık dosya başlatmasının avantajlarından yararlanmak için, SQL Server (MSSQLSERVER) hizmet hesabına SE_MANAGE_VOLUME_NAME sahip olur ve **Birim bakımı görevleri** güvenlik ilkesine eklersiniz. Azure için SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (NT Service\MSSQLSERVER) **toplu bakım görevleri** oluştur güvenlik ilkesine eklenmez. Diğer bir deyişle, bir Azure platform görüntüsünde SQL Server anlık dosya başlatma etkin değildir. SQL Server hizmet hesabını **toplu bakım görevleri Için gerçekleştir** güvenlik ilkesine ekledikten sonra, SQL Server hizmetini yeniden başlatın. Bu özelliğin kullanımıyla ilgili güvenlik konuları olabilir. Daha fazla bilgi için bkz. [veritabanı dosyası başlatma](https://msdn.microsoft.com/library/ms175935.aspx).

* **Otomatik büyüme** 'nın yalnızca beklenmeyen büyüme için bir acil durum olduğu kabul ettiğini unutmayın. Verilerinizi ve günlük büyümesini otomatik büyüme ile günde bir güne göre yönetmeyin. Otomatik büyüme kullanılıyorsa, boyut anahtarını kullanarak dosyayı önceden büyütün.

* Performansı olumsuz etkileyebilecek gereksiz ek yükü önlemek için, **oto küçültme** 'nın devre dışı bırakıldığından emin olun.

* Tüm veritabanlarını sistem veritabanları dahil olmak üzere veri disklerine taşıyın. Daha fazla bilgi için bkz. [sistem veritabanlarını taşıma](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server hata günlüğünü ve izleme dosyası dizinlerini veri disklerine taşıyın. Bu, SQL Server örneğinize sağ tıklayıp Özellikler ' i seçerek SQL Server Yapılandırma Yöneticisi yapılabilir. Hata günlüğü ve izleme dosyası ayarları **Başlangıç parametreleri** sekmesinde değiştirilebilir. Döküm dizini **Gelişmiş** sekmesinde belirtilir. Aşağıdaki ekran görüntüsünde hata günlüğü başlangıç parametresinin nerede aranacağı gösterilmektedir.

    ![SQL hata günlüğü ekran görüntüsü](./media/performance-guidelines-best-practices/sql_server_error_log_location.png)

* Varsayılan yedekleme ve veritabanı dosya konumlarını ayarlayın. Bu makaledeki önerileri kullanın ve sunucu özellikleri penceresinde değişiklikleri yapın. Yönergeler için bkz. [veri ve günlük dosyaları Için varsayılan konumları görüntüleme veya değiştirme (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişiklikleri nerede yapacağınız gösterilmektedir.

    ![SQL veri günlüğü ve yedekleme dosyaları](./media/performance-guidelines-best-practices/sql_server_default_data_log_backup_locations.png)
* GÇ ve tüm disk belleği etkinliklerini azaltmak için kilitli sayfaları etkinleştirin. Daha fazla bilgi için bkz. [bellek Içinde sayfaları kilitleme seçeneği (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* SQL Server 2012 çalıştırıyorsanız, Service Pack 1 toplu güncelleştirme 10 ' u yükleyebilirsiniz. Bu güncelleştirme, SQL Server 2012 ' de geçici tablo seçme ekstresini çalıştırdığınızda g/ç 'de düşük performans düzeltmesini içerir. Bilgi için bu [Bilgi Bankası makalesine](https://support.microsoft.com/kb/2958012)bakın.

* Azure 'da/dışına aktarma yaparken herhangi bir veri dosyasını sıkıştırmayı göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özelliğe özgü rehberlik

Bazı dağıtımlar, daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajları elde edebilir. Aşağıdaki listede, daha iyi performans elde etmenize yardımcı olabilecek bazı SQL Server özellikler vurgulanmıştır:

### <a name="back-up-to-azure-storage"></a>Azure depolama 'ya yedekleme
Azure sanal makinelerde çalışan SQL Server için yedeklemeler gerçekleştirirken, [URL 'ye SQL Server yedekleme](https://msdn.microsoft.com/library/dn435916.aspx)kullanabilirsiniz. Bu özellik SQL Server 2012 SP1 CU2 UYGULAMAZSANıZ ile başlayarak kullanılabilir ve ekli veri disklerine yedekleme için önerilir. Azure depolama 'ya yedekleme/geri yükleme yaptığınızda, [URL En Iyi uygulamaları ve sorun giderme ve Azure depolama 'Da depolanan yedeklemelerden geri yükleme SQL Server için](https://msdn.microsoft.com/library/jj919149.aspx)belirtilen önerileri izleyin. Ayrıca, [Azure sanal makinelerinde SQL Server Için Otomatik yedeklemeyi](../../../azure-sql/virtual-machines/windows/automated-backup-sql-2014.md)kullanarak bu yedeklemeleri otomatikleştirebilir.

SQL Server 2012 ' dan önce [Azure 'da SQL Server yedekleme](https://www.microsoft.com/download/details.aspx?id=40740)'yi kullanabilirsiniz. Bu araç, birden fazla yedek Stripe hedefi kullanarak yedekleme aktarım hızını artırmaya yardımcı olabilir.

### <a name="sql-server-data-files-in-azure"></a>Azure 'da veri dosyaları SQL Server

[Azure 'daki SQL Server veri dosyaları](https://msdn.microsoft.com/library/dn385720.aspx)olan bu yeni özellik, SQL Server 2014 ' den başlayarak kullanılabilir. Azure 'daki veri dosyalarıyla SQL Server çalıştırmak, Azure veri disklerini kullanarak karşılaştırılabilir performans özelliklerini gösterir.

### <a name="failover-cluster-instance-and-storage-spaces"></a>Yük devretme kümesi örneği ve depolama alanları

Depolama alanları 'nı kullanıyorsanız, **onay** sayfasındaki kümeye düğüm eklerken, **tüm uygun depolamayı kümeye ekleyin**etiketli onay kutusunun işaretini kaldırın. 

![Uygun depolamanın işaretini kaldır](./media/performance-guidelines-best-practices/uncheck-eligible-cluster-storage.png)

Depolama alanları 'nı kullanıyorsanız ve **tüm uygun depolama alanını kümeye ekle**seçeneğinin işaretini kaldırırsanız, Windows, kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları kümeden kaldırılana ve PowerShell kullanılarak yeniden iliştirene kadar disk Yöneticisi veya Explorer 'da görünmez. Depolama alanları, depolama havuzlarında birden çok diski gruplandırır. Daha fazla bilgi için bkz. [depolama alanları](/windows-server/storage/storage-spaces/overview).

## <a name="next-steps"></a>Sonraki adımlar

Depolama ve performans hakkında daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server Için depolama yapılandırma yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/)

En iyi güvenlik uygulamaları için bkz. [Azure sanal makinelerinde SQL Server Için güvenlik konuları](security-considerations-best-practices.md).

[Azure sanal makinelerine genel bakış ' a SQL Server](sql-server-on-azure-vm-iaas-what-is-overview.md)diğer SQL Server sanal makine makalelerini inceleyin. SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](frequently-asked-questions-faq.md) bölümüne bakın.
