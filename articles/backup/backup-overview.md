---
title: Azure Backup nedir?
description: Azure Yedekleme hizmetine ve bunun iş sürekliliği nize ve olağanüstü durum kurtarma (BCDR) stratejinize nasıl katkıda bulunur' a genel bir bakış sağlar.
ms.topic: overview
ms.date: 04/24/2019
ms.custom: mvc
ms.openlocfilehash: cf48090b2c32f0c3a1c8170873cb8d6a771fe21f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240299"
---
# <a name="what-is-the-azure-backup-service"></a>Azure Yedekleme hizmeti nedir?

Azure Backup hizmeti, Microsoft Azure bulutundaki verilerinizi yedekleyip kurtarmaya yönelik basit, güvenli ve uygun maliyetli çözümler sunar.

> [!VIDEO https://www.youtube.com/embed/elODShatt-c]

## <a name="what-can-i-back-up"></a>Neyi yedekleyebilirim?

- **Şirket içi** - [Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını](backup-support-matrix-mars-agent.md)kullanarak dosyaları, klasörleri, sistem durumunu yedekleyin. Veya şirket içi VM'leri[(Hyper-V](back-up-hyper-v-virtual-machines-mabs.md) ve [VMWare)](backup-azure-backup-server-vmware.md)ve diğer [şirket içi iş yüklerini](backup-mabs-protection-matrix.md) korumak için DPM veya Azure Yedekleme Sunucusu (MABS) aracısını kullanın
- **Azure VM'ler** - [Tüm Windows/Linux VM'lerini yedekleyin](backup-azure-vms-introduction.md) (yedekleme uzantıları nı kullanarak) veya [MARS aracısını](backup-azure-manage-mars.md)kullanarak dosyaları, klasörleri ve sistem durumunu yedekleyin.
- **Azure Files,** - [Azure Dosyası paylaşımlarını bir depolama hesabına yedekleme](backup-afs.md)
- **Azure VM'lerde** -  SQL[Server, Azure VM'lerde çalışan SQL Server veritabanlarını yedekleme](backup-azure-sql-database.md)
- **Azure VM'lerde** - çalışan Azure VM[Yedekleme SAP HANA veritabanlarındaki](backup-azure-sap-hana-database.md) SAP HANA veritabanları

![Azure Yedeklemegenel Bakış](./media/backup-overview/azure-backup-overview.png)

## <a name="why-use-azure-backup"></a>Azure Backup'ı neden kullanmalısınız?

Azure Yedekleme şu önemli avantajları sunar:

- **Şirket içi yedekleme :** Azure Yedekleme, şirket içi kaynaklarınızı buluta yedeklemek için basit bir çözüm sunar. Karmaşık şirket içi yedekleme çözümleri dağıtmaya gerek kalmadan kısa ve uzun vadeli yedekleme alın.
- **Azure IaaS VM'leri yedekleme**: Azure Yedekleme, orijinal verilerin kazara yok edilmesine karşı korunmak için bağımsız ve yalıtılmış yedeklemeler sağlar. Yedeklemeler, kurtarma noktalarının yerleşik yönetimiyle birlikte Kurtarma Hizmetleri kasasında depolanır. Yapılandırma ve ölçeklenebilirlik basittir, yedeklemeler optimize edilir ve gerektiğinde kolayca geri yükleyebilirsiniz.
- **Kolayca ölçeklendirin** - Azure Yedekleme, bakım veya izleme yükü olmadan yüksek kullanılabilirlik sağlamak için Azure bulutunun altında yatan gücü ve sınırsız ölçeği kullanır.
- **Sınırsız veri aktarımı elde edin**: Azure Yedekleme, aktardığınız gelen veya giden veri miktarını sınırlamaz veya aktarılan veriler için ücret almaz.
  - Giden veriler, geri yükleme işlemi sırasında bir Kurtarma Hizmetleri kasasından aktarılan verileri tanımlar.
  - Büyük miktarda veri almak için Azure İçe Alma/Dışa Aktarma hizmetini kullanarak çevrimdışı bir ilk yedekleme yaparsanız, gelen verilerle ilişkili bir maliyet vardır.  [Daha fazla bilgi edinin](backup-azure-backup-import-export.md).
- **Verileri güvende tutun**: Azure Yedekleme, verilerin [aktarım sırasında](backup-azure-security-feature.md) ve [yerinde](backup-azure-security-feature-cloud.md)güvenliğini sağlamak için çözümler sunar.
- **Merkezi izleme ve yönetim**: Azure Yedekleme, Kurtarma Hizmetleri kasasında [yerleşik izleme ve uyarı özellikleri](backup-azure-monitoring-built-in-monitor.md) sağlar. Bu yetenekler herhangi bir ek yönetim altyapısı olmadan kullanılabilir. Ayrıca [Azure Monitor'u kullanarak](backup-azure-monitoring-use-azuremonitor.md)izleme ve raporlama ölçeğinizi de artırabilirsiniz.
- **Uygulama tutarlı yedeklemeleri alın**: Uygulama tutarlı yedekleme, yedekleme kopyasını geri yüklemek için bir kurtarma noktasının gerekli tüm verilere sahip olduğu anlamına gelir. Azure Yedekleme, verileri geri yüklemek için ek düzeltmelerin gerekmemesini sağlayan uygulama tutarlı yedeklemeler sağlar. Uygulamayla tutarlı verilerin geri yüklenmesi, geri yükleme süresini azaltarak hizmetlerinizin kısa süre içinde çalışır hale gelmesini sağlar.
- **Kısa ve uzun vadeli verileri koruyun**: Kurtarma Hizmetleri [kasalarını](backup-azure-recovery-services-vault-overview.md) kısa ve uzun vadeli veri saklama için kullanabilirsiniz.
- **Otomatik depolama yönetimi** - Karma ortamlar genelde heterojen depolamaya (bazıları şirket içi, bazıları ise bulutta olan) ihtiyaç duyar. Azure Yedekleme ile şirket içi depolama aygıtlarını kullanmanın bir bedeli yoktur. Azure Backup, yedekleme alanını otomatik olarak ayırıp yönetir ve "kullandıkça öde" modelini kullanır. Yani sadece tükettiğiniz depolama için ödeme. Fiyatlandırma hakkında [daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/details/backup)
- **Birden çok depolama seçeneği** - Azure Yedekleme, depolama alanınızı/verilerinizi yüksek oranda kullanılabilir tutmak için iki tür çoğaltma sunar.
  - [Yerel olarak yedekli depolama (LRS),](../storage/common/storage-redundancy-lrs.md) verilerinizi bir veri merkezindeki depolama ölçeği biriminde üç kez çoğaltır (verilerinizin üç kopyasını oluşturur). Verilerin tüm kopyaları aynı bölgenin içinde yer alır. LRS, verilerinizi yerel donanım hatalarına karşı korumak için düşük maliyetli bir seçenektir.
  - [Coğrafi yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md) varsayılan ve önerilen çoğaltma seçeneğidir. GRS, verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS'nin maliyeti LRS'den daha pahalıdır, ancak GRS bölgesel bir kesinti olsa bile verileriniz için daha yüksek bir dayanıklılık düzeyi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Farklı yedekleme senaryoları için mimari ve bileşenleri [gözden geçirin.](backup-architecture.md)
- Yedekleme ve [Azure VM yedeklemesi](backup-support-matrix-iaas.md)için destek gereksinimlerini ve sınırlamalarını [doğrulayın.](backup-support-matrix.md)
