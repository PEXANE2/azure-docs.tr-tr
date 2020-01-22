---
title: Azure portal Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri kasasındaki Azure dosya paylaşımlarını yedeklemek için Azure portal nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: b2e2053857147513a95b3ae72b82d55450ebcffa
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294519"
---
# <a name="back-up-azure-file-shares-in-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında Azure dosya paylaşımlarını yedekleme

Bu makalede, [Azure dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)yedeklemek için Azure Portal nasıl kullanılacağı açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:

* Kurtarma Hizmetleri kasası oluşturma
* Dosya paylaşımlarını bulma ve yedeklemeleri yapılandırma
* Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırma

## <a name="prerequisites"></a>Ön koşullar

* Dosya paylaşımının barındırıldığı depolama hesabıyla aynı bölgede bir [Kurtarma Hizmetleri Kasası](#create-a-recovery-services-vault) oluşturun veya oluşturun.

* Dosya paylaşımının [Desteklenen depolama hesabı türlerinden](#limitations-for-azure-file-share-backup-during-preview)birinde bulunduğundan emin olun.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Önizleme sırasında Azure dosya paylaşımı yedeklemesine yönelik sınırlamalar

Azure dosya paylaşımları için yedekleme önizleme aşamasındadır. Hem genel amaçlı v1 hem de genel amaçlı v2 depolama hesaplarında Azure dosya paylaşımları desteklenir. Bunlar Azure dosya paylaşımlarının yedeklenmesi ile ilgili sınırlamalardır:

* [Bölge yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) (ZRS) çoğaltması olan depolama hesaplarında Azure dosya paylaşımlarının yedeklenmesi desteği şu anda [Bu bölgelerle](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#in-which-geos-can-i-back-up-azure-file-shares)sınırlıdır.
* Azure Backup Şu anda Azure dosya paylaşımlarının zamanlanan bir kez günlük yedeklemesini yapılandırmayı destekliyor.
* Günlük zamanlanan maksimum yedekleme sayısı birdir.
* Günlük zamanlanan maksimum istek üzerine yedekleme sayısı dörttür.
* Kurtarma Hizmetleri kasanızdaki yedeklemelerin yanlışlıkla silinmesini önlemek için depolama hesabındaki [kaynak kilitlerini](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) kullanın.
* Azure Backup tarafından oluşturulan anlık görüntülerin silmeyin. Anlık görüntülerin silinmesi, kurtarma noktalarının kaybolması ve/veya geri yükleme işlemlerinin başarısız olmasıyla sonuçlanabilir
* Azure Backup tarafından korunan dosya paylaşımlarını silmeyin. Geçerli çözüm, dosya paylaşımının silindiği ve bu nedenle tüm geri yükleme noktalarının kaybolacağı bir Azure Backup tarafından alınan tüm anlık görüntüleri silecek.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Depolama çoğaltmasını değiştirme

Varsayılan olarak, [kasalar coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanır.

* Kasa birincil yedekleme mekanizmanız ise GRS kullanmanızı öneririz.

* [Yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) seçeneğini düşük maliyetli bir seçenek olarak kullanabilirsiniz.

Depolama çoğaltma türünü aşağıdaki gibi değiştirin:

1. Yeni kasada, **Ayarlar** bölümünde **Özellikler** ' e tıklayın.

2. **Özellikler**' de, **yedekleme yapılandırması**altında **Güncelleştir**' e tıklayın.

3. Depolama çoğaltma türünü seçin ve **Kaydet**' e tıklayın.

    ![Yedekleme yapılandırmasını Güncelleştir](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Kasa ayarlandıktan ve yedekleme öğeleri içerdiğinde depolama çoğaltma türünü değiştiremezsiniz. Bunu yapmak istiyorsanız kasayı yeniden oluşturmanız gerekir.
>

## <a name="discover-file-shares-and-configure-backup"></a>Dosya paylaşımlarını bulma ve yedeklemeyi yapılandırma

1. [Azure Portal](https://portal.azure.com/), dosya paylaşımının yedeklenmesini sağlamak için kullanmak Istediğiniz kurtarma hizmetleri kasasını açın.

2. **Kurtarma Hizmetleri Kasası** panosunda **+ yedekleme**' yi seçin.

   ![Kurtarma Hizmetleri Kasası](./media/backup-afs/recovery-services-vault.png)

   a. **Yedekleme hedefi**' nde, **iş yükünüzün çalıştığı yeri** **Azure**'da belirleyin.

    ![Azure dosya paylaşma 'yı yedekleme hedefi olarak seçme](./media/backup-afs/backup-goal.png)

    b.    **Neleri yedeklemek**istiyorsunuz menüsünde, açılan menüden **Azure dosya paylaşma** ' yı seçin.

    c.    Azure dosya paylaşma uzantısını kasaya kaydetmek için **Yedekle** ' ye tıklayın.

      ![Azure dosya paylaşımından kasa ile ilişkilendirmek için Yedekle 'ye tıklayın](./media/backup-afs/register-extension.png)

3. **Yedekle**' ye tıkladığınızda, yedekleme dikey penceresi açılır ve bulunan desteklenen depolama hesapları listesinden bir depolama hesabı seçmenizi ister. Bu kasayla ilişkilendirilir veya kasada aynı bölgede bulunur, ancak henüz herhangi bir kurtarma hizmetleri kasasıyla ilişkilendirilmemiştir.

   ![Depolama hesabı seçme](./media/backup-afs/select-storage-account.png)

4. Bulunan depolama hesapları listesinden bir hesap seçin ve **Tamam**' ı tıklatın. Azure, depolama hesabında yedeklenebilecek dosya paylaşımlarını arar. Dosya paylaşımlarınızı son zamanlarda eklediyseniz ve listede görmüyorsanız, dosya paylaşımlarının görünmesi biraz zaman ayırın.

    ![Dosya paylaşımları bulunuyor](./media/backup-afs/discovering-file-shares.png)

5. **Dosya paylaşımları** listesinde, yedeklemek istediğiniz bir veya daha fazla dosya paylaşımını seçin ve **Tamam**' a tıklayın.

6. Dosya paylaşımlarınızı seçtikten sonra, **yedekleme** menüsü **yedekleme ilkesi**' ne geçer. Bu menüden, var olan bir yedekleme ilkesi seçin ya da yeni bir tane oluşturun ve ardından **yedeklemeyi etkinleştir**' e tıklayın.

    ![Yedekleme ilkesi seçin](./media/backup-afs/select-backup-policy.png)

Yedekleme ilkesi oluşturulduktan sonra, planlanan zamanda Dosya Paylaşımlarının anlık görüntüsü alınır ve seçilen süre için kurtarma noktası korunur.

## <a name="create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturma

Bazen yedekleme ilkesinde zamanlanan saatlerin dışında bir yedekleme anlık görüntüsü veya kurtarma noktası oluşturmak isteyebilirsiniz. Yedekleme ilkesini yapılandırdıktan sonra, isteğe bağlı yedekleme oluşturmak için sık karşılaşılan bir neden vardır. Yedekleme ilkesindeki zamanlamaya bağlı olarak, bir anlık görüntünün alınması için saatler veya günler geçebilir. Yedekleme ilkesi devreye girene kadar verilerinizi korumak için, bir isteğe bağlı yedekleme başlatın. Dosya paylaşımlarınız üzerinde planlı değişiklikler yapmadan önce isteğe bağlı yedekleme oluşturmak genellikle gereklidir.

### <a name="to-create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturmak için

1. Dosya paylaşımınızı yedeklemek için kullandığınız kurtarma hizmetleri kasasını açın ve **genel bakış** dikey penceresinin **korumalı öğeler** bölümünde **yedekleme öğeleri** ' ne tıklayın.

   ![Yedekleme öğeleri ' ne tıklayın](./media/backup-afs/backup-items.png)

2. **Yedekleme öğeleri**' ne tıkladığınızda, **genel bakış** dikey penceresinin yanında, tüm **yedekleme yönetimi türlerini** listelemek için yeni bir dikey pencere aşağıda gösterildiği gibi görüntülenir:

   ![Yedekleme yönetim türlerinin listesi](./media/backup-afs/backup-management-types.png)

3. **Yedekleme yönetim türleri**listesinden **Azure depolama (Azure dosyaları)** öğesini seçin. Tüm dosya paylaşımlarının ve bu kasa kullanılarak yedeklenen karşılık gelen depolama hesaplarının bir listesini görürsünüz.

   ![Azure depolama (Azure dosyaları) yedekleme öğeleri](./media/backup-afs/azure-files-backup-items.png)

4. Azure dosya paylaşımları listesinden istediğiniz dosya paylaşımını seçin. **Yedekleme öğesi** ayrıntıları görüntülenir. **Yedekleme öğesi** menüsünde **Şimdi Yedekle**' ye tıklayın. Bu isteğe bağlı bir yedekleme işi olduğundan, kurtarma noktasıyla ilişkilendirilmiş bir bekletme ilkesi yoktur.

   ![Şimdi Yedekle 'ye tıklayın](./media/backup-afs/backup-now.png)

5. **Şimdi Yedekle** dikey penceresi açılır. Kurtarma noktasını bekletmek istediğiniz son günü belirtin. İsteğe bağlı yedekleme için en fazla 10 yıl bekletmeye sahip olabilirsiniz.

   ![Bekletme tarihi seçin](./media/backup-afs/retention-date.png)

6. İsteğe bağlı yedekleme işinin çalıştırılmasını onaylamak için **Tamam** ' ı tıklatın.

7. Yedekleme işi çalıştırma işleminin tamamlanmasını izlemek için Portal bildirimlerini izleyin. İş ilerlemesini kasa panosunda izleyebilirsiniz > **yedekleme işleri** > **devam**ediyor.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşımlarını nasıl geri](restore-afs.md) yükleyeceğinizi öğrenin

* [Azure dosya paylaşımının yedeklerini yönetme](manage-afs-backup.md) hakkında bilgi edinin
