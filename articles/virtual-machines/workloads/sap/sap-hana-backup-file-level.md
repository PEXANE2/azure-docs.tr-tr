---
title: SAP HANA Azure Yedekleme dosya düzeyinde | Microsoft Dokümanlar
description: Azure sanal makinelerinde SAP HANA için iki önemli yedekleme olasılığı vardır, bu makale dosya düzeyinde SAP HANA Azure Yedekleme'yi kapsar
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
ms.openlocfilehash: 93b67936166eb73db5e9a15db42c2c6135794108
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78271396"
---
# <a name="sap-hana-azure-backup-on-file-level"></a>DOSYA DÜZEYINDE SAP HANA Azure Yedekleme

## <a name="introduction"></a>Giriş

Bu makale, Azure [Sanal Makinelerde SAP HANA yedekleme kılavuzuna](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)ilişkin bir makaledir ve başlangıç hakkında genel bir bakış ve bilgi ve Azure Yedekleme hizmeti ve depolama anlık görüntüleri hakkında daha fazla ayrıntı sağlar. 

Azure'daki farklı VM türleri, farklı sayıda VHD'nin eklenmesine izin verir. Tam ayrıntılar [Azure'daki Linux sanal makineleri için Boyutlar'da](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)belgelenmiştir. Bu belgede belirtilen testler için, 64 bağlı veri diski sağlayan bir GS5 Azure VM kullandık. Daha büyük SAP HANA sistemleri için, büyük olasılıkla en iyi disk IO veri girişi için yazılım şeritleme ile birlikte, veri ve günlük dosyaları için önemli sayıda disk alınmış olabilir. Azure VM'lerde SAP HANA dağıtımları için önerilen disk yapılandırmaları hakkında daha fazla bilgi için [SAP HANA Azure sanal makine depolama yapılandırmaları makalesini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)okuyun. Yapılan öneriler, yerel yedeklemeler için disk alanı önerilerini de kapsAmaktadır.

Dosya düzeyinde yedekleme/geri yüklemeyi yönetmenin standart yolu, SAP HANA Studio veya SAP HANA SQL deyimleri aracılığıyla dosya tabanlı yedeklemedir. Daha fazla bilgi için [SAP HANA SQL ve Sistem Görünümleri Başvurusu](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf)makalesini okuyun.

![Bu şekil SAP HANA Studio'daki yedek menü öğesinin iletişim kutusunu gösterir](media/sap-hana-backup-file-level/backup-menue-dialog.png)

Bu şekil, SAP HANA Studio'daki yedek menü öğesinin iletişim kutusunu gösterir. Tür &quot;dosyasını seçerken,&quot; SAP HANA'nın yedekleme dosyalarını yazdığı dosya sisteminde bir yol belirtilmesi gerekir. Geri yükleme aynı şekilde çalışır.

Bu seçim basit ve yalındır gibi görünse de, bazı hususlar vardır. Azure VM'de ekilebilen veri diski sayısı sınırlaması vardır. Veritabanı ve disk iş verme gereksinimlerinin boyutuna bağlı olarak, SAP HANA yedekleme dosyalarını VM'nin dosya sistemlerinde depolama kapasitesi olmayabilir ve bu da birden çok veri diski arasında yazılım şeridi içerebilir. Bu yedekleme dosyalarını taşımak ve terabaytlarca veriyi işlerken dosya boyutu kısıtlamalarını ve performansını yönetmek için çeşitli seçenekler bu makalede daha sonra sağlanır.

Toplam kapasiteyle ilgili daha fazla özgürlük sunan bir diğer seçenek de Azure blob depolamasIdır. Tek bir blob da 1 TB ile sınırlı iken, tek bir blob konteyner toplam kapasitesi şu anda 500 TB olduğunu. Ayrıca, müşterilere maliyet avantajı olan serin &quot;&quot; blob depolama, seçmek için seçim verir. Azure Blob depolama alanına bakın: serin blob depolama yla ilgili ayrıntılar için [sıcak, serin ve arşiv erişim katmanları.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal)

Ek güvenlik için SAP HANA yedeklemelerini depolamak için coğrafi olarak çoğaltılmış bir depolama hesabı kullanın. Depolama artıklığı ve depolama çoğaltma ile ilgili ayrıntılar için [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy) artıklığına bakın.

SAP HANA yedeklemeleri için özel VHD'leri coğrafi olarak çoğaltılmış özel bir yedekleme depolama hesabına yerleyebilir. Yoksa SAP HANA yedeklemelerini coğrafi olarak çoğaltılmış bir depolama hesabına veya farklı bir bölgede bulunan bir depolama hesabına tutan VHD'leri kopyalayabilirsiniz.

## <a name="azure-blobxfer-utility-details"></a>Azure blobxfer yardımcı program ayrıntıları

Dizinleri ve dosyaları Azure depolama da depolamak için CLI veya PowerShell'i kullanmak veya [Azure SDK'larından](https://azure.microsoft.com/downloads/)birini kullanarak bir araç geliştirmek olabilir. Verileri Azure depolamasına kopyalamak için kullanıma hazır bir yardımcı program olan AzCopy de vardır. [(bkz. AzCopy Command-Line Utility ile veri aktarımı).](../../../storage/common/storage-use-azcopy.md)

Bu nedenle, blobxfer SAP HANA yedekleme dosyalarını kopyalamak için kullanılmıştır. Bu açık kaynak, üretim ortamlarında birçok müşteri tarafından kullanılan ve [GitHub](https://github.com/Azure/blobxfer)mevcuttur. Bu araç, verilerin doğrudan Azure blob depolamasına veya Azure dosya paylaşımına kopyalanmasına olanak tanır. Ayrıca, birden çok dosyaiçeren bir dizini kopyalarken md5 karma veya otomatik paralellik gibi bir dizi kullanışlı özellik de sunar.

## <a name="sap-hana-backup-performance"></a>SAP HANA yedekleme performansı
Bu bölümde, performans konuları ele alınmıştır. Azure depolamasına daha iyi iş elde etmek için sürekli bir geliştirme olduğundan, elde edilen sayılar en son durumu temsil etmeyebilir. Sonuç olarak, yapılandırmanız ve Azure bölgeniz için tek tek testler gerçekleştirmeniz gerekir.

![Bu ekran görüntüsü SAP HANA Studio SAP HANA yedekleme konsolu](media/sap-hana-backup-file-level/backup-console-hana-studio.png)

Bu ekran görüntüsü SAP HANA Studio'nun SAP HANA yedekleme konsolunu gösterir. Tek diskteki XFS dosya sistemini kullanarak HANA VM'ye bağlı tek bir Azure Standart HDD depolama diskinde 230 GB'lık bir yedekleme nin gerçekleştiriedilmesi yaklaşık 42 dakika sürdü.

![Bu ekran görüntüsü SAP HANA test VM YaST olduğunu](media/sap-hana-backup-file-level/one-backup-disk.png)

Bu ekran görüntüsü SAP HANA test VM YaST olduğunu. SAP HANA yedeklemesi için 1-TB tek diski görebilirsiniz. 230 GB yedeklemek yaklaşık 42 dakika sürdü. Buna ek olarak, beş 200 GB disk eklenmiş ve raid md0 yazılımı oluşturulmuştur ve bu beş Azure veri diskinin üzerine şeritleme de eklenmiştir.

![Beş ekli Azure standart depolama veri diski arasında şeritleme ile yazılım RAID'de aynı yedeklemeyi yineleme](media/sap-hana-backup-file-level/five-backup-disks.png)

Bağlı beş Azure standart depolama veri diski arasında şeritleme ile yazılım RAID'de aynı yedeklemeyi yinelemek, yedekleme süresini 42 dakikadan 10 dakikaya indirdi. Diskler VM önbelleğe almadan bağlıydı. Bu alıştırma, iyi yedekleme süresi elde etmek için disk yazma iş çıktısının önemini göstermektedir. En iyi performans için süreci daha da hızlandırmak için Azure Standart SSD depolama alanına veya Azure Premium Depolama'ya geçebilirsiniz. Genel olarak, Azure standart HDD depolama sı önerilmez ve yalnızca gösteri amacıyla kullanılmıştır. Öneri, üretim sistemleri için en az Azure Standart SSD depolama veya Azure Premium Depolama kullanmaktır.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>SAP HANA yedekleme dosyalarını Azure blob depolamasına kopyalama
Belirtilen performans numaraları, yedekleme süresi numaraları ve kopyalama süresi numaraları Azure teknolojisinin en son durumunu temsil etmeyebilir. Microsoft, daha fazla iş ve daha düşük gecikme süreleri sunmak için Azure depolama alanını sürekli olarak geliştiriyor. Bu nedenle sayılar yalnızca gösteri amaçlıdır. Yöntemle karar verebilmek için seçtiğiniz Azure bölgesinde bireysel ihtiyacınızı test etmeniz sizin için en iyisidir.

SAP HANA yedekleme dosyalarını hızlı bir şekilde depolamak için başka bir seçenek de Azure blob depolamadır. Tek bir blob konteyneri, yeterli SAP HANA yedeklemesini tutmak için M32ts, M32ls, M64ls ve GS5 VM Azure türlerini kullanarak SAP HANA sistemleri için yeterli olan yaklaşık 500 TB'lik bir sınıra sahiptir. Müşteriler &quot;sıcak&quot; &quot;ve soğuk&quot; leke depolama arasında seçim seçeneğine sahiptir (bkz. Azure [Blob depolama: sıcak, serin ve arşiv erişim katmanları).](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal)

Blobxfer aracı yla SAP HANA yedekleme dosyalarını doğrudan Azure blob depolama alanına kopyalamak kolaydır.

![Burada tam bir SAP HANA dosya yedekleme dosyaları görebilirsiniz](media/sap-hana-backup-file-level/list-of-backups.png)

Tam bir SAP HANA dosya yedeklemedosyalarını görebilirsiniz. Dört dosyadan en büyüğü yaklaşık 230 GB boyutuna sahip.

![230 GB'ı Azure standart depolama hesabı blob kapsayıcısına kopyalamak yaklaşık 3000 saniye sürdü](media/sap-hana-backup-file-level/copy-duration-blobxfer.png)

İlk testte md5 esrarı kullanmamak, 230 GB'ı Azure standart depolama hesabı blob kapsayıcısına kopyalamak yaklaşık 3000 saniye sürdü.

HANA Studio yedekleme konsolu bir HANA yedekleme dosyalarının maksimum dosya boyutunu kısıtlamak için izin verir. Örnek ortamda, büyük bir 230 GB dosyası yerine birden çok daha küçük yedekleme dosyasına sahip olarak performansı artırdı.

HANA tarafında yedek dosya boyutu sınırını ayarlamak, dosyalar sırayla yazıldığından yedekleme süresini&#39;. Dosya boyutu sınırı 60 GB olarak ayarlandı, bu nedenle yedekleme 230 GB tek dosya yerine dört büyük veri dosyası oluşturdu. Yedekleme hedeflerinizin blob boyutlarının dosya boyutlarında sınırlamaları varsa, birden çok yedekleme dosyası kullanmak HANA veritabanlarını yedeklemek için bir gereklilik haline gelebilir.

![Blobxfer aracının paralelliğini test etmek için HANA yedeklemelerinin maksimum dosya boyutu 15 GB olarak ayarlandı](media/sap-hana-backup-file-level/parallel-copy-multiple-backup-files.png)

Blobxfer aracının paralelliğini test etmek için HANA yedeklemelerinin maksimum dosya boyutu 15 GB olarak ayarlandı ve bu da 19 yedekleme dosyasıyla sonuçlandı. Bu yapılandırma, blobxfer'ın 230 GB'ı Azure blob depolamasına kopyalama sıiçin 3000 saniyeden 875 saniyeye kadar zaman kazandırdı.

Azure blob depolama gibi yerel disklere karşı gerçekleştirilen yedeklemeleri diğer konumlara kopyalamayı araştırırken, nihai bir paralel kopyalama işlemi tarafından kullanılan bant genişliğinin bireysel VM türünüzin ağına veya depolama kotasına göre muhasebeleştirdiğini unutmayın. Sonuç olarak, kopyanın süresini, VM'de çalışan normal iş yükünün gerektirdiği ağ ve depolama bant genişliğiyle dengelemeniz gerekir. 


## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>SAP HANA yedekleme dosyalarını NFS paylaşımına kopyalama

Microsoft Azure, [Azure NetApp Dosyaları](https://azure.microsoft.com/services/netapp/)üzerinden yerel NFS paylaşımları sunar. Yedeklemeleri depolamak ve yönetmek için farklı hacimlerde düzinelerce TB oluşturabilirsiniz. Ayrıca, NetApp'in teknolojisine dayalı olarak bu birimleri anlık görüntüleyebilirsiniz. Azure NetApp Files (ANF), farklı depolama alanı işlerinden elde edilen üç farklı hizmet düzeyinde sunulur. Daha fazla bilgi için [Azure NetApp Dosyaları için](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)makale Hizmet düzeylerini okuyun. [Quickstart: Azure NetApp Dosyalarını ayarla ve bir NFS hacmi oluşturabilirsiniz.](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-quickstart-set-up-account-create-volumes?tabs=azure-portal)

ANF aracılığıyla Azure'un yerel NFS birimlerini kullanmanın yanı sıra, Azure'da NFS paylaşımları sağlayan kendi dağıtımları oluşturmanın çeşitli olasılıkları vardır. Tüm bu çözümleri kendiniz dağıtmak ve yönetmek için gereken dezavantajı vardır. Bu olasılıklardan bazıları bu makalelerde belgelenmiştir:

- [SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- [SAP NetWeaver için Red Hat Enterprise Linux üzerinde Azure Sanal Makineler'de GlusterFS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)

Yukarıda açıklanan araçlarla oluşturulan NFS paylaşımları, HANA yedeklemelerini yerel disklere karşı gerçekleştirilen yedeklemelere karşı doğrudan yürütmek veya bu NFS paylaşımlarına kopyalamak için kullanılabilir.

> [!NOTE]
> SAP HANA destek NFS v3 ve NFS v4.x. CIFS dosya sistemi ile SMB gibi başka bir biçim karşı HANA yedekleme yazmak için desteklenmez. Ayrıca bakınız [SAP destek notu #1820529](https://launchpad.support.sap.com/#/notes/1820529)

## <a name="copy-sap-hana-backup-files-to-azure-files"></a>SAP HANA yedekleme dosyalarını Azure Dosyalarına kopyalama

Azure Dosyaları paylaşımını bir Azure Linux VM'ine monte etmek mümkündür. Linux [ile Azure Dosya depolamanasıl kullanılır](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-linux) makalesi yapılandırmanın nasıl gerçekleştirilene ilişkin ayrıntıları sağlar. Azure Dosyaları veya Azure premium dosyalarıyla ilgili sınırlamalar için [Azure Dosyaları ölçeklenebilirliği ve performans hedeflerini](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets)okuyun.

> [!NOTE]
> CIFS dosya sistemine sahip Kobİ, SAP HANA tarafından HANA yedeklemelerini karşı yazmak için desteklenmez. Ayrıca bakınız [SAP destek notu #1820529.](https://launchpad.support.sap.com/#/notes/1820529) Sonuç olarak, bu çözümü yalnızca doğrudan yerel ekli disklere karşı yürütülen bir HANA veritabanı yedeklemesinin son hedefi olarak kullanabilirsiniz
> 

Azure Premium Dosyaları'na karşı yapılan bir testte, toplam hacmi 230 GB olan 19 yedekleme dosyasını kopyalamak yaklaşık 929 saniye sürdü. Azure Premium Dosyaları'nı kullanmanın çok daha iyi olmasını bekliyoruz. Ancak, hızlı bir kopyanın ilgi alanlarını iş yükünüzün ağ bant genişliğindeki gereksinimleriyle dengelemeniz gerektiğini unutmayın. Her Azure VM türü ağ bant genişliği kotasını uyguladığından, iş yükünüz ve yedekleme dosyalarının kopyasıyla bu kotanın aralığında kalmanız gerekir.

![Bu rakam, 19 SAP HANA yedekleme dosyasını kopyalamanın yaklaşık 929 saniye sürdüğünü göstermektedir.](media/sap-hana-backup-file-level/parallel-copy-to-azure-files.png)


SAP HANA yedekleme dosyalarını Azure dosyalarında depolamak ilginç bir seçenek olabilir. Özellikle Azure Premium Dosyaları'nın gecikme gecikmesi ve iş bölümü yle.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Sanal Makinelerde SAP HANA için yedekleme kılavuzu,](sap-hana-backup-guide.md) başlarken genel bir bakış ve bilgi verir.
* Yüksek kullanılabilirlik oluşturmayı ve AZURE'da SAP HANA'nın olağanüstü durum kurtarmasını nasıl planlayabilirsiniz (büyük örnekler), Bkz. [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve Azure'da olağanüstü durum kurtarma.](hana-overview-high-availability-disaster-recovery.md)
