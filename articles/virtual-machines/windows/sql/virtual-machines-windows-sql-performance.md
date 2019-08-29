---
title: Azure 'da SQL Server için performans yönergeleri | Microsoft Docs
description: Microsoft Azure VM 'lerde SQL Server performansını iyileştirmek için yönergeler sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/26/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 6a386096d8a94c240e9a00457d87d04254e02920
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102002"
---
# <a name="performance-guidelines-for-sql-server-in-azure-virtual-machines"></a>Azure sanal makineler 'de SQL Server için performans yönergeleri

## <a name="overview"></a>Genel Bakış

Bu makalede, Microsoft Azure sanal makinesindeki SQL Server performansını iyileştirmeye yönelik rehberlik sunulmaktadır. Azure sanal makinelerinde SQL Server çalıştırılırken, şirket içi sunucu ortamında SQL Server için geçerli olan veritabanı performansı ayarlama seçeneklerini kullanmaya devam etmenizi öneririz. Bununla birlikte genel buluttaki bir ilişkisel veritabanının performansı, sanal makinenin boyutu ve veri disklerinin yapılandırması gibi birçok faktöre bağlıdır.

[Azure Portal sağlanan görüntüleri SQL Server](quickstart-sql-vm-create-portal.md) genel depolama yapılandırması en iyi yöntemlerini izleyin (depolamanın nasıl yapılandırıldığı hakkında daha fazla bilgi için bkz. [SQL Server VM 'ler için depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md)). Sağlamaktan sonra, bu makalede ele alınan diğer iyileştirmeleri uygulamayı düşünün. Seçimlerinizi iş yükünüze dayandırın ve test aracılığıyla doğrulayın.

> [!TIP]
> Genellikle maliyetleri iyileştirmek ve performansı iyileştirmek arasında bir denge vardır. Bu makale, Azure VM 'lerinde SQL Server *en iyi* performansı elde etmek için odaklanır. İş yükünüz daha az güç alıyorsa, aşağıda listelenen her iyileştirme gerektirmeyebilir. Bu önerileri değerlendirdiğiniz için performans ihtiyaçlarınızı, maliyetlerinizi ve iş yükü desenlerini göz önünde bulundurun.

## <a name="quick-check-list"></a>Hızlı onay listesi

Aşağıda Azure sanal makinelerinde SQL Server en iyi performansı elde etmek için hızlı bir denetim listesi verilmiştir:

| Alan | İyileştirmeleri |
| --- | --- |
| [VM boyutu](#vm-size-guidance) | - SQL Enterprise Edition için [DS3_v2](../sizes-general.md) veya üzeri.<br/><br/> - SQL Standard ve web sürümleri için [DS2_v2](../sizes-general.md) veya üzeri. |
| [Depolama](#storage-guidance) | - [Premium SSD 'ler](../disks-types.md)kullanın. Standart depolama yalnızca geliştirme ve test için önerilir.<br/><br/> - [Depolama hesabını](../../../storage/common/storage-create-storage-account.md) ve SQL Server VM aynı bölgede saklayın.<br/><br/> * Depolama hesabında Azure [coğrafi olarak yedekli depolamayı](../../../storage/common/storage-redundancy.md) (coğrafi çoğaltma) devre dışı bırakın. |
| [Diskler](#disks-guidance) | -En az 2 [P30 disk](../disks-types.md#premium-ssd) (günlük dosyası için 1 ve tempdb dahil veri dosyaları için 1) kullanın. ~ 50.000 ıOPS gerektiren iş yükleri için bir Ultra SSD kullanmayı deneyin. <br/><br/> -Veritabanı depolama veya günlük kaydı için işletim sistemi veya geçici diskler kullanmaktan kaçının.<br/><br/> -Veri dosyalarını ve TempDB veri dosyalarını barındıran diskler üzerinde okuma önbelleğini etkinleştirin.<br/><br/> -Günlük dosyasını barındıran disk (ler) i önbelleğe almayı etkinleştirmeyin.  **Önemli**: Azure VM diskinin önbellek ayarlarını değiştirirken SQL Server hizmetini durdurun.<br/><br/> -Daha fazla GÇ üretilen işi sağlamak için birden çok Azure veri diski dizili.<br/><br/> -Belgelenmiş ayırma boyutlarıyla biçimlendirin. <br/><br/> -Görev açısından kritik SQL Server iş yükleri `D:\` için yerel SSD sürücüsüne tempdb yerleştirin (doğru VM boyutunu seçtikten sonra). [Tempdb 'yi depolamak Için SSDs kullanarak](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)blogda daha fazla bilgi.  |
| [G/Ç](#io-guidance) |-Veritabanı sayfa sıkıştırmasını etkinleştirin.<br/><br/> -Veri dosyaları için anlık dosya başlatmayı etkinleştirin.<br/><br/> -Veritabanının otomatik büyümesini sınırlayın.<br/><br/> -Veritabanının bir daha küçültmeyi devre dışı bırakın.<br/><br/> -Sistem veritabanları dahil olmak üzere tüm veritabanlarını veri disklerine taşıyın.<br/><br/> -SQL Server hata günlüğü ve izleme dosyası dizinlerini veri disklerine taşıyın.<br/><br/> -Kurulum varsayılan yedekleme ve veritabanı dosya konumları.<br/><br/> -Kilitli sayfaları etkinleştirin.<br/><br/> -SQL Server performans düzeltmeleri uygulayın. |
| [Özelliğe özgü](#feature-specific-guidance) | -Doğrudan blob depolamaya yedekleme. |

Bu iyileştirmelerin *nasıl* ve *neden* yapılacağı hakkında daha fazla bilgi için lütfen aşağıdaki bölümlerde sunulan ayrıntıları ve Kılavuzu gözden geçirin.

## <a name="vm-size-guidance"></a>VM boyut Kılavuzu

Performans duyarlı uygulamalar için aşağıdaki [sanal makine boyutlarını](../sizes.md)kullanmanız önerilir:

* **SQL Server Enterprise sürümü**: DS3_v2 veya üzeri
* **SQL Server Standard ve web sürümleri**: DS2_v2 veya üzeri

[DSv2 serisi](../sizes-general.md#dsv2-series) VM 'Ler, en iyi performans için önerilen Premium depolamayı destekler. Burada önerilen boyutlar temellerdir, ancak seçtiğiniz gerçek makine boyutu iş yükü taleplerine göre değişir. DSv2 serisi VM 'Ler, çeşitli iş yükleri için iyi olan genel amaçlı sanal makinelerdir, diğer makine boyutları ise belirli iş yükü türleri için iyileştirilmiştir. Örneğin, [d serisi](../sizes-memory.md#m-series) en büyük SQL Server iş yükleri için en yüksek vCPU sayısını ve belleği sunar. [GS serisi](../sizes-previous-gen.md#gs-series) ve [DSv2 serisi 11-15](../sizes-memory.md#dsv2-series-11-15) , büyük bellek gereksinimleri için iyileştirilmiştir. Bu serinin her ikisi de [kısıtlı çekirdek boyutlarında](../../windows/constrained-vcpu.md)kullanılabilir ve daha düşük işlem taleplerine sahip iş yükleri için para tasarrufu sağlar. [LS Serisi](../sizes-storage.md) makineler, yüksek disk aktarım hızı ve GÇ için iyileştirilmiştir. Belirli SQL Server iş yükünüzü göz önünde bulundurmanız ve bunu bir VM Serisi ve boyutu seçiminize uygulamanız önemlidir.

## <a name="storage-guidance"></a>Depolama yönergeleri

DS serisi (DSv2 serisi ve GS serisi VM 'Ler ile birlikte), [Premium SSD](../disks-types.md)'leri destekler. Tüm üretim iş yükleri için Premium SSD 'ler önerilir.

> [!WARNING]
> Standart HDD 'Ler ve SSD 'Ler, değişen gecikme süreleri ve bant genişliğine sahiptir ve yalnızca geliştirme ve test iş yükleri için önerilir. Üretim iş yükleri Premium SSD 'Ler kullanmalıdır.

Ayrıca, aktarım gecikmelerini azaltmak için Azure depolama hesabınızı SQL Server sanal makinelerinize göre aynı veri merkezinde oluşturmanızı öneririz. Bir depolama hesabı oluştururken, Coğrafi çoğaltmayı devre dışı bırakın, birden fazla diskte tutarlı bir yazma sırası garanti edilmez. Bunun yerine, iki Azure veri merkezi arasında SQL Server olağanüstü durum kurtarma teknolojisini yapılandırmayı düşünün. Daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server Için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Diskler Kılavuzu

Azure VM 'de üç ana disk türü vardır:

* **Işletim sistemi diski**: Bir Azure sanal makinesi oluşturduğunuzda platform, işletim sistemi diskinizin VM 'sine en az bir disk ( **C** sürücüsü olarak etiketlenir) ekler. Bu disk, depolamada bir Sayfa Blobu olarak depolanan bir VHD 'dir.
* **Geçici disk**: Azure sanal makineleri, geçici disk adlı başka bir disk ( **D**: sürücüsü olarak etiketlenir) içerir. Bu, düğüm üzerinde karalama alanı için kullanılabilecek bir disktir.
* **Veri diskleri**: Ayrıca, sanal makinenize veri diskleri olarak ek diskler ekleyebilirsiniz ve bunlar depolama alanında sayfa Blobları olarak depolanır.

Aşağıdaki bölümlerde bu farklı diskleri kullanmaya yönelik öneriler açıklanır.

### <a name="operating-system-disk"></a>İşletim sistemi diski

İşletim sistemi diski, işletim sisteminin çalışan bir sürümü olarak önyüklenebilir ve takabilmeniz ve **C** sürücüsü olarak ETIKETLEDIĞINIZ bir VHD 'dir.

İşletim sistemi diskinde varsayılan önbelleğe alma ilkesi **okuma/yazma**. Performans duyarlı uygulamalar için, işletim sistemi diski yerine veri disklerini kullanmanızı öneririz. Aşağıdaki veri disklerinin bölümüne bakın.

### <a name="temporary-disk"></a>Geçici disk

**D**: sürücüsü olarak etiketlenen geçici depolama sürücüsü Azure Blob depolama alanına kalıcı değil. Kullanıcı veritabanı dosyalarınızı veya Kullanıcı işlem günlüğü dosyalarını **D**: sürücüsünde saklamayın.

D serisi, Dv2-serisi ve G serisi VM 'Ler için, bu VM 'lerdeki geçici sürücü SSD tabanlıdır. İş yükünüz TempDB (geçici nesneler veya karmaşık birleşimler gibi) üzerinde yoğun bir şekilde kullanıyorsa, TempDB 'yi **D** sürücüsünde depolamak daha yüksek tempdb verimlilik ve daha düşük tempdb gecikme süresine neden olabilir. Örnek senaryo için aşağıdaki blog gönderisine ilişkin TempDB tartışmasına bakın: [Azure VM 'de SQL Server Için depolama yapılandırması yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm).

Premium SSD 'Leri (DS serisi, DSv2-serisi ve GS serisi) destekleyen VM 'Ler için, TempDB 'yi okuma önbelleği etkinken Premium SSD 'leri destekleyen bir diskte depolamanızı öneririz.

Bu önerinin bir istisnası vardır: _tempdb kullanımınız yazma yoğunluklu ise, tempdb 'yi yerel **D** sürücüsünde depolayarak bu makine boyutlarına bağlı olarak daha yüksek performans elde edebilirsiniz._ Daha fazla bilgi için [tempdb Ile SSD](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/) blogunu kullanma konusunu gözden geçirin. 

### <a name="data-disks"></a>Veri diskleri

* Veri **ve günlük dosyaları için veri disklerini kullanın**: Disk şeritleme kullanmıyorsanız, bir diskin günlük dosyalarını ve diğeri de verileri ve TempDB 'yi (yukarıda belirtilen görev açısından kritik ve yazma ağır iş yükleri dışında) içeren iki Premium SSD P30 diski kullanın. Her Premium SSD,, makalesinde gösterildiği gibi, boyutuna bağlı olarak bir dizi IOPS ve bant genişliği (MB/s) sağlar. [](../disks-types.md) Depolama alanları gibi bir disk şeritleme tekniği kullanıyorsanız, biri günlük dosyası (ler) ve diğer veri dosyaları için olmak üzere iki havuza sahip olacak şekilde en iyi performansa ulaşmanız gerekir. Ancak SQL Server yük devretme kümesi örnekleri (FCı) kullanmayı planlıyorsanız, bir havuz yapılandırmanız gerekir.

   > [!TIP]
   > - Çeşitli disk ve iş yükü yapılandırmalarında test sonuçları için aşağıdaki blog gönderisine bakın: [Azure VM 'de SQL Server Için depolama yapılandırması yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/).
   > - ~ 50.000 ıOPS gerektiren SQL Server 'Lar için görev açısından kritik performans için, 10-P30 disklerini bir Ultra SSD değiştirmeyi düşünün. Daha fazla bilgi için aşağıdaki blog gönderisine bakın: [Ultra SSD Ile görev açısından kritik performans](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm/).

   > [!NOTE]
   > Portalda bir SQL Server VM sağladığınızda, depolama yapılandırmanızı Düzenle seçeneğiniz vardır. Azure, yapılandırmanıza bağlı olarak bir veya daha fazla disk yapılandırır. Birden çok disk, dizme ile tek bir depolama havuzunda birleştirilir. Hem veri hem de günlük dosyaları, bu yapılandırmada birlikte bulunur. Daha fazla bilgi için bkz. [SQL Server VM 'ler Için depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md).

* **Disk şeridi**: Daha fazla verimlilik için, ek veri diskleri ekleyebilir ve disk şeritleme kullanabilirsiniz. Veri disklerinin sayısını öğrenmek için, günlük dosyanız için gereken ıOPS ve bant genişliği sayısını ve verileriniz ile TempDB dosyalarını analiz etmeniz gerekir. Farklı VM boyutlarının, IOPS sayısı ve desteklenen bant genişliği için farklı sınırlara sahip olduğuna dikkat edin. [VM boyutu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)başına IOPS üzerindeki tablolara bakın. Aşağıdaki yönergeleri kullanın:

  * Windows 8/Windows Server 2012 veya üzeri için, [depolama alanlarını](https://technet.microsoft.com/library/hh831739.aspx) aşağıdaki yönergelerle kullanın:

      1. Bölüm hatalı hizalaması nedeniyle performans etkisini önlemek için, veri ambarı iş yükleri için Interleave (Stripe boyutu) 64 KB (65536 262144 256 bayt) olarak ayarlayın. Bunun PowerShell ile ayarlanması gerekir.
      2. Sütun sayısını ayarla = fiziksel disk sayısı. 8 ' den fazla disk yapılandırırken PowerShell kullanın (Sunucu Yöneticisi Kullanıcı arabirimi). 

    Örneğin, aşağıdaki PowerShell ayırma boyutu 64 KB ve sütun sayısı 2 ile yeni bir depolama havuzu oluşturur:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Windows 2008 R2 veya önceki sürümlerde, dinamik diskleri (işletim sistemi şeritli birimler) kullanabilir ve şerit boyutu her zaman 64 KB 'tır. Bu seçeneğin Windows 8/Windows Server 2012 itibarıyla kullanım dışı olduğunu unutmayın. Daha fazla bilgi için bkz. [sanal disk hizmetindeki Destek bildirisi Windows Storage YÖNETIM API 'sine geçiyor](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * [SQL Server yük devretme kümesi örnekleriyle](virtual-machines-windows-portal-sql-create-failover-cluster.md) [depolama alanları doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) kullanıyorsanız, tek bir havuz yapılandırmanız gerekir. Bu tek havuzda farklı birimler oluşturulabilse de, bunların hepsi aynı önbelleğe alma ilkesi gibi aynı özellikleri paylaşabildiğine unutmayın.

  * Yük beklentilerinize göre depolama havuzunuzun ilişkili disk sayısını belirleme. Farklı VM boyutlarının farklı sayıda ekli veri diskine izin verdiğini aklınızda bulundurun. Daha fazla bilgi için bkz. [sanal makineler Için boyutlar](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Premium SSD 'leri (geliştirme/test senaryoları) kullanmıyorsanız, [sanal makine boyutunuzu](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) desteklediği en fazla veri diski sayısını eklemek ve disk şeritleme kullanılması önerilir.

* **Önbelleğe alma ilkesi**: Depolama yapılandırmanıza bağlı olarak, ilkeyi önbelleğe almak için aşağıdaki önerilere göz önünde edin.

  * Veri ve günlük dosyaları için ayrı diskler kullanıyorsanız, veri dosyalarınızı ve TempDB veri dosyalarını barındıran veri disklerinde okuma önbelleğini etkinleştirin. Bu, önemli bir performans avantajı oluşmasına neden olabilir. Günlük dosyasını tutan diskte önbelleğe alma özelliğini etkinleştirmeyin çünkü bu, performansın küçük bir düşüşine neden olur.

  * Tek bir depolama havuzunda disk şeridi kullanıyorsanız, çoğu iş yükü okuma önbelleklemesi avantajına sahip olur. Günlük ve veri dosyaları için ayrı depolama havuzlarınız varsa, yalnızca veri dosyaları için depolama havuzunda okuma önbelleğini etkinleştirin. Bazı ağır yazma iş yükleri için, önbelleğe alma olmadan daha iyi performans elde edilebilir. Bu, yalnızca test aracılığıyla belirlenebilir.

  * Önceki öneriler Premium SSD 'Ler için geçerlidir. Premium SSD kullanmıyorsanız, herhangi bir veri diskinde önbelleğe alma etkinleştirmeyin.

  * Disk önbelleğe almayı yapılandırma yönergeleri için aşağıdaki makalelere bakın. Klasik (ASM) dağıtım modeli için bkz: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ve [set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Azure Resource Manager dağıtım modeli için bkz: [Set-AzOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) ve [set-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdatadisk).

     > [!WARNING]
     > Herhangi bir veritabanı bozulması olasılığını ortadan kaldırmak için Azure VM disklerinin önbellek ayarını değiştirirken SQL Server hizmetini durdurun.

* **NTFS ayırma birimi boyutu**: Veri diski biçimlendirilirken, veri ve günlük dosyaları ve TempDB için 64 KB 'lik bir ayırma birimi boyutu kullanmanız önerilir.

* **Disk yönetimi en iyi uygulamaları**: Bir veri diski kaldırılırken veya önbellek türünü değiştirirken, değişiklik sırasında SQL Server hizmetini durdurun. Önbelleğe alma ayarları işletim sistemi diskinde değiştirildiğinde, Azure VM 'yi sonlandırır, önbellek türünü değiştirir ve VM 'yi yeniden başlatır. Bir veri diskinin önbellek ayarları değiştirildiğinde, VM durdurulmaz, ancak veri diski değişiklik sırasında VM 'den ayrılır ve yeniden eklenir.

  > [!WARNING]
  > Bu işlemler sırasında SQL Server hizmeti durdurulamaması veritabanının bozulmasına neden olabilir.


## <a name="io-guidance"></a>G/ç Kılavuzu

* Uygulama ve isteklerinizi paralel hale getirmek yaparken Premium SSD 'Lerin en iyi sonuçları elde edilir. Premium SSD 'ler, GÇ sırası derinliğinin 1 ' den büyük olduğu senaryolar için tasarlanmıştır, bu nedenle tek iş parçacıklı seri istekler için çok az veya hiç performans kazancı (depolama alanı yoğun olsalar bile) görürsünüz. Örneğin, bu, SQBIR gibi performans analizi araçlarının tek iş parçacıklı test sonuçlarını etkileyebilir.

* G/ç yoğunluklu iş yüklerinin performansını artırmaya yardımcı olması için [veritabanı sayfa sıkıştırmasını](https://msdn.microsoft.com/library/cc280449.aspx) kullanmayı göz önünde bulundurun. Ancak, veri sıkıştırması, veritabanı sunucusundaki CPU tüketimini artırabilir.

* İlk dosya ayırma için gereken süreyi azaltmak üzere anlık dosya başlatmayı etkinleştirmeyi düşünün. Anlık dosya başlatmasının avantajlarından yararlanabilmek için, SE_MANAGE_VOLUME_NAME ile SQL Server (MSSQLSERVER) hizmet hesabına izin verirsiniz ve **Birim bakımı görevleri** güvenlik ilkesine eklersiniz. Azure için SQL Server platform görüntüsü kullanıyorsanız, varsayılan hizmet hesabı (NT Service\MSSQLSERVER) **toplu bakım görevleri** oluştur güvenlik ilkesine eklenmez. Diğer bir deyişle, bir Azure platform görüntüsünde SQL Server anlık dosya başlatma etkin değildir. SQL Server hizmet hesabını **toplu bakım görevleri Için gerçekleştir** güvenlik ilkesine ekledikten sonra, SQL Server hizmetini yeniden başlatın. Bu özelliğin kullanımıyla ilgili güvenlik konuları olabilir. Daha fazla bilgi için bkz. [veritabanı dosyası başlatma](https://msdn.microsoft.com/library/ms175935.aspx).

* **otomatik büyüme** beklenmeyen büyüme için yalnızca bir yedek olarak kabul edilir. Verilerinizi ve günlük büyümesini otomatik büyüme ile günde bir güne göre yönetmeyin. Otomatik büyüme kullanılıyorsa, boyut anahtarını kullanarak dosyayı önceden büyütün.

* Performansı olumsuz etkileyebilecek gereksiz ek yükü önlemek için, **oto küçültme** 'nın devre dışı bırakıldığından emin olun.

* Tüm veritabanlarını sistem veritabanları dahil olmak üzere veri disklerine taşıyın. Daha fazla bilgi için bkz. [sistem veritabanlarını taşıma](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server hata günlüğünü ve izleme dosyası dizinlerini veri disklerine taşıyın. Bu, SQL Server örneğinize sağ tıklayıp Özellikler ' i seçerek SQL Server Yapılandırma Yöneticisi yapılabilir. Hata günlüğü ve izleme dosyası ayarları **Başlangıç parametreleri** sekmesinde değiştirilebilir. Döküm dizini **Gelişmiş** sekmesinde belirtilir. Aşağıdaki ekran görüntüsünde hata günlüğü başlangıç parametresinin nerede aranacağı gösterilmektedir.

    ![SQL hata günlüğü ekran görüntüsü](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Varsayılan yedekleme ve veritabanı dosyası konumlarını ayarlayın. Bu makaledeki önerileri kullanın ve sunucu özellikleri penceresinde değişiklikleri yapın. Yönergeler için bkz. [veri ve günlük dosyaları Için varsayılan konumları görüntüleme veya değiştirme (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Aşağıdaki ekran görüntüsünde, bu değişiklikleri nerede yapacağınız gösterilmektedir.

    ![SQL veri günlüğü ve yedekleme dosyaları](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* GÇ ve tüm disk belleği etkinliklerini azaltmak için kilitli sayfaları etkinleştirin. Daha fazla bilgi için bkz. [bellek Içinde sayfaları kilitleme seçeneği (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* SQL Server 2012 çalıştırıyorsanız, Service Pack 1 toplu güncelleştirme 10 ' u yükleyebilirsiniz. Bu güncelleştirme, SQL Server 2012 ' de geçici tablo seçme ekstresini çalıştırdığınızda g/ç 'de düşük performans düzeltmesini içerir. Bilgi için bu [Bilgi Bankası makalesine](https://support.microsoft.com/kb/2958012)bakın.

* Azure 'da/dışına aktarma yaparken herhangi bir veri dosyasını sıkıştırmayı göz önünde bulundurun.

## <a name="feature-specific-guidance"></a>Özelliğe özgü rehberlik

Bazı dağıtımlar, daha gelişmiş yapılandırma teknikleri kullanarak ek performans avantajları elde edebilir. Aşağıdaki listede, daha iyi performans elde etmenize yardımcı olabilecek bazı SQL Server özellikler vurgulanmıştır:

### <a name="backup-to-azure-storage"></a>Azure depolama 'ya yedekleme
Azure sanal makinelerde çalışan SQL Server için yedeklemeler gerçekleştirirken, [URL 'ye SQL Server yedekleme](https://msdn.microsoft.com/library/dn435916.aspx)kullanabilirsiniz. Bu özellik SQL Server 2012 SP1 CU2 UYGULAMAZSANıZ ile başlayarak kullanılabilir ve ekli veri disklerine yedekleme için önerilir. Azure depolama 'ya yedekleme/geri yükleme yaptığınızda, [URL En Iyi uygulamaları ve sorun giderme ve Azure depolama 'Da depolanan yedeklemelerden geri yükleme SQL Server için](https://msdn.microsoft.com/library/jj919149.aspx)belirtilen önerileri izleyin. Ayrıca, [Azure sanal makinelerinde SQL Server Için Otomatik yedeklemeyi](virtual-machines-windows-sql-automated-backup.md)kullanarak bu yedeklemeleri otomatikleştirebilir.

SQL Server 2012 ' dan önce [Azure 'da SQL Server yedekleme](https://www.microsoft.com/download/details.aspx?id=40740)'yi kullanabilirsiniz. Bu araç, birden fazla yedek Stripe hedefi kullanarak yedekleme aktarım hızını artırmaya yardımcı olabilir.

### <a name="sql-server-data-files-in-azure"></a>Azure 'da veri dosyaları SQL Server

[Azure 'daki SQL Server veri dosyaları](https://msdn.microsoft.com/library/dn385720.aspx)olan bu yeni özellik, SQL Server 2014 ' den başlayarak kullanılabilir. Azure 'daki veri dosyalarıyla SQL Server çalıştırmak, Azure veri disklerini kullanarak karşılaştırılabilir performans özelliklerini gösterir.

### <a name="failover-cluster-instance-and-storage-spaces"></a>Yük devretme kümesi örneği ve depolama alanları

Depolama alanları 'nı kullanıyorsanız, **onay** sayfasındaki kümeye düğüm eklerken, **tüm uygun depolamayı kümeye ekle**etiketli onay kutusunu temizleyin. 

![Uygun depolamanın işaretini kaldır](media/virtual-machines-windows-sql-performance/uncheck-eligible-cluster-storage.png)

Depolama alanları 'nı kullanıyorsanız ve **tüm uygun depolama alanını kümeye ekle**seçeneğinin işaretini kaldırırsanız, Windows, kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları kümeden kaldırılana ve PowerShell kullanılarak yeniden iliştirene kadar disk Yöneticisi veya Explorer 'da görünmez. Depolama alanları, depolama havuzlarında birden çok diski gruplandırır. Daha fazla bilgi için bkz. [depolama alanları](/windows-server/storage/storage-spaces/overview).

## <a name="next-steps"></a>Sonraki adımlar

Depolama ve performans hakkında daha fazla bilgi için bkz. [Azure VM 'de SQL Server Için depolama yapılandırma yönergeleri](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/09/25/storage-configuration-guidelines-for-sql-server-on-azure-vm/)

En iyi güvenlik uygulamaları için bkz. [Azure sanal makinelerinde SQL Server Için güvenlik konuları](virtual-machines-windows-sql-security.md).

[Azure sanal makinelerine genel bakış ' a SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)diğer SQL Server sanal makine makalelerini inceleyin. SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.
