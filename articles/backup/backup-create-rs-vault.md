---
title: Kurtarma Hizmetleri kasaları oluşturma ve yapılandırma
description: Bu makalede, yedeklemeleri ve kurtarma noktalarını depolayan kurtarma hizmetleri kasalarını oluşturma ve yapılandırma hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 05/30/2019
ms.custom: references_regions
ms.openlocfilehash: 048b0e3fae776be5577948f91d84bb2be7fa2d59
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88827367"
---
# <a name="create-and-configure-a-recovery-services-vault"></a>Kurtarma Hizmetleri Kasası oluşturma ve yapılandırma

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="set-storage-redundancy"></a>Depolama artıklığı ayarlama

Azure Backup kasa için depolamayı otomatik olarak işler. Bu depolamanın nasıl çoğaltılacağı belirtmeniz gerekir.

> [!NOTE]
> Bir kurtarma hizmetleri Kasası için **depolama çoğaltma türünü** değiştirme (yerel olarak yedekli/coğrafi olarak yedekli), kasadaki yedeklemeleri yapılandırmadan önce yapılmalıdır. Yedeklemeyi yapılandırdıktan sonra, değiştirme seçeneği devre dışı bırakılır.
>
>- Yedeklemeyi henüz yapılandırmadıysanız, ayarları gözden geçirmek ve değiştirmek için [aşağıdaki adımları izleyin](#set-storage-redundancy) .
>- Yedeklemeyi zaten yapılandırdıysanız ve GRS 'den LRS 'ye geçiş yapmanız gerekiyorsa, [Bu geçici çözümleri gözden geçirin](#how-to-change-from-grs-to-lrs-after-configuring-backup).

1. **Kurtarma Hizmetleri kasaları** bölmesinden yeni kasayı seçin. **Ayarlar** bölümünde **Özellikler**' i seçin.
1. **Özellikler**' de, **yedekleme yapılandırması**altında **Güncelleştir**' i seçin.

1. Depolama çoğaltma türünü seçin ve **Kaydet**' i seçin.

     ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Azure 'u birincil yedek depolama uç noktası olarak kullanıyorsanız, varsayılan **coğrafi olarak yedekli** ayarını kullanmaya devam edebilirsiniz.
   - Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin.
   - [Coğrafi](../storage/common/storage-redundancy.md) ve [Yerel](../storage/common/storage-redundancy.md) artıklık hakkında daha fazla bilgi edinin.

>[!NOTE]
>Kasa için depolama çoğaltma ayarları, geçerli çözüm bağlı olduğu ve kasaya aktarılan bir veri olmadığı için Azure dosya paylaşma yedeklemesi için uygun değildir. Anlık görüntüler, yedeklenen dosya paylaşımıyla aynı depolama hesabında depolanır.

## <a name="set-cross-region-restore"></a>Çapraz bölge geri yüklemeyi ayarla

Geri yükleme seçeneklerinden biri olan çapraz bölge geri yükleme (CRR), Azure sanal makinelerini bir [Azure eşlenmiş bölgesi](../best-practices-availability-paired-regions.md)olan ikincil bir bölgeye geri yüklemenize olanak tanır. Bu seçenek şunları yapmanıza olanak sağlar:

- Denetim veya uyumluluk gereksinimi olduğunda ayrıntıya ait ayrıntıları yürütün
- Birincil bölgede bir olağanüstü durum varsa VM 'yi veya diskini geri yükleyin.

Bu özelliği seçmek için **yedekleme yapılandırması** bölmesinden **çapraz bölge geri yüklemeyi etkinleştir** ' i seçin.

Bu işlem için, depolama düzeyinde olduğu kadar fiyatlandırma etkileri vardır.

>[!NOTE]
>Başlamadan önce:
>
>- Desteklenen yönetilen türlerin ve bölgelerin listesi için [destek matrisini](backup-support-matrix.md#cross-region-restore) gözden geçirin.
>- Çapraz bölge geri yükleme (CRR) özelliği artık tüm Azure genel bölgelerinde önizlenebilir.
>- CRR, herhangi bir GRS Kasası için kasa düzeyi katılım özelliğidir (varsayılan olarak kapalıdır).
>- Bu işlem yapıldıktan sonra, yedekleme öğelerinin ikincil bölgelerde kullanılabilmesi 48 saat kadar sürebilir.
>- Şu anda CRR yalnızca yedekleme yönetimi türü ARM Azure VM için desteklenir (klasik Azure VM desteklenmez).  Ek yönetim türleri CRR 'yi desteklediklerinde, bunlar **otomatik olarak** kaydedilir.
>- Koruma ilk kez başlatıldığında çapraz bölge geri yüklemesi şu anda GRS veya LRS 'ye geri döndürülemez.

### <a name="configure-cross-region-restore"></a>Çapraz bölge geri yüklemeyi yapılandırma

GRS yedekliliği ile oluşturulan bir kasa, çapraz bölge geri yükleme özelliğini yapılandırma seçeneğini içerir. Her GRS kasasının bir başlığı olur ve bu, belgelere bağlanır. Kasa için CRR 'yi yapılandırmak için, bu özelliği etkinleştirme seçeneğini içeren yedekleme yapılandırması bölmesine gidin.

 ![Yedekleme yapılandırması başlığı](./media/backup-azure-arm-restore-vms/banner.png)

1. Portaldan kurtarma hizmetleri Kasası > ayarlar > Özellikler ' e gidin.
2. İşlevselliği etkinleştirmek için **Bu kasada çapraz bölge geri yüklemeyi etkinleştir '** i seçin.

   ![Bu kasada çapraz bölge geri yüklemeyi etkinleştir ' i seçmeden önce](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![Bu kasada çapraz bölge geri yüklemeyi etkinleştir ' i seçtikten sonra](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

[İkincil bölgedeki yedekleme öğelerini görüntülemeyi](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region)öğrenin.

[İkincil bölgede geri yüklemeyi](backup-azure-arm-restore-vms.md#restore-in-secondary-region)öğrenin.

[İkincil bölge geri yükleme işlerini izlemeyi](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs)öğrenin.

## <a name="set-encryption-settings"></a>Şifreleme ayarlarını ayarla

Varsayılan olarak, kurtarma hizmetleri kasasındaki veriler platform tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu şifrelemeyi etkinleştirmek için sonlandırmadan açık bir eylem gerekmez ve kurtarma hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.  Bu kasadaki yedekleme verilerini şifrelemek için kendi anahtarınızı getirmeyi tercih edebilirsiniz. Bu, müşteri tarafından yönetilen anahtarlar olarak adlandırılır. Yedekleme verilerini kendi anahtarınızı kullanarak şifrelemek isterseniz, herhangi bir öğe bu kasaya korunmadan önce şifreleme anahtarı belirtilmelidir. Anahtarınızla şifrelemeyi etkinleştirdikten sonra geri alınamaz.

### <a name="configuring-a-vault-to-encrypt-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak şifrelemek için bir kasa yapılandırma

Kasalarınızı müşteri tarafından yönetilen anahtarlarla şifrelemek üzere yapılandırmak için bu adımların bu sırada izlenmesi gerekir:

1. Kurtarma Hizmetleri kasanızda yönetilen kimliği etkinleştirin

1. Azure Key Vault şifreleme anahtarına erişmek için kasaya izin atayın

1. Azure Key Vault geçici silme ve Temizleme korumasını etkinleştirme

1. Şifreleme anahtarını kurtarma hizmetleri kasasına atama

Bu adımların her birine ilişkin yönergeler, [Bu makalede](encryption-at-rest-with-cmk.md#configuring-a-vault-to-encrypt-using-customer-managed-keys)bulunabilir.

## <a name="modifying-default-settings"></a>Varsayılan ayarları değiştirme

Kasadaki yedeklemeleri yapılandırmadan önce **depolama çoğaltma türü** ve **güvenlik ayarları** için varsayılan ayarları incelemenizi kesinlikle öneririz.

- **Depolama çoğaltma türü** varsayılan olarak **coğrafi olarak yedekli** (GRS) olarak ayarlanır. Yedeklemeyi yapılandırdıktan sonra, değiştirme seçeneği devre dışı bırakılır.
  - Yedeklemeyi henüz yapılandırmadıysanız, ayarları gözden geçirmek ve değiştirmek için [aşağıdaki adımları izleyin](#set-storage-redundancy) .
  - Yedeklemeyi zaten yapılandırdıysanız ve GRS 'den LRS 'ye geçiş yapmanız gerekiyorsa, [Bu geçici çözümleri gözden geçirin](#how-to-change-from-grs-to-lrs-after-configuring-backup).

- Varsayılan olarak **geçici silme** , yeni oluşturulan kasaların yanlışlıkla veya kötü amaçlı silmelerden yedekleme verilerini korumak için **etkinleştirilir** . Ayarları gözden geçirmek ve değiştirmek için [aşağıdaki adımları izleyin](./backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete) .

### <a name="how-to-change-from-grs-to-lrs-after-configuring-backup"></a>Yedekleme yapılandırıldıktan sonra GRS 'den LRS 'ye değiştirme

GRS 'den yerel olarak yedekli depolama (LRS) ' ye taşımaya karar vermeden önce, senaryonuza uyan daha düşük maliyetli ve daha yüksek veri dayanıklılığı arasındaki denge konusunu gözden geçirin. GRS 'den LRS 'ye taşımanız gerekiyorsa iki seçeneğiniz vardır. Bunlar, yedekleme verilerini bekletmek için iş gereksinimlerinize bağlıdır:

- [Önceki yedeklenen verileri korumanız gerekmez](#dont-need-to-preserve-previous-backed-up-data)
- [Önceki yedeklenen verilerin korunması gerekir](#must-preserve-previous-backed-up-data)

#### <a name="dont-need-to-preserve-previous-backed-up-data"></a>Önceki yedeklenen verileri korumanız gerekmez

Yeni bir LRS kasasındaki iş yüklerini korumak için, geçerli koruma ve verilerin, GRS kasasında ve yedeklemelerde yeniden yapılandırılmış olması gerekir.

>[!WARNING]
>Aşağıdaki işlem bozucu olduğundan geri alınamaz. Korunan sunucuyla ilişkili tüm yedekleme verileri ve yedekleme öğeleri kalıcı olarak silinecek. Dikkatli olun.

GRS kasasında geçerli korumayı durdurun ve silin:

1. GRS Kasası özelliklerinde geçici silme devre dışı bırakın. Geçici silme devre dışı bırakmak için [aşağıdaki adımları](backup-azure-security-feature-cloud.md#disabling-soft-delete-using-azure-portal) izleyin.

1. Korumayı durdurun ve mevcut GRS kasasından yedeklemeleri silin. Kasa panosu menüsünde **yedekleme öğeleri**' ni seçin. Burada listelenen ve LRS kasasına taşınması gereken öğelerin, yedekleme verileriyle birlikte kaldırılması gerekir. [Bulutta korunan öğeleri silme](backup-azure-delete-vault.md#delete-protected-items-in-the-cloud) ve [Şirket içi korumalı öğeleri silme](backup-azure-delete-vault.md#delete-protected-items-on-premises)bölümüne bakın.

1. AFS 'yi (Azure dosya paylaşımlarını), SQL Server 'ı veya SAP HANA sunucularını taşımayı planlıyorsanız, bunları da silmeniz gerekir. Kasa panosu menüsünde, **Yedekleme altyapısı**' nı seçin. Bkz. [SQL Server kaydını silme](manage-monitor-sql-database-backup.md#unregister-a-sql-server-instance), [Azure dosya paylaşımları ile ilişkili bir depolama hesabının kaydını silme](manage-afs-backup.md#unregister-a-storage-account)ve [bir SAP HANA örneğinin kaydını silme](sap-hana-db-manage.md#unregister-an-sap-hana-instance).

1. GRS kasasından kaldırıldıktan sonra, yeni LRS kasasında iş yükünüz için yedeklemeleri yapılandırmaya devam edin.

#### <a name="must-preserve-previous-backed-up-data"></a>Önceki yedeklenen verilerin korunması gerekir

Geçerli korunan verileri GRS kasasında tutmanız ve yeni bir LRS kasasında korumaya devam etmeniz gerekiyorsa, bazı iş yükleri için sınırlı seçenekler vardır:

- MARS için, verileri tutma ve aracıyı yeni LRS kasasında kaydetme [ile korumayı durdurabilirsiniz](backup-azure-manage-mars.md#stop-protecting-files-and-folder-backup) .

  - Azure Backup hizmet, GRS kasasının tüm mevcut kurtarma noktalarını tutmaya devam edecektir.
  - Kurtarma noktalarını GRS kasasında tutmak için ödeme yapmanız gerekir.
  - Yalnızca GRS kasasındaki süre dolma edilmemiş kurtarma noktaları için yedeklenmiş verileri geri yükleyebileceksiniz.
  - LRS kasasında verilerin yeni bir ilk kopyasının oluşturulması gerekir.

- Bir Azure VM için, GRS kasasında VM için [verileri koruma ile korumayı durdurabilir](backup-azure-manage-vms.md#stop-protecting-a-vm) , VM 'yi başka bir kaynak grubuna taşıyabilir ve LRS KASASıNDAKI VM 'yi koruyabilirsiniz. Bir VM 'yi başka bir kaynak grubuna taşımak için [rehberlik ve sınırlamalar](../azure-resource-manager/management/move-limitations/virtual-machines-move-limitations.md) bölümüne bakın.

  Bir VM tek seferde yalnızca bir kasada korunabilir. Ancak, yeni kaynak grubundaki VM, farklı bir VM olarak kabul edildiği için LRS kasasında korunabilir.

  - Azure Backup hizmet, GRS kasasında yedeklenen kurtarma noktalarını korur.
  - Kurtarma noktalarını GRS kasasında tutmak için ödeme yapmanız gerekir (Ayrıntılar için [Azure Backup fiyatlandırmasına](azure-backup-pricing.md) bakın).
  - Gerekirse, GRS kasasından VM 'yi geri yükleyebileceksiniz.
  - Yeni kaynaktaki sanal makinenin LRS kasasındaki ilk yedekleme bir ilk çoğaltma olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) Kurtarma Hizmetleri kasaları.
[Hakkında bilgi edinin](backup-azure-delete-vault.md) Kurtarma Hizmetleri kasalarını silin.
