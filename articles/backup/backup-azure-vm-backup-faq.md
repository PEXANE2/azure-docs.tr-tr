---
title: Azure Backup ile Azure VM 'lerini yedekleme hakkında sık sorulan sorular
description: Azure Backup ile Azure VM 'Leri yedekleme hakkında sık sorulan soruların yanıtları.
ms.reviewer: sogup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: dacurwin
ms.openlocfilehash: 078c8763a08df339b9291807102e2d187d2a882f
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68827580"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Sık sorulan sorular-Azure VM 'lerini yedekleme

Bu makalede, Azure VM 'Leri [Azure Backup](backup-introduction-to-azure-backup.md) hizmetiyle yedekleme hakkında sık sorulan sorular yanıtlanmaktadır.


## <a name="backup"></a>Yedekle

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>Hangi VM görüntülerini oluştururken yedekleme için etkinleştirilebilir?
Bir VM oluşturduğunuzda, [desteklenen işletim sistemlerini](backup-support-matrix-iaas.md#supported-backup-actions) çalıştıran VM 'ler için yedeklemeyi etkinleştirebilirsiniz

### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>Yedekleme maliyeti VM maliyetine dahil mi?

Hayır. Yedekleme maliyetleri, bir VM 'nin maliyetlerinden ayrıdır. [Azure Backup fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/)hakkında daha fazla bilgi edinin.

### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>Bir VM için yedeklemeyi etkinleştirmek için hangi izinler gereklidir?

Sanal makine katılımcısı ise, VM 'de yedeklemeyi etkinleştirebilirsiniz. Özel bir rol kullanıyorsanız, VM 'de yedeklemeyi etkinleştirmek için aşağıdaki izinlere sahip olmanız gerekir:

- Microsoft.RecoveryServices/Vaults/write
- Microsoft.RecoveryServices/Vaults/read
- Microsoft. RecoveryServices/konumlar/*
- Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/*/Read
- Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/Read
- Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/yazma
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write
- Microsoft.RecoveryServices/Vaults/backupPolicies/read
- Microsoft.RecoveryServices/Vaults/backupPolicies/write

Kurtarma Hizmetleri kasanızın ve sanal makinenizin farklı kaynak grupları varsa, kurtarma hizmetleri kasasının kaynak grubunda yazma izinlerinizin olduğundan emin olun.  


### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı bir yedekleme işi, zamanlanmış yedeklemeler ile aynı bekletme zamanlamasını kullanıyor mu?
Hayır. İsteğe bağlı yedekleme işi için bekletme aralığını belirtin. Varsayılan olarak, portaldan tetiklendiğinde 30 gün boyunca korunur.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup erişim Key Vault için izinler sağlayın. PowerShell 'de [Azure Backup PowerShell](backup-azure-vms-automation.md) belgelerindeki **yedeklemeyi etkinleştirme** bölümünde açıklandığı gibi izinleri belirtin.

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>VM disklerini yönetilen disklere geçirdim. Yedeklemelerim çalışmaya devam edecek mi?
Evet, yedeklemeler sorunsuz şekilde çalışır. Herhangi bir şeyi yeniden yapılandırmanız gerekmez.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>Yedeklemeyi Yapılandır sihirbazında neden VM’mi göremiyorum?
Sihirbaz yalnızca kasala aynı bölgedeki VM 'Leri listeler ve henüz yedeklenmemektedir.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>VM 'im kapatıldı. İsteğe bağlı veya zamanlanmış bir yedekleme işi mi olacak?
Evet. Yedeklemeler bir makine kapatıldığında çalışır. Kurtarma noktası kilitlenmeyle tutarlı olarak işaretlenir.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Devam eden bir yedekleme işini iptal edebilir miyim?
Evet. Bir **anlık görüntü** durumunda yedekleme işini iptal edebilirsiniz. Anlık görüntüden veri aktarımı devam ediyorsa bir işi iptal edemezsiniz.

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprg_geo_number-will-my-backups-continue-to-work"></a>Azure Backup hizmeti tarafından oluşturulan kaynak grubunda kilidi etkinleştirdim (ör. `AzureBackupRG_<geo>_<number>`), yedeklemelerim çalışmaya devam eder mi?
Azure Backup hizmeti tarafından oluşturulan kaynak grubunu kilitlerseniz, en fazla 18 geri yükleme noktası sınırı olduğundan yedeklemeler başarısız olur.

Sonraki yedeklemelerin başarılı olması için kullanıcının kilidi kaldırması ve bu kaynak grubundan geri yükleme noktası koleksiyonunu temizlenmesi gerekir, geri yükleme noktası koleksiyonunu kaldırmak için [aşağıdaki adımları izleyin](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) .


### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Azure Backup standart SSD yönetilen diskini destekliyor mu?
Evet, Azure Backup [Standart SSD yönetilen diskleri](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/)destekler.

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>Yazma Hızlandırıcısı (WA) özellikli bir diskle VM 'yi yedekleyebiliriz?
Anlık görüntüler, WA özellikli diskte alınamaz. Ancak Azure Backup hizmeti, WA özellikli diski yedekten hariç tutabilir.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Yazma Hızlandırıcısı (WA) diskleri olan ve SAP HANA yüklü bir VM kullanıyorum. Nasıl yaparım? yedekleme yapılsın mı?
Azure Backup, WA özellikli diski yedekleyebilir, ancak bunu yedekten hariç tutabilir. Ancak, WA özellikli disk hakkında bilgi yedeklenmediği için yedekleme veritabanı tutarlılığı sağlamaz. İşletim sistemi diski yedeklemesini ve WA etkin olmayan disklerin yedeklemesini istiyorsanız bu yapılandırmayla diskleri yedekleyebilirsiniz.

Bir SAP HANA yedeklemesi için, 15 dakikalık bir RPO 'SU için özel önizleme çalıştırıyoruz. SQL DB yedeklemesine benzer bir şekilde oluşturulmuştur ve SAP HANA tarafından sertifikalandırılmış üçüncü taraf çözümler için Backınt arabirimini kullanır. İlginizi çekiyorsunuz, `AskAzureBackupTeam@microsoft.com` **Azure VM 'lerinde SAP HANA yedeklenmesi için özel önizleme için ilgili kayıt**ile birlikte bize e-posta gönderin.

### <a name="what-is-the-maximum-delay-i-can-expect-in-backup-start-time-from-the-scheduled-backup-time-i-have-set-in-my-vm-backup-policy"></a>Yedekleme başlangıç zamanında, VM yedekleme ilkemdeki ayarlanmış olan zamanlanmış yedekleme zamanından maksimum gecikme süresini tahmin edebilirim?
Zamanlanan yedekleme, zamanlanan yedekleme zamanının 2 saat içinde tetiklenecektir. For ex. 100 VM 'Lerde yedekleme başlangıç zamanı 2:00 ' de zamanlandı ve 4:00 en fazla 7:100 ' e kadar tüm 100Vm 'Ler yedekleme işini devam edecektir. Zamanlanan yedeklemeler kesinti nedeniyle duraklatıldıysa/yeniden denenirse yedekleme, bu zamanlanmış 2 saat penceresinin dışında başlayabilir.

### <a name="what-is-the-minimum-allowed-retention-range-for-daily-backup-point"></a>Günlük yedekleme noktası için izin verilen en düşük saklama aralığı nedir?
Azure sanal makine yedekleme ilkesi en az 9999 günlük bekletme aralığı destekler. Var olan bir VM yedekleme ilkesinde 7 günden az olan değişiklikler, en az 7 günlük bekletme aralığını karşılamak için bir güncelleştirme gerektirir.

## <a name="restore"></a>Geri yükle

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>Yalnızca disklerin mi yoksa tam bir VM 'nin mi geri yüklenemeyeceğine karar mi Nasıl yaparım??
Bir Azure VM için hızlı oluşturma seçeneği olarak bir VM geri yüklemeyi düşünün. Bu seçenek disk adlarını, diskler tarafından kullanılan kapsayıcıları, genel IP adreslerini ve ağ arabirimi adlarını değiştirir. Değişiklik, bir VM oluşturulduğunda benzersiz kaynakları korur. VM bir kullanılabilirlik kümesine eklenmez.

Şunları yapmak istiyorsanız diski geri yükle seçeneğini kullanabilirsiniz:
  * Oluşturulan VM 'yi özelleştirin. Örneğin, boyutunu değiştirin.
  * Yedekleme sırasında orada olmayan yapılandırma ayarlarını ekleyin.
  * Oluşturulan kaynaklar için adlandırma kuralını denetleyin.
  * VM 'yi bir kullanılabilirlik kümesine ekleyin.
  * PowerShell veya şablon kullanılarak yapılandırılması gereken başka bir ayar ekleyin.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>Yönetilen disklere yükselttikten sonra, yönetilmeyen VM disklerinin yedeklerini geri yükleyebilir miyim?
Evet, diskler yönetilmeyen 'dan yönetilene geçirilmeden önce alınan yedeklemeleri kullanabilirsiniz.
- Varsayılan olarak, bir geri yükleme VM işi yönetilmeyen bir VM oluşturur.
- Ancak, diskleri geri yükleyebilir ve yönetilen bir VM oluşturmak için kullanabilirsiniz.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>VM’yi nasıl yönetilen disklere geçirilmeden önceki bir geri yükleme noktasına geri yüklerim?
Varsayılan olarak, bir geri yükleme VM işi, yönetilmeyen disklere sahip bir VM oluşturur. Yönetilen disklerle bir VM oluşturmak için:
1. [Yönetilmeyen disklere geri yükleyin](tutorial-restore-disk.md#restore-a-vm-disk).
2. [Geri yüklenen diskleri yönetilen disklere dönüştürün](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk).
3. [Yönetilen disklerle BIR VM oluşturun](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk).

PowerShell 'de bunu yapma hakkında [daha fazla bilgi edinin](backup-azure-vms-automation.md#restore-an-azure-vm) .

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>Silinen VM 'yi geri yükleyebilir miyim?
Evet. VM 'yi silseniz bile kasadaki karşılık gelen yedekleme öğesine gidebilir ve bir kurtarma noktasından geri yükleme yapabilirsiniz.

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>Bir VM 'yi aynı Kullanılabilirlik kümelerine geri yükleme
Yönetilen disk Azure VM için, kullanılabilirlik kümelerine geri yükleme, yönetilen diskler olarak geri yüklenirken şablon içinde bir seçenek sunarak etkinleştirilir. Bu şablon, **kullanılabilirlik kümeleri**adlı giriş parametresine sahiptir.

### <a name="how-do-we-get-faster-restore-performances"></a>Daha hızlı geri yükleme performanslarını nasıl sağlıyoruz?
[Anında geri yükleme](backup-instant-restore-capability.md) özelliği, anlık görüntülerden daha hızlı yedeklemeler ve anında geri yüklemeler sağlanmasına yardımcı olur.

## <a name="manage-vm-backups"></a>VM yedeklemelerini yönetme

### <a name="what-happens-if-i-modify-a-backup-policy"></a>Bir yedekleme ilkesini değiştirdiğimde ne olur?
VM, değiştirilen veya yeni ilkedeki zamanlama ve bekletme ayarları kullanılarak yedeklenir.

- Bekletme genişletildiğinde, var olan kurtarma noktaları işaretlenir ve yeni ilkeye uygun olarak tutulur.
- Bekletme azaltıldığında, kurtarma noktaları sonraki temizleme işinde ayıklanmayı işaretlenir ve sonra silinir.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>Azure Backup tarafından yedeklenen bir VM 'yi farklı bir kaynak grubuna taşımak Nasıl yaparım? istiyor musunuz?

1. Yedeklemeyi geçici olarak durdurun ve yedekleme verilerini koruyun.
2. VM 'yi hedef kaynak grubuna taşıyın.
3. Yedekleme, aynı veya yeni kasada yeniden etkinleştirildi.

Taşıma işleminden önce oluşturulan kullanılabilir geri yükleme noktalarından VM 'yi geri yükleyebilirsiniz.

### <a name="is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy"></a>Aynı yedekleme ilkesiyle ilişkilendirilebilen VM sayısıyla ilgili bir sınır var mı?
Evet, portaldan aynı yedekleme ilkesiyle ilişkilendirilebilen 100 VM sınırının bir sınırı vardır. 100 ' den fazla VM için, aynı zamanlamaya veya farklı zamanlamaya sahip birden fazla yedekleme ilkesi oluşturmanız önerilir.
