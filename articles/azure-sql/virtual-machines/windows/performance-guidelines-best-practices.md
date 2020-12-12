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
ms.subservice: performance
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/09/2020
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 2b4e8d980ee2b5c69687fc7ad8975e26fe38071a
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97360127"
---
# <a name="performance-guidelines-for-sql-server-on-azure-virtual-machines"></a>Azure Sanal Makineleri'nde SQL Server için performans yönergeleri
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede Microsoft Azure Sanal Makineler SQL Server performansını iyileştirmeye yönelik rehberlik sunulmaktadır.

## <a name="overview"></a>Genel Bakış

Azure sanal makinelerinde SQL Server çalıştırılırken, şirket içi sunucu ortamlarında SQL Server için geçerli olan veritabanı performansı ayarlama seçeneklerini kullanmaya devam etmenizi öneririz. Bununla birlikte genel buluttaki bir ilişkisel veritabanının performansı, sanal makinenin boyutu ve veri disklerinin yapılandırması gibi birçok faktöre bağlıdır.

[Azure Portal sağlanan görüntüleri SQL Server](sql-vm-create-portal-quickstart.md) genel depolama [yapılandırması en iyi yöntemlerini](storage-configuration.md)izleyin. Sağlamaktan sonra, bu makalede ele alınan diğer iyileştirmeleri uygulamayı düşünün. Seçimlerinizi iş yükünüze dayandırın ve test aracılığıyla doğrulayın.

> [!TIP]
> Genellikle maliyetleri iyileştirmek ve performansı iyileştirmek arasında bir denge vardır. Bu makale, Azure sanal makinelerinde SQL Server için *en iyi* performansı almaya odaklanır. İş yükünüz daha az güç alıyorsa, aşağıda listelenen her iyileştirme gerektirmeyebilir. Bu önerileri değerlendirdiğiniz için performans ihtiyaçlarınızı, maliyetlerinizi ve iş yükü desenlerini göz önünde bulundurun.

## <a name="quick-checklist"></a>Hızlı denetim listesi

Aşağıda Azure sanal makinelerinde SQL Server en iyi performansı için hızlı bir denetim listesi verilmiştir:

| Alan | İyileştirmeler |
| --- | --- |
| [VM boyutu](#vm-size-guidance) | - [Standard_M8-4ms](/azure/virtual-machines/m-series), [E4ds_v4](../../../virtual-machines/edv4-edsv4-series.md#edv4-series)veya [DS12_v2](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) ya da daha yüksek gibi 4 veya daha fazla vCPU ile VM boyutlarını kullanın. <br/><br/> -SQL Server iş yüklerinin en iyi performansı için [bellek için iyileştirilmiş](../../../virtual-machines/sizes-memory.md) sanal makine boyutlarını kullanın. <br/><br/> - [DSv2 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md), [Edsv4](../../../virtual-machines/edv4-edsv4-series.md) serisi, [d-](/azure/virtual-machines/m-series)ve [Mv2](../../../virtual-machines/mv2-series.md) serisi, OLTP Iş yükleri için gereken en iyi bellekten sanal çekirdek oranını sunmaktadır. Her iki e serisi sanal makine, görev açısından kritik iş yükleri için gereken en yüksek bellek-vCore oranını sunar ve ayrıca veri ambarı iş yükleri için de idealdir. <br/><br/> -Görev açısından kritik ve veri ambarı iş yükleri için daha yüksek bellekten vCore oranı gerekebilir. <br/><br/> -SQL Server ayarları ve depolama seçenekleri en iyi SQL Server performans için yapılandırıldığından Azure sanal makine marketi görüntülerinden yararlanın. <br/><br/> -Hedef iş yükünün performans özelliklerini toplayın ve işletmeniz için uygun VM boyutunu tespit etmek üzere bunları kullanın.|
| [Depolama](#storage-guidance) | -TPC-E ve TPC_C kıyaslamalar ile Azure sanal makinelerinde SQL Server performansının ayrıntılı testi için, [OLTP performansını iyileştirme](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794)başlıklı bloga başvurun. <br/><br/> -En iyi fiyat/performans avantajları için [Premium SSD 'ler](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794) kullanın. Veri dosyaları için [salt okuma önbelleğini](../../../virtual-machines/premium-storage-performance.md#disk-caching) yapılandırma ve günlük dosyası için önbellek yok. <br/><br/> -İş yükü için 1 MS 'den az depolama gecikmeleri gerekliyse [Ultra diskleri](../../../virtual-machines/disks-types.md#ultra-disk) kullanın. Daha fazla bilgi edinmek için bkz. [Ultra diske geçiş](storage-migrate-to-ultradisk.md) . <br/><br/> -Disk türünü seçmeden önce [uygulamayı izleyerek](../../../virtual-machines/premium-storage-performance.md#application-performance-requirements-checklist) SQL Server verileri, günlüğü ve Temp DB dosyaları için depolama gecikmesi gereksinimlerini toplayın. < 1 MS depolama gecikmeleri gerekliyse, Ultra diskler ' i kullanın, aksi takdirde Premium SSD kullanın. Düşük gecikme süreleri yalnızca günlük dosyası için gerekliyse ve veri dosyaları için gerekli değilse, yalnızca günlük dosyası için gerekli ıOPS ve üretilen iş düzeylerinde [Ultra disk sağlayın](../../../virtual-machines/disks-enable-ultra-ssd.md) . <br/><br/>  -Standart depolama yalnızca geliştirme ve test amaçları için ya da yedekleme dosyaları için önerilir ve üretim iş yükleri için kullanılmamalıdır. <br/><br/> - [Depolama hesabını](../../../storage/common/storage-account-create.md) ve SQL Server VM aynı bölgede saklayın.<br/><br/> -Depolama hesabında Azure [coğrafi olarak yedekli depolamayı](../../../storage/common/storage-redundancy.md) (coğrafi çoğaltma) devre dışı bırakın.  |
| [Diskler](#disks-guidance) | -En az 2 [PREMIUM SSD disk](../../../virtual-machines/disks-types.md#premium-ssd) kullanın (günlük dosyası için 1 ve veri dosyaları için 1). <br/><br/> -< 1 MS GÇ gecikme süreleri gerektiren iş yükleri için, M serisi için yazma hızlandırıcıyı etkinleştirin ve es ve DS serisi için Ultra SSD diskler kullanmayı düşünün. <br/><br/> -Veri dosyalarını barındıran diskler üzerinde [salt okuma işlemini](../../../virtual-machines/premium-storage-performance.md#disk-caching) etkinleştirin.<br/><br/> - [SQL Server verileri, günlüğü ve tempdb dosyaları için depolamayı yapılandırırken](storage-configuration.md) iş yükünüze göre %20 ' ye AIT ek IOPS/verimlilik kapasitesi ekleyin <br/><br/> -Veritabanı depolama veya günlük kaydı için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br/><br/> -Günlük dosyasını barındıran disk (ler) i önbelleğe almayı etkinleştirmeyin.  **Önemli**: bir Azure sanal makineler diskinin önbellek ayarlarını değiştirirken SQL Server hizmetini durdurun.<br/><br/> -Daha fazla depolama verimi sağlamak için birden çok Azure veri diski dizili.<br/><br/> -Belgelenmiş ayırma boyutlarıyla biçimlendirin. <br/><br/> - `D:\` Görev açısından kritik SQL Server iş yükleri için yerel SSD sürücüsüne tempdb yerleştirin (doğru VM boyutunu seçtikten sonra). VM 'yi Azure portal veya Azure hızlı başlangıç şablonlarından oluşturup [GEÇICI veritabanını yerel diske yerleştirirseniz](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583), başka bir işlem yapmanız gerekmez; Tüm diğer durumlar için, başlatmalardaki hatalardan kaçınmak üzere  [tempdb 'yi depolamak üzere SSD 'Yi kullanmak](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-TempDB-and-buffer-pool-extensions/) için blogdaki adımları izleyin. Yerel sürücünün kapasitesi, geçici DB boyutunuz için yeterli değilse, geçici DB 'yi [salt okuma önbelleği](../../../virtual-machines/premium-storage-performance.md#disk-caching)olan Premium SSD disklerinde [dizili](../../../virtual-machines/premium-storage-performance.md) bir depolama havuzuna yerleştirin. |
| [G/Ç](#io-guidance) |-Veritabanı sayfa sıkıştırmasını etkinleştirin.<br/><br/> -Veri dosyaları için anlık dosya başlatmayı etkinleştirin.<br/><br/> -Veritabanının otomatik büyümesini sınırlayın.<br/><br/> -Veritabanının bir daha küçültmeyi devre dışı bırakın.<br/><br/> -Sistem veritabanları dahil olmak üzere tüm veritabanlarını veri disklerine taşıyın.<br/><br/> -SQL Server hata günlüğü ve izleme dosyası dizinlerini veri disklerine taşıyın.<br/><br/> -Varsayılan yedekleme ve veritabanı dosya konumlarını yapılandırın.<br/><br/> - [Bellekte kilitli sayfaları etkinleştirin](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows).<br/><br/> -SQL Server yüklü sürümü için [en son toplu güncelleştirmeleri](/sql/database-engine/install-windows/latest-updates-for-microsoft-sql-server) değerlendirin ve uygulayın. |
| [Özelliğe özgü](#feature-specific-guidance) | -Doğrudan Azure Blob depolamaya yedekleme.<br/><br/>-12 TB 'den büyük veritabanları için [dosya anlık görüntüsü yedeklemeleri](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) kullanın. <br/><br/>-Birden çok Temp DB dosyası, çekirdek başına 1 dosya, en fazla 8 dosya kullanın.<br/><br/>-Işletim sistemi için en fazla sunucu belleğini %90 veya en fazla 50 GB olacak şekilde ayarlayın. <br/><br/>-Geçici NUMA 'yı etkinleştirin. |


<br/>
Bu iyileştirmelerin *nasıl* ve *neden* yapılacağı hakkında daha fazla bilgi için lütfen aşağıdaki bölümlerde sunulan ayrıntıları ve Kılavuzu gözden geçirin.
<br/><br/>

## <a name="getting-started"></a>Başlarken

Azure VM 'de yeni bir SQL Server oluşturuyorsanız ve geçerli bir kaynak sistemi geçiriyorsanız, satıcı gereksinimlerinize göre yeni SQL Server VM oluşturun.  Bir SQL Server VM için satıcı gereksinimleri, şirket içinde dağıttığınız şeydir. 

Bulut için oluşturulmuş yeni bir uygulamayla yeni bir SQL Server VM oluşturuyorsanız, verileriniz ve kullanım gereksinimleriniz geliştikçe SQL Server VM kolayca boyut getirebilirsiniz.
Düşük katmanlı D serisi, B serisi veya AV2 serisi ile geliştirme ortamlarını başlatın ve ortamınızı zaman içinde büyütün. 

Üretim OLTP ortamı için önerilen minimum değer 4 sanal çekirdek, 32 GB bellek ve 8 ' in bellekten sanal çekirdek oranına sahiptir. Yeni ortamlar için 4 sanal çekirdek makinelerle başlayın ve veri ve işlem gereksinimleriniz değiştiğinde 8, 16, 32 sanal çekirdek veya daha fazlasını ölçeklendirin. OLTP üretilen iş için, her vCore için 5000 ıOPS 'ye sahip VM 'Leri hedef SQL Server. 

Portalda depolama yapılandırmasıyla SQL Server VM Market görüntülerini kullanın. Bu, iş yükleriniz için gereken boyut, ıOPS ve aktarım hızını almak için gereken depolama havuzlarını doğru şekilde oluşturmayı kolaylaştırır. Premium Depolama ve Premium Depolama önbelleğini destekleyen SQL Server VM 'Leri seçmek önemlidir. Daha fazla bilgi için [depolama](#storage-guidance) bölümüne bakın. 

SQL Server veri ambarı ve görev açısından kritik ortamların genellikle 8 bellekten sanal çekirdek oranının ötesine ölçeklendirilmesi gerekir. Orta ortamlar için, 16 çekirdek bellek oranı ve daha büyük veri ambarı ortamları için 32 çekirdek-bellek oranını seçebilirsiniz. 

SQL Server veri ambarı ortamları, genellikle daha büyük makinelerin paralel işlenmesinden faydalanır. Bu nedenle, d serisi ve Mv2 serisi daha büyük veri ambarı ortamları için güçlü seçeneklerdir.

## <a name="vm-size-guidance"></a>VM boyut Kılavuzu

Geçerli bir şirket içi SQL Server veritabanını Azure VM 'lerinde SQL Server geçirmek için temel olarak kaynak makinenizden vCPU ve bellek yapılandırmasını kullanın. [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) avantajlarından yararlanmak ve SQL Server lisanslama maliyetlerine kaydetmek için çekirdek lisansınızı Azure 'a taşıyın.

**Microsoft, üretim SQL Server iş yükleri için başlangıç noktası olarak 8 ' den fazla bellek çekirdeği oranı önerir.** Üretim dışı iş yükleri için daha küçük oranlar kabul edilebilir. 

İş yükünüze (OLTP veya veri ambarı) göre SQL Server performans için en uygun [bellek için iyileştirilmiş](../../../virtual-machines/sizes-memory.md), [genel amaçlı](../../../virtual-machines/sizes-general.md), [depolama Için iyileştirilmiş](../../../virtual-machines/sizes-storage.md)veya [Kısıtlanmış Vcore](../../../virtual-machines/constrained-vcpu.md) sanal makine boyutunu seçin. 

### <a name="memory-optimized"></a>Bellek için iyileştirilmiş

[Bellek için iyileştirilmiş sanal makine boyutları](../../../virtual-machines/sizes-memory.md) SQL Server VM 'ler için birincil hedeftir ve Microsoft tarafından önerilen seçenektir. Bellek için iyileştirilmiş sanal makineler, daha güçlü bellek-CPU oranları ve orta-büyük önbellek seçenekleri sunar. 

#### <a name="m-and-mv2-series"></a>A ve Mv2 serisi

[B serisi](/azure/virtual-machines/m-series) , en büyük SQL Server iş yükleri Için sanal çekirdek sayısı ve belleği sunar.  

[Mv2 serisi](../../../virtual-machines/mv2-series.md) , en yüksek sanal çekirdek sayısına ve belleğe sahiptir ve görev açısından kritik ve veri ambarı iş yükleri için önerilir. Mv2 serisi örnekler, büyük bellek içi veritabanlarını ve iş yüklerini, ilişkisel veritabanı sunucuları, büyük önbellekler ve bellek içi analizler için ideal olan yüksek bellekle CPU oranıyla desteklemek üzere benzersiz işlem performansı sağlayan bellek için iyileştirilmiş VM boyutlarıdır.

[Standard_M64ms](/azure/virtual-machines/m-series) , örneğin 28 bellekten sanal çekirdek oranına sahiptir.

D ve Mv2 serisi özelliklerinin bazıları SQL Server performansa yönelik olarak, [Premium Depolama](../../../virtual-machines/premium-storage-performance.md) ve [Premium depolama önbelleğe alma](../../../virtual-machines/premium-storage-performance.md#disk-caching) desteği, [Ultra disk](../../../virtual-machines/disks-enable-ultra-ssd.md) desteği ve [yazma hızlandırmasını](../../../virtual-machines/how-to-enable-write-accelerator.md)kapsar.

#### <a name="edsv4-series"></a>Edsv4 serisi

[Edsv4 serisi](../../../virtual-machines/edv4-edsv4-series.md) , bellek yoğun uygulamalar için tasarlanmıştır. Bu sanal makinelerin büyük bir yerel depolama SSD kapasitesi, güçlü yerel disk ıOPS 'si, 504 GiB 'a kadar RAM ve gelişmiş işlem, Gen2 VM 'lerle önceki Ev3/Esv3 boyutları ile karşılaştırılır. Bu sanal makinelerde, standart SQL Server iş yükleri için ideal olan 8 ' in neredeyse tutarlı bir bellek-vCore oranı vardır. 

Bu VM Serisi, düşük gecikme süresi, yüksek hızlı yerel depolama 'dan faydalanabilecek bellek yoğunluklu kurumsal uygulamalar ve uygulamalar için idealdir.

Edsv4 serisi sanal makineler [Premium depolamayı](../../../virtual-machines/premium-storage-performance.md)ve [Premium depolama önbelleği](../../../virtual-machines/premium-storage-performance.md#disk-caching)'ni destekler.

#### <a name="dsv2-series-11-15"></a>DSv2 serisi 11-15

[DSv2 serisi 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) , önceki D serisi ile aynı bellek ve disk yapılandırmalarına sahiptir. Bu seride, tüm sanal makinelerde 7 ' nin tutarlı, bellekten CPU oranı vardır. 

[DSv2 serisi 11-15](../../../virtual-machines/dv2-dsv2-series-memory.md#dsv2-series-11-15) , [Premium Depolama](../../../virtual-machines/premium-storage-performance.md) ve [Premium Depolama önbelleğini](../../../virtual-machines/premium-storage-performance.md#disk-caching)destekler; bu, en iyi performans için önerilir.

### <a name="general-purpose"></a>Genel Amaçlı

[Genel amaçlı sanal makine boyutları](../../../virtual-machines/sizes-general.md) , geliştirme ve test, Web sunucuları ve daha küçük veritabanı sunucuları gibi daha küçük giriş düzeyi iş yükleri için dengeli bellekten sanal çekirdek oranları sağlamak üzere tasarlanmıştır. 

Genel amaçlı sanal makinelerle daha küçük olan bellekten sanal çekirdek oranları nedeniyle, SQL Server bellek tabanlı performans sayaçlarını dikkatle izlemek önemlidir, çünkü bu, gereken arabellek önbelleği belleğini. Daha fazla bilgi için bkz. [bellek performansı temeli](#memory) . 

Üretim iş yükleri için başlangıç önerisi 8 ' in bellekten sanal çekirdek oranı olduğundan, SQL Server çalıştıran bir genel amaçlı VM için önerilen en düşük yapılandırma 4 vCPU ve 32 GB bellek olur. 

#### <a name="ddsv4-series"></a>Ddsv4 serisi

[Ddsv4-Series](../../../virtual-machines/ddv4-ddsv4-series.md) , vCPU, bellek ve geçici disk için, ancak daha küçük bellekten sanal çekirdek desteğiyle bir bileşim sunar. 

Ddsv4 VM 'Leri, daha düşük gecikme süresi ve daha yüksek hızlı yerel depolama içerir.

Bu makineler, geçici depolama ve departman ilişkisel veritabanlarına hızlı erişim gerektiren yan yana SQL ve uygulama dağıtımları için idealdir. Bu serideki tüm sanal makineler arasında standart bir bellek-sanal çekirdek oranı vardır. 

Bu nedenle, bu serideki, 8 sanal çekirdekler ve 32 GB bellek içeren başlangıç sanal makinesi olarak D8ds_v4 kullanmanız önerilir. En büyük makine, 64 sanal çekirdek ve 256 GB bellek içeren D64ds_v4.

[Ddsv4 serisi](../../../virtual-machines/ddv4-ddsv4-series.md) sanal makineler, [Premium Depolama](../../../virtual-machines/premium-storage-performance.md) ve [Premium Depolama önbelleğini](../../../virtual-machines/premium-storage-performance.md#disk-caching)destekler.

> [!NOTE]
> [Ddsv4 serisi](../../../virtual-machines/ddv4-ddsv4-series.md) , SQL Server iş yükleri için önerilen 8 ' ın bellekten sanal çekirdek oranına sahip değildir. Bu nedenle, bu sanal makinelerin yalnızca daha küçük uygulama ve geliştirme iş yükleri için kullanılması düşünülmektedir.

#### <a name="b-series"></a>B serisi

[Burstable B serisi](../../../virtual-machines/sizes-b-series-burstable.md) sanal makine boyutları, kavram kanıtı ve çok küçük uygulama ve geliştirme sunucuları gibi tutarlı performansa gerek olmayan iş yükleri için idealdir. 

[Burstable B serisi](../../../virtual-machines/sizes-b-series-burstable.md) sanal makine boyutlarının çoğu, 4 ' ün bellekten Vcore oranına sahiptir. Bu makinelerin en büyüğü 20 sanal çekirdek ve 80 GB bellek içeren [Standard_B20ms](../../../virtual-machines/sizes-b-series-burstable.md) .

Bu seri, uygulamalar makine boyutuna bağlı olarak değişen Burstable kredilerle iş saatlerinde **veri bloğu** yapabilme özelliğine sahip olduğu için benzersizdir. 

Krediler tükendiğinde VM, taban çizgisi makine performansına geri döner.

B serisinin avantajı, özellikle de işlem gücü için gün boyunca gerekli olan diğer VM boyutlarına göre elde edebileceğiniz işlem tasarruflarıdır.

Bu seri [Premium depolamayı](../../../virtual-machines/premium-storage-performance.md)destekler, ancak  [Premium Depolama önbelleğini](../../../virtual-machines/premium-storage-performance.md#disk-caching)desteklemez.

> [!NOTE] 
> [Burstable B serisi](../../../virtual-machines/sizes-b-series-burstable.md) , SQL Server iş yükleri için önerilen 8 ' ın bellekten sanal çekirdek oranına sahip değildir. Bu nedenle, bu sanal makineleri yalnızca daha küçük uygulamalar, Web sunucuları ve geliştirme iş yükleri için kullanmayı göz önünde bulundurun.

#### <a name="av2-series"></a>Av2 Serisi

[AV2 serisi](../../../virtual-machines/av2-series.md) VM 'ler, geliştirme ve test, düşük trafikli web sunucuları, küçük ve orta ölçekli uygulama veritabanları ve kavram kanıtı gibi giriş düzeyi iş yükleri için idealdir.

Yalnızca [Standard_A2m_v2](../../../virtual-machines/av2-series.md) (2 sanal çekirdek ve 16GB bellek), [Standard_A4m_v2](../../../virtual-machines/av2-series.md) (4 sanal çekirdek ve 32gb bellek) ve [Standard_A8m_v2](../../../virtual-machines/av2-series.md) (8 sanal çekirdek ve 64 GB bellek), bu en önemli üç sanal makine Için 8 ' in bellek-Vcore oranına sahiptir. 

Bu sanal makineler, daha küçük geliştirme ve test SQL Server makinelere yönelik iyi seçeneklerdir. 

8 sanal çekirdek [Standard_A8m_v2](../../../virtual-machines/av2-series.md) , küçük uygulama ve Web sunucuları için de iyi bir seçenek olabilir.

> [!NOTE] 
> AV2 serisi, Premium depolamayı desteklemez ve bu nedenle, bellekten sanal çekirdek oranına sahip sanal makinelerle birlikte üretim SQL Server iş yükleri için önerilmez.

### <a name="storage-optimized"></a>Depolama için iyileştirilmiş

[Depolama için IYILEŞTIRILMIŞ VM boyutları](../../../virtual-machines/sizes-storage.md) belirli kullanım örneklerine yöneliktir. Bu sanal makineler özellikle iyileştirilmiş disk aktarım hızı ve GÇ ile tasarlanmıştır. Bu sanal makine serisi, büyük veri senaryoları, veri depolama ve büyük işlem veritabanlarına yöneliktir. 

#### <a name="lsv2-series"></a>Lsv2 serisi

[Lsv2 serisi](../../../virtual-machines/lsv2-series.md) , yüksek aktarım hızı, düşük gecikme süresi ve yerel NVMe depolama özellikleri. Lsv2 serisi VM 'Ler, dayanıklı veri disklerini kullanmak yerine doğrudan VM 'ye eklenen düğümdeki yerel diski kullanacak şekilde iyileştirilmiştir. 

Bu sanal makineler, büyük veri, veri ambarı, raporlama ve ETL iş yükleri için güçlü seçeneklerdir. Yerel NVMe depolamanın yüksek aktarım hızı ve IOPS, veritabanınıza yüklenecek dosyaları ve kaynak verilerin kaynak sistemden ya da Azure Blob depolama veya Azure Data Lake gibi diğer depolardan yeniden oluşturulabilen diğer senaryolara yönelik olarak işlenmesi için iyi bir kullanım durumdur. [Lsv2 serisi](../../../virtual-machines/lsv2-series.md) VM 'Ler aynı anda en fazla 30 dakika boyunca disk performansını da aşırı alabilir.

Bu sanal makineler 1,92, her bir vCPU için 8 GiB bellek ve her 8 vCPU için 8 TB 'lık NVMe SSD ile 80 vCPU arasında olur. Bu, [L80s_v2](../../../virtual-machines/lsv2-series.md), bu serinin en büyük VM 'si için, 10 gb 'Lık 80 sanal cpu ve 640 Bib belleği, 10 x 1.92 TB olan NVMe depolaması ile aynıdır.  Bu sanal makinelerin tümünde 8 ' e kadar tutarlı bir bellek-vCore oranı vardır.

NVMe depolaması, sanal makinenizi yeniden başlatırsanız bu disklerde verilerin kaybedilmesi anlamına gelir.

Lsv2 ve LS Serisi [Premium](../../../virtual-machines/premium-storage-performance.md)depolamayı destekler, ancak Premium depolama önbelleğe alma işlemini desteklemez. IOPS 'yi artırmak için yerel bir önbelleğin oluşturulması desteklenmez. 

> [!WARNING]
> Veri dosyalarınızı kısa ömürlü NVMe depolamada depolamak, VM serbest bırakıldığında veri kaybına neden olabilir. 

### <a name="constrained-vcores"></a>Kısıtlanmış sanal çekirdekler

Yüksek performanslı SQL Server iş yükleri genellikle daha fazla sanal çekirdek sayısı olmadan daha büyük miktarda bellek, GÇ ve aktarım hızına ihtiyaç duyar. 

Birçok OLTP iş yükü, çok sayıda küçük işlem tarafından yönetilen uygulama veritabanlarıdır. OLTP iş yükleri sayesinde, verilerin yalnızca küçük bir miktarı okunmakta veya değiştirilir, ancak kullanıcı sayılarına göre çalışan işlem birimleri çok daha yüksektir. SQL Server belleğin önbelleğe almak için kullanılabilir olması, en son erişilen verileri performans için depolaması ve fiziksel okumaların belleğe hızla okunabileceği emin olmanız önemlidir. 

Bu OLTP ortamlarında daha yüksek miktarda bellek, hızlı depolama ve en iyi şekilde gerçekleştirmek için gereken g/ç bant genişliği gerekir. 

Daha yüksek SQL Server lisanslama maliyetleri olmadan bu performans düzeyini korumak için Azure, [Kısıtlanmış vCPU sayısı](../../../virtual-machines/constrained-vcpu.md)ile VM boyutları sunar. 

Bu, üst sanal makinenin aynı bellek, depolama ve g/ç bant genişliğini koruyarak, kullanılabilir sanal çekirdekleri azaltarak lisanslama maliyetlerini denetlemeye yardımcı olur.

VCPU sayısı, özgün VM boyutunun dörtte birini bir buçuk ile kısıtlanıyor olabilir. Sanal makine için kullanılabilir olan sanal çekirdekleri azaltmak, daha yüksek bellek-sanal çekirdek oranlarına ulaşacaktır.

Bu yeni VM boyutları, daha kolay tanımlanabilmesi için etkin vCPU sayısını belirten bir soneke sahiptir. 

Örneğin, [M64-32ms](../../../virtual-machines/constrained-vcpu.md) 32 yalnızca, [M64ms](/azure/virtual-machines/m-series) için bellek, GÇ ve aktarım hızı ile [M64-16Ms](../../../virtual-machines/constrained-vcpu.md) yalnızca 16 sanal çekirdekleri gerektirir SQL Server sanal çekirdekler için yalnızca lisanslama gerektirir.  [M64-16ms](../../../virtual-machines/constrained-vcpu.md) 'nin M64ms lisans maliyetine ait bir SQL Server çeyreği olsa da, sanal makinenin işlem maliyeti aynı olacaktır.

> [!NOTE] 
> - Orta ila büyük veri ambarı iş yükleri, [kısıtlı vCore sanal](../../../virtual-machines/constrained-vcpu.md)makinelerinden yine de faydalanabilir, ancak veri ambarı iş yükleri genellikle daha az Kullanıcı ve paralel olarak çalışan sorgu planları aracılığıyla daha büyük miktarlarda veri adresleyen süreçler tarafından belirlenir. 
> - İşletim sistemi lisansını içeren işlem maliyeti, üst sanal makineyle aynı kalacaktır. 

## <a name="storage-guidance"></a>Depolama yönergeleri

TPC-E ve TPC-C kıyaslamalarıyla Azure sanal makinelerinde SQL Server performansının ayrıntılı testi için, [OLTP performansını iyileştirme](https://techcommunity.microsoft.com/t5/SQL-Server/Optimize-OLTP-Performance-with-SQL-Server-on-Azure-VM/ba-p/916794)başlıklı bloga başvurun. 

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

`D:\`Görev açısından kritik SQL Server iş yükleri için (doğru VM boyutunu seçtikten sonra) tempdb 'yi yerel SSD sürücüsüne yerleştirin. VM 'yi Azure portal veya Azure hızlı başlangıç şablonlarından oluşturup [GEÇICI veritabanını yerel diske yerleştirirseniz](https://techcommunity.microsoft.com/t5/SQL-Server/Announcing-Performance-Optimized-Storage-Configuration-for-SQL/ba-p/891583), başka bir işlem yapmanız gerekmez; Tüm diğer durumlar için, başlatmalardaki hatalardan kaçınmak üzere  [tempdb 'yi depolamak üzere SSD 'Yi kullanmak](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-TempDB-and-buffer-pool-extensions/) için blogdaki adımları izleyin. Yerel sürücünün kapasitesi, geçici DB boyutunuz için yeterli değilse, geçici DB 'yi [salt okuma önbelleği](../../../virtual-machines/premium-storage-performance.md#disk-caching)olan Premium SSD disklerinde [dizili](../../../virtual-machines/premium-storage-performance.md) bir depolama havuzuna yerleştirin.

Premium SSD 'Leri destekleyen VM 'Ler için TempDB 'yi okuma önbelleği etkinken Premium SSD 'leri destekleyen bir diskte da saklayabilirsiniz.


### <a name="data-disks"></a>Veri diskleri

* **Veri ve günlük dosyaları için PREMIUM SSD disklerini kullanın**: disk şeritleme kullanmıyorsanız, bir diskin günlük dosyasını içerdiği ve diğeri verileri içerdiği ıkı Premium SSD diski kullanın. Her Premium SSD [,, makalesinde](../../../virtual-machines/disks-types.md)gösterildiği gibi, boyutuna bağlı olarak BIR dizi IOPS ve bant GENIŞLIĞI (MB/s) sağlar. Depolama alanları gibi bir disk şeritleme tekniği kullanıyorsanız, biri günlük dosyası (ler) ve diğer veri dosyaları için olmak üzere iki havuza sahip olacak şekilde en iyi performansa ulaşmanız gerekir. Ancak, SQL Server yük devretme kümesi örnekleri (FCı) kullanmayı planlıyorsanız, bir havuz yapılandırmanız veya bunun yerine [Premium dosya paylaşımlarını](failover-cluster-instance-premium-file-share-manually-configure.md) kullanmanız gerekir.

   > [!TIP]
   > - Çeşitli disk ve iş yükü yapılandırmalarında test sonuçları için aşağıdaki blog gönderisine bakın: [Azure sanal makinelerinde SQL Server Için depolama yapılandırma yönergeleri](/archive/blogs/sqlserverstorageengine/storage-configuration-guidelines-for-sql-server-on-azure-vm).
   > - ~ 50.000 ıOPS gerektiren SQL Server 'Lar için görev açısından kritik performans için, 10-P30 disklerini bir Ultra SSD değiştirmeyi düşünün. Daha fazla bilgi için şu blog gönderisine bakın: [Ultra SSD Ile görev açısından kritik performans](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/).

   > [!NOTE]
   > Portalda bir SQL Server VM sağladığınızda, depolama yapılandırmanızı Düzenle seçeneğiniz vardır. Azure, yapılandırmanıza bağlı olarak bir veya daha fazla disk yapılandırır. Birden çok disk, dizme ile tek bir depolama havuzunda birleştirilir. Hem veri hem de günlük dosyaları, bu yapılandırmada birlikte bulunur. Daha fazla bilgi için bkz. [SQL Server VM 'ler Için depolama yapılandırması](storage-configuration.md).

* **Disk şeridi**: daha fazla verimlilik için, ek veri diskleri ekleyebilir ve disk şeritleme kullanabilirsiniz. Veri disklerinin sayısını öğrenmek için, günlük dosyanız için gereken ıOPS ve bant genişliği sayısını ve verileriniz ile TempDB dosyalarını analiz etmeniz gerekir. Farklı VM boyutlarının, IOPS sayısı ve desteklenen bant genişliği için farklı sınırlara sahip olduğuna dikkat edin. [VM boyutu](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)başına IOPS üzerindeki tablolara bakın. Aşağıdaki yönergeleri kullanın:

  * Windows 8/Windows Server 2012 veya üzeri için, [depolama alanlarını](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v=ws.11)) aşağıdaki yönergelerle kullanın:

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

  * Windows 2008 R2 veya önceki sürümlerde, dinamik diskleri (işletim sistemi şeritli birimler) kullanabilir ve şerit boyutu her zaman 64 KB 'tır. Bu seçenek, Windows 8/Windows Server 2012 itibarıyla kullanım dışıdır. Daha fazla bilgi için bkz. [sanal disk hizmetindeki Destek bildirisi Windows Storage YÖNETIM API 'sine geçiyor](/windows/win32/w8cookbook/vds-is-transitioning-to-wmiv2-based-windows-storage-management-api).

  * [SQL Server yük devretme kümesi örnekleriyle](failover-cluster-instance-storage-spaces-direct-manually-configure.md) [depolama alanları doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) kullanıyorsanız, tek bir havuz yapılandırmanız gerekir. Bu tek havuzda farklı birimler oluşturulabilse de, hepsi aynı önbelleğe alma ilkesi gibi aynı özellikleri paylaşır.

  * Yük beklentilerinize göre depolama havuzunuzun ilişkili disk sayısını belirleme. Farklı VM boyutlarının farklı sayıda ekli veri diskine izin verdiğini aklınızda bulundurun. Daha fazla bilgi için bkz. [sanal makineler Için boyutlar](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Premium SSD 'leri (geliştirme/test senaryoları) kullanmıyorsanız, [sanal makine boyutunuzu](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) desteklediği en fazla veri diski sayısını eklemek ve disk şeritleme kullanılması önerilir.

* **Önbelleğe alma ilkesi**: depolama yapılandırmanıza bağlı olarak önbelleğe alma ilkesi için aşağıdaki önerilere göz önünde edin.

  * Veri ve günlük dosyaları için ayrı diskler kullanıyorsanız, veri dosyalarınızı ve TempDB veri dosyalarını barındıran veri disklerinde okuma önbelleğini etkinleştirin. Bu, önemli bir performans avantajı oluşmasına neden olabilir. Günlük dosyasını tutan diskte önbelleğe alma özelliğini etkinleştirmeyin çünkü bu, performansın küçük bir düşüşine neden olur.

  * Tek bir depolama havuzunda disk şeridi kullanıyorsanız, çoğu iş yükü okuma önbelleklemesi avantajına sahip olur. Günlük ve veri dosyaları için ayrı depolama havuzlarınız varsa, yalnızca veri dosyaları için depolama havuzunda okuma önbelleğini etkinleştirin. Bazı ağır yazma iş yükleri için, önbelleğe alma olmadan daha iyi performans elde edilebilir. Bu, yalnızca test aracılığıyla belirlenebilir.

  * Önceki öneriler Premium SSD 'Ler için geçerlidir. Premium SSD kullanmıyorsanız, herhangi bir veri diskinde önbelleğe alma etkinleştirmeyin.

  * Disk önbelleğe almayı yapılandırma yönergeleri için aşağıdaki makalelere bakın. Klasik (ASM) dağıtım modeli için bkz: [set-AzureOSDisk](/previous-versions/azure/jj152847(v=azure.100)) ve [set-AzureDataDisk](/previous-versions/azure/jj152851(v=azure.100)). Azure Resource Manager dağıtım modeli için bkz: [set-AzOSDisk](/powershell/module/az.compute/set-azvmosdisk) ve [set-AzVMDataDisk](/powershell/module/az.compute/set-azvmdatadisk).

     > [!WARNING]
     > Herhangi bir veritabanı bozulması olasılığını ortadan kaldırmak için Azure sanal makineler disklerinin önbellek ayarını değiştirirken SQL Server hizmetini durdurun.

* **NTFS ayırma birimi boyutu**: veri diski biçimlendirilirken veri ve günlük dosyaları için BIR 64 KB ayırma birimi boyutu ve tempdb de kullanmanız önerilir. TempDB geçici diske yerleştirilmişse (D:\ sürücü) bu sürücüden yararlanarak elde edilen performans, 64 KB 'lik bir ayırma birimi boyutu gereksinimini ortadan kaldırarak ortaya çıktı. 

* **Disk yönetimi en iyi uygulamaları**: bir veri diski kaldırılırken veya önbellek türünü değiştirirken, değişiklik sırasında SQL Server hizmetini durdurun. Önbelleğe alma ayarları işletim sistemi diskinde değiştirildiğinde, Azure VM 'yi sonlandırır, önbellek türünü değiştirir ve VM 'yi yeniden başlatır. Bir veri diskinin önbellek ayarları değiştirildiğinde, VM durdurulmaz, ancak veri diski değişiklik sırasında VM 'den ayrılır ve yeniden eklenir.

  > [!WARNING]
  > Bu işlemler sırasında SQL Server hizmeti durdurulamaması veritabanının bozulmasına neden olabilir.


## <a name="io-guidance"></a>G/ç Kılavuzu

* Uygulama ve isteklerinizi paralel hale getirmek yaparken Premium SSD 'Lerin en iyi sonuçları elde edilir. Premium SSD 'ler, GÇ sırası derinliğinin 1 ' den büyük olduğu senaryolar için tasarlanmıştır, bu nedenle tek iş parçacıklı seri istekler için çok az veya hiç performans kazancı (depolama alanı yoğun olsalar bile) görürsünüz. Örneğin, bu, SQBIR gibi performans analizi araçlarının tek iş parçacıklı test sonuçlarını etkileyebilir.

* G/ç yoğunluklu iş yüklerinin performansını artırmaya yardımcı olması için [veritabanı sayfa sıkıştırmasını](/sql/relational-databases/data-compression/data-compression) kullanmayı göz önünde bulundurun. Ancak, veri sıkıştırması, veritabanı sunucusundaki CPU tüketimini artırabilir.

* İlk dosya ayırma için gereken süreyi azaltmak üzere anlık dosya başlatmayı etkinleştirmeyi düşünün. Anlık dosya başlatmasının avantajlarından yararlanmak için, SQL Server (MSSQLSERVER) hizmet hesabına SE_MANAGE_VOLUME_NAME sahip olur ve **Birim bakımı görevleri** güvenlik ilkesine eklersiniz. Azure için SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (NT Service\MSSQLSERVER) **toplu bakım görevleri** oluştur güvenlik ilkesine eklenmez. Diğer bir deyişle, bir Azure platform görüntüsünde SQL Server anlık dosya başlatma etkin değildir. SQL Server hizmet hesabını **toplu bakım görevleri Için gerçekleştir** güvenlik ilkesine ekledikten sonra, SQL Server hizmetini yeniden başlatın. Bu özelliğin kullanımıyla ilgili güvenlik konuları olabilir. Daha fazla bilgi için bkz. [veritabanı dosyası başlatma](/sql/relational-databases/databases/database-instant-file-initialization).

* **Otomatik büyüme** 'nın yalnızca beklenmeyen büyüme için bir acil durum olduğu kabul ettiğini unutmayın. Verilerinizi ve günlük büyümesini otomatik büyüme ile günde bir güne göre yönetmeyin. Otomatik büyüme kullanılıyorsa, boyut anahtarını kullanarak dosyayı önceden büyütün.

* Performansı olumsuz etkileyebilecek gereksiz ek yükü önlemek için, **oto küçültme** 'nın devre dışı bırakıldığından emin olun.

* Tüm veritabanlarını sistem veritabanları dahil olmak üzere veri disklerine taşıyın. Daha fazla bilgi için bkz. [sistem veritabanlarını taşıma](/sql/relational-databases/databases/move-system-databases).

* SQL Server hata günlüğünü ve izleme dosyası dizinlerini veri disklerine taşıyın. Bu, SQL Server örneğinize sağ tıklayıp Özellikler ' i seçerek SQL Server Yapılandırma Yöneticisi yapılabilir. Hata günlüğü ve izleme dosyası ayarları **Başlangıç parametreleri** sekmesinde değiştirilebilir. Döküm dizini **Gelişmiş** sekmesinde belirtilir. Aşağıdaki ekran görüntüsünde hata günlüğü başlangıç parametresinin nerede aranacağı gösterilmektedir.

    ![SQL hata günlüğü ekran görüntüsü](./media/performance-guidelines-best-practices/sql_server_error_log_location.png)

* Varsayılan yedekleme ve veritabanı dosya konumlarını ayarlayın. Bu makaledeki önerileri kullanın ve sunucu özellikleri penceresinde değişiklikleri yapın. Yönergeler için bkz. [veri ve günlük dosyaları Için varsayılan konumları görüntüleme veya değiştirme (SQL Server Management Studio)](/sql/database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files). Aşağıdaki ekran görüntüsünde, bu değişiklikleri nerede yapacağınız gösterilmektedir.

    ![SQL veri günlüğü ve yedekleme dosyaları](./media/performance-guidelines-best-practices/sql_server_default_data_log_backup_locations.png)
* GÇ ve tüm disk belleği etkinliklerini azaltmak için kilitli sayfaları etkinleştirin. Daha fazla bilgi için bkz. [bellek Içinde sayfaları kilitleme seçeneği (Windows)](/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows).

* SQL Server 2012 çalıştırıyorsanız, Service Pack 1 toplu güncelleştirme 10 ' u yükleyebilirsiniz. Bu güncelleştirme, SQL Server 2012 ' de geçici tablo seçme ekstresini çalıştırdığınızda g/ç 'de düşük performans düzeltmesini içerir. Bilgi için bu [Bilgi Bankası makalesine](https://support.microsoft.com/kb/2958012)bakın.

* Azure 'da/dışına aktarma yaparken herhangi bir veri dosyasını sıkıştırmayı göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özelliğe özgü rehberlik

Bazı dağıtımlar, daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajları elde edebilir. Aşağıdaki listede, daha iyi performans elde etmenize yardımcı olabilecek bazı SQL Server özellikler vurgulanmıştır:

### <a name="back-up-to-azure-storage"></a>Azure depolama 'ya yedekleme
Azure sanal makinelerde çalışan SQL Server için yedeklemeler gerçekleştirirken, [URL 'ye SQL Server yedekleme](/sql/relational-databases/backup-restore/sql-server-backup-to-url)kullanabilirsiniz. Bu özellik SQL Server 2012 SP1 CU2 UYGULAMAZSANıZ ile başlayarak kullanılabilir ve ekli veri disklerine yedekleme için önerilir. Azure depolama 'ya yedekleme/geri yükleme yaptığınızda, [URL En Iyi uygulamaları ve sorun giderme ve Azure depolama 'Da depolanan yedeklemelerden geri yükleme SQL Server için](/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting)belirtilen önerileri izleyin. Ayrıca, [Azure sanal makinelerinde SQL Server Için Otomatik yedeklemeyi](../../../azure-sql/virtual-machines/windows/automated-backup-sql-2014.md)kullanarak bu yedeklemeleri otomatikleştirebilir.

SQL Server 2012 ' dan önce [Azure 'da SQL Server yedekleme](https://www.microsoft.com/download/details.aspx?id=40740)'yi kullanabilirsiniz. Bu araç, birden fazla yedek Stripe hedefi kullanarak yedekleme aktarım hızını artırmaya yardımcı olabilir.

### <a name="sql-server-data-files-in-azure"></a>Azure 'da veri dosyaları SQL Server

[Azure 'daki SQL Server veri dosyaları](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure)olan bu yeni özellik, SQL Server 2014 ' den başlayarak kullanılabilir. Azure 'daki veri dosyalarıyla SQL Server çalıştırmak, Azure veri disklerini kullanarak karşılaştırılabilir performans özelliklerini gösterir.

### <a name="failover-cluster-instance-and-storage-spaces"></a>Yük devretme kümesi örneği ve depolama alanları

Depolama alanları 'nı kullanıyorsanız, **onay** sayfasındaki kümeye düğüm eklerken, **tüm uygun depolamayı kümeye ekleyin** etiketli onay kutusunun işaretini kaldırın. 

![Uygun depolamanın işaretini kaldır](./media/performance-guidelines-best-practices/uncheck-eligible-cluster-storage.png)

Depolama alanları 'nı kullanıyorsanız ve **tüm uygun depolama alanını kümeye ekle** seçeneğinin işaretini kaldırırsanız, Windows, kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları kümeden kaldırılana ve PowerShell kullanılarak yeniden iliştirene kadar disk Yöneticisi veya Explorer 'da görünmez. Depolama alanları, depolama havuzlarında birden çok diski gruplandırır. Daha fazla bilgi için bkz. [depolama alanları](/windows-server/storage/storage-spaces/overview).

## <a name="multiple-instances"></a>Birden çok örnek 

Birden çok SQL Server örneğini tek bir sanal makineye dağıtmak için aşağıdaki en iyi yöntemleri göz önünde bulundurun: 

- Her bir SQL Server örneği için en yüksek sunucu belleğini ayarlayın, bu da işletim sistemi için kalan bellek olduğundan emin olur. Sanal makineye ayrılan bellek miktarını değiştirirseniz SQL Server örneklerine yönelik bellek kısıtlamalarını güncelleştirdiğinizden emin olun. 
- Her birinin farklı iş yükü desenleri olduğundan ve birbirini etkilemelerini istemediğiniz için veri, günlük ve TempDB için ayrı LUN 'ler vardır. 
- Uygulamanızın SLA 'lerinizde en yoğun iş yükü kapasitesini işleyebileceğinden emin olmak için ortamınızı yoğun üretim benzeri iş yükleri altında kapsamlı bir şekilde test edin. 

Aşırı yüklenmiş sistemlerin işaretleri, çalışan iş parçacığı tükenmesi, yavaş yanıt süreleri ve/veya durdurulmuş dağıtıcı sistem belleği içerebilir, ancak bunlarla sınırlı değildir. 



## <a name="collect-performance-baseline"></a>Performans temelini topla

Daha kapsamlı bir yaklaşım için, PerfMon/LogMan kullanarak performans sayaçlarını toplayın ve kaynak ortamının genel baskılarına ve olası performans sorunlarını daha iyi anlamak için SQL Server bekleme istatistiklerini yakalayın. 

[Uygulama performansı denetim listesini](../../../virtual-machines/premium-storage-performance.md#application-performance-requirements-checklist)takip eden en yoğun zamanlarda kaynak Iş yükünün CPU, bellek, [IOPS](../../../virtual-machines/premium-storage-performance.md#iops), [verimlilik](../../../virtual-machines/premium-storage-performance.md#throughput)ve [gecikmesini](../../../virtual-machines/premium-storage-performance.md#latency) toplayarak başlayın. 

Tipik iş gününde iş yükleri gibi yoğun saatlerde veri toplayın, ayrıca günlük işleme ve hafta sonu ETL iş yükleri gibi diğer yüksek yük işlemleri de elde edin. Çeyrek sonu işleme gibi genellikle yoğun iş yükleri için kaynaklarınızın ölçeğini ölçeklendirmeniz ve iş yükü tamamlandığında ölçeklendirilmesi gerekir. 

İş yükünüzün performans gereksinimlerine ölçeklenebilen [VM boyutunu](../../../virtual-machines/sizes-memory.md) seçmek için performans analizini kullanın.


### <a name="iops-and-throughput"></a>IOPS ve aktarım hızı

SQL Server performans, g/ç alt sistemine göre büyük ölçüde değişir. Veritabanınız fiziksel belleğe uygun olmadığı için SQL Server sürekli olarak veritabanı sayfalarını arabellek havuzunda içine ve dışına çıkarır. SQL Server için veri dosyaları farklı şekilde değerlendirilmelidir. Günlük dosyalarına erişim, TempDB dahil olmak üzere veri dosyalarının rasgele olarak erişildiği bir işlemin geri alınması gerektiği durumlar haricinde sıralıdır. Yavaş bir g/ç alt sistemidir varsa, kullanıcılarınız yavaş yanıt süreleri ve zaman aşımları nedeniyle tamamlanmamış görevler gibi performans sorunlarıyla karşılaşabilir. 

Azure Marketi Sanal makineleri, varsayılan olarak veri dosyalarından ayrı bir fiziksel diskte bulunan günlük dosyalarına sahiptir. TempDB veri dosyalarının sayısı ve boyutu en iyi uygulamaları karşılar ve kısa ömürlü D:/'yi hedeflenmiştir sürücü.. 

Aşağıdaki PerfMon sayaçları, SQL Server gereken GÇ aktarım hızını doğrulamaya yardımcı olabilir: 
* **\Logicaldisk\disk Okuma/sn** (okuma ve Yazma IOPS)
* **\Logicaldisk\disk yazma/sn** (okuma ve Yazma IOPS) 
* **\Logicaldisk\disk bayt/sn** (veri, günlük ve tempdb dosyaları için işleme gereksinimleri)

En yoğun düzeylerde ıOPS ve aktarım hızı gereksinimlerini kullanarak ölçülerinizin kapasitesiyle eşleşen VM boyutlarını değerlendirin. 

İş yükünüz 20 K okuma ıOPS ve 10.000 yazma ıOPS gerektiriyorsa, depolama alanları kullanılarak 2 P30 disk şeritli şekilde E16s_v3 (en fazla 32 K önbelleğe alınmış ve 25600 önbelleğe alınmış ıOPS ile) veya M16_s (20 KB 'a kadar önbelleğe alınmış ve 10.000 'den önbelleğe alınmış ıOPS ile) seçeneğini belirleyebilirsiniz. 

VM 'Ler, ıOPS ve aktarım hızı için farklı ölçek sınırlarına sahip olduğu için iş yükünün hem aktarım hızını hem de ıOPS gereksinimlerini anladığınızdan emin olun.

### <a name="memory"></a>Bellek

Ayrıca, işletim sistemi tarafından kullanılan dış belleği ve SQL Server dahili olarak kullanılan belleği izleyin. Her iki bileşenin de basıncını belirlenmesi, sanal makinelerin boyutlandırmasından ve ayarlamaya yönelik fırsatları belirlemesine yardımcı olur. 

Aşağıdaki PerfMon sayaçları, bir SQL Server sanal makinenin bellek durumunu doğrulamaya yardımcı olabilir: 
* [\Bellek\kullanılabilir MBayt](/azure/monitoring/infrastructure-health/vmhealth-windows/winserver-memory-availmbytes)
* [\SQLServer: bellek Manager\Target sunucu belleği (KB)](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)
* [\SQLServer: bellek Manager\Total sunucu belleği (KB)](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)
* [\SQLServer: buffer Manager\Lazy yazma/sn](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)
* [\SQLServer: buffer Manager\Page Life erkeklerin](/sql/relational-databases/performance-monitor/sql-server-buffer-manager-object)

### <a name="compute--processing"></a>İşlem/Işleme

Azure 'da işlem, Şirket içinden farklı yönetilir. Şirket içi sunucular, yönetim ek yükü ve yeni donanım edinme maliyeti nedeniyle yükseltme olmadan son birkaç yıla göre oluşturulmuştur. Sanallaştırma bu sorunlardan bazılarını azaltır, ancak kaynak tüketimine yapılan önemli değişiklikler tüm fiziksel ortamın yeniden dengelenmesi gereken anlamına gelir. 

Bu, farklı bir donanım serisi üzerinde yeni bir sanal makinenin, hatta farklı bir bölgede bile elde edilmesi kolay bir şekilde Azure 'da bir sınama değildir. 

Azure 'da, sanal makine kaynaklarından çok daha fazla yararlanmak istiyorsunuz, bu nedenle Azure sanal makineleri, iş yükünü etkilemeden ortalama CPU 'YU mümkün olduğunca yüksek tutmaya yönelik olarak yapılandırılmalıdır. 

Aşağıdaki PerfMon sayaçları, bir SQL Server sanal makinenin işlem durumunu doğrulamaya yardımcı olabilir:
* **\İşlemci bilgileri (_Total) \% Işlemci zamanı**
* **\ Process (sqlservr) \% Işlemci zamanı**

> [!NOTE] 
> İdeal olarak, %90 ' ün üzerinde en üst düzeye sahip olan ancak sürekli bir süre için %80 ' a 100 ulaşmadan, işlem sürenizin% ' ı kullanmayı hedefmeye çalışın. Temelde, yalnızca uygulama için gereken işlemi sağlamak ve iş gerektirdiğinde ölçeği yukarı veya aşağı doğru şekilde planlamak isteyeceksiniz. 

## <a name="next-steps"></a>Sonraki adımlar

En iyi güvenlik uygulamaları için bkz. [Azure sanal makinelerinde SQL Server Için güvenlik konuları](security-considerations-best-practices.md).

[Azure sanal makinelerine genel bakış ' a SQL Server](sql-server-on-azure-vm-iaas-what-is-overview.md)diğer SQL Server sanal makine makalelerini inceleyin. SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](frequently-asked-questions-faq.md) bölümüne bakın.
