---
title: Azure portal Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri kasasındaki Azure dosya paylaşımlarını yedeklemek için Azure portal nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: da2c7fa4cc5c3b7b948604a6f6d3999671cb3697
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82101336"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında Azure dosya paylaşımlarını yedekleme

Bu makalede, [Azure dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)yedeklemek için Azure Portal nasıl kullanılacağı açıklanmaktadır.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

* Kurtarma Hizmetleri kasası oluşturun.
* Dosya paylaşımlarını bulma ve yedeklemeleri yapılandırma.
* Bir geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

* Dosya paylaşımının barındırılmasına yönelik depolama hesabıyla aynı bölgede bir [Kurtarma Hizmetleri Kasası](#create-a-recovery-services-vault) oluşturun veya oluşturun.
* Dosya paylaşımının [Desteklenen depolama hesabı türlerinden](azure-file-share-support-matrix.md)birinde bulunduğundan emin olun.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Depolama çoğaltmasını değiştirme

Varsayılan olarak, [kasalar coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanır.

* Kasa birincil yedekleme mekanizmanız ise GRS kullanmanızı öneririz.
* [Yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) seçeneğini düşük maliyetli bir seçenek olarak kullanabilirsiniz.

Depolama çoğaltma türünü değiştirmek için:

1. Yeni kasada, **Ayarlar** bölümünün altındaki **Özellikler** ' i seçin.

1. **Özellikler** sayfasında, **yedekleme yapılandırması**altında **Güncelleştir**' i seçin.

1. Depolama çoğaltma türünü seçin ve **Kaydet**' i seçin.

    ![Yedekleme yapılandırmasını Güncelleştir](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Kasa ayarlandıktan ve yedekleme öğeleri içerdiğinde depolama çoğaltma türünü değiştiremezsiniz. Bunu yapmak istiyorsanız, kasayı yeniden oluşturmanız gerekir.
>

## <a name="discover-file-shares-and-configure-backup"></a>Dosya paylaşımlarını bulma ve yedeklemeyi yapılandırma

1. [Azure Portal](https://portal.azure.com/), dosya paylaşımının yedeklenmesini sağlamak için kullanmak Istediğiniz kurtarma hizmetleri kasasını açın.

1. **Kurtarma Hizmetleri Kasası** panosunda **+ yedekleme**' yi seçin.

   ![Kurtarma Hizmetleri kasası](./media/backup-afs/recovery-services-vault.png)

    a. **Yedekleme hedefi**' nde, **iş yükünüzün çalıştığı yeri** **Azure**'da belirleyin.

    ![Azure dosya paylaşma 'yı yedekleme hedefi olarak seçme](./media/backup-afs/backup-goal.png)

    b.  **Neleri yedeklemek istiyorsunuz?** bölümünde, açılan listeden **Azure dosya paylaşma** ' yı seçin.

    c.  Azure dosya paylaşımının uzantısını kasaya kaydetmek için **Yedekle** ' yi seçin.

    ![Azure dosya paylaşımının kasasıyla ilişkilendirilmesi için yedekleme 'yi seçin](./media/backup-afs/register-extension.png)

1. **Yedekleme**seçeneğini belirledikten sonra, **yedekleme** bölmesi açılır ve bulunan desteklenen depolama hesapları listesinden bir depolama hesabı seçmenizi ister. Bu kasayla ilişkilendirilir veya kasada aynı bölgede bulunur, ancak henüz herhangi bir kurtarma hizmetleri kasasıyla ilişkilendirilmemiştir.

1. Bulunan depolama hesapları listesinden bir hesap seçin ve **Tamam**' ı seçin. Azure, yedeklenebilir dosya paylaşımları için depolama hesabını arar. Dosya paylaşımlarınızı son zamanlarda eklediyseniz ve listede görmüyorsanız, dosya paylaşımlarının görünmesi için bir süre bekleyin.

    ![Dosya paylaşımları bulunuyor](./media/backup-afs/discovering-file-shares.png)

1. **Dosya paylaşımları** listesinde, yedeklemek istediğiniz bir veya daha fazla dosya paylaşımını seçin. **Tamam ' ı**seçin.

   ![Dosya paylaşımlarını seçin](./media/backup-afs/select-file-shares.png)

1. Dosya paylaşımlarınızı seçtikten sonra, **yedekleme** menüsü **yedekleme ilkesi**' ne geçer. Bu menüden, mevcut bir yedekleme ilkesi seçin ya da yeni bir tane oluşturun. Sonra **yedeklemeyi etkinleştir**' i seçin.

    ![Yedekleme ilkesi seçin](./media/backup-afs/select-backup-policy.png)

Bir yedekleme İlkesi ayarladıktan sonra, dosya paylaşımlarının bir anlık görüntüsü zamanlanan saatte alınır. Kurtarma noktası, seçilen dönem için de korunur.

>[!NOTE]
>Azure Backup artık Azure dosya paylaşma yedeklemesi için günlük/haftalık/aylık/yıllık saklama ilkelerini destekler.

## <a name="create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturma

Bazen yedekleme ilkesinde zamanlanan saatlerin dışında bir yedekleme anlık görüntüsü veya kurtarma noktası oluşturmak isteyebilirsiniz. Yedekleme ilkesini yapılandırdıktan sonra, isteğe bağlı yedekleme oluşturmak için sık karşılaşılan bir neden vardır. Yedekleme ilkesindeki zamanlamaya göre, bir anlık görüntü alınana kadar saat veya gün olabilir. Yedekleme ilkesi devreye girene kadar verilerinizi korumak için, bir isteğe bağlı yedekleme başlatın. Dosya paylaşımlarınız üzerinde planlı değişiklikler yapmadan önce isteğe bağlı yedekleme oluşturmak genellikle gereklidir.

### <a name="create-a-backup-job-on-demand"></a>İsteğe bağlı bir yedekleme işi oluşturun

1. Dosya paylaşımınızı yedeklemek için kullandığınız kurtarma hizmetleri kasasını açın. **Genel bakış** bölmesinde, **korumalı öğeler** bölümünde **yedekleme öğeleri** ' ni seçin.

   ![Yedekleme öğelerini seçin](./media/backup-afs/backup-items.png)

1. **Yedekleme öğeleri**' ni seçtikten sonra, **genel bakış** bölmesinin yanında tüm **yedekleme yönetimi türlerini** listeleyen yeni bir bölmesi görüntülenir.

   ![Yedekleme yönetim türlerinin listesi](./media/backup-afs/backup-management-types.png)

1. **Yedekleme yönetimi türü** listesinden **Azure depolama (Azure dosyaları)** öğesini seçin. Tüm dosya paylaşımlarının ve bu kasa kullanılarak yedeklenen karşılık gelen depolama hesaplarının bir listesini görürsünüz.

   ![Azure depolama (Azure dosyaları) yedekleme öğeleri](./media/backup-afs/azure-files-backup-items.png)

1. Azure dosya paylaşımları listesinden istediğiniz dosya paylaşımını seçin. **Yedekleme öğesi** ayrıntıları görüntülenir. **Yedekleme öğesi** menüsünde **Şimdi Yedekle**' yi seçin. Bu yedekleme işi isteğe bağlı olduğundan, kurtarma noktasıyla ilişkili bir bekletme ilkesi yok.

   ![Şimdi Yedekle 'yi seçin](./media/backup-afs/backup-now.png)

1. **Şimdi Yedekle** bölmesi açılır. Kurtarma noktasını bekletmek istediğiniz son günü belirtin. İsteğe bağlı yedekleme için en fazla 10 yıl bekletmeye sahip olabilirsiniz.

   ![Bekletme tarihi seçin](./media/backup-afs/retention-date.png)

1. Çalışan isteğe bağlı yedekleme işini onaylamak için **Tamam ' ı** seçin.

1. Yedekleme işi çalıştırma işleminin tamamlanmasını izlemek için Portal bildirimlerini izleyin. İş ilerlemesini kasa panosunda izleyebilirsiniz. **Devam eden** **yedekleme işleri** > seçin.

>[!NOTE]
>Azure Backup, karşılık gelen hesaptaki herhangi bir dosya paylaşımının korumasını yapılandırırken depolama hesabını kilitler. Bu, yedeklenen dosya paylaşımlarına sahip bir depolama hesabının yanlışlıkla silinmesine karşı koruma sağlar.

## <a name="best-practices"></a>En iyi uygulamalar

* Azure Backup tarafından oluşturulan anlık görüntüleri silmeyin. Anlık görüntülerin silinmesi, kurtarma noktalarının kaybolması ve/veya geri yükleme işlemlerinin başarısız olmasıyla sonuçlanabilir

* Azure Backup tarafından depolama hesabında gerçekleştirilen kilidi kaldırmayın. Kilidi silerseniz, depolama hesabınız yanlışlıkla silinmeye açıktır ve siliniyorsa, anlık görüntülerinizi veya yedeklerinizi kaybedersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:

* [Azure dosya paylaşımlarını geri yükleme](restore-afs.md)
* [Azure dosya paylaşımı yedeklemelerini yönetme](manage-afs-backup.md)
