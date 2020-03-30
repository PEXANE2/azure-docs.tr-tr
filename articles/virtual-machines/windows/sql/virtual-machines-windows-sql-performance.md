---
title: Azure'da SQL Server için performans kuralları | Microsoft Dokümanlar
description: Microsoft Azure VM'lerde SQL Server performansını en iyi duruma getirilmesi için yönergeler sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
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
ms.openlocfilehash: 880f1c601cf4132fdec9e5d25b1bf1f2ff175ab7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650546"
---
# <a name="performance-guidelines-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineleri'nde SQL Server için performans yönergeleri

## <a name="overview"></a>Genel Bakış

Bu makalede, Microsoft Azure Virtual Machine'de SQL Server performansını en iyi duruma getirilmesi için kılavuz sağlanmaktadır. Azure Sanal Makineler'de SQL Server'ı çalıştırırken, şirket içi sunucu ortamındaki SQL Server için geçerli olan aynı veritabanı performansı ayarlama seçeneklerini kullanmaya devam etmenizi öneririz. Bununla birlikte genel buluttaki bir ilişkisel veritabanının performansı, sanal makinenin boyutu ve veri disklerinin yapılandırması gibi birçok faktöre bağlıdır.

[Azure portalında sağlanan SQL Server görüntüleri](quickstart-sql-vm-create-portal.md) genel depolama yapılandırması en iyi uygulamalarını izleyin (depolama nın nasıl yapılandırıldığı hakkında daha fazla bilgi için SQL Server [VM'ler için Depolama yapılandırmasına](virtual-machines-windows-sql-server-storage-configuration.md)bakın). Hüküm verdikten sonra, bu makalede tartışılan diğer optimizasyonları uygulamayı düşünün. Seçimlerinizi iş yükünüze dayandırın ve test yoluyla doğrulayın.

> [!TIP]
> Genellikle maliyetler için optimizasyon ve performans için optimize arasında bir denge vardır. Bu makale, Azure VM'lerde SQL Server için *en iyi* performansı elde etmek üzerine odaklanmıştır. İş yükünüz daha az zahmetliyse, aşağıda listelenen her optimizasyonu gerektirmeyebilir. Bu önerileri değerlendirirken performans ihtiyaçlarınızı, maliyetlerinizi ve iş yükü desenlerinizi göz önünde bulundurun.

## <a name="quick-check-list"></a>Hızlı kontrol listesi

SQL Server'ın Azure Sanal Makinelerde en iyi performansı için hızlı bir kontrol listesi aşağıda veda edin:

| Alan | İyileştirmeler |
| --- | --- |
| [VM boyutu](#vm-size-guidance) | - [VM](../../ev3-esv3-series.md) boyutlarını E4S_v3 veya daha yüksek veya [DS12_v2](../../dv2-dsv2-series-memory.md) veya daha yüksek gibi 4 veya daha fazla vCPU ile kullanın.<br/><br/> - [Es, Eas, Ds ve Das Serisi,](../../sizes-general.md) OLTP iş yükü performansı için gerekli olan vCPU oranına optimum bellek sunar. <br/><br/> - [M Serisi,](../../m-series.md) kritik görev performansı için gereken en yüksek bellek vCPU oranını sunar ve veri ambarı iş yükleri için idealdir. <br/><br/> - [Uygulama performans gereksinimleri denetim listesini](../premium-storage-performance.md#application-performance-requirements-checklist) izleyerek hedef iş yükünün [IOPS,](../premium-storage-performance.md#iops) [iş ve](../premium-storage-performance.md#throughput) gecikme [gereksinimlerini](../premium-storage-performance.md#latency) en yoğun zamanlarda toplayın ve ardından iş yükünüzün performans gereksinimlerine ölçeklendirilebilen [VM Boyutunu](../sizes-general.md) seçin.|
| [Depolama](#storage-guidance) | - TPC-E ve TPC_C kıyaslamalarla Azure VM'lerde SQL Server performansının ayrıntılı test edilmesi için, [Optimize OLTP performansına](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794)bakın. <br/><br/> - En iyi fiyat/performans avantajları için [premium SSD'leri](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794) kullanın. Veri dosyaları için [ReadOnly önbelleğini](../premium-storage-performance.md#disk-caching) ve günlük dosyası için önbellek eklemeyi yapılandırın. <br/><br/> - İş yükü tarafından 1 ms'den az depolama gecikmesi gerekiyorsa [Ultra Diskler](../disks-types.md#ultra-disk) kullanın. <br/><br/> - Disk türünü seçmeden önce [uygulamayı izleyerek](../premium-storage-performance.md#application-performance-requirements-checklist) SQL Server verileri, günlük ve Geçici DB dosyaları için depolama gecikmesi gereksinimlerini toplayın. <1ms depolama gecikmesi gerekiyorsa, Ultra Diskler kullanın, aksi takdirde premium SSD kullanın. Veri dosyaları için değil de yalnızca günlük dosyası için düşük gecikmeler gerekiyorsa, Ultra Disk'i yalnızca günlük Dosyası için gerekli IOPS ve iş çıkış düzeylerinde [verin.](../disks-enable-ultra-ssd.md) <br/><br/> -  [Premium dosya paylaşımları,](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share.md) sql server failover cluster Instance için paylaşılan depolama alanı olarak önerilir. Premium dosya paylaşımları önbelleğe alınmayı desteklemez ve premium SSD disklere kıyasla sınırlı performans sunar. Bağımsız SQL örnekleri için premium dosya paylaşımları üzerinden premium SSD yönetilen diskleri seçin; ancak, bakım kolaylığı ve esnek ölçeklenebilirlik için başarısız küme örneği paylaşılan depolama için premium dosya paylaşımlarından yararlanın. <br/><br/> - Standart depolama yalnızca geliştirme ve test amacıyla veya yedekleme dosyaları için önerilir ve üretim iş yükleri için kullanılmamalıdır. <br/><br/> - [Depolama hesabını](../../../storage/common/storage-create-storage-account.md) ve SQL Server VM'yi aynı bölgede saklayın.<br/><br/> - Depolama hesabında Azure [coğrafi yedekli depolamayı](../../../storage/common/storage-redundancy.md) (coğrafi çoğaltma) devre dışı açın.  |
| [Disk](#disks-guidance) | - En az 2 [premium SSD disk](../disks-types.md#premium-ssd) kullanın (günlük dosyası için 1 ve veri dosyaları için 1). <br/><br/> - 1 ms'<gecikme gerektiren iş yükleri için M serisi için yazma hızlandırıcısını etkinleştirin ve Es ve DS serisi için Ultra SSD diskler kullanmayı düşünün. <br/><br/> - Yalnızca veri dosyalarını barındıran disk(ler)te [önbelleğe alma](../premium-storage-performance.md#disk-caching) nızı etkinleştirin.<br/><br/> - [SQL Server verileri, günlük ve TempDB dosyaları için depolamayı yapılandırırken](virtual-machines-windows-sql-server-storage-configuration.md) iş yükünüzün gerektirdiğinden %20 daha fazla premium IOPS/iş gücü kapasitesi ekleyin <br/><br/> - Veritabanı depolama veya günlüğe kaydetme için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br/><br/> - Günlük dosyasını barındıran disk(ler)te önbelleğe alma nızı etkinleştirmeyin.  **Önemli**: Azure VM diskinin önbellek ayarlarını değiştirirken SQL Server hizmetini durdurun.<br/><br/> - Daha fazla depolama alanı verisi elde etmek için birden fazla Azure veri diskini şeritle.<br/><br/> - Belgelenmiş ayırma boyutlarıyla biçimlendirin. <br/><br/> - Kritik görev deki SQL Server `D:\` iş yükleri için tempdb'yi yerel SSD sürücüsüne yerleştirin (doğru VM boyutunu seçtikten sonra). Azure portalından veya Azure hızlı başlatma şablonlarından VM oluşturur ve [Yerel Disk'e Geçici DB'yi yerseniz,](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583) başka bir eyleme gerek yoktur; diğer tüm durumlar için yeniden başlatıldıktan sonra hataları önlemek [için TempDB depolamak için SSD'leri kullanma](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/) için blogdaki adımları izleyin. Yerel sürücünün kapasitesi Temp DB boyutunuz için yeterli değilse, temp DB'yi [yalnızca okunabilir önbelleğe alınmış](../premium-storage-performance.md#disk-caching)birinci sınıf SSD disklerde [bulunan](../premium-storage-performance.md) bir depolama havuzuna yerleştirin. |
| [G/Ç](#io-guidance) |- Veritabanı sayfası sıkıştırmasını etkinleştirin.<br/><br/> - Veri dosyaları için anında dosya başlatmayı etkinleştirin.<br/><br/> - Veritabanının otomatik büyümesini sınırlandırın.<br/><br/> - Veritabanının otomatik küçültmesini devre dışı katın.<br/><br/> - Sistem veritabanları da dahil olmak üzere tüm veritabanlarını veri disklerine taşıyın.<br/><br/> - SQL Server hata günlüğü ve izleme dosya dizinlerini veri disklerine taşıyın.<br/><br/> - Varsayılan yedekleme ve veritabanı dosya konumlarını yapılandırın.<br/><br/> - [Kilitli sayfaları bellekte etkinleştirin.](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows?view=sql-server-2017)<br/><br/> - SQL Server performans düzeltmeleri uygulayın. |
| [Özelliğe özel](#feature-specific-guidance) | - Doğrudan blob depolamaya geri dön.<br/><br/>- 12 TB'den büyük veritabanları için [dosya anlık yedeklemelerini](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) kullanın. <br/><br/>- Birden fazla Temp DB dosyası, çekirdek başına 1 dosya, en fazla 8 dosya kullanın.<br/><br/>- İşletim Sistemi için maksimum sunucu belleği %90 veya 50 GB'a kadar ayarlayın. <br/><br/>- Yumuşak NUMA'yı etkinleştirin. |

Bu optimizasyonların *nasıl* ve *neden* yapılacılailgili daha fazla bilgi için, lütfen aşağıdaki bölümlerde verilen ayrıntıları ve yönetmelikleri inceleyin.

## <a name="vm-size-guidance"></a>VM boyutu kılavuzu

Yoğun zamanlarda iş yükünün işlemci, bellek ve depolama iş gereksinimi gereksinimlerini toplayarak başlayın. \LogicalDisk\Disk Okuma/Sn ve \LogicalDisk\Disk Yazma/Ss performans sayaçları IOPS gereksinimlerini toplamak ve yazmak için kullanılabilir ve \LogicalDisk\Disk Bayt/Ss sayacı Veri, Log ve Geçici DB dosyaları için [depolama iş lenecek bilgi gereksinimlerini](../premium-storage-performance.md#disk-caching) toplamak için kullanılabilir. IOPS ve en üst düzeydeki iş geliştirme gereksinimleri tanımlandıktan sonra vm boyutlarını değerlendirin bu kapasiteyi sunar. Örneğin, iş yükünüzün 20 K okuma IOPS ve 10K en yüksek düzeyde IOPS yazmasını gerektiriyorsa, 2 P30 diskli E16s_v3 (32 K önbelleğe alınmış ve 25600'e kadar önbelleğe alınmış IOPS ile) veya M16_s (20 K'ye kadar önbelleğe alınmış ve 10K'ya kadar çatlamamış IOPS ile) seçebilirsiniz. VM'lerin IOPS ve iş bölümü için farklı ölçek sınırları olduğundan, iş yükünün hem iş yükünün hem iş hem de IOPS gereksinimlerini anladığınızdan emin olun.<br/><br/>[DSv_3](../../dv3-dsv3-series.md) ve [Es_v3 serisi](../../ev3-esv3-series.md) Intel Haswell veya Broadwell işlemciler ile genel amaçlı donanım barındırılan. [M serisi,](../../m-series.md) en büyük SQL Server iş yükleri için en yüksek vCPU sayısını ve belleği sunar ve Skylake işlemci ailesiyle bellek optimize edilmiş donanımda barındırılan donanıma sahip olur. Bu VM serisi, ana bilgisayar düzeyinde okuma önbelleğiyle en iyi performans için önerilen premium depolamayı destekler. Hem Es_v3 hem de M serisi, daha düşük işlem ve yüksek depolama kapasitesi talepleri yle iş yüklerine para tasarrufu sağlayan [kısıtlı çekirdek boyutlarında](../../windows/constrained-vcpu.md)da mevcuttur. 

## <a name="storage-guidance"></a>Depolama yönergeleri

TPC-E ve TPC_C kıyaslamalarla Azure VM'lerde SQL Server performansının ayrıntılı test edilmesi için, [OPTIMIZE OLTP performansını](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794)bloga bakın. 

Tüm üretim iş yükleri için premium SSD'li azure blob önbelleği önerilir. 

> [!WARNING]
> Standart HDD'ler ve SSD'ler değişen gecikmelere ve bant genişliğine sahiptir ve yalnızca geliştirme/test iş yükleri için önerilir. Üretim iş yükleri premium SSD'ler kullanmalıdır.

Ayrıca, aktarım gecikmelerini azaltmak için Azure depolama hesabınızı SQL Server sanal makineleriniz ile aynı veri merkezinde oluşturmanızı öneririz. Bir depolama hesabı oluştururken, coğrafi çoğaltmayı birden çok diskte tutarlı yazma sırası olarak devre dışı süreceği garanti edilmez. Bunun yerine, iki Azure veri merkezi arasında bir SQL Server olağanüstü durum kurtarma teknolojisi yapılandırmayı düşünün. Daha fazla bilgi için Azure [Sanal Makinelerde SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma'ya](virtual-machines-windows-sql-high-availability-dr.md)bakın.

## <a name="disks-guidance"></a>Diskler kılavuzu

Azure VM'de üç ana disk türü vardır:

* **İşletim sistemi :** Bir Azure Sanal Makinesi oluşturduğunuzda, platform işletim sistemi diskiniz için VM'ye en az bir disk **(C** sürücüsü olarak etiketlenmiş) bağlar. Bu disk, depolama alanında sayfa blob olarak depolanan bir VHD'dir.
* **Geçici disk**: Azure sanal makineleri geçici disk **(D**: sürücü olarak etiketlenmiş) adı verilen başka bir disk içerir. Bu, düğümüzerindeki, karalama alanı için kullanılabilecek bir disktir.
* **Veri diskleri**: Sanal makinenize veri diski olarak ek diskler de ekleyebilirsiniz ve bunlar sayfa blobs olarak depolama da depolanır.

Aşağıdaki bölümlerde bu farklı diskleri kullanmak için öneriler açıklayınız.

### <a name="operating-system-disk"></a>İşletim sistemi diski

İşletim sistemi diski, işletim sisteminin çalışan bir sürümü olarak önyükleme ve monte edebilirsiniz ve **C** sürücü olarak etiketlenmiş bir VHD olduğunu.

İşletim sistemi diskinde varsayılan önbelleğe alma ilkesi **Oku/Yaz'** dır. Performansa duyarlı uygulamalar için işletim sistemi diski yerine veri diskleri kullanmanızı öneririz. Aşağıdaki Veri Diskleri bölümüne bakın.

### <a name="temporary-disk"></a>Geçici disk

**D** sürücüsü olarak etiketlenen geçici depolama sürücüsü Azure blob depolamasına kalıcı olarak kullanılmaz. D: sürücü üzerinde kullanıcı veritabanı dosyaları veya **D**kullanıcı hareket günlüğü dosyaları saklamayın.

Görev açısından kritik SQL Server `D:\` iş yükleri için TempDB'yi yerel SSD sürücüsüne yerleştirin (doğru VM boyutunu seçtikten sonra). Azure portalından veya Azure hızlı başlatma şablonlarından VM oluşturur ve [Yerel Disk'e Geçici DB yerseniz,](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583)başka bir eyleme gerek yoktur; diğer tüm durumlar için yeniden başlatıldıktan sonra hataları önlemek [için TempDB depolamak için SSD'leri kullanma](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/) için blogdaki adımları izleyin. Yerel sürücünün kapasitesi Temp DB boyutunuz için yeterli değilse, temp DB'yi [yalnızca okunabilir önbelleğe alınmış](../premium-storage-performance.md#disk-caching)birinci sınıf SSD disklerde [bulunan](../premium-storage-performance.md) bir depolama havuzuna yerleştirin.

Premium SSD'leri destekleyen VM'ler için, TempDB'yi okuma önbelleğe alma özelliğiyle premium SSD'leri destekleyen bir diskte de depolayabilirsiniz.


### <a name="data-disks"></a>Veri diskleri

* **Veri ve günlük dosyaları için premium SSD diskleri kullanın**: Disk şeridi kullanmıyorsanız, bir diskin günlük dosyasını içerdiği ve diğerinin verileri içerdiği iki premium SSD disk kullanın. Her premium SSD, makalede betimlenen boyutuna bağlı olarak bir dizi IOPS ve bant genişliği (MB/s) sağlar, [bir disk türü seçin.](../disks-types.md) Depolama Alanları gibi bir disk şeritleme tekniği kullanıyorsanız, biri günlük dosyası(lar) diğeri veri dosyaları için olmak üzere iki havuza sahip olarak en iyi performansı elde elabilirsiniz. Ancak, SQL Server failover küme örnekleri (FCI) kullanmayı planlıyorsanız, bir havuz yapılandırmanız veya bunun yerine [premium dosya paylaşımlarını](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share.md) kullanmanız gerekir.

   > [!TIP]
   > - Çeşitli disk ve iş yükü yapılandırmalarında test sonuçları için aşağıdaki blog gönderilerine bakın: [Azure VM'deki SQL Server için Depolama Yapılandırma Yönergeleri.](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/)
   > - ~ 50.000 IOPS'ye ihtiyaç duyan SQL Sunucuları için kritik görev performansı için, 10-P30 diskleri Ultra SSD ile değiştirmeyi düşünün. Daha fazla bilgi için aşağıdaki blog gönderilerine bakın: [Ultra SSD ile kritik görev performansı.](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/)

   > [!NOTE]
   > Portalda bir SQL Server VM diğinizde, depolama yapılandırmanızı düzenleme seçeneğiniz de var. Yapılandırmanıza bağlı olarak, Azure bir veya daha fazla diski yapılandırır. Birden çok disk, şeritleme ile tek bir depolama havuzunda birleştirilir. Hem veri hem de günlük dosyaları bu yapılandırmada birlikte bulunur. Daha fazla bilgi [için SQL Server VM'ler için Depolama yapılandırmasına](virtual-machines-windows-sql-server-storage-configuration.md)bakın.

* **Disk Şeritleme**: Daha fazla iş için ek veri diskleri ekleyebilir ve Disk Şeridi'ni kullanabilirsiniz. Veri disklerinin sayısını belirlemek için, günlük dosyanız(lar) ve verileriniz ve TempDB dosyanız(lar) için gereken IOPS ve bant genişliği sayısını çözümlemeniz gerekir. Farklı VM boyutları iOP sayısı ve desteklenen bant genişliği farklı sınırları olduğuna dikkat edin, [VM boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)başına IOPS tabloları bakın. Aşağıdaki yönergeleri kullanın:

  * Windows 8/Windows Server 2012 veya sonraki kullanımları için Depolama [Alanları'nı](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergelerle kullanın:

      1. Bölme yanlış hizalaması nedeniyle performans etkisini önlemek için ara izni (şerit boyutu) OLTP iş yükleri için 64 KB (65.536 bayt) ve veri depolama iş yükleri için 256 KB (262.144 bayt) olarak ayarlayın. Bu PowerShell ile ayarlanmalıdır.
      2. Sütun sayısını ayarlama = fiziksel disk sayısı. 8'den fazla disk iyapılandırırken PowerShell'i kullanın (Server Manager UI değil). 

    Örneğin, aşağıdaki PowerShell, 64 KB'ye ve sütun sayısını 2'ye kadar olan ara izin boyutuna sahip yeni bir depolama havuzu oluşturur:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 veya daha önce için dinamik diskler (işletim sistemi çizgili birimler) kullanabilirsiniz ve şerit boyutu her zaman 64 KB'dir. Bu seçenek, Windows 8/Windows Server 2012 itibariyle amortismana hazırdır. Bilgi için, Sanal Disk Hizmeti'ndeki destek bildirimine bakın [Windows Depolama Yönetimi API'sine geçiş oluyor.](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx)

  * [SQL Server Failover Cluster Örnekleri](virtual-machines-windows-portal-sql-create-failover-cluster.md)ile Depolama Alanları Direct [(S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) kullanıyorsanız, tek bir havuz yapılandırmanız gerekir. Bu tek havuzda farklı birimler oluşturulabilir, ancak bunların tümü aynı önbelleğe alma ilkesi gibi aynı özellikleri paylaşır.

  * Yük beklentilerinize göre depolama havuzunuzla ilişkili disk sayısını belirleyin. Farklı VM boyutlarının farklı sayıda bağlı veri diski sağladığını unutmayın. Daha fazla bilgi [için Sanal Makineler için Boyutlar'a](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

  * Premium SSD(dev/test senaryoları) kullanmıyorsanız, öneri, [VM boyutunuz](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tarafından desteklenen maksimum veri diski sayısını eklemek ve Disk Şeridi'ni kullanmaktır.

* **Önbelleğe alma ilkesi**: Depolama yapılandırmanıza bağlı olarak önbelleğe alma ilkesi için aşağıdaki önerilere dikkat edin.

  * Veri ve günlük dosyaları için ayrı diskler kullanıyorsanız, veri dosyalarınızı ve TempDB veri dosyalarını barındıran veri disklerinde okuma önbelleğe alma'yı etkinleştirin. Bu önemli bir performans avantajı neden olabilir. Günlük dosyasını tutan diskte önbelleğe alınmayı etkinleştirme, çünkü bu durum performansta küçük bir düşüşe neden olur.

  * Tek bir depolama havuzunda disk şeritleme kullanıyorsanız, çoğu iş yükü okuma önbelleğe alma dan yararlanacaktır. Günlük ve veri dosyaları için ayrı depolama havuzlarınız varsa, yalnızca veri dosyaları için depolama havuzunda okuma önbelleğe alma'yı etkinleştirin. Bazı ağır yazma iş yüklerinde önbelleğe alma olmadan daha iyi performans elde edilebilir. Bu sadece test yoluyla belirlenebilir.

  * Önceki öneriler premium SSD'ler için geçerlidir. Premium SSD kullanmıyorsanız, veri disklerinde önbelleğe alma olanağı sağlamayın.

  * Disk önbelleğe alma yapılandırma yönergeleri için aşağıdaki makalelere bakın. Klasik (ASM) dağıtım modeli için bkz: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ve [Set-AzureDataDisk.](https://msdn.microsoft.com/library/azure/jj152851.aspx) Azure Kaynak Yöneticisi dağıtım modeli için bkz: [Set-AzOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) ve [Set-AzVMDataDisk.](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdatadisk)

     > [!WARNING]
     > Herhangi bir veritabanı bozulması olasılığını önlemek için Azure VM disklerinin önbellek ayarını değiştirirken SQL Server hizmetini durdurun.

* **NTFS ayırma birimi boyutu**: Veri diskini biçimlendirirken, veri ve günlük dosyalarının yanı sıra TempDB için 64-KB ayırma birimi boyutu kullanmanız önerilir. Geçici diske TempDB yerleştirilirse (D:\ sürücü) bu sürücüden yararlanarak elde edilen performans, 64K ayırma birimi boyutuna duyulan ihtiyaçtan daha ağır basar. 

* **Disk yönetimi en iyi uygulamaları**: Bir veri diskini kaldırırken veya önbellek türünü değiştirirken, değişiklik sırasında SQL Server hizmetini durdurun. Os diskinde önbelleğe alma ayarları değiştirildiğinde, Azure VM'yi durdurur, önbellek türünü değiştirir ve VM'yi yeniden başlatır. Bir veri diskinin önbellek ayarları değiştirildiğinde, VM durdurulamaz, ancak veri diski değişiklik sırasında VM'den ayrılır ve sonra yeniden eklenir.

  > [!WARNING]
  > Bu işlemler sırasında SQL Server hizmetinin durdurulamaması veritabanı bozulmasına neden olabilir.


## <a name="io-guidance"></a>G/Ç kılavuzu

* Uygulama ve isteklerinizi paralellediğinizde premium SSD'ler ile en iyi sonuçlar elde edilir. Premium SSD'ler, IO sıra derinliğinin 1'den büyük olduğu senaryolar için tasarlanmıştır, böylece tek iş parçacığı seri istekleri için çok az performans artışı görürsünüz veya hiç performans artışı görmezsiniz (depolama yoğun olsalar bile). Örneğin, bu, SQLIO gibi performans çözümleme araçlarının tek iş parçacığı test sonuçlarını etkileyebilir.

* G/Ç yoğun iş yüklerinin performansını artırmaya yardımcı olabileceğinden [veritabanı sayfası sıkıştırmayı](https://msdn.microsoft.com/library/cc280449.aspx) kullanmayı düşünün. Ancak, veri sıkıştırma veritabanı sunucusunda CPU tüketimini artırabilir.

* İlk dosya ayırma için gereken süreyi azaltmak için anında dosya başlatmayı etkinleştirmeyi düşünün. Anında dosya başlatma dan yararlanmak için, SE_MANAGE_VOLUME_NAME ile SQL Server (MSSQLSERVER) hizmet hesabını verir ve **Ses Düzeyi Bakım Görevleri gerçekleştir** güvenlik ilkesine eklersiniz. Azure için bir SQL Server platformu görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (NT Service\MSSQLSERVER) **Ses Düzeyi Bakım Görevleri Gerçekleştir** güvenlik ilkesine eklenmez. Başka bir deyişle, SQL Server Azure platform görüntüsünde anında dosya başlatma etkinleştirilir. SQL Server hizmet hesabını **Ses Düzeyi Bakım Görevleri güvenlik** ilkesine ekledikten sonra, SQL Server hizmetini yeniden başlatın. Bu özelliği kullanmak için güvenlik hususları olabilir. Daha fazla bilgi için [Veritabanı Dosyası Başlatma'ya](https://msdn.microsoft.com/library/ms175935.aspx)bakın.

* **autogrow** sadece beklenmedik büyüme için bir acil durum olarak kabul edilir. Verilerinizi yönetmeyin ve otomatik büyüme ile günlük bazda büyüme nizi kaydedin. Otomatik büyüme kullanılırsa, Boyut anahtarını kullanarak dosyayı önceden büyütün.

* Performansı olumsuz etkileyebilecek gereksiz ek yükü önlemek için **otomatik küçültmenin** devre dışı bırakıldığından emin olun.

* Sistem veritabanları da dahil olmak üzere tüm veritabanlarını veri disklerine taşıyın. Daha fazla bilgi için, [Sistem Veritabanlarını Taşı'ya](https://msdn.microsoft.com/library/ms345408.aspx)bakın.

* SQL Server hata günlüğünü ve izleme dosya dizinlerini veri disklerine taşıyın. Bu, SQL Server örneğinize sağ tıklayarak ve özellikleri seçerek SQL Server Configuration Manager'da yapılabilir. **Başlangıç Parametreleri** sekmesinde hata günlüğü ve izleme dosyası ayarları değiştirilebilir. Dump Directory **Gelişmiş** sekmesinde belirtilir. Aşağıdaki ekran görüntüsü, hata günlüğü başlangıç parametresinin nerede arayacağını gösterir.

    ![SQL ErrorLog Ekran Görüntüsü](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Varsayılan yedekleme ve veritabanı dosya konumlarını kurulum. Bu makaledeki önerileri kullanın ve Sunucu özellikleri penceresinde ki değişiklikleri yapın. Talimatlar için bkz. Veri [ve Günlük Dosyaları (SQL Server Management Studio) için Varsayılan Konumları Görüntüle veya Değiştir.](https://msdn.microsoft.com/library/dd206993.aspx) Aşağıdaki ekran görüntüsü, bu değişikliklerin nerede yapılacağı gösteriş yapar.

    ![SQL Veri Günlüğü ve Yedekleme dosyaları](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* IO'yi ve arama etkinliklerini azaltmak için kilitli sayfaları etkinleştirin. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms190730.aspx)

* SQL Server 2012'yi çalıştırıyorsanız, Service Pack 1 Kümülatif Güncelleştirme 10'u yükleyin. Bu güncelleştirme, SELECT'i SQL Server 2012'de geçici tablo deyimine uyguladığınızda G/Ç'deki düşük performans düzeltmesini içerir. Bilgi için bu [bilgi bankası makalesine](https://support.microsoft.com/kb/2958012)bakın.

* Azure'a geçiş yaparken/yaparken veri dosyalarını sıkıştırmayı düşünün.

## <a name="feature-specific-guidance"></a>Özelliğe özel kılavuz

Bazı dağıtımlar, daha gelişmiş yapılandırma tekniklerini kullanarak ek performans avantajları elde edebilir. Aşağıdaki liste, daha iyi performans elde etmek için yardımcı olabilecek bazı SQL Server özelliklerini vurgulamaktadır:

### <a name="back-up-to-azure-storage"></a>Azure Depolama'ya yedekleme
Azure sanal makinelerinde çalışan SQL Server için yedekleme yaparken, [SQL Server Backup to URL'yi](https://msdn.microsoft.com/library/dn435916.aspx)kullanabilirsiniz. Bu özellik SQL Server 2012 SP1 CU2 ile başlayarak kullanılabilir ve ekli veri disklerine yedekleme için önerilir. Azure depolamaalanını yedeklediğinizde/geri yüklediğinizde, SQL Server Backup'da sağlanan önerileri, [Azure Depolama'da Depolanan Yedeklemelerden URL En İyi Uygulamalar a ve Sorun Giderme ve Geri Yükleme'ye uygulayın.](https://msdn.microsoft.com/library/jj919149.aspx) Azure [Sanal Makinelerde SQL Server için Otomatik Yedekleme'yi](virtual-machines-windows-sql-automated-backup.md)kullanarak bu yedeklemeleri otomatikleştirebilirsiniz.

SQL Server 2012'den önce [SQL Server Backup to Azure Tool'u](https://www.microsoft.com/download/details.aspx?id=40740)kullanabilirsiniz. Bu araç, birden çok yedek şerit hedefleri kullanarak yedekleme iş buzunu artırmaya yardımcı olabilir.

### <a name="sql-server-data-files-in-azure"></a>Azure'da SQL Server Data dosyaları

Bu yeni özellik, [Azure'daki SQL Server Data Files](https://msdn.microsoft.com/library/dn385720.aspx), SQL Server 2014'ten başlayarak kullanılabilir. SQL Server'ı Azure'da veri dosyalarıyla çalıştırmak, Azure veri disklerini kullanmak gibi karşılaştırılabilir performans özellikleri gösterir.

### <a name="failover-cluster-instance-and-storage-spaces"></a>Failover küme örneği ve Depolama Alanları

Depolama Alanları kullanıyorsanız, **Onay** sayfasında kümeye düğüm eklerken, kümeye uygun **tüm depolama alanını ekle**etiketli onay kutusunu temizleyin. 

![Uygun depolama alanının denetimini kaldırın](media/virtual-machines-windows-sql-performance/uncheck-eligible-cluster-storage.png)

Depolama Alanları kullanıyorsanız ve **kümeye tüm uygun depolama alanını ekle'nin**işaretlerini geri almayacaksanız, Windows kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları kümeden kaldırılıp PowerShell kullanılarak yeniden eklenene kadar Disk Yöneticisi veya Gezgin'de görünmezler. Depolama Alanları depolama havuzlarına birden çok disk gruplanır. Daha fazla bilgi için [Depolama Alanları'na](/windows-server/storage/storage-spaces/overview)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Depolama ve performans hakkında daha fazla bilgi için [Azure VM'de SQL Server için Depolama Yapılandırma Yönergeleri'ne](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/) bakın

Güvenlik en iyi uygulamaları için Azure [Sanal Makinelerde SQL Server için Güvenlik Hususları'na](virtual-machines-windows-sql-security.md)bakın.

[Azure Sanal Makinelere Genel Bakış'ta SQL Server'daki](virtual-machines-windows-sql-server-iaas-overview.md)diğer SQL Server Virtual Machine makalelerini inceleyin. SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.
