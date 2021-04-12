---
title: Azure disk yedekleme hakkında sık sorulan sorular
description: Azure disk yedekleme hakkında sık sorulan soruların yanıtlarını alın
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 41f2862541d7cb0e3276b5fd24f39668d20fc0a3
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2021
ms.locfileid: "107106964"
---
# <a name="frequently-asked-questions-about-azure-disk-backup"></a>Azure disk yedekleme hakkında sık sorulan sorular

Bu makalede, Azure disk yedekleme hakkında sık sorulan sorular yanıtlanmaktadır. [Azure disk yedekleme](disk-backup-overview.md) bölgesi kullanılabilirliği, desteklenen senaryolar ve sınırlamalar hakkında daha fazla bilgi için bkz. [destek matrisi](disk-backup-support-matrix.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="can-i-back-up-the-disk-using-the-azure-disk-backup-solution-if-the-same-disk-is-backed-up-using-azure-virtual-machine-backup"></a>Aynı disk Azure sanal makine yedeklemesi kullanılarak yedekleniyorsa, Azure disk yedekleme çözümünü kullanarak diski yedekleyebilir miyim?

Azure Backup, disk yedeklemesi ve [Azure VM yedekleme](backup-azure-vms-introduction.md) çözümlerini kullanarak yönetilen disk yedeklemesi için yan yana destek sunar. Bu, bir günlük uygulamayla tutarlı bir şekilde sanal makine yedeklemesi ve ayrıca işletim sistemi diskinin veya belirli bir veri diskinin daha sık yedeklemeleri gerektiğinde ve bu da üretim uygulaması performansını etkilemeden kilitlenme tutarlılığı sağlar.

### <a name="how-do-i-find-the-snapshot-resource-group-that-i-used-to-configure-backup-for-a-disk"></a>Bir disk için yedeklemeyi yapılandırmak üzere kullandığım anlık görüntü kaynak grubunu bulun Nasıl yaparım??

**Yedekleme örneği** ekranında, **temel** bileşenler bölümünde anlık görüntü kaynak grubu alanını bulabilirsiniz. Yedekleme merkezi 'nden veya yedekleme kasasından karşılık gelen diskin yedekleme örneğinizi arayabilir ve seçebilirsiniz.

![Anlık görüntü kaynak grubu alanı](./media/disk-backup-faq/snapshot-resource-group.png)

### <a name="what-is-a-snapshot-resource-group"></a>Anlık görüntü kaynak grubu nedir?

Azure disk yedekleme, yönetilen disk için işlemsel katman yedeklemesi sunmaktadır. Diğer bir deyişle, zamanlanmış ve isteğe bağlı yedekleme işlemleri sırasında oluşturulan anlık görüntüler, aboneliğinizdeki bir kaynak grubunda depolanır. Artımlı anlık görüntüler aboneliğinizde depolandığından, Azure Backup anında geri yükleme sunar. Bu kaynak grubu, anlık görüntü kaynak grubu olarak bilinir. Daha fazla bilgi için bkz. [yedeklemeyi yapılandırma](backup-managed-disks.md#configure-backup).

### <a name="why-must-the-snapshot-resource-group-be-in-same-subscription-as-that-of-the-disk-being-backed-up"></a>Anlık görüntü kaynak grubu neden yedeklenen disk ile aynı abonelikte olmalıdır?

Bu diskin aboneliği dışındaki belirli bir disk için artımlı bir anlık görüntü oluşturamazsınız. Bu nedenle, yedeklenecek diskle aynı abonelik içindeki kaynak grubunu seçin. Yönetilen diskler için [Artımlı anlık görüntü](../virtual-machines/disks-incremental-snapshots.md#restrictions) hakkında daha fazla bilgi edinin.

### <a name="why-do-i-need-to-provide-role-assignments-to-be-able-to-configure-backups-perform-scheduled-and-on-demand-backups-and-restore-operations"></a>Yedeklemeleri yapılandırabilmek, zamanlanmış ve isteğe bağlı yedeklemeler gerçekleştirmek ve geri yüklemek için neden rol atamaları sağlamam gerekir?

Azure disk yedekleme, aboneliklerinizde yönetilen diskleri bulma, koruma ve geri yükleme için en az ayrıcalık yaklaşımını kullanır. Bunu başarmak için, Azure Backup diğer Azure kaynaklarına erişmek için [yedekleme kasasının](backup-vault-overview.md) yönetilen kimliğini kullanır. Sistem tarafından atanan yönetilen kimlik, kaynak başına bir kaynakla kısıtlıdır ve bu kaynağın yaşam döngüsüne bağlıdır. Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak yönetilen kimliğe izin verebilirsiniz. Yönetilen kimlik, yalnızca Azure kaynaklarıyla kullanılabilecek özel bir türün hizmet sorumlusundan oluşur. [Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)hakkında daha fazla bilgi edinin. Varsayılan olarak, yedekleme kasasının yedeklenmek üzere diske erişme, düzenli anlık görüntüler oluşturma, bekletme süresinden sonra anlık görüntüleri silme ve bir diski yedekten geri yükleme izni yoktur. Yedekleme kasasının yönetilen kimliğine açıkça rol atamaları vererek, aboneliklerdeki kaynaklarla ilgili izinleri yönetme denetimi olursunuz.

### <a name="why-does-backup-policy-limit-the-retention-duration"></a>Yedekleme ilkesi neden saklama süresini sınırlıyor?

Azure disk yedekleme, disk başına 200 anlık görüntüsü ile sınırlı olan Artımlı anlık görüntüler kullanır. Yedekleme ilkesi, zamanlanan yedeklemelerin dışında isteğe bağlı yedeklemeler yapmanıza olanak tanımak için toplam yedeklemeleri 180 olarak sınırlandırır. Yönetilen diskler için [Artımlı anlık görüntüler](../virtual-machines/disks-incremental-snapshots.md#restrictions) hakkında daha fazla bilgi edinin.

### <a name="how-does-the-hourly-and-daily-backup-frequency-work-in-the-backup-policy"></a>Saatlik ve günlük yedekleme sıklığı yedekleme ilkesinde nasıl çalışır?

Azure disk yedekleme günde birden çok yedekleme sunar. Daha sık yedeklemeler gerekiyorsa **saatlik** yedekleme sıklığını seçin. Yedeklemeler, seçilen **zaman** aralığı temelinde zamanlanır. Örneğin, **her 4 saatte** bir seçeneğini belirlerseniz yedeklemeler günde eşit olarak dağıtılırsa yedeklemeler yaklaşık 4 saatte bir alınır. Günlük yedekleme yeterince yeterliyse **günlük** yedekleme sıklığını seçin. Günlük yedekleme sıklığında, yedeklemelerinizin alınacağı günün saatini belirtebilirsiniz. Gün zamanının yedeklemenin tamamlandığı zaman değil, yedekleme başlangıç saatini gösterdiğine dikkat edin. Yedekleme işlemini tamamlaması için gereken süre, ardışık yedeklemeler arasındaki karmaşıklık oranı dahil olmak üzere çeşitli faktörlere bağımlıdır. Ancak, Azure disk yedekleme, üretim uygulaması performansını etkilemeden [Artımlı anlık görüntüleri](../virtual-machines/disks-incremental-snapshots.md) kullanan aracısız bir yedeğiniz.

### <a name="why-does-the-backup-vaults-redundancy-setting-not-apply-to-the-backups-stored-in-operational-tier-the-snapshot-resource-group"></a>Yedekleme kasasının artıklık ayarı, işlemsel katmanda (anlık görüntü kaynak grubu) depolanan yedeklemeler için neden uygulanmaz?

Azure Backup, üst diskin depolama türünden bağımsız olarak, Standart HDD depolamada son anlık görüntüden bu yana yalnızca disklere yapılan Delta değişikliklerini depolayan yönetilen disklerin [Artımlı anlık görüntülerini](../virtual-machines/disks-incremental-snapshots.md#restrictions) kullanır. Daha fazla güvenilirlik için, ZRS 'yi destekleyen bölgelerde Artımlı anlık görüntüler bölge yedekli depolama (ZRS) üzerinde varsayılan olarak depolanır. Şu anda Azure disk yedekleme, yedeklemeleri Yedekleme Kasası deposuna kopyalamamakta olan yönetilen disklerin işlemsel yedeklemelerini destekler. Bu nedenle yedekleme kasasının yedek depolama artıklığı ayarı kurtarma noktaları için uygulanmaz.

### <a name="can-i-use-backup-center-to-configure-backups-and-manage-backup-instances-for-azure-disks"></a>Yedekleme merkezini kullanarak yedeklemeleri yapılandırabilir ve Azure diskleri için yedekleme örneklerini yönetebilir miyim?

Evet, Azure disk yedekleme, kuruluşların her ölçekte yedeklemeleri yönetmek, izlemek, çalıştırmak ve analiz etmek için Azure 'da **tek bir birleştirilmiş yönetim deneyimi** sunan [yedekleme merkezi](backup-center-overview.md)ile tümleşiktir. Ayrıca, kasadaki Korunan yedekleme örneklerini yedeklemek, geri yüklemek ve yönetmek için yedekleme kasasını da kullanabilirsiniz.

### <a name="why-do-i-need-to-create-a-backup-vault-and-not-use-a-recovery-services-vault"></a>Neden bir yedekleme Kasası oluşturmalıyım ve kurtarma hizmetleri Kasası kullanmıyorum?

Yedekleme Kasası, Azure Backup desteklediği bazı yeni iş yükleri için yedekleme verilerini barındıran Azure 'da bir depolama varlıktır. Yedekleme kasalarını, PostgreSQL için Azure veritabanı sunucuları, Azure diskleri ve Azure Backup destekleyeceği daha yeni iş yükleri gibi çeşitli Azure hizmetleri için yedekleme verilerini tutmak üzere kullanabilirsiniz. Yedekleme kasaları, yedekleme verilerinizi düzenlemenizi kolaylaştırır, ancak yönetim yükünü en aza indirir. Daha fazla bilgi edinmek için [yedekleme kasaları](./backup-vault-overview.md) bölümüne bakın.

### <a name="can-the-disk-to-be-backed-up-and-the-backup-vault-be-in-different-subscriptions"></a>Yedeklenecek disk ve Yedekleme Kasası farklı aboneliklerde mi çalışıyor?

Evet, yedeklenecek kaynak tarafından yönetilen disk ve Yedekleme Kasası farklı aboneliklerde olabilir.

### <a name="can-the-disk-to-be-backed-up-and-the-backup-vault-be-in-different-regions"></a>Yedeklenecek disk ve Yedekleme Kasası farklı bölgelerde mi olabilir?

Hayır, şu anda yedeklenecek kaynak tarafından yönetilen disk ve Yedekleme Kasası aynı bölgede olmalıdır.

### <a name="can-i-restore-a-disk-into-a-different-subscription"></a>Bir diski farklı bir aboneliğe geri yükleyebilir miyim?

Evet, diski yedeklemenin alındığı kaynak tarafından yönetilen diskten farklı bir aboneliğe geri yükleyebilirsiniz.

### <a name="can-i-back-up-multiple-disks-together"></a>Birden çok diski birlikte yedekleyebilir miyim?

Hayır, bir sanal makineye bağlı birden çok diskin zaman içindeki anlık görüntüleri desteklenmez. Daha fazla bilgi için bkz. [yedeklemeyi yapılandırma](backup-managed-disks.md#configure-backup) ve sınırlamalar hakkında daha fazla bilgi edinmek için [destek matrisine](disk-backup-support-matrix.md)bakın.

### <a name="what-is-a-target-resource-group"></a>Hedef kaynak grubu nedir?

Geri yükleme işlemi sırasında, diski geri yüklemek istediğiniz aboneliği ve bir kaynak grubunu seçebilirsiniz. Azure Backup, seçili kaynak grubundaki kurtarma noktasından yeni diskler oluşturacak. Bu, hedef kaynak grubu olarak adlandırılır. Yedekleme kasasının yönetilen kimliğinin, geri yükleme işlemini başarıyla gerçekleştirebilmesi için hedef kaynak grubundaki rol atamasının gerekli olduğunu unutmayın. Daha fazla bilgi için [geri yükleme belgelerine](restore-managed-disks.md)bakın.

### <a name="what-are-the-permissions-used-by-azure-backup-during-backup-and-restore-operation"></a>Yedekleme ve geri yükleme işlemi sırasında Azure Backup tarafından kullanılan izinler nelerdir?

Aşağıda, yedeklenecek **diske** atanan **disk yedekleme okuyucusu** rolünde kullanılan eylemler verilmiştir:

"Microsoft. COMPUTE/Disks/Read"

"Microsoft. COMPUTE/Disks/beginGetAccess/Action"

**Anlık görüntü kaynak grubuna** atanan **disk anlık görüntüsü katılımcısı** rolünde kullanılan eylemler aşağıda verilmiştir:

"Microsoft. COMPUTE/Snapshot/Delete"

"Microsoft. COMPUTE/Snapshot/Write"

"Microsoft. COMPUTE/Snapshot/Read"

"Microsoft. Storage/storageAccounts/Write"

"Microsoft. Storage/storageAccounts/Read"

"Microsoft. Storage/storageAccounts/Delete"

"Microsoft. resources/abonelikler/resourceGroups/Read"

"Microsoft. Storage/storageAccounts/ListKeys/Action"

"Microsoft. COMPUTE/Snapshots/beginGetAccess/Action"

"Microsoft. COMPUTE/Snapshots/endGetAccess/Action"

"Microsoft. COMPUTE/Disks/beginGetAccess/Action"

**Hedef kaynak grubuna** atanan **disk geri yükleme işletmeni** rolünde kullanılan eylemler aşağıda verilmiştir:

"Microsoft. COMPUTE/Disks/Write"

"Microsoft. COMPUTE/Disks/Read"

"Microsoft. resources/abonelikler/resourceGroups/Read"

>[!NOTE]
>Bu rollerdeki izinler, Azure Backup hizmeti tarafından eklenen özelliklere göre gelecekte değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Yedekleme destek matrisi](disk-backup-support-matrix.md)