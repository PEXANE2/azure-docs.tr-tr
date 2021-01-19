---
title: Azure Backup’taki yenilikler
description: Azure Backup yeni özellikler hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/11/2020
ms.openlocfilehash: 62a6146990863c339917777b2624fee76ebe60d8
ms.sourcegitcommit: 9d9221ba4bfdf8d8294cf56e12344ed05be82843
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98569428"
---
# <a name="whats-new-in-azure-backup"></a>Azure Backup’taki yenilikler

Azure Backup, Azure 'daki verilerinizin korunmasını geliştiren yeni özellikleri sürekli olarak geliştirir ve serbest bırakır. Bu yeni özellikler, veri korumanızı yeni iş yükü türlerine genişletir, güvenliği artırır ve yedekleme verilerinizin kullanılabilirliğini geliştirir. Ayrıca yeni yönetim, izleme ve otomasyon özellikleri de ekler.

Yeni yayınlar hakkında daha fazla bilgi edinmek için bu sayfayı kitap halinde işaretleyerek veya [güncelleştirmelere abone](https://azure.microsoft.com/updates/?query=backup)olabilirsiniz.

## <a name="updates-summary"></a>Güncelleştirme Özeti

- Ocak 2021
  - [Azure disk yedekleme (önizlemede)](disk-backup-overview.md)
  - [Müşteri tarafından yönetilen anahtarlar kullanılarak bekleyen şifreleme artık genel kullanıma sunuldu](encryption-at-rest-with-cmk.md)
- Kasım 2020
  - [Azure dosya paylaşma (AFS) yedeklemesi için Azure Resource Manager şablonu](#azure-resource-manager-template-for-afs-backup)
  - [Azure VM 'lerinde SAP HANA veritabanları için artımlı yedeklemeler](#incremental-backups-for-sap-hana-databases)
- Eylül 2020
  - [Yedekleme Merkezi](#backup-center)
  - [PostgreSQL için Azure Veritabanı’nı Yedekleme](#backup-azure-database-for-postgresql)
  - [Seçmeli disk yedekleme ve geri yükleme](#selective-disk-backup-and-restore)
  - [Azure VM 'lerinde SQL Server ve SAP HANA veritabanları için çapraz bölge geri yükleme](#cross-region-restore-for-sql-server-and-sap-hana)
  - [En fazla 32 diski olan VM 'lerin yedeklenmesi için destek](#support-for-backup-of-vms-with-up-to-32-disks)
  - [Azure VM 'lerinde SQL için Basitleştirilmiş yedekleme yapılandırma deneyimi](#simpler-backup-configuration-for-sql-in-azure-vms)
  - [RHEL Azure sanal makinelerinde yedekleme SAP HANA](#backup-sap-hana-in-rhel-azure-virtual-machines)
  - [Yedekleme verileri için bölge yedekli depolama (ZRS)](#zone-redundant-storage-zrs-for-backup-data)
  - [Azure VM 'lerinde SQL Server ve SAP HANA iş yükleri için geçici silme](#soft-delete-for-sql-server-and-sap-hana-workloads)

## <a name="azure-disk-backup-in-preview"></a>Azure disk yedekleme (önizlemede)

Azure disk yedekleme, düzenli anlık görüntü oluşturmayı otomatikleştirerek ve yedekleme ilkesini kullanarak yapılandırılmış bir süre boyunca tutarak [Azure yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/managed-disks-overview) için anlık görüntü yaşam döngüsü yönetimi sağlayan bir anahtar oluşturma çözümü sunmaktadır. Disk anlık görüntülerini sıfır altyapı maliyetiyle ve özel betik oluşturmaya veya herhangi bir yönetim yüküne gerek kalmadan yönetebilirsiniz. Bu, günde birden çok yedekleme desteği olan [Artımlı anlık görüntüleri](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots) kullanarak yönetilen bir diskin zaman içinde yedeğini alan çökme ile tutarlı bir yedekleme çözümüdür. Ayrıca, aynı zamanda aracılı bir çözümdür ve üretim uygulaması performansını etkilemez. Hem işletim sistemi hem de veri disklerinin (paylaşılan diskler dahil), çalışmakta olan bir Azure sanal makinesine bağlı olup olmadığına bakılmaksızın yedekleme ve geri yükleme desteği sağlar.

Daha fazla bilgi için bkz. [Azure disk yedekleme (önizlemede)](disk-backup-overview.md).

## <a name="encryption-at-rest-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak bekleyen şifreleme

Müşteri tarafından yönetilen anahtarlar kullanılarak bekleyen şifreleme desteği genel kullanıma sunuldu. Bu sayede, Azure Anahtar Kasası 'nda depolanan anahtarları kullanarak kurtarma hizmetleri kasalarınızda yedekleme verilerini şifreleyebilirsiniz. Kurtarma Hizmetleri kasasındaki yedeklemeleri şifrelemek için kullanılan şifreleme anahtarı, kaynağı şifrelemek için kullanılan olanlardan farklı olabilir. Veriler, Key Vault depolanan anahtarlarınız kullanılarak korunan bir AES 256 tabanlı veri şifreleme anahtarı (DEK) kullanılarak korunur. Platform tarafından yönetilen anahtarlar kullanılarak şifrelemeye kıyasla (varsayılan olarak kullanılabilir), bu anahtarlar üzerinde daha fazla denetim sağlar ve uyumluluk gereksinimlerinizi daha iyi karşılayabilmeniz için size yardımcı olabilir.

Daha fazla bilgi için bkz. [müşteri tarafından yönetilen anahtarlar kullanılarak yedekleme verilerinin şifrelenmesi](encryption-at-rest-with-cmk.md).

## <a name="azure-resource-manager-template-for-afs-backup"></a>AFS yedeklemesi için Azure Resource Manager şablonu

Azure Backup artık Azure Resource Manager (ARM) şablonu kullanarak mevcut Azure dosya paylaşımları için yedeklemeyi yapılandırmayı destekliyor. Şablon, kurtarma hizmetleri kasası ve yedekleme ilkesi için uygun ayrıntıları belirterek mevcut bir Azure dosya paylaşımının korumasını yapılandırır. Bu, isteğe bağlı olarak yeni bir kurtarma hizmetleri kasası ve yedekleme ilkesi oluşturur ve dosya paylaşımının bulunduğu depolama hesabını kurtarma hizmetleri kasasına kaydeder.

Daha fazla bilgi için bkz. [Azure Backup için Azure Resource Manager şablonları](backup-rm-template-samples.md).

## <a name="incremental-backups-for-sap-hana-databases"></a>SAP HANA veritabanları için artımlı yedeklemeler

Azure Backup artık Azure VM 'lerinde barındırılan SAP HANA veritabanları için artımlı yedeklemeleri desteklemektedir. Bu, SAP HANA verilerinizin daha hızlı ve daha verimli yedeklemeler sağlar.

Daha fazla bilgi için bkz. [yedekleme ilkesi oluşturma sırasında kullanılabilen çeşitli seçenekler](sap-hana-faq-backup-azure-vm.md#policy) ve [SAP HANA veritabanları için yedekleme ilkesi oluşturma](tutorial-backup-sap-hana-db.md#creating-a-backup-policy).

## <a name="backup-center"></a>Yedekleme Merkezi

Azure Backup, bir merkezi konsolundan tüm yedeklemelerinizi yönetmek için yeni bir yerel yönetim özelliği etkinleştirdi. Yedekleme merkezi, Azure 'un yerel yönetim deneyimleri ile tutarlı bir şekilde ölçeklenebilir bir şekilde veri korumayı izleme, çalıştırma, yönetme ve iyileştirmenize olanak tanır.

Yedekleme merkezi ile abonelikler, konumlar, kaynak grupları, kasaların ve hatta Azure açık Thouse kullanan kiracılar arasında stoğunuzun toplu bir görünümünü alırsınız. Yedekleme merkezi aynı zamanda yedekleme, geri yükleme, ilkelerin oluşturulması veya kasaların tek bir yerden yapılandırılması gibi, yedeklemeyle ilgili etkinliklerinizi tetikleyebileceğiniz bir işlem merkezidir. Ayrıca, Azure Ilkesiyle sorunsuz tümleştirme sayesinde ortamınızı yönetebilir ve bir yedekleme perspektifinden uyumluluğu izleyebilirsiniz. Azure Backup 'e özgü, yerleşik Azure Ilkeleri de yedeklemeleri aynı ölçekte yapılandırmanıza olanak tanır.

Daha fazla bilgi için bkz. [yedekleme merkezine genel bakış](backup-center-overview.md).

## <a name="backup-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı’nı Yedekleme

Azure Backup ve Azure veritabanı Hizmetleri, Azure PostgreSQL için kurumsal sınıf bir yedekleme çözümü oluşturmak üzere bir araya geliyor (şimdi önizlemede). Artık, yedeklemelerin 10 yıla kadar bekletilmesini sağlayan müşteri denetimli bir yedekleme ilkesiyle veri koruma ve uyumluluk ihtiyaçlarınızı karşılayabilirsiniz. Bu şekilde, yedekleme ve geri yükleme işlemlerini ayrı veritabanı düzeyinde yönetmeye yönelik ayrıntılı denetime sahip olursunuz. Benzer şekilde, bir PostgreSQL sürümüne veya blob depolama alanına kolayca geri yükleyebilirsiniz.

Daha fazla bilgi için bkz. [PostgreSQL Için Azure veritabanı yedeklemesi](backup-azure-database-postgresql.md).

## <a name="selective-disk-backup-and-restore"></a>Seçmeli disk yedekleme ve geri yükleme

Azure Backup, sanal makine yedekleme çözümünü kullanarak bir VM 'deki tüm disklerin (işletim sistemi ve veri) yedeklenmesini destekler. Artık, seçmeli diskler Yedekleme ve geri yükleme işlevselliğini kullanarak bir VM 'deki veri disklerinin bir alt kümesini yedekleyebilirsiniz. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve ekonomik bir çözüm sunar. Her kurtarma noktası yalnızca yedekleme işlemine dahil edilen diskleri içerir.

Daha fazla bilgi için bkz. [Azure sanal makineleri Için seçmeli disk yedekleme ve geri yükleme](selective-disk-backup-restore.md).

## <a name="cross-region-restore-for-sql-server-and-sap-hana"></a>SQL Server ve SAP HANA için çapraz bölge geri yükleme

Bölgeler arası geri yükleme 'nin kullanıma sunulmasıyla birlikte, ortamınızda bir birincil bölgedeki gerçek kalma süresi sorunlarını azaltmak için, artık ikincil bir bölgedeki geri yüklemeleri başlatabilirsiniz. Bu, ikincil bölgenin tamamen müşterinin denetimi geri yüklemelerini sağlar. Azure Backup, bu geri yüklemeler için ikincil bölgeye çoğaltılan yedeklenen verileri kullanır.

Artık, Azure sanal makineler için çapraz bölge geri yükleme desteğinin yanı sıra, özelliği de Azure sanal makinelerinde SQL ve SAP HANA veritabanlarını geri yüklemek için genişletilmiştir.

Daha fazla bilgi için bkz. [SQL veritabanları Için çapraz bölge geri yükleme](restore-sql-database-azure-vm.md#cross-region-restore) ve [SAP HANA veritabanları Için çapraz bölge geri yükleme](sap-hana-db-restore.md#cross-region-restore).

## <a name="support-for-backup-of-vms-with-up-to-32-disks"></a>En fazla 32 diski olan VM 'lerin yedeklenmesi için destek

Bu aşamada Azure Backup VM başına 16 yönetilen disk destekliyordu. Azure Backup, VM başına 32 ile yönetilen diskin yedeklenmesini desteklemektedir.

Daha fazla bilgi için [VM depolama desteği matrisine](backup-support-matrix-iaas.md#vm-storage-support)bakın.

## <a name="simpler-backup-configuration-for-sql-in-azure-vms"></a>Azure VM 'lerinde SQL için daha basit yedekleme yapılandırması

Azure VM 'lerinde SQL Server yedeklemeleri yapılandırmak, Azure portal VM bölmesi ile tümleştirilmiş satır içi yedekleme yapılandırması sayesinde artık daha da kolaydır. Yalnızca birkaç adımda, mevcut tüm veritabanlarının yanı sıra gelecekte eklenen tüm veritabanlarını korumak için SQL Server yedeklemesini etkinleştirebilirsiniz.

Daha fazla bilgi için bkz. [VM bölmesinden bir SQL Server yedekleme](backup-sql-server-vm-from-vm-pane.md).

## <a name="backup-sap-hana-in-rhel-azure-virtual-machines"></a>RHEL Azure sanal makinelerinde yedekleme SAP HANA

Azure Backup, Azure için yerel yedekleme çözümüdür ve SAP tarafından geri sertifikalı olarak sertifikalıdır. Azure Backup, SAP HANA çalıştıran en yaygın Linux işletim sistemlerinden biri olan Red Hat Enterprise Linux (RHEL) için destek eklemiştir.

Daha fazla bilgi için [SAP HANA veritabanı yedekleme senaryosu destek matrisine](sap-hana-backup-support-matrix.md#scenario-support)bakın.

## <a name="zone-redundant-storage-zrs-for-backup-data"></a>Yedekleme verileri için bölge yedekli depolama (ZRS)

Azure depolama, değişen artıklık seçenekleriyle yüksek performans, yüksek kullanılabilirlik ve yüksek veri dayanıklılığı için harika bir denge sağlar. Azure Backup, yedeklemelerinizi yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) olarak depolamak için bu avantajları, yedekleme verilerini de genişletmenizi sağlar. Artık, bölgesel olarak yedekli depolama (ZRS) desteğiyle ilgili ek dayanıklılık seçenekleri mevcuttur.

Daha fazla bilgi için bkz. [Kurtarma Hizmetleri Kasası için depolama yedekliliği ayarlama](backup-create-rs-vault.md#set-storage-redundancy).

## <a name="soft-delete-for-sql-server-and-sap-hana-workloads"></a>SQL Server ve SAP HANA iş yükleri için geçici silme

Kötü amaçlı yazılım, fidye ve yetkisiz erişim gibi güvenlik sorunları hakkında sorunlar artıyor. Bu güvenlik sorunları hem para hem de veri bakımından maliyetli olabilir. Bu tür saldırılara karşı koruma sağlamak için Azure Backup, silme işleminden sonra bile yedekleme verilerini korumaya yardımcı olmak için güvenlik özellikleri sağlar.

Bu tür bir özellik, geçici bir DELETE. Kötü niyetli bir aktör bir yedeği silse (veya yedekleme verileri yanlışlıkla silinse), yedekleme verileri 14 ek gün boyunca tutulur ve bu yedekleme öğesinin veri kaybı olmadan kurtarılmasını sağlar. "Geçici silme" durumundaki yedekleme verileri için 14 günlük ek bekletme, size herhangi bir maliyet vermez.

Artık, Azure VM 'Leri için geçici silme desteğinin yanı sıra Azure VM 'lerinde SQL Server ve SAP HANA iş yükleri da geçici silme ile korunur.

Daha fazla bilgi için bkz. [Azure VM 'de SQL Server Için geçici silme ve Azure VM iş yükleri SAP HANA](soft-delete-sql-saphana-in-azure-vm.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Backup kılavuz ve en iyi uygulamalar](guidance-best-practices.md)
