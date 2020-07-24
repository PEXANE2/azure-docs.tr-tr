---
title: Dosya düzeyinde Azure Backup SAP HANA | Microsoft Docs
description: Azure sanal makinelerinde SAP HANA için iki önemli yedekleme olasılıkları vardır. Bu makalede dosya düzeyinde SAP HANA Azure Backup yer almaktadır
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2020
ms.author: juergent
ms.openlocfilehash: b9d66dc4f0e2e637ac8512022336f257f5d585a9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87035749"
---
# <a name="sap-hana-azure-backup-on-file-level"></a>Dosya düzeyinde Azure Backup SAP HANA

## <a name="introduction"></a>Giriş

Bu makale, [Azure sanal makineler 'de SAP HANA Için Yedekleme kılavuzuna](./sap-hana-backup-guide.md)yönelik bir genel bakış ve bu konuda, Azure Backup hizmeti ve depolama anlık görüntüleri hakkında daha fazla bilgi ve Başlarken hakkında daha fazla ayrıntı sunan bir makaledir. 

Azure 'daki farklı VM türleri, farklı sayıda VHD 'nin eklenmiş olduğunu sağlar. Ayrıntılar, [Azure 'Da Linux sanal makineleri Için boyutlar](../../linux/sizes.md)bölümünde belgelenmiştir. Bu belgelerde başvurulan testler için, 64 bağlı veri diskine izin veren bir GS5 Azure VM 'si kullandık. Daha büyük SAP HANA sistemler için, veri ve günlük dosyaları için çok sayıda disk zaten alınmış olabilir ve muhtemelen en iyi disk GÇ işleme için yazılım şeridi oluşturma ile birlikte. Azure VM 'lerinde SAP HANA dağıtımlar için önerilen disk yapılandırmalarına ilişkin daha fazla bilgi için [Azure sanal makine depolama yapılandırması SAP HANA](./hana-vm-operations-storage.md)makalesini okuyun. Yapılan öneriler, yerel yedeklemeler için de disk alanı önerilerini de dahil.

Yedekleme/geri yükleme 'yi dosya düzeyinde yönetmenin standart yolu, SAP HANA Studio aracılığıyla veya SAP HANA SQL deyimleriyle dosya tabanlı bir yedekleme kullanmaktır. Daha fazla bilgi için [SQL ve sistem görünümleri başvurusu SAP HANA](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf)makalesini okuyun.

![Bu şekilde SAP HANA Studio 'daki yedekleme menü öğesinin iletişim kutusu gösterilmektedir](media/sap-hana-backup-file-level/backup-menue-dialog.png)

Bu şekilde SAP HANA Studio 'daki yedekleme menü öğesinin iletişim kutusu gösterilmektedir. Tür dosyası seçerken &quot; , &quot; dosyanın yedekleme dosyalarını SAP HANA yazdığı dosya sisteminde bir yol belirtmesi gerekir. Geri yükleme aynı şekilde çalışmaktadır.

Bu seçim basit ve düz ileri bir deyişle, bazı önemli noktalar vardır. Bir Azure VM 'nin iliştirilebilecek veri diski sayısı sınırlaması vardır. Veritabanının boyutuna ve birden çok veri diskinde yazılım şeritleme gerektirebilecek disk işleme gereksinimlerine bağlı olarak, SAP HANA yedekleme dosyalarını VM 'nin dosya sistemlerinde depolama kapasitesi olmayabilir. Bu yedekleme dosyalarını taşımaya yönelik çeşitli seçenekler ve terabayt veri işlerken dosya boyutu kısıtlamalarını ve performansı yönetme Bu makalede daha sonra verilmiştir.

Toplam kapasiteye ilişkin daha fazla özgürlük sunan başka bir seçenek de Azure Blob Depolama ' dır. Tek bir blob da 1 TB ile sınırlandırıldığı sürece, tek bir blob kapsayıcısının toplam kapasitesi Şu anda 500 TB 'tır. Buna ek olarak, müşterilere &quot; &quot; maliyet avantajı olan seyrek erişimli BLOB depolama alanı olarak adlandırılan bir seçenek sunar. Seyrek Erişimli BLOB depolama hakkındaki ayrıntılar için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](../../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal) .

Ek güvenlik için, SAP HANA yedeklemeleri depolamak için coğrafi olarak çoğaltılan bir depolama hesabı kullanın. Depolama artıklığı ve depolama çoğaltması hakkındaki ayrıntılar için bkz. [Azure Storage yedekliği](../../../storage/common/storage-redundancy.md) .

Bunlardan biri, coğrafi olarak çoğaltılan ayrılmış bir yedekleme depolama hesabında SAP HANA yedeklemeleri için ayrılmış VHD 'ler yerleştirebilir. Ya da bir tane, SAP HANA yedeklemelerini tutan VHD 'leri coğrafi olarak çoğaltılan bir depolama hesabına veya farklı bir bölgedeki depolama hesabına kopyalayabilir.

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer yardımcı programı ayrıntıları

Dizinleri ve dosyaları Azure depolama üzerinde depolamak için, bir tane CLı veya PowerShell kullanabilir ya da [Azure SDK 'lardan](https://azure.microsoft.com/downloads/)birini kullanarak bir araç geliştirebilirsiniz. Ayrıca Azure depolama 'ya veri kopyalamak için kullanıma yönelik kullanıma yönelik bir yardımcı program ve AzCopy de vardır. (bkz. [AzCopy komut satırı yardımcı programıyla veri aktarma](../../../storage/common/storage-use-azcopy-v10.md)).

Bu nedenle, SAP HANA yedekleme dosyalarını kopyalamak için blobxfer kullanıldı. Bu, üretim ortamlarında birçok müşteri tarafından kullanılan ve [GitHub](https://github.com/Azure/blobxfer)'da kullanılabilen açık kaynaktır. Bu araç, bir birinin verileri doğrudan Azure Blob depolama veya Azure dosya paylaşımında kopyalamasına olanak sağlar. Ayrıca, birden çok dosya içeren bir dizin kopyalanırken MD5 karma veya otomatik paralellik gibi bir dizi kullanışlı özellik sunar.

## <a name="sap-hana-backup-performance"></a>SAP HANA yedekleme performansı
Bu bölümde, performans konuları ele alınmıştır. Azure depolama 'ya daha iyi üretilen iş elde etmek için kararlı bir geliştirme olduğundan, elde edilen sayılar en son durumu temsil edebilir. Sonuç olarak, yapılandırmanız ve Azure bölgeniz için bireysel testler gerçekleştirmeniz gerekir.

![Bu ekran görüntüsü, SAP HANA Studio 'daki SAP HANA yedekleme konsoludur](media/sap-hana-backup-file-level/backup-console-hana-studio.png)

Bu ekran görüntüsünde, SAP HANA Studio 'nun SAP HANA yedekleme konsolu gösterilmektedir. Tek bir diskte XFS dosya sistemi kullanılarak HANA VM 'ye bağlı tek bir Azure Standart HDD depolama diskinde 230 GB yedeklemesini gerçekleştirmek yaklaşık 42 dakika sürer.

![Bu ekran görüntüsü SAP HANA test sanal makinesinde YaST 'dir](media/sap-hana-backup-file-level/one-backup-disk.png)

Bu ekran görüntüsü, SAP HANA test sanal makinesinde YaST 'dir. SAP HANA yedekleme için 1 TB tek diski görebilirsiniz. 230 GB 'yi yedeklemek yaklaşık 42 dakika sürer. Ayrıca, 5 200 GB diskler eklenmiş ve yazılım RAID md0, bu beş Azure veri diskinin üzerine şeritle oluşturulmuştur.

![Beş bağlantılı Azure Standart depolama veri diski üzerinde şeritlerle aynı yedeklemeyi yazılım RAID 'de yineleme](media/sap-hana-backup-file-level/five-backup-disks.png)

Beş bağlantılı Azure Standart depolama veri diski üzerinde şeritle aynı yedeklemeyi yazılım RAID 'de yineleme, yedekleme süresini 42 dakikadan sonra 10 dakikaya getirdi. Diskler VM 'ye önbelleklemeden iliştirilmedi. Bu alıştırma, iyi yedekleme zamanına ulaşmak için disk yazma aktarım hızının önemini gösterir. En iyi performansı elde etmek için Azure Standart SSD Storage veya Azure Premium Storage 'a geçebilirsiniz. Genel olarak, Azure Standart HDD depolaması önerilmez ve yalnızca tanıtım amacıyla kullanılmıştır. Öneri, en az Azure Standart SSD depolama veya üretim sistemleri için Azure Premium Depolama kullanmaktır.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>SAP HANA yedekleme dosyalarını Azure Blob depolamaya kopyalama
Belirtilen performans numaraları, yedekleme süresi numaraları ve kopyalama süresi numaraları, Azure teknolojisinin en son durumunu temsil edemeyebilir. Microsoft, daha fazla verimlilik ve daha düşük gecikme süreleri sunmak için Azure Storage 'ı artmasıyla. Bu nedenle, sayılar yalnızca tanıtım amaçlıdır. Yöntemi, sizin için en uygun yöntem olan Azure bölgesinde bireysel gereksinimlerinize göre test etmeniz gerekir.

SAP HANA yedekleme dosyalarını hızlı bir şekilde depolamak için başka bir seçenek Azure Blob Depolama ' dır. Tek bir blob kapsayıcısının 500 TB 'lik bir sınırı vardır. Bu, SAP HANA sistemleri için yeterli SAP HANA, M32ls, M64ls ve GS5 VM türlerini kullanarak yeterli yedeklemesi sağlar. Müşteriler &quot; &quot; , sık ve &quot; soğuk &quot; BLOB depolama (bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](../../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal)) arasında seçim yapmış.

Blobxfer aracı ile SAP HANA yedekleme dosyalarını doğrudan Azure Blob depolamaya kopyalamak kolaydır.

![Burada bir tam SAP HANA dosya yedeğinin dosyalarını görebilirsiniz](media/sap-hana-backup-file-level/list-of-backups.png)

Tam bir SAP HANA dosya yedeğinin dosyalarını görebilirsiniz. Dört dosyanın en büyük biri yaklaşık 230 GB boyutuna sahiptir.

![230 GB 'yi bir Azure Standart depolama hesabı blob kapsayıcısına kopyalamak yaklaşık 3000 saniye sürdü](media/sap-hana-backup-file-level/copy-duration-blobxfer.png)

Başlangıç testinde MD5 karma 'i kullanmayan, 230 GB 'yi bir Azure Standart depolama hesabı blob kapsayıcısına kopyalamak yaklaşık 3000 saniye sürdü.

HANA Studio yedekleme konsolu, bir birinin HANA yedekleme dosyalarının en büyük dosya boyutunu kısıtlayasağlar. Örnek ortamda, büyük bir 230 GB dosya yerine birden çok daha küçük yedekleme dosyası oluşturarak performansı artırmış olur.

Dosyalar sırayla yazıldığı için, HANA tarafında yedekleme dosyası boyut sınırının ayarlanması, yedekleme süresini iyileştirir&#39;t. Dosya boyutu sınırı 60 GB olarak ayarlanmıştır, bu nedenle yedekleme, 230 GB tek dosya yerine dört büyük veri dosyası oluşturdu. Yedekleme hedeflerinizin blob boyutlarının dosya boyutları üzerinde sınırlamalar varsa, birden çok yedekleme dosyası kullanmak HANA veritabanlarını yedeklemeye yönelik bir zorunludur olabilir.

![Blobxfer aracının paralelliğini test etmek için, HANA yedeklemeleri için maksimum dosya boyutu 15 GB olarak ayarlanmıştır.](media/sap-hana-backup-file-level/parallel-copy-multiple-backup-files.png)

Blobxfer aracının paralelliğini test etmek için, HANA yedeklemeleri için maksimum dosya boyutu 15 GB olarak ayarlanmıştır ve bu, 19 yedekleme dosyası ile sonuçlanır. Bu yapılandırma, blobxfer 'in 230 GB 'ı Azure Blob depolama alanına 3000 saniye ila 875 saniyeye kopyalaması için zaman getirdi.

Azure Blob depolama gibi yerel disklere karşı gerçekleştirilen yedeklemelerin kopyalanmasını araştırırken, nihai bir paralel kopyalama işlemi tarafından kullanılan bant genişliğinin, tek bir sanal makine türünün ağ veya depolama kotasına göre denetlendiğini aklınızda bulundurun. Sonuç olarak, sanal makinede çalışan normal iş yükünün gerekli olduğu ağ ve depolama bant genişliğine karşı kopyanın süresini dengeetmeniz gerekir. 


## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>SAP HANA yedekleme dosyalarını NFS paylaşımıyla Kopyala

Microsoft Azure, [Azure NetApp Files](https://azure.microsoft.com/services/netapp/)aracılığıyla yerel NFS paylaşımları sunmaktadır. Yedeklemeleri depolamak ve yönetmek için kapasiteye göre farklı miktarda TBs oluşturabilirsiniz. Ayrıca, bu birimleri NetApp 'ın teknolojisine göre anlık görüntüye de getirebilirsiniz. Azure NetApp Files (ANF), farklı depolama verimi veren üç farklı hizmet düzeyinde sunulur. Daha fazla ayrıntı için [Azure NetApp Files Için hizmet düzeyleri](../../../azure-netapp-files/azure-netapp-files-service-levels.md)makalesini okuyun. [Hızlı başlangıç: ayarlama Azure NetApp Files ve NFS birimi oluşturma](../../../azure-netapp-files/azure-netapp-files-quickstart-set-up-account-create-volumes.md?tabs=azure-portal)makalesinde açıklandığı gıbı, ANF 'den bir NFS birimi oluşturabilir ve bağlayabilirsiniz.

ANF aracılığıyla Azure 'un yerel NFS hacimlerini kullanmanın yanı sıra, Azure 'da NFS paylaşımları sağlayan dağıtımlar oluşturmak için çeşitli olanaklar vardır. Her şey, bu çözümleri kendiniz dağıtmanız ve yönetmeniz için gereken dezavantaja sahiptir. Bu olasılıkların bazıları şu makalelerde belgelenmiştir:

- [SUSE Linux Enterprise Server üzerinde Azure VM 'lerinde NFS için yüksek kullanılabilirlik](./high-availability-guide-suse-nfs.md)
- [SAP NetWeaver için Red Hat Enterprise Linux üzerinde Azure Sanal Makineler'de GlusterFS](./high-availability-guide-rhel-glusterfs.md)

Yukarıda açıklanan NFS paylaşımları, yerel disklere karşı gerçekleştirilen yedeklemeleri bu NFS paylaşımlarına karşı doğrudan yürütmek için veya ile ilgili olarak, HANA yedeklemelerini doğrudan yürütmek üzere kullanılabilir.

> [!NOTE]
> SAP HANA NFS v3 ve NFS v4. x desteği. CIFS dosya sistemiyle SMB gibi başka bir biçim de HANA yedeklemelerini yazmak için desteklenmez. Ayrıca bkz. [sap destek notuna #1820529](https://launchpad.support.sap.com/#/notes/1820529)

## <a name="copy-sap-hana-backup-files-to-azure-files"></a>SAP HANA yedekleme dosyalarını Azure dosyalarına kopyalama

Bir Azure Linux sanal makinesinde Azure dosya paylaşımının bağlanması mümkündür. [Azure dosya depolaması 'Nı Linux ile kullanma](../../../storage/files/storage-how-to-use-files-linux.md) makalesi, yapılandırmanın nasıl gerçekleştirileceği hakkında ayrıntılı bilgi sağlar. Azure dosyaları veya Azure Premium dosyaları hakkında sınırlamalar için [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../../../storage/files/storage-files-scale-targets.md)makalesini okuyun.

> [!NOTE]
> CIFS dosya sistemine sahip SMB, SAP HANA tarafından HANA yedeklemeleri yazmak için desteklenmez. Ayrıca bkz. [sap destek notuna #1820529](https://launchpad.support.sap.com/#/notes/1820529). Sonuç olarak, bu çözümü yalnızca yerel olarak eklenen disklere karşı doğrudan yürütülen bir HANA veritabanı yedeklemesinin son hedefi olarak kullanabilirsiniz.
> 

Azure Premium dosyaları yerine Azure dosyalarına karşı gerçekleştirilen bir testte, 19 adet yedekleme dosyasını Toplam 230 GB hacimiyle kopyalamak için 929 saniye içinde yer alır. Azure Premium dosyalarını kullanma süresini daha iyi bir şekilde bekliyoruz. Ancak, hızlı bir kopyanın ilgi alanlarınızı, iş yükünüzün ağ bant genişliğine sahip olduğu gereksinimlere göre dengelemenize dikkat etmeniz gerekir. Her Azure VM türü ağ bant genişliği kotasını zorladığından, iş yükünüz ve yedekleme dosyalarının kopyası ile bu kotanın içinde kalması gerekir.

![Bu şekilde, 19 SAP HANA yedekleme dosyalarını kopyalamanın yaklaşık 929 saniye sürdüğünü gösterir](media/sap-hana-backup-file-level/parallel-copy-to-azure-files.png)


SAP HANA yedekleme dosyalarını Azure dosyalarında depolama, ilginç bir seçenek olabilir. Özellikle Azure Premium dosyalarının gelişmiş gecikme süresi ve performansı.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure sanal makinelerinde SAP HANA Için Yedekleme Kılavuzu](sap-hana-backup-guide.md) , Başlarken hakkında genel bakış ve bilgiler sağlar.
* Azure 'da SAP HANA olağanüstü durum kurtarma için yüksek kullanılabilirlik ve plan (büyük örnekler) oluşturma hakkında bilgi edinmek için bkz. [Azure 'da SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
