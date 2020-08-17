---
title: Kurtarma Hizmetleri kasalarına genel bakış
description: Kurtarma Hizmetleri kasaları ve Azure Backup kasaları arasında genel bakış ve karşılaştırma.
ms.topic: conceptual
ms.date: 08/17/2020
ms.openlocfilehash: 2b292a39e38ef5e298f45c2babbee9fbd20c39ea
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88261880"
---
# <a name="recovery-services-vaults-overview"></a>Kurtarma Hizmetleri kasalarına genel bakış

Bu makalede, bir kurtarma hizmetleri kasasının özellikleri açıklanmaktadır. Kurtarma Hizmetleri Kasası, Azure 'da veri barındıran bir depolama varlıktır. Veriler genellikle, sanal makineler (VM 'Ler), iş yükleri, sunucular veya iş istasyonları için verilerin veya yapılandırma bilgilerinin kopyalarıdır. Kurtarma Hizmetleri kasalarını, IaaS VM 'Leri (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure hizmetleri için yedekleme verilerini tutmak üzere kullanabilirsiniz. Kurtarma Hizmetleri kasaları, System Center DPM, Windows Server, Azure Backup Sunucusu ve daha fazlasını destekler. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir. Kurtarma Hizmetleri kasaları, şu gibi özellikler sağlayan Azure Azure Resource Manager modelini temel alır:

- **Yedekleme verilerini güvenli hale getirmeye yardımcı olmak Için geliştirilmiş yetenekler**: kurtarma hizmetleri kasaları ile Azure Backup bulut yedeklemelerini korumak için güvenlik özellikleri sağlar. Güvenlik özellikleri, üretim ve yedekleme sunucuları tehlikeye atılsa bile yedeklemelerinizi güvenli hale getirmek ve verileri güvenli bir şekilde kurtarmanız için de güvence altına alınır. [Daha fazla bilgi](backup-azure-security-feature.md)

- **Karma BT ortamınız Için merkezi izleme**: kurtarma hizmetleri kasaları Ile yalnızca [Azure IaaS sanal](backup-azure-manage-vms.md) makinelerinizi ve ayrıca şirket [içi varlıklarınızı](backup-azure-manage-windows-server.md#manage-backup-items) merkezi bir portalda izleyebilirsiniz. [Daha fazla bilgi](backup-azure-monitoring-built-in-monitor.md)

- **Rol tabanlı Access Control (RBAC)**: RBAC, Azure 'da ayrıntılı erişim yönetimi denetimi sağlar. [Azure çeşitli yerleşik roller sağlar](../role-based-access-control/built-in-roles.md)ve Azure Backup [kurtarma noktalarını yönetmek için üç yerleşik rol](backup-rbac-rs-vault.md)içerir. Kurtarma Hizmetleri kasaları, yedekleme ve geri yükleme erişimini sınırlayan RBAC ile uyumludur. [Daha fazla bilgi](backup-rbac-rs-vault.md)

- **Geçici silme**: kötü bir aktör bir yedeği silse (veya yedekleme verileri yanlışlıkla silinse), yedekleme verileri 14 ek gün boyunca tutulur ve bu yedekleme öğesinin veri kaybı olmadan kurtarılmasını sağlar. "Geçici silme" durumundaki yedekleme verileri için 14 günlük ek bekletme, müşteriye hiçbir ücret vermez. [Daha fazla bilgi edinin](backup-azure-security-feature-cloud.md).

- **Çapraz bölge geri yükleme**: çapraz bölge geri yükleme (CRR) Azure sanal makinelerini Azure eşlenmiş bölgesi olan ikincil bir bölgede geri yüklemenize olanak tanır. Azure birincil bölgede bir olağanüstü durum bildiriyorsa, ikincil bölgede çoğaltılan veriler, ortamları için birincil bölgedeki gerçek kalma süresi olağanüstü durumdan etkilenmeden ikincil bölgeye geri yüklenebilir. [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#cross-region-restore).

## <a name="storage-settings-in-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasındaki depolama ayarları

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri Kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

- Azure Backup kasa için depolamayı otomatik olarak işler. [Depolama ayarlarının nasıl değiştirilebiliyorsa](./backup-create-rs-vault.md#set-storage-redundancy)öğrenin.

- Depolama artıklığı hakkında daha fazla bilgi edinmek için [coğrafi](../storage/common/storage-redundancy.md) ve [Yerel](../storage/common/storage-redundancy.md) artıklık üzerinde bu makalelere bakın.

### <a name="additional-resources"></a>Ek kaynaklar

- [Kasa destekleniyor ve desteklenmeyen senaryolar](backup-support-matrix.md#vault-support)
- [Kasa hakkında sık sorulan sorular](backup-azure-backup-faq.md)

## <a name="azure-advisor"></a>Azure Danışmanı

[Azure Danışmanı](../advisor/index.yml) , Azure 'un kullanımını iyileştirmenize yardımcı olan kişiselleştirilmiş bir bulut danışmanıdır. Azure kullanımınızı analiz eder ve dağıtımlarınızı iyileştirmek ve güvence altına almak için zamanında öneriler sağlar. Dört kategoride öneriler sağlar: yüksek kullanılabilirlik, güvenlik, performans ve maliyet.

Azure Danışmanı, yedeklenmeyen VM 'Ler için saatlik [öneriler](../advisor/advisor-high-availability-recommendations.md#protect-your-virtual-machine-data-from-accidental-deletion) sağlar, bu nedenle önemli VM 'leri yedeklemeyi hiç kaçırmayın. Ayrıca, önerileri erteleniyor göre de denetleyebilirsiniz.  Kasayı (yedeklemelerin depolanacağı yer) ve yedekleme ilkesini (yedek kopyaların yedeklerini ve bekletilmesini) belirterek, öneriyi seçebilir ve sanal makinelerde yedeklemeyi etkinleştirebilirsiniz.

![Azure Danışmanı](./media/backup-azure-recovery-services-vault-overview/azure-advisor.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri kullanın:</br>
[IaaS VM 'sini yedekleme](backup-azure-arm-vms-prepare.md)</br>
[Azure Backup Sunucusu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Windows Server yedekleme](backup-windows-with-mars-agent.md)
