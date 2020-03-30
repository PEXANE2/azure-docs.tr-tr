---
title: Azure portalındaki Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri kasasında Azure dosya paylaşımlarını yedeklemek için Azure portalını nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: c1dea6925bad96be178f875567077fafa4db9326
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938090"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasındaAzure dosya paylaşımlarını yedekleme

Bu makalede, [Azure dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)yedeklemek için Azure portalının nasıl kullanılacağı açıklanmaktadır.

Bu makalede, nasıl öğreneceksiniz:

* Kurtarma Hizmetleri kasası oluşturun.
* Dosya paylaşımlarını keşfedin ve yedeklemeleri yapılandırın.
* Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işi çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

* Dosya paylaşımını barındıran depolama hesabıyla aynı bölgede bir [Kurtarma Hizmetleri kasası](#create-a-recovery-services-vault) tanımlayın veya oluşturun.
* Dosya paylaşımının desteklenen depolama hesabı [türlerinden](#limitations-for-azure-file-share-backup-during-preview)birinde bulunduğundan emin olun.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Önizleme sırasında Azure dosya paylaşımı yedeklemesine yönelik sınırlamalar

Azure dosya paylaşımları için yedekleme önizlemede sunulmaktadır. Hem genel amaçlı v1 hem de genel amaçlı v2 depolama hesaplarındaki Azure dosya paylaşımları desteklenir. Azure dosya paylaşımlarını yedeklemenin sınırlamaları şunlardır:

* [Bölge yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) (ZRS) çoğaltma ile depolama hesaplarında Azure dosya paylaşımlarının yedekleme desteği şu anda [bu bölgelerle](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#in-which-geos-can-i-back-up-azure-file-shares)sınırlıdır.
* Azure Yedekleme şu anda Azure dosya paylaşımlarının zamanlanmış bir kez yedeklemesini yapılandırmayı destekler.
* Günlük zamanlanan maksimum yedekleme sayısı birdir.
* Günlük zamanlanan maksimum istek üzerine yedekleme sayısı dörttür.
* Kurtarma Hizmetleri kasanızdaki yedeklemelerin yanlışlıkla silinmesini önlemek için depolama hesabındaki [kaynak kilitlerini](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) kullanın.
* Azure Yedekleme tarafından oluşturulan anlık görüntüleri silmeyin. Anlık görüntüleri siler, kurtarma noktalarının kaybına veya hataların geri yüklenmelerine neden olabilir.
* Azure Yedekleme tarafından korunan dosya paylaşımlarını silmeyin. Geçerli çözüm, dosya paylaşımı silindikten sonra Azure Yedekleme tarafından çekilen tüm anlık görüntüleri siler, böylece tüm geri yükleme noktaları kaybolur.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Depolama çoğaltma değiştirme

Varsayılan olarak, kasalar [coğrafi yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanır.

* Kasa birincil yedekleme mekanizmanızsa, GRS kullanmanızı öneririz.
* Düşük maliyetli bir seçenek olarak [yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) kullanabilirsiniz.

Depolama çoğaltma türünü değiştirmek için:

1. Yeni kasada **Ayarlar** bölümünün altındaki **Özellikler'i** seçin.

1. **Özellikler** sayfasında, **Yedekleme Yapılandırması** **altında, Güncelleştir'i**seçin.

1. Depolama çoğaltma türünü seçin ve **Kaydet'i**seçin.

    ![Yedekleme Yapılandırması'nı güncelleştir](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Kasa ayarlandıktan ve yedek öğeler içeren depolama çoğaltma türünü değiştiremezsiniz. Bunu yapmak istiyorsan, kasayı yeniden oluşturman gerekiyor.
>

## <a name="discover-file-shares-and-configure-backup"></a>Dosya paylaşımlarını keşfedin ve yedeklemeyi yapılandırır

1. Azure [portalında,](https://portal.azure.com/)dosya paylaşımını yedeklemek için kullanmak istediğiniz Kurtarma Hizmetleri kasasını açın.

1. Kurtarma **Hizmetleri kasa** panosunda **+Yedekleme'yi**seçin.

   ![Kurtarma Hizmetleri kasası](./media/backup-afs/recovery-services-vault.png)

    a. **Yedekleme Hedefi'nde,** **iş yükünüz nerede çalışıyor?** **Azure**

    ![Yedekleme hedefi olarak Azure Dosya Paylaşımı'nı seçin](./media/backup-afs/backup-goal.png)

    b.  **Neyi yedeklemek istiyorsunuz?** **Azure File Share**

    c.  Azure dosya paylaşım uzantısını kasaya kaydetmek için **Yedekleme'yi** seçin.

    ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekleme'yi seçin](./media/backup-afs/register-extension.png)

1. **Yedekleme'yi**seçtikten sonra **Yedek** bölmesi açılır ve desteklenen depolama hesapları listesinden bir depolama hesabı seçmeni ister. Ya bu kasayla ilişkililer ya da kasayla aynı bölgede bulunuyorlar, ama henüz kurtarma hizmetleri kasasıyla ilişkilendirilmediler.

   ![Depolama hesabı seçme](./media/backup-afs/select-storage-account.png)

1. Keşfedilen depolama hesapları listesinden bir hesap seçin ve **Tamam'ı**seçin. Azure, yedeklenebilen dosya paylaşımları için depolama hesabında arama lar yapabilir. Dosya paylaşımlarınızı yakın zamanda eklediyseniz ve bunları listede göremiyorsanız, dosya paylaşımların görünmesi için biraz zaman bekleyin.

    ![Dosya paylaşımlarını bulma](./media/backup-afs/discovering-file-shares.png)

1. Dosya **Paylaşımları** listesinden, yedeklemek istediğiniz dosya paylaşımlarından birini veya birkaçını seçin. **Tamam'ı**seçin.

1. Dosya paylaşımlarınızı seçtikten sonra **Yedek** menüsü **Yedekleme ilkesine**geçer. Bu menüden, varolan bir yedekleme ilkesini seçin veya yeni bir ilke oluşturun. Ardından **Yedeklemeyi Etkinleştir'i**seçin.

    ![Yedekleme ilkesini seçin](./media/backup-afs/select-backup-policy.png)

Bir yedekleme ilkesi ayarladıktan sonra, dosya paylaşımlarının anlık görüntüsü zamanlanan zamanda alınır. Kurtarma noktası da seçilen dönem için korunur.

## <a name="create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturma

Bazen, yedekleme ilkesinde zamanlanan saatlerin dışında bir yedekleme anlık görüntüsü veya kurtarma noktası oluşturmak isteyebilirsiniz. İsteğe bağlı yedekleme oluşturmak için yaygın bir neden, yedekleme ilkesini yapılandırdıktan hemen sonradır. Yedekleme ilkesindeki zamanlamaya bağlı olarak, anlık görüntünün alınması saatler veya günler sürebilir. Yedekleme ilkesi devreye girene kadar verilerinizi korumak için, bir isteğe bağlı yedekleme başlatın. Dosya paylaşımlarınızda planlı değişiklikler yapmadan önce genellikle isteğe bağlı yedekleme oluşturmak gerekir.

### <a name="create-a-backup-job-on-demand"></a>İsteğe bağlı yedekleme işi oluşturma

1. Dosya paylaşımınızı yedeklemek için kullandığınız Kurtarma Hizmetleri kasasını açın. Genel **Bakış** bölmesinde, **Korumalı öğeler** bölümünün altındaki **Yedek öğeleri** seçin.

   ![Yedek öğeleri seçin](./media/backup-afs/backup-items.png)

1. **Yedekleme öğelerini**seçtikten sonra, Genel **Bakış** bölmesinin yanında tüm Yedekleme **Yönetimi Türlerini** listeleyen yeni bir bölme görüntülenir.

   ![Yedekleme Yönetim Türleri Listesi](./media/backup-afs/backup-management-types.png)

1. Yedekleme **Yönetimi Türü** listesinden **Azure Depolama (Azure Dosyaları)** seçeneğini belirleyin. Bu kasakullanılarak yedeklenen tüm dosya paylaşımlarının ve ilgili depolama hesaplarının bir listesini görürsünüz.

   ![Azure Depolama (Azure Dosyaları) yedekleme öğeleri](./media/backup-afs/azure-files-backup-items.png)

1. Azure dosya paylaşımları listesinden istediğiniz dosya paylaşımını seçin. **Yedekleme Öğesi** ayrıntıları görüntülenir. Yedek **Öğe** menüsünde, **şimdi Yedek'i**seçin. Bu yedekleme işi isteğe bağlı olduğundan, kurtarma noktasıyla ilişkili bekletme ilkesi yoktur.

   ![Şimdi Yedekleme'yi seçin](./media/backup-afs/backup-now.png)

1. **Şimdi Yedek** bölmesi açılır. Kurtarma noktasını bekletmek istediğiniz son günü belirtin. İsteğe bağlı yedekleme için en fazla 10 yıl bekletme elde edebilirsiniz.

   ![Bekletme tarihini seçin](./media/backup-afs/retention-date.png)

1. Çalışan isteğe bağlı yedekleme işini onaylamak için **Tamam'ı** seçin.

1. Yedekleme işinin tamamlanmasını izlemek için portal bildirimlerini izleyin. Kasa panosunda iş ilerlemesini izleyebilirsiniz. **Devam Ediyor** **Yedek İşler'i** > seçin.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:
* [Azure dosya paylaşımlarını geri yükleme](restore-afs.md)
* [Azure dosya paylaşımı yedeklemelerini yönetme](manage-afs-backup.md)
