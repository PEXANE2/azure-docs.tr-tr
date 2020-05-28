---
title: Azure dosya paylaşımları için yanlışlıkla silme koruması
description: Geçici silme işleminin, Azure dosya paylaşımlarınızı yanlışlıkla silinmeye karşı nasıl koruyabileceği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/02/2020
ms.openlocfilehash: 09d74a135fc43a7758004d77af2ec4c478345a2c
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122412"
---
# <a name="accidental-delete-protection-for-azure-file-shares-using-azure-backup"></a>Azure Backup kullanarak Azure dosya paylaşımları için korumayı yanlışlıkla silme

Cybersaldýra veya yanlışlıkla silinmeye karşı koruma sağlamak için, ilgili depolama hesabındaki herhangi bir dosya paylaşımı için yedeklemeyi yapılandırdığınızda bir depolama hesabındaki tüm dosya paylaşımları için [geçici silme](https://docs.microsoft.com/azure/storage/files/storage-files-prevent-file-share-deletion) etkinleştirilir. Geçici silme sayesinde, kötü bir aktör dosya paylaşımını silerse bile dosya paylaşımının içeriği ve kurtarma noktaları (anlık görüntüler) en az 14 ek gün boyunca tutulur ve veri kaybı olmadan dosya paylaşımlarının kurtarılmasına olanak tanır.  

Geçici silme yalnızca standart ve Premium Depolama hesapları için desteklenir ve şu anda [Bu bölgelerdeki](azure-file-share-support-matrix.md)Azure Backup tarafında etkinleştirilir.

Aşağıdaki akış grafiğinde, bir depolama hesabındaki dosya paylaşımları için geçici silme etkinleştirildiğinde bir yedekleme öğesinin farklı adımları ve durumları gösterilmektedir:

 ![Geçici silme akışı grafiği](./media/soft-delete-afs/soft-delete-flow-chart.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="when-will-soft-delete-be-enabled-for-file-shares-in-my-storage-account"></a>Depolama hesabımın dosya paylaşımları için geçici silme ne zaman etkinleştirilir?

Yedekleme 'yi bir depolama hesabındaki herhangi bir dosya paylaşımı için ilk kez yapılandırdığınızda, Azure Backup hizmeti ilgili depolama hesabındaki tüm dosya paylaşımları için geçici silme imkanı sunar.

### <a name="can-i-configure-the-number-of-days-for-which-my-snapshots-and-restore-points-will-be-retained-in-soft-deleted-state-after-i-delete-the-file-share"></a>Dosya paylaşımından sildikten sonra anlık görüntülerimin ve geri yükleme noktalarının geçici olarak silinen durumda tutulacağı gün sayısını yapılandırabilir miyim?

Evet, saklama süresini gereksinimlerinize göre ayarlayabilirsiniz. [Bu belgede](https://docs.microsoft.com/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal) , saklama süresini yapılandırma adımları açıklanmaktadır. Yedeklenen dosya paylaşımlarına sahip depolama hesapları için en düşük bekletme ayarı 14 gün olmalıdır.

### <a name="does-azure-backup-reset-my-retention-setting-because-i-configured-it-to-less-than-14-days"></a>Bunu 14 günden daha az bir şekilde yapılandırdım Azure Backup bekletme ayarımı sıfırladı mi?

Bir güvenlik perspektifinden, yedeklenen dosya paylaşımlarına sahip depolama hesapları için en az 14 günlük bekletme elde etmenizi öneririz. Her bir yedekleme işi çalıştırıldığında, Azure Backup ayarı 14 günden daha az bir değere tanımlarsa, 14 güne sıfırlar.

### <a name="what-is-the-cost-incurred-during-the-retention-period"></a>Bekletme dönemi boyunca tahakkuk eden maliyet nedir?

Geçici olarak silinen süre boyunca, korunan örnek maliyeti ve anlık görüntü depolama maliyeti olduğu gibi kalır.  Ayrıca, standart dosya paylaşımları için düzenli hızda kullanılan kapasite ve Premium dosya paylaşımları için anlık görüntü depolama ücreti üzerinden ücretlendirilirsiniz.

### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-deleted-state"></a>Verilerim geçici olarak silinmiş durumdaysa geri yükleme işlemi gerçekleştirebilir miyim?

Geri yükleme işlemlerini gerçekleştirmek için önce geçici olarak silinen dosya paylaşımının silinmesini yapmanız gerekir. Silmeyi geri alma işlemi, dosya paylaşımının zaman içinde herhangi bir noktaya geri yüklediğiniz yedeklenen duruma gelmesini sağlar. Dosya paylaşımınızın silmeyi geri alma hakkında bilgi edinmek için [Bu bağlantıyı](https://docs.microsoft.com/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal#restore-soft-deleted-file-share) ziyaret edin veya [silmeyi geri al dosya paylaşma betiğine](./scripts/backup-powershell-script-undelete-file-share.md)bakın.

### <a name="how-can-i-purge-the-data-of-a-file-share-in-a-storage-account-that-has-at-least-one-protected-file-share"></a>En az bir korumalı dosya paylaşımıyla bir depolama hesabındaki bir dosya paylaşımının verilerini nasıl temizolabilirim?

Depolama hesabında en az bir korumalı dosya paylaşımınız varsa, bu hesaptaki tüm dosya paylaşımları için geçici silme özelliğinin etkinleştirildiği ve verilerinizin silme işleminden sonra 14 gün boyunca saklanacağı anlamına gelir. Ancak verileri hemen temizlemek ve korunması istemiyorsanız, aşağıdaki adımları izleyin:

1. Geçici silme etkinken dosya paylaşımından zaten sildiyseniz, [önce dosya paylaşımının dosya paylaşımından veya](https://docs.microsoft.com/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal#restore-soft-deleted-file-share) [silmeyi geri al dosya paylaşma betiğini](./scripts/backup-powershell-script-undelete-file-share.md)kullanarak silmeyi geri alın.
2. [Bu belgede](https://docs.microsoft.com/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal#disable-soft-delete)bahsedilen adımları izleyerek depolama hesabınızda dosya paylaşımları için geçici silme devre dışı bırakın.
3. Şimdi, içeriğini temizlemek istediğiniz dosya paylaşımından hemen silin.

>[!NOTE]
>Sonraki zamanlanmış yedekleme işinin depolama hesabınızda korunan dosya paylaşımında çalışması için 2. adımı gerçekleştirmeniz gerekir. Yedekleme işi her çalıştığında, depolama hesabındaki tüm dosya paylaşımları için geçici silme işlemini yeniden etkinleştirilir.

>[!WARNING]
>2. adımdaki geçici silme devre dışı bırakıldıktan sonra, dosya paylaşımlarına karşı gerçekleştirilen silme işlemleri kalıcı silme işlemidir. Bu, geçici silme devre dışı bırakıldıktan sonra yedeklenen dosya paylaşımından yanlışlıkla sildiğiniz tüm anlık görüntülerinizi kaybedecek ve verilerinizi kurtaramayacaksınız demektir.

### <a name="in-the-context-of-a-file-shares-soft-delete-setting-what-changes-does-azure-backup-do-when-i-unregister-a-storage-account"></a>Bir dosya paylaşımının geçici silme ayarı bağlamında, bir depolama hesabının kaydını sildiğimde ne Azure Backup değişiklikler yapılır?

Kayıt silme sırasında, Azure Backup dosya paylaşımları için bekletme süresi ayarını denetler ve 14 günden büyük veya 14 günden az olursa, saklama süresini olduğu gibi bırakır. Ancak, bekletme 14 gün ise, Azure Backup tarafından etkinleştirilmekte olduğunu düşünyoruz ve kayıt silme işlemi sırasında geçici silme işlemini devre dışı bıraktık. Saklama ayarını olduğu gibi tutarken depolama hesabının kaydını silmek istiyorsanız, kayıt silme işlemini tamamladıktan sonra depolama hesabı bölmesinden yeniden etkinleştirin. Yapılandırma adımları için [Bu bağlantıya](https://docs.microsoft.com/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal#restore-soft-deleted-file-share) başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Portal Azure dosya paylaşımlarını nasıl yedekleyeceğinizi](backup-afs.md) öğrenin
