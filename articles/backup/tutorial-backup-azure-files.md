---
title: Öğretici - Azure Dosyaları dosya paylaşımlarını yedekleme
description: Bu eğitimde, Kurtarma Hizmetleri kasasını yapılandırmak ve Azure dosya paylaşımlarını yedeklemek için Azure portalını nasıl kullanacağınızı öğrenin.
ms.date: 06/10/2019
ms.topic: tutorial
ms.openlocfilehash: ec9074a39f2ece7878c0c3ef828dc21748d0ab89
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76293938"
---
# <a name="back-up-azure-file-shares-in-the-azure-portal"></a>Azure portalındaki Azure dosya paylaşımlarını yedekleme

Bu öğretici, [Azure dosya paylaşımlarını](../storage/files/storage-files-introduction.md)yedeklemek için Azure portalının nasıl kullanılacağını açıklar.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Azure Dosyalarını yedeklemek için Kurtarma Hizmetleri kasasını yapılandırma
> * Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Azure dosya paylaşımını yedekleyebilmeniz için önce [desteklenen Depolama Hesabı türlerinden](tutorial-backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview) birinde yer aldığından emin olmalısınız. Bunu doğruladıktan sonra dosya paylaşımlarınızı koruyabilirsiniz.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Önizleme sırasında Azure dosya paylaşımı yedeklemesine yönelik sınırlamalar

Azure dosya paylaşımları için yedekleme önizlemede sunulmaktadır. Hem genel amaçlı v1 hem de genel amaçlı v2 depolama hesaplarındaki Azure dosya paylaşımları desteklenir. Aşağıdaki yedekleme senaryoları, Azure dosya paylaşımları için desteklenmemektedir:

* Azure Yedekleme'yi kullanarak Azure Dosyalarını korumak için cli yok.
* Günlük zamanlanan maksimum yedekleme sayısı birdir.
* Günlük zamanlanan maksimum istek üzerine yedekleme sayısı dörttür.
* Kurtarma Hizmetleri kasanızdaki yedeklemelerin yanlışlıkla silinmesini önlemek için depolama hesabındaki [kaynak kilitlerini](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) kullanın.
* Azure Backup tarafından oluşturulan anlık görüntülerin silmeyin. Anlık görüntülerin silinmesi, kurtarma noktalarının kaybolması ve/veya geri yükleme işlemlerinin başarısız olmasıyla sonuçlanabilir
* Azure Yedekleme tarafından korunan dosya paylaşımlarını silmeyin. Geçerli çözüm, dosya paylaşımı silindikten sonra Azure Yedekleme tarafından çekilen tüm anlık görüntüleri siler ve dolayısıyla tüm geri yükleme noktalarını kaybeder

[Bölge yedekli depolama](../storage/common/storage-redundancy-zrs.md) (ZRS) çoğaltma ile Depolama HesaplarındaAzure Dosya Paylaşımları için yedekleme şu anda yalnızca Orta ABD (CUS), Doğu ABD (EUS), Doğu ABD 2 (EUS2), Kuzey Avrupa (NE), Güneydoğu Asya (SEA), Batı Avrupa (WE) ve Batı ABD 2 (WUS2) kullanılabilir.

## <a name="configuring-backup-for-an-azure-file-share"></a>Azure dosya paylaşımı için yedeklemeyi yapılandırma

Bu öğreticide zaten yerleşik bir Azure dosya paylaşımınız olduğu varsayılır. Azure dosya paylaşımınızı yedeklemek için:

1. Dosya paylaşımınızla aynı bölgede bir Kurtarma Hizmetleri kasası oluşturun. Zaten bir kasanız varsa, kasanızın Genel Bakış sayfasını açın ve **Yedekle**'ye tıklayın.

    ![Kasanızın Genel Bakış sayfasında Yedekleme'yi tıklatın](./media/tutorial-backup-azure-files/overview-backup-page.png)

2. Yedekleme **Hedefi** menüsünde, **yedeklemek istediğiniz nedir?**

    ![Yedekleme hedefi olarak Azure Dosya Paylaşımı'nı seçin](./media/tutorial-backup-azure-files/choose-azure-fileshare-from-backup-goal.png)

3. Azure dosya paylaşımınızı Kurtarma Hizmetleri kasasına yapılandırmak için **Yedekle**'ye tıklayın.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/tutorial-backup-azure-files/set-backup-goal.png)

    Kasa Azure dosya paylaşımıyla ilişkilendirildikten sonra, Yedekleme menüsü açılır ve bir Depolama hesabı seçmeniz istenir. Menü, kasanızın bulunduğu bölgede, kurtarma hizmetleri kasasıyla zaten ilişkili olmayan tüm desteklenen Depolama Hesaplarını görüntüler.

   ![Depolama hesabınızı seçin](./media/tutorial-backup-azure-files/list-of-storage-accounts.png)

4. Depolama hesapları listesinde bir hesap seçin ve **Tamam**'a tıklayın. Azure, depolama hesabında yedeklenebilecek dosya paylaşımlarını arar. Dosya paylaşımlarınızı kısa süre önce eklediyseniz ve listede görmüyorsanız, dosya paylaşımlarının gösterilmesi için biraz zaman tanıyın.

   ![Dosya Paylaşımları keşfediliyor](./media/tutorial-backup-azure-files/discover-file-shares.png)

5. Dosya **Paylaşımları** listesinden, yedeklemek istediğiniz dosya paylaşımlarından birini veya birkaçını seçin ve **Tamam'ı**tıklatın.

6. Dosya Paylaşımlarınızı seçtikten sonra, Yedekle menüsü **Yedekleme ilkesi**'ne dönüşür. Bu menüde mevcut yedekleme ilkelerinden birini seçin veya yeni ilke oluşturun ve ardından **Yedeklemeyi Etkinleştir**'e tıklayın.

   ![Yedekleme ilkesi seçin veya yeni bir ilke oluşturun](./media/tutorial-backup-azure-files/apply-backup-policy.png)

    Yedekleme ilkesi oluşturulduktan sonra, planlanan zamanda Dosya Paylaşımlarının anlık görüntüsü alınır ve seçilen süre için kurtarma noktası korunur.

## <a name="create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturma

Yedekleme ilkesini yapılandırdıktan sonra, verilerinizin bir sonraki zamanlanmış yedeklemeye kadar korunmasını sağlamak için isteğe bağlı yedekleme oluşturmak istersiniz.

### <a name="to-create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturmak için

1. Dosya paylaşımı kurtarma noktalarını içeren Kurtarma Hizmetleri kasasını açın ve **Yedekleme Öğeleri**'ne tıklayın. Yedekleme Öğesi türlerinin listesi gösterilir.

   ![Yedek Öğeler Listesi](./media/tutorial-backup-azure-files/list-of-backup-items.png)

2. Listeden **Azure Depolama (Azure Dosyaları)** öğesini seçin. Azure dosya paylaşımlarının listesi görüntülenir.

   ![Azure dosya paylaşımları listesi](./media/tutorial-backup-azure-files/list-of-azure-files-backup-items.png)

3. Azure dosya paylaşımları listesinden istediğiniz dosya paylaşımını seçin. Seçili dosya paylaşımının Yedekleme Öğesi menüsü açılır.

   ![Seçili dosya paylaşımı için Yedek Öğe menüsü](./media/tutorial-backup-azure-files/backup-item-menu.png)

4. Yedekleme Öğesi menüsünde **Şimdi Yedekle**'ye tıklayın. Bu isteğe bağlı bir yedekleme işi olduğundan, kurtarma noktasıyla ilişkilendirilmiş bir bekletme ilkesi yoktur. **Şimdi Yedekle** iletişim kutusu açılır. Kurtarma noktasını bekletmek istediğiniz son günü belirtin.

   ![Kurtarma noktası bekletme için tarihi seçin](./media/tutorial-backup-azure-files/backup-now-menu.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalı kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
>
> * Azure Dosyalarını yedeklemek için Kurtarma Hizmetleri kasasını yapılandırma
> * Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırma

Azure dosya paylaşımının yedeklemesinden geri yüklemek için sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Azure dosya paylaşımlarının yedeklemesinden geri yükleme](restore-afs.md)
