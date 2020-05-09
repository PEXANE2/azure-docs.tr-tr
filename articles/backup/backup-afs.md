---
title: Azure portal Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri kasasındaki Azure dosya paylaşımlarını yedeklemek için Azure portal nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: a77f7fd0ec21eae60a7313a9ffa889fbef4372c6
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82978037"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında Azure dosya paylaşımlarını yedekleme

Bu makalede, [Azure dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)yedeklemek için Azure Portal nasıl kullanılacağı açıklanmaktadır.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

* Kurtarma Hizmetleri kasası oluşturun.
* Dosya paylaşımlarını bulma ve yedeklemeleri yapılandırma.
* Bir geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

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

1. [Azure Portal](https://portal.azure.com/), dosya paylaşımının yedeklemesini yapılandırmak için kullanmak Istediğiniz kurtarma hizmetleri kasasını açın.

1. **Kurtarma Hizmetleri Kasası** bölmesinde, üstteki menüden **+ Backup** ' ı seçin.

   ![Kurtarma Hizmetleri kasası](./media/backup-afs/recovery-services-vault.png)

    1. **Yedekleme hedefi** bölmesinde, **Iş yükünüzün nerede çalıştığını** , açılan listeden **Azure** seçeneğini belirleyerek **Azure** 'a ayarlayın.

          ![İş yükü olarak Azure 'u seçin](./media/backup-afs/backup-goal.png)

    2. **Neleri yedeklemek istiyorsunuz?** bölümünde, açılan listeden **Azure dosya paylaşma** ' yı seçin.

          ![Azure FileShare seçin](./media/backup-afs/select-azure-file-share.png)

    3. Azure dosya paylaşımının uzantısını kasaya kaydetmek için **Yedekle** ' yi seçin.

          ![Azure dosya paylaşımının kasasıyla ilişkilendirilmesi için yedekleme 'yi seçin](./media/backup-afs/register-extension.png)

1. **Yedekle**' yi seçtikten sonra **yedekleme** bölmesi açılır. Korumak istediğiniz dosya paylaşımının barındırıldığı depolama hesabını seçmek için, **depolama hesabı** metin kutusunun altındaki bağlantı **Seç** metnine tıklayın.

   ![Seçme bağlantısını seçin](./media/backup-afs/choose-select-link.png)

1. **Depolama hesabı Seç bölmesi** sağ tarafta açılarak, bulunan desteklenen depolama hesaplarının bir kümesini listeler. Bu kasayla ilişkilendirilir veya kasada aynı bölgede bulunur, ancak henüz herhangi bir kurtarma hizmetleri kasasıyla ilişkilendirilmemiştir.

1. Bulunan depolama hesapları listesinden bir hesap seçin ve **Tamam**' ı seçin.

   ![Bulunan depolama hesaplarından seçin](./media/backup-afs/select-discovered-storage-account.png)

1. Sonraki adım, yedeklemek istediğiniz dosya paylaşımlarını seçdir. **Yedeklenecek dosya paylaşımları** bölümünde **Ekle** düğmesine tıklayın.

   ![Yedeklenecek dosya paylaşımlarını seçin](./media/backup-afs/select-file-shares-to-back-up.png)

1. Sağ tarafta **dosya paylaşımları seçin** bağlam bölmesi açılır. Azure, yedeklenebilir dosya paylaşımları için depolama hesabını arar. Dosya paylaşımlarınızı son zamanlarda eklediyseniz ve listede görmüyorsanız, dosya paylaşımlarının görünmesi için bir süre bekleyin.

1. **Dosya paylaşımları Seç** listesinden yedeklemek istediğiniz bir veya daha fazla dosya paylaşımını seçin. **Tamam**’ı seçin.

   ![Dosya paylaşımlarını seçin](./media/backup-afs/select-file-shares.png)

1. Dosya paylaşımınız için bir yedekleme ilkesi seçmek üzere üç seçeneğiniz vardır:

   * Varsayılan ilkeyi seçin.<br>
   Bu seçenek, 30 gün boyunca tutulacak günlük yedeklemeyi etkinleştirmenizi sağlar. Kasada mevcut bir yedekleme ilkeniz yoksa, yedekleme bölmesi varsayılan ilke ayarlarıyla açılır. Varsayılan ayarları seçmek istiyorsanız, doğrudan **yedeklemeyi etkinleştir**' e tıklayabilirsiniz.

   * Yeni ilke oluşturma <br>

      1. Dosya paylaşımınız için yeni bir yedekleme ilkesi oluşturmak üzere **yedekleme ilkesi** bölümündeki aşağı açılan listenin altındaki bağlantı metnine tıklayın.<br>

         ![Yeni ilke oluştur](./media/backup-afs/create-new-policy.png)

      1. **Yedekleme ilkesi** bağlam bölmesi sağ tarafta açılır. Metin kutusunda bir ilke adı belirtin ve gereksinimlerinize göre bekletme dönemini seçin. Yalnızca günlük bekletme seçeneği varsayılan olarak etkindir. Haftalık, aylık veya yıllık saklama kullanmak istiyorsanız, ilgili onay kutusunu seçin ve istenen saklama değerini sağlayın.

      1. Saklama değerlerini ve geçerli bir ilke adını belirttikten sonra Tamam ' a tıklayın.<br>

         ![İlke adı ve bekletme değerleri verme](./media/backup-afs/policy-name.png)

   * Mevcut yedekleme ilkelerinden birini seçin <br>

   Korumayı yapılandırmak için mevcut yedekleme ilkelerinden birini seçmek için **yedekleme ilkesi** açılan listesinden istenen ilkeyi seçin.<br>

   ![Mevcut ilkeyi seçin](./media/backup-afs/choose-existing-policy.png)

1. Dosya paylaşımının korunmasına başlamak için **yedeklemeyi etkinleştir** ' e tıklayın.

   ![Yedeklemeyi Etkinleştir ' i seçin](./media/backup-afs/enable-backup.png)

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
