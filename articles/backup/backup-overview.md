---
title: Azure Backup nedir?
description: Azure Backup hizmetine genel bir bakış sağlar ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur.
ms.topic: overview
ms.date: 04/24/2019
ms.custom: mvc
ms.openlocfilehash: 9954c8fa6affca7d2c2b73d7176280587d749476
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89017898"
---
# <a name="what-is-the-azure-backup-service"></a>Azure Backup hizmeti nedir?

Azure Backup hizmeti, Microsoft Azure bulutundaki verilerinizi yedekleyip kurtarmaya yönelik basit, güvenli ve uygun maliyetli çözümler sunar.

> [!VIDEO https://www.youtube.com/embed/elODShatt-c]

## <a name="what-can-i-back-up"></a>Neleri yedekleyebilirim?

- **Şirket içi** - [Microsoft Azure kurtarma hizmetleri (mars) Aracısı](backup-support-matrix-mars-agent.md)'nı kullanarak dosyaları, klasörleri, sistem durumunu yedekleyin. Ya da şirket içi VM 'Leri ([Hyper-V](back-up-hyper-v-virtual-machines-mabs.md) ve [VMware](backup-azure-backup-server-vmware.md)) ve diğer [Şirket ıçı iş yüklerini](backup-mabs-protection-matrix.md) korumak için DPM veya Azure Backup sunucusu (mabs) aracısını kullanın
- **Azure VM 'leri**  -  [Mars Aracısı](backup-azure-manage-mars.md)'Nı kullanarak [Windows/Linux VM 'lerinin tamamını yedekleyin](backup-azure-vms-introduction.md) (yedekleme uzantıları kullanarak) veya dosyaları, klasörleri ve sistem durumunu yedekleyin.
- **Azure dosya paylaşımları**  -  [Azure dosya paylaşımlarını bir depolama hesabına yedekleme](backup-afs.md)
- **Azure VM**  -   'lerinde SQL Server [Azure VM 'lerinde çalışan SQL Server veritabanları yedekleme](backup-azure-sql-database.md)
- **Azure VM**  -  'lerinde SAP HANA veritabanları [Azure VM 'lerinde çalışan SAP HANA veritabanları yedekleme](backup-azure-sap-hana-database.md)

![Azure Backup genel bakış](./media/backup-overview/azure-backup-overview.png)

## <a name="why-use-azure-backup"></a>Azure Backup'ı neden kullanmalısınız?

Azure Backup, bu önemli avantajları sunar:

- **Şirket içi yedeklemeyi boşaltma**: Azure Backup şirket içi kaynaklarınızı buluta yedeklemeye yönelik basit bir çözüm sunar. Karmaşık şirket içi yedekleme çözümlerini dağıtmaya gerek olmadan kısa ve uzun süreli yedekleme yapın.
- **Azure IaaS VM 'Lerini yedekleme**: Azure Backup özgün verilerin yanlışlıkla yok edilmesiyle karşı koruma sağlamak için bağımsız ve yalıtılmış yedeklemeler sağlar. Yedekler, yerleşik kurtarma noktası yönetim özelliklerine sahip Kurtarma Hizmetleri kasasında depolanır. Yapılandırma ve ölçeklenebilirlik basittir, yedeklemeler en iyi duruma getirilir ve gerektiğinde kolayca geri yükleyebilirsiniz.
- **Kolayca ölçeklendirin** Azure Backup, bakım veya izleme ek yükü olmadan yüksek kullanılabilirlik sağlamak için Azure bulutunun temel alınan gücünü ve sınırsız ölçeğini kullanır.
- **Sınırsız veri aktarımı alın**: Azure Backup, aktardığınız gelen veya giden verilerin miktarını sınırlamaz veya aktarılan veriler için ücret alınmaz.
  - Giden veriler, geri yükleme işlemi sırasında bir Kurtarma Hizmetleri kasasından aktarılan verileri tanımlar.
  - Azure Içeri/dışarı aktarma hizmetini kullanarak büyük miktarda veriyi içeri aktarmaya yönelik çevrimdışı bir ilk yedekleme gerçekleştirirseniz, gelen verilerle ilişkili bir maliyet vardır.  [Daha fazla bilgi edinin](backup-azure-backup-import-export.md).
- **Verileri güvende tutun**: Azure Backup [yoldaki](backup-azure-security-feature.md) ve [bekleyen](backup-azure-security-feature-cloud.md)verilerin güvenliğini sağlamak için çözümler sağlar.
- **Merkezi izleme ve yönetim**: Azure Backup, bir kurtarma hizmetleri kasasında [yerleşik izleme ve uyarı özellikleri](backup-azure-monitoring-built-in-monitor.md) sağlar. Bu özellikler ek yönetim altyapısı olmadan kullanılabilir. Ayrıca, [Azure izleyici 'yi kullanarak](backup-azure-monitoring-use-azuremonitor.md)izlemenin ve raporlamanın ölçeğini de artırabilirsiniz.
- **Uygulamayla tutarlı yedeklemeler al**: uygulamayla tutarlı bir yedekleme, bir kurtarma noktasının yedek kopyayı geri yüklemek için gerekli tüm verilere sahip olduğu anlamına gelir. Azure Backup, verileri geri yüklemek için ek düzeltmelerin gerekli olmamasını sağlayan uygulamayla tutarlı yedeklemeler sağlar. Uygulamayla tutarlı verilerin geri yüklenmesi, geri yükleme süresini azaltarak hizmetlerinizin kısa süre içinde çalışır hale gelmesini sağlar.
- **Kısa ve uzun süreli verileri koruyun**: kısa süreli ve uzun süreli veri saklama Için [Kurtarma Hizmetleri kasalarını](backup-azure-recovery-services-vault-overview.md) kullanabilirsiniz.
- **Otomatik depolama yönetimi** - Karma ortamlar genelde heterojen depolamaya (bazıları şirket içi, bazıları ise bulutta olan) ihtiyaç duyar. Azure Backup, şirket içi depolama cihazlarını kullanma maliyeti yoktur. Azure Backup, yedekleme alanını otomatik olarak ayırıp yönetir ve "kullandıkça öde" modelini kullanır. Böylece yalnızca kullandığınız depolama alanı için ödeme yaparsınız. Fiyatlandırma hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/backup) .
- **Birden çok depolama seçeneği** Azure Backup depolama/verilerinizi yüksek oranda kullanılabilir tutmak için iki tür çoğaltma sunar.
  - [Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md) verilerinizi üç kez çoğaltır (verilerinizin üç kopyasını oluşturur) bir veri merkezinde bulunan bir depolama ölçek birimi. Verilerin tüm kopyaları aynı bölgenin içinde yer alır. LRS, verilerinizi yerel donanım hatalarına karşı korumak için düşük maliyetli bir seçenektir.
  - [Coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy.md) , varsayılan ve önerilen çoğaltma seçeneğidir. GRS, verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS 'nin maliyeti LRS 'den fazladır, ancak bölgesel bir kesinti olsa bile, verileriniz için GRS daha yüksek düzeyde dayanıklılık sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Farklı yedekleme senaryolarına yönelik mimari ve bileşenleri [gözden geçirin](backup-architecture.md) .
- Yedekleme ve [Azure VM yedeklemesi](backup-support-matrix-iaas.md)için destek gereksinimlerini ve sınırlamalarını [doğrulayın](backup-support-matrix.md) .
