---
title: SSS - Azure VM'leri yedekleme
description: Bu makalede, Azure Yedekleme hizmetiyle Azure VM'lerini yedeklemeyle ilgili sık sorulan soruların yanıtlarını keşfedin.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: accfc57055f70254814c889de875f5360878bcd9
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81757460"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Sık sorulan sorular-Azure VM'lerini yedekle

Bu makalede, [Azure Yedekleme](backup-introduction-to-azure-backup.md) hizmetiyle Azure VM'lerini yedekleme yle ilgili sık sorulan sorular yanıtlanmaktadır.

## <a name="backup"></a>Backup

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>Oluşturduğumda hangi VM görüntüleri yedekleme için etkinleştirilebilir?

Bir VM oluşturduğunuzda, [desteklenen işletim sistemleri](backup-support-matrix-iaas.md#supported-backup-actions)çalıştıran VM'ler için yedeklemeyi etkinleştirebilirsiniz.

### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>Yedekleme maliyeti VM maliyetine dahil mi?

Hayır. Yedekleme maliyetleri VM'nin maliyetlerinden ayrıdır. [Azure Yedekleme fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/)hakkında daha fazla bilgi edinin.

### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>VM için yedeklemeyi etkinleştirmek için hangi izinler gereklidir?

VM'ye katkıda bulunuyorsanız, VM'de yedeklemeyi etkinleştirebilirsiniz. Özel bir rol kullanıyorsanız, VM'de yedeklemeyi etkinleştirmek için aşağıdaki izinlere ihtiyacınız vardır:

- Microsoft.RecoveryServices/Vaults/write
- Microsoft.RecoveryServices/Vaults/read
- Microsoft.RecoveryServices/locations/*
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write
- Microsoft.RecoveryServices/Vaults/backupPolicies/read
- Microsoft.RecoveryServices/Vaults/backupPolicies/write

Kurtarma Hizmetleri kasanızda ve VM'nizde farklı kaynak grupları varsa, Kurtarma Hizmetleri kasası için kaynak grubunda yazma izinleriniz olduğundan emin olun.  

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı yedekleme işi, zamanlanmış yedeklemelerle aynı bekletme zamanlamasını kullanır mı?

Hayır. İsteğe bağlı yedekleme işi için bekletme aralığını belirtin. Varsayılan olarak, portaldan tetiklendiğinde 30 gün boyunca saklanır.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?

Anahtar Kasası'na erişmek için Azure Yedekleme'ye izin sağlayın. Azure Yedekleme PowerShell belgelerinde **yedeklemeyi etkinleştir** bölümünde açıklandığı gibi [PowerShell'deki](backup-azure-vms-automation.md) izinleri belirtin.

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>VM diskleri yönetilen disklere geçirmiyorum. Yedeklemelerim çalışmaya devam edecek mi?

Evet, yedeklemeler sorunsuz çalışır. Hiçbir şeyi yeniden yapılandırmaya gerek yok.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>Yedeklemeyi Yapılandır sihirbazında neden VM’mi göremiyorum?

Sihirbaz, yalnızca kasayla aynı bölgede vm'leri listeler ve bunlar zaten yedeklenmemektedir.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>VM'im kapatıldı. İsteğe bağlı veya zamanlanmış yedekleme çalışacak mı?

Evet. Bir makine kapatıldığında yedekler çalışır. Kurtarma noktası kilitlenme tutarlı olarak işaretlenir.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Devam etmekte olan bir yedekleme işini iptal edebilir miyim?

Evet. **Anlık görüntü alma** durumunda yedekleme işini iptal edebilirsiniz. Anlık görüntüden veri aktarımı devam ediyorsa, bir işi iptal edemezsiniz.

### <a name="i-enabled-a-lock-on-the-resource-group-created-by-azure-backup-service-for-example-azurebackuprg_geo_number-will-my-backups-continue-to-work"></a>Azure Yedekleme Hizmeti tarafından oluşturulan kaynak grubunda bir kilit `AzureBackupRG_<geo>_<number>`oluşturdum (örneğin, ). Yedeklemelerim çalışmaya devam edecek mi?

Azure Yedekleme Hizmeti tarafından oluşturulan kaynak grubunu kilitlerseniz, en fazla 18 geri yükleme noktası sınırı olduğu için yedeklemeler başarısız olmaya başlar.

Kilidi kaldırın ve gelecekteki yedeklemeleri başarılı kılmak için bu kaynak grubundan geri yükleme noktası koleksiyonunu temizleyin. Geri yükleme noktası koleksiyonunu kaldırmak için [aşağıdaki adımları izleyin.](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal)

### <a name="does-azure-backup-support-standard-ssd-managed-disks"></a>Azure yedeklemesi standart SSD yönetilen diskleri destekliyor mu?

Evet, Azure Yedekleme [standart SSD yönetilen diskleri](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/)destekler.

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>Yazma Hızlandırıcı (WA) özellikli bir diskile bir VM yedekleyebilir miyiz?

Anlık görüntüler WA özellikli diskten alınamaz. Ancak, Azure Yedekleme hizmeti WA özellikli diski yedeklemeden hariç tutabilir.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Ben Yazma Hızlandırıcı (WA) diskler ve SAP HANA yüklü bir VM var. Nasıl yedeka alabiliyorum?

Azure Yedekleme, WA özellikli diski yedekleyebilir, ancak yedeklemeden hariç tutabilir. Ancak, WA özellikli diskteki bilgiler yedeklenmediği için yedekleme veritabanı tutarlılığı sağlamaz. İşletim sistemi disk yedeklemesi ve WA özellikli olmayan disklerin yedeklemesini istiyorsanız, bu yapılandırmayla diskleri yedekleyebilirsiniz.

Azure Yedekleme, SAP HANA veritabanları için 15 dakikalık bir RPO ile akış lı yedekleme çözümü sağlar. SAP HANA'nın yerel API'lerinden yararlanan yerel bir yedekleme desteği sağlamak için SAP tarafından backint sertifikasına işlenmiştir. Azure [VM'lerde SAP HANA veritabanlarını yedekleme hakkında](https://docs.microsoft.com/azure/backup/sap-hana-db-about)daha fazla bilgi edinin.

### <a name="what-is-the-maximum-delay-i-can-expect-in-backup-start-time-from-the-scheduled-backup-time-i-have-set-in-my-vm-backup-policy"></a>VM yedekleme politikamda belirlediğim zamanlanan yedekleme saatinden yedekleme başlangıç saatinde bekleyebileceğim maksimum gecikme nedir?

Zamanlanan yedekleme, zamanlanan yedekleme saatinden sonraki 2 saat içinde tetiklenir. Örneğin, 100 VM'nin yedekleme başlangıç saati 02:00'de zamanlanmışsa, en geç 04:00'e kadar tüm 100 VM'nin yedekleme işi devam edecektir. Zamanlanmış yedeklemeler bir kesinti nedeniyle duraklatılmışsa ve devam ettirilmişveya yeniden denenmişse, yedekleme bu zamanlanmış iki saatlik pencerenin dışında başlayabilir.

### <a name="what-is-the-minimum-allowed-retention-range-for-a-daily-backup-point"></a>Günlük yedekleme noktası için izin verilen minimum bekletme aralığı nedir?

Azure Sanal Makine yedekleme ilkesi, yedi günden 9999 güne kadar minimum bekletme aralığını destekler. Yedi günden az olan varolan bir VM yedekleme ilkesinde yapılacak herhangi bir değişiklik, yedi günlük minimum bekletme aralığını karşılamak için bir güncelleştirme gerektirir.

### <a name="what-happens-if-i-change-the-case-of-the-name-of-my-vm-or-my-vm-resource-group"></a>VM'min veya VM kaynak grubumun adını değiştirirsem ne olur?

VM veya VM kaynak grubunuzun kasasını (üst veya alt) değiştirirseniz, yedek madde adının durumu değişmez. Ancak, bu beklenen Azure Yedekleme davranışı. Servis talebi değişikliği yedekleme öğesinde görünmez, ancak arka uçta güncelleştirilir.

### <a name="can-i-back-up-or-restore-selective-disks-attached-to-a-vm"></a>VM'ye bağlı seçici diskleri yedekleyebilir veya geri yükleyebilir miyim?

Azure Yedekleme artık seçici disk yedeklemesini destekler ve Azure Sanal Makine yedekleme çözümlerini kullanarak geri yüklemeyi destekler.

Azure Yedekleme bugün, Sanal Makine yedekleme çözümlerini kullanarak VM'deki tüm diskleri (işletim sistemi ve verileri) birlikte yedeklemeyi destekler. Diski dışlama işleviyle, VM'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneğine sahip olursunuz. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve uygun maliyetli bir çözüm sağlar. Her kurtarma noktası yedekleme işlemine dahil edilen disklerin verilerini içerir ve bu da geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesini geri yüklemenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yüklemek için geçerlidir.

Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com

## <a name="restore"></a>Geri Yükleme

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>Yalnızca diskleri mi yoksa tam vm'yi mi geri yükleyeceğime nasıl karar veririm?

Azure VM için hızlı oluşturma seçeneği olarak VM geri yüklemesini düşünün. Bu seçenek, disk adlarını, diskler tarafından kullanılan kapsayıcıları, genel IP adreslerini ve ağ arabirimi adlarını değiştirir. Bir VM oluşturulduğunda değişiklik benzersiz kaynakları korur. VM kullanılabilirlik kümesine eklenmez.

İsterseniz geri yükleme diski seçeneğini kullanabilirsiniz:

- Oluşturulan VM'yi özelleştirin. Örneğin, boyutunu değiştirin.
- Yedekleme sırasında orada olmayan yapılandırma ayarları ekleyin.
- Oluşturulan kaynaklar için adlandırma kuralını denetle.
- VM'yi kullanılabilirlik kümesine ekleyin.
- PowerShell veya şablon kullanılarak yapılandırılması gereken diğer ayarları ekleyin.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>Yönetilen disklere yükselttikten sonra yönetilmeyen VM disklerinin yedeklerini geri yükleyebilir miyim?

Evet, diskler yönetilmeyenden yönetilmeye geçmeden önce alınan yedeklemeleri kullanabilirsiniz.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>VM’yi nasıl yönetilen disklere geçirilmeden önceki bir geri yükleme noktasına geri yüklerim?

Geri yükleme işlemi aynı kalır. Kurtarma noktası, VM'nin yönetilmeyen diskleri olduğu bir noktadaysa, [diskleri yönetilmeyen olarak geri yükleyebilirsiniz.](tutorial-restore-disk.md#unmanaged-disks-restore) VM yönetilen diskler olsaydı, o zaman [yönetilen diskler olarak diskler yükleyebilirsiniz.](tutorial-restore-disk.md#managed-disk-restore) Sonra [bu disklerden bir VM oluşturabilirsiniz.](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk)

PowerShell'de bu konuda [daha fazla bilgi edinin.](backup-azure-vms-automation.md#restore-an-azure-vm)

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>Silinen VM'yi geri yükleyebilir miyim?

Evet. VM'yi silseniz bile kasadaki ilgili yedekleme öğesine gidebilir ve kurtarma noktasından geri yükleyebilirsiniz.

### <a name="how-do-i-restore-a-vm-to-the-same-availability-sets"></a>Aynı kullanılabilirlik kümelerine vm'yi nasıl geri yükleyebilirim?

Yönetilen Disk Azure VM'leri için, kullanılabilirlik kümelerine geri dönmek, yönetilen diskler olarak geri dönerken şablonda bir seçenek sağlayarak etkinleştirilir. Bu şablonda **Kullanılabilirlik kümeleri**adı verilen giriş parametresi vardır.

### <a name="how-do-we-get-faster-restore-performances"></a>Performansları nasıl daha hızlı geri getirebiliriz?

[Anlık Geri Yükleme](backup-instant-restore-capability.md) özelliği, anlık görüntülerden daha hızlı yedekleme ve anında geri yükleme sağlar.

### <a name="what-happens-when-we-change-the-key-vault-settings-for-the-encrypted-vm"></a>Şifreli VM'nin anahtar kasa ayarlarını değiştirdiğimizde ne olur?

Şifreli VM'nin anahtar kasa ayarlarını değiştirdikten sonra, yedeklemeler yeni ayrıntı kümesiyle çalışmaya devam eder. Ancak, değişiklikten önce bir kurtarma noktasından geri yükleme den sonra, ondan VM oluşturabilirsiniz önce önemli bir kasada sırları geri yüklemeniz gerekir. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/backup/backup-azure-restore-key-secret)bakın.

Gizli/anahtar roll-over gibi işlemler bu adımı gerektirmez ve geri yüklemeden sonra aynı KeyVault kullanılabilir.

### <a name="can-i-access-the-vm-once-restored-due-to-a-vm-having-broken-relationship-with-domain-controller"></a>Bir VM etki alanı denetleyicisi ile ilişkisi bozuldu nedeniyle bir kez geri vm erişebilir miyim?

Evet, bir VM etki alanı denetleyicisi ile ilişkisi bozuldu nedeniyle bir kez geri yüklendi vm erişebilirsiniz. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#post-restore-steps) bakın

## <a name="manage-vm-backups"></a>VM yedeklemelerini yönetme

### <a name="what-happens-if-i-modify-a-backup-policy"></a>Bir yedekleme ilkesini değiştirirsem ne olur?

VM, değiştirilen veya yeni ilkedeki zamanlama ve bekletme ayarları kullanılarak yedeklenir.

- Bekletme uzatılırsa, varolan kurtarma noktaları işaretlenir ve yeni ilkeuyarınca tutulur.
- Bekletme azaltılırsa, kurtarma noktaları bir sonraki temizleme işinde budama için işaretlenir ve daha sonra silinir.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>Azure Yedekleme tarafından desteklenen bir VM'yi farklı bir kaynak grubuna nasıl taşıyabilirim?

1. Yedeklemeyi geçici olarak durdurun ve yedekleme verilerini koruyun.
2. VM'yi hedef kaynak grubuna taşıyın.
3. Aynı veya yeni kasada yedeklemeyi yeniden etkinleştirin.

VM'yi taşıma işleminden önce oluşturulan kullanılabilir geri yükleme noktalarından geri yükleyebilirsiniz.

### <a name="is-there-a-limit-on-number-of-vms-that-can-beassociated-with-the-same-backup-policy"></a>Aynı yedekleme ilkesiyle ilişkilendirilebilen VM sayısında bir sınır var mı?

Evet, portaldan aynı yedekleme ilkesiyle ilişkilendirilebilen 100 VM sınırı vardır. 100'den fazla VM için, aynı zamanlamaya veya farklı zamanlamaya sahip birden çok yedekleme ilkeleri oluşturmanızı öneririz.
