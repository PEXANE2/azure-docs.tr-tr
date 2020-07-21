---
title: Kurtarma Hizmetleri kasalarına genel bakış
description: Kurtarma Hizmetleri kasaları ve Azure Backup kasaları arasında genel bakış ve karşılaştırma.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: 11a218badfab141c41430c3f48a5e930bfa1af8b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539049"
---
# <a name="recovery-services-vaults-overview"></a>Kurtarma Hizmetleri kasalarına genel bakış

Bu makalede, bir kurtarma hizmetleri kasasının özellikleri açıklanmaktadır. Kurtarma Hizmetleri Kasası, Azure 'da veri barındıran bir depolama varlıktır. Veriler genellikle, sanal makineler (VM 'Ler), iş yükleri, sunucular veya iş istasyonları için verilerin veya yapılandırma bilgilerinin kopyalarıdır. Kurtarma Hizmetleri kasalarını, IaaS VM 'Leri (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure hizmetleri için yedekleme verilerini tutmak üzere kullanabilirsiniz. Kurtarma Hizmetleri kasaları, System Center DPM, Windows Server, Azure Backup Sunucusu ve daha fazlasını destekler. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir.

Bir Azure aboneliği içinde, her bölge için abonelik başına en çok 500 kurtarma hizmetleri Kasası oluşturabilirsiniz.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Kurtarma Hizmetleri kasalarını ve yedekleme kasalarını karşılaştırma

Hala yedekleme kasaları varsa, bunlar kurtarma hizmetleri kasalarına otomatik olarak yükseltilir. Kasım 2017 ' ye kadar tüm yedekleme kasaları kurtarma hizmetleri kasalarına yükseltildi.

Kurtarma Hizmetleri kasaları Azure Azure Resource Manager modelini temel alır, ancak yedekleme kasaları Azure Service Manager modelini temel alır. Bir yedekleme kasasını kurtarma hizmetleri kasasına yükselttiğinizde, yükseltme işlemi sırasında ve sonrasında yedekleme verileri değişmeden kalır. Kurtarma Hizmetleri kasaları, yedekleme kasaları için kullanılamayan özellikler sağlar; örneğin:

- **Yedekleme verilerini güvenli hale getirmeye yardımcı olmak Için geliştirilmiş yetenekler**: kurtarma hizmetleri kasaları ile Azure Backup bulut yedeklemelerini korumak için güvenlik özellikleri sağlar. Güvenlik özellikleri, üretim ve yedekleme sunucuları tehlikeye atılsa bile yedeklemelerinizi güvenli hale getirmek ve verileri güvenli bir şekilde kurtarmanız için de güvence altına alınır. [Daha fazla bilgi edinin](backup-azure-security-feature.md)

- **Karma BT ortamınız Için merkezi izleme**: kurtarma hizmetleri kasaları Ile yalnızca [Azure IaaS sanal](backup-azure-manage-vms.md) makinelerinizi ve ayrıca şirket [içi varlıklarınızı](backup-azure-manage-windows-server.md#manage-backup-items) merkezi bir portalda izleyebilirsiniz. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rol tabanlı Access Control (RBAC)**: RBAC, Azure 'da ayrıntılı erişim yönetimi denetimi sağlar. [Azure çeşitli yerleşik roller sağlar](../role-based-access-control/built-in-roles.md)ve Azure Backup [kurtarma noktalarını yönetmek için üç yerleşik rol](backup-rbac-rs-vault.md)içerir. Kurtarma Hizmetleri kasaları, yedekleme ve geri yükleme erişimini sınırlayan RBAC ile uyumludur. [Daha fazla bilgi edinin](backup-rbac-rs-vault.md)

- **Azure sanal makinelerinin tüm yapılandırmasını koruyun**: kurtarma hizmetleri kasaları, Premium diskler, yönetilen diskler ve şifrelenmiş VM 'ler dahil kaynak yöneticisi tabanlı VM 'leri korur. Bir yedekleme kasasını kurtarma hizmetleri kasasına yükseltmek, Service Manager tabanlı sanal makinelerinizi Kaynak Yöneticisi tabanlı sanal makinelere yükseltme olanağı sunar. Kasayı yükseltirken, Service Manager tabanlı VM kurtarma noktalarınızı koruyabilir ve yükseltilen (Kaynak Yöneticisi özellikli) VM 'Ler için korumayı yapılandırabilirsiniz. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **IaaS VM 'leri Için anında geri yükleme**: kurtarma hizmetleri kasalarını kullanarak, tüm VM 'yi geri yüklemeden dosya ve klasörleri bir IaaS VM 'sinden geri yükleyebilirsiniz ve bu sayede daha hızlı geri yükleme sürelerini mümkün olur. IaaS sanal makineleri için anında geri yükleme, hem Windows hem de Linux VM 'Leri için kullanılabilir. [Daha fazla bilgi edinin](backup-instant-restore-capability.md)

## <a name="storage-settings-in-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasındaki depolama ayarları

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri Kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Azure Backup kasa için depolamayı otomatik olarak işler. [Depolama ayarlarının nasıl değiştirilebiliyorsa](./backup-create-rs-vault.md#set-storage-redundancy)öğrenin.

Depolama artıklığı hakkında daha fazla bilgi edinmek için [coğrafi](../storage/common/storage-redundancy.md) ve [Yerel](../storage/common/storage-redundancy.md) artıklık üzerinde bu makalelere bakın.

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Portalda kurtarma hizmetleri kasalarınızı yönetme

Yedekleme hizmeti diğer Azure hizmetleriyle tümleştirilebildiğinden, Azure portal kurtarma hizmetleri kasalarının oluşturulması ve yönetilmesi kolaydır. Bu tümleştirme, *hedef hizmet bağlamında*bir kurtarma hizmetleri Kasası oluşturabileceğiniz veya yönetebileceğiniz anlamına gelir. Örneğin, bir VM 'nin kurtarma noktalarını görüntülemek için VM 'nizi seçin ve Işlemler menüsünde **Yedekle** ' ye tıklayın.

![Kurtarma Hizmetleri Kasası ayrıntıları sanal makinesi](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

VM 'nin yedeği yapılandırılmamışsa, yedeklemeyi yapılandırmanızı ister. Yedekleme yapılandırıldıysa, geri yükleme noktalarının listesi dahil olmak üzere VM hakkındaki yedekleme bilgilerini görürsünüz.  

![Kurtarma Hizmetleri Kasası ayrıntıları sanal makinesi](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

Önceki örnekte, **Contosovm** , sanal makinenin adıdır. **Contosovm-demokasa** , kurtarma hizmetleri kasasının adıdır. Kurtarma noktalarını depolayan kurtarma hizmetleri kasasının adını hatırlamak zorunda değilsiniz, bu bilgilere sanal makineden erişebilirsiniz.  

Bir kurtarma hizmetleri Kasası birden çok sunucuyu koruduğunda, kurtarma hizmetleri kasasından bakmak daha mantıklı olabilir. Abonelikteki tüm kurtarma hizmetleri kasalarını arayabilir ve listeden bir seçim yapabilirsiniz.

Aşağıdaki bölümler, her bir etkinlik türünde bir kurtarma hizmetleri kasasının nasıl kullanılacağını açıklayan makalelerin bağlantılarını içerir.

> [!NOTE]
> Kurtarma Hizmetleri Kasası, 24 saat içinde silinmişse aynı adla oluşturulamaz. Farklı bir kaynak adı kullanın veya farklı bir kaynak grubu seçin veya 24 saat sonra yeniden deneyin.

### <a name="back-up-data"></a>Verileri yedekleme

- [Azure VM 'yi yedekleme](backup-azure-vms-first-look-arm.md)
- [Windows Server veya Windows iş istasyonunu yedekleme](./backup-windows-with-mars-agent.md)
- [DPM iş yüklerini Azure 'a yedekleme](backup-azure-dpm-introduction.md)
- [Azure Backup Sunucusu kullanarak iş yüklerini yedeklemeye hazırlama](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Kurtarma noktalarını yönet

- [Azure VM yedeklemelerini yönetme](backup-azure-manage-vms.md)
- [Dosya ve klasörleri yönetme](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Verileri kasadan geri yükleme

- [Azure VM 'den tek tek dosyaları kurtarma](backup-azure-restore-files-from-vm.md)
- [Azure VM 'yi geri yükleme](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Kasadan güvenli hale getirin

- [Kurtarma Hizmetleri kasalarında bulut yedekleme verilerinin güvenliğini sağlama](backup-azure-security-feature.md)

## <a name="azure-advisor"></a>Azure Danışmanı

[Azure Danışmanı](../advisor/index.yml) , Azure 'un kullanımını iyileştirmenize yardımcı olan kişiselleştirilmiş bir bulut danışmanıdır. Azure kullanımınızı analiz eder ve dağıtımlarınızı iyileştirmek ve güvence altına almak için zamanında öneriler sağlar. Dört kategoride öneriler sağlar: yüksek kullanılabilirlik, güvenlik, performans ve maliyet.

Azure Danışmanı, yedeklenmeyen VM 'Ler için saatlik [öneriler](../advisor/advisor-high-availability-recommendations.md#protect-your-virtual-machine-data-from-accidental-deletion) sağlar, bu nedenle önemli VM 'leri yedeklemeyi hiç kaçırmayın. Ayrıca, önerileri erteleniyor göre de denetleyebilirsiniz.  Öneriyi (yedeklemelerin depolanacağı yer) ve yedekleme ilkesini (yedek kopyaların yedeklerini ve bekletilmesini) belirterek, öneriye tıklayarak sanal makinelerde yedeklemeyi etkinleştirebilirsiniz.

![Azure Danışmanı](./media/backup-azure-recovery-services-vault-overview/azure-advisor.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri kullanın:</br>
[IaaS VM 'sini yedekleme](backup-azure-arm-vms-prepare.md)</br>
[Azure Backup Sunucusu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Windows Server yedekleme](backup-windows-with-mars-agent.md)
