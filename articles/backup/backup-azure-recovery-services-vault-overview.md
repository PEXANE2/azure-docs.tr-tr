---
title: Kurtarma Hizmetleri kasalarına genel bakış
description: Kurtarma Hizmetleri kasaları ile Azure Yedekleme kasaları arasında genel bir bakış ve karşılaştırma.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: 94a3e5a0865bcc8c0a9ecb866ca013f20a558e1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673231"
---
# <a name="recovery-services-vaults-overview"></a>Kurtarma Hizmetleri kasalarına genel bakış

Bu makalede, Kurtarma Hizmetleri kasasının özellikleri açıklanmaktadır. Kurtarma Hizmetleri kasası, Azure'da verileri barındıran bir depolama kuruluşudur. Veriler genellikle sanal makineler (VM'ler), iş yükleri, sunucular veya iş istasyonları için veri kopyaları veya yapılandırma bilgileridir. IaaS VM'ler (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure hizmetleri için yedekleme verilerini tutmak için Kurtarma Hizmetleri kasalarını kullanabilirsiniz. Kurtarma Hizmetleri kasaları System Center DPM, Windows Server, Azure Backup Server ve daha fazlasını destekler. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir.

Azure aboneliğinde, bölge başına abonelik başına en fazla 500 Kurtarma Hizmeti kasası oluşturabilirsiniz.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Kurtarma Hizmetleri kasaları ve Yedekleme kasalarını karşılaştırma

Yedekleme kasalarınız hala varsa, kurtarma hizmetleri kasalarına otomatik olarak yükseltiliyorlar. Kasım 2017'ye kadar, tüm Yedekleme kasaları Kurtarma Hizmetleri kasalarına yükseltildi.

Kurtarma Hizmetleri kasaları Azure'un Azure Kaynak Yöneticisi modeline, Yedekleme kasaları ise Azure Servis Yöneticisi modeline dayanır. Yedekleme kasasını Kurtarma Hizmetleri kasasına yükselttiğinde, yedekleme verileri yükseltme işlemi sırasında ve sonrasında bozulmadan kalır. Kurtarma Hizmetleri kasaları, Yedekleme kasaları için mevcut olmayan özellikler sağlar:

- **Yedekleme verilerinin güvenliğinin sağlanmasına yardımcı olacak gelişmiş özellikler**: Kurtarma Hizmetleri kasaları ile Azure Yedekleme, bulut yedeklemelerini korumak için güvenlik özellikleri sağlar. Güvenlik özellikleri, üretim ve yedekleme sunucuları tehlikeye atılsa bile yedeklemelerinizi güvenli hale getirebilir ve verileri güvenli bir şekilde kurtarabilirsiniz. [Daha fazlasını öğrenin](backup-azure-security-feature.md)

- **Karma BT ortamınız için merkezi izleme**: Kurtarma Hizmetleri kasaları ile yalnızca [Azure IaaS VM'lerinizi](backup-azure-manage-vms.md) değil, [şirket içi varlıklarınızı](backup-azure-manage-windows-server.md#manage-backup-items) da merkezi bir portaldan izleyebilirsiniz. [Daha fazlasını öğrenin](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Role-Based Access Control (RBAC) : RBAC,** Azure'da ince taneli erişim yönetimi denetimi sağlar. [Azure çeşitli yerleşik roller sağlar](../role-based-access-control/built-in-roles.md)ve Azure Yedekleme'nin kurtarma [noktalarını yönetmek için](backup-rbac-rs-vault.md)üç yerleşik rolü vardır. Kurtarma Hizmetleri kasaları, yedeklemeyi kısıtlayan ve tanımlanan kullanıcı rolleri kümesine erişimi geri yükleyen RBAC ile uyumludur. [Daha fazlasını öğrenin](backup-rbac-rs-vault.md)

- **Azure Sanal Makinelerin tüm yapılandırmalarını koruyun**: Kurtarma Hizmetleri kasaları, Premium Diskler, Yönetilen Diskler ve Şifreli SANAL'lar dahil olmak üzere Kaynak Yöneticisi tabanlı Sanal M'leri korur. Yedekleme kasasını Kurtarma Hizmetleri kasasına yükseltmek, Servis Yöneticisi tabanlı VM'lerinizi Kaynak Yöneticisi tabanlı VM'lere yükseltme fırsatı verir. Kasayı yükseltirken, Servis Yöneticisi tabanlı VM kurtarma noktalarınızı koruyabilir ve yükseltilen (Kaynak Yöneticisi etkin) VM'ler için korumayı yapılandırabilirsiniz. [Daha fazlasını öğrenin](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **IaaS VM'ler için anında geri yükleme**: Kurtarma Hizmetleri kasalarını kullanarak, tüm VM'yi geri yüklemeden dosyaları ve klasörleri geri yükleyebilirsiniz, bu da daha hızlı geri yükleme süreleri sağlar. IaaS VM'ler için anında geri yükleme hem Windows hem de Linux VM'leri için kullanılabilir. [Daha fazlasını öğrenin](backup-instant-restore-capability.md)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Kurtarma Hizmetleri kasalarınızı portalda yönetme

Yedekleme hizmeti diğer Azure hizmetlerine entegre olduğundan, Azure portalında Kurtarma Hizmetleri kasalarının oluşturulması ve yönetilmesi kolaydır. Bu tümleştirme, hedef hizmet bağlamında bir Kurtarma Hizmetleri *kasası*oluşturabileceğiniz veya yönetebileceğiniz anlamına gelir. Örneğin, bir VM'nin kurtarma noktalarını görüntülemek için VM'nizi seçin ve İşlemler menüsünde **Yedekleme'yi** tıklatın.

![Kurtarma hizmetleri kasa ayrıntıları VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

VM'de yedekleme yapılandırılmamışsa, yedeklemeyi yapılandırmanızı ister. Yedekleme yapılandırıldıysa, geri yükleme noktaları nın listesi de dahil olmak üzere VM hakkında yedek bilgileri görürsünüz.  

![Kurtarma hizmetleri kasa ayrıntıları VM](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

Önceki örnekte, **ContosoVM** sanal makinenin adıdır. **ContosoVM-demovault** Kurtarma Hizmetleri kasasının adıdır. Kurtarma noktalarını depolayan Kurtarma Hizmetleri kasasının adını hatırlamanız gerekmez, bu bilgilere sanal makineden erişebilirsiniz.  

Bir Kurtarma Hizmetleri kasası birden çok sunucuyu koruyorsa, Kurtarma Hizmetleri kasasına bakmak daha mantıklı olabilir. Abonelikteki tüm Kurtarma Hizmetleri kasalarını arayabilir ve listeden birini seçebilirsiniz.

Aşağıdaki bölümler, her etkinlik türünde Kurtarma Hizmetleri kasasının nasıl kullanılacağını açıklayan makalelere bağlantılar içerir.

> [!NOTE]
> Kurtarma Hizmetleri kasası, 24 saat içinde silinmişse aynı adla oluşturulamaz. Farklı bir kaynak adı kullanın veya farklı bir kaynak grubu seçin veya 24 saat sonra yeniden deneyin.

### <a name="back-up-data"></a>Verileri yedekleme

- [Azure VM'yi yedekleme](backup-azure-vms-first-look-arm.md)
- [Windows Server veya Windows iş istasyonunu yedekleme](backup-try-azure-backup-in-10-mins.md)
- [DPM iş yüklerini Azure'a yedekleme](backup-azure-dpm-introduction.md)
- [Azure Yedekleme Sunucusu'ni kullanarak iş yüklerini yedeklemeye hazırlanın](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Kurtarma noktalarını yönetme

- [Azure VM yedeklemelerini yönetme](backup-azure-manage-vms.md)
- [Dosya ve klasörleri yönetme](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Kasadan verileri geri yükleme

- [Azure VM'den tek tek dosyaları kurtarma](backup-azure-restore-files-from-vm.md)
- [Azure VM'yi geri yükleme](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Kasayı emniyete ala

- [Kurtarma Hizmetleri kasalarında bulut yedekleme verilerinin güvenliğini sağlama](backup-azure-security-feature.md)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri kullanarak:</br>
[Bir IaaS VM yedekleme](backup-azure-arm-vms-prepare.md)</br>
[Azure Yedekleme Sunucusu'nı yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Windows Server'ı yedekleme](backup-windows-with-mars-agent.md)
