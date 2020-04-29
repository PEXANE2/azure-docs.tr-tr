---
title: Azure dosya paylaşımlarını geri yükleme
description: Bir dosya paylaşımının tamamını veya belirli dosyaları Azure Backup tarafından oluşturulan bir geri yükleme noktasından geri yüklemek için Azure portal nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 01/12/2020
ms.openlocfilehash: c22078ebd89f5f6f8299e1424d4d9e21edce8b92
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77586960"
---
# <a name="restore-azure-file-shares"></a>Azure dosya paylaşımlarını geri yükleme

Bu makalede, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)tarafından oluşturulan bir geri yükleme noktasından bir dosya paylaşımının tamamını veya belirli dosyaları geri yüklemek için Azure Portal nasıl kullanılacağı açıklanmaktadır.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

* Tam bir Azure dosya paylaşımının geri yüklenmesi.
* Dosya veya klasörleri tek tek geri yükleyin.
* Geri yükleme işlemi durumunu izleyin.

## <a name="steps-to-perform-a-restore-operation"></a>Geri yükleme işlemi gerçekleştirme adımları

Geri yükleme işlemi gerçekleştirmek için aşağıdaki adımları izleyin.

### <a name="select-the-file-share-to-restore"></a>Geri yüklenecek dosya payını seçin

1. [Azure Portal](https://portal.azure.com/), dosya paylaşımının yedeklemesini yapılandırmak Için kullandığınız kurtarma hizmetleri kasasını açın.

1. Genel Bakış bölmesinde, **korumalı öğeler** bölümünde **yedekleme öğeleri** ' ni seçin.

    ![Yedekleme öğelerini seçin](./media/restore-afs/backup-items.png)

1. **Yedekleme öğeleri**' ni seçtikten sonra, genel bakış bölmesinin yanında tüm yedekleme yönetimi türlerini listeleyen yeni bir bölme açılır.

    ![Yedekleme yönetim türleri](./media/restore-afs/backup-management.png)

1. **Yedekleme öğeleri** bölmesinde, **yedekleme yönetimi türü**altında **Azure depolama (Azure dosyaları)** öğesini seçin. Bu kasa kullanılarak yedeklenen tüm dosya paylaşımlarının ve bunlara karşılık gelen depolama hesaplarının bir listesini görürsünüz.

    ![Tüm dosya paylaşımlarının listesi](./media/restore-afs/file-shares.png)

1. Azure dosya paylaşımları listesinden, geri yükleme işlemini gerçekleştirmek istediğiniz dosya paylaşımını seçin.

### <a name="full-share-recovery"></a>Tam paylaşma kurtarması

Dosya paylaşımının tamamını özgün konuma veya alternatif bir konuma geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

1. Geri yüklenecek dosya paylaşımından dosya paylaşımının 5. [adımında, geri](#select-the-file-share-to-restore) yüklenecek dosya payını seçtikten sonra görüntülenen **yedekleme öğesi** bölmesindeki **paylaşma içeriğini geri yükle** seçeneğini belirleyin.

   ![Paylaşma geri yükle seçeneğini belirleyin](./media/restore-afs/restore-share.png)

1. **Paylaşma**'yı seçtikten **sonra geri yükleme bölmesi,** seçili dosya paylaşımında kullanılabilen geri yükleme noktalarının bir listesini görüntüleyen bir **geri yükleme noktası** menüsüyle açılır.

1. Geri yükleme işlemini gerçekleştirmek için kullanmak istediğiniz geri yükleme noktasını seçin ve **Tamam**' ı seçin.

    ![Geri yükleme noktası seç](./media/restore-afs/restore-point.png)

1. **Tamam**' ı seçtikten sonra **geri yükleme** bölmesi menüsü **geri yükleme konumuna**geçer. **Geri yükleme konumu**' nda, verilerin nereye veya nasıl geri yükleneceğini belirtin. Aşağıdaki iki seçenekten birini seçin:

    * **Özgün konum**: dosya paylaşımının tamamını özgün kaynakla aynı konuma geri yükleyin.
    * **Alternatif konum**: dosya paylaşımının tamamını alternatif bir konuma geri yükleyin ve özgün dosya paylaşımının olduğu gibi tutun.

#### <a name="restore-to-the-original-location"></a>Özgün konuma geri yükleme

1. **Kurtarma hedefi**olarak **özgün konum** ' u seçin ve çakışmalar varsa, atlayıp atlayamayacağını veya üzerine yaz ' ı seçin. Uygun seçimi yaptıktan sonra **Tamam**' ı seçin.

    ![Özgün konum seç](./media/restore-afs/original-location.png)

1. Geri yükleme işlemini başlatmak için **geri yükle ' yi seçin.**

    ![Başlamak için geri yükle 'yi seçin](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. **Kurtarma hedefi**olarak **alternatif konum** ' u seçin.
1. Yedeklenen içeriği **depolama hesabı** açılır listesinden geri yüklemek istediğiniz hedef depolama hesabını seçin.
1. **Dosya Paylaşımı Seç** açılan listesi, adım 2 ' de seçtiğiniz depolama hesabında bulunan dosya paylaşımlarını görüntüler. Yedeklenen içeriği geri yüklemek istediğiniz dosya payını seçin.
1. **Klasör adı** kutusunda, geri yüklenen içerikle hedef dosya paylaşımında oluşturmak istediğiniz bir klasör adı belirtin.
1. Çakışmalar varsa atlayın veya üzerine yaz seçeneğini belirleyin.
1. Tüm kutulara uygun değerleri girdikten sonra **Tamam**' ı seçin.

    ![Alternatif Konum Seç](./media/restore-afs/alternate-location.png)

1. Geri yükleme işlemini başlatmak için **geri yükle ' yi seçin.**

    ![Başlamak için geri yükle 'yi seçin](./media/restore-afs/click-restore.png)

### <a name="item-level-recovery"></a>Öğe düzeyinde kurtarma

Özgün konumdaki veya alternatif bir konumdaki tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

1. Geri yüklenecek dosya **yedeklemesini** [Seç](#select-the-file-share-to-restore) bölümünün 5. adımında geri yüklenecek dosya paylaşımından açılan dosya **Kurtarma** seçeneğini belirleyin.

    ![Dosya kurtarma seçin](./media/restore-afs/file-recovery.png)

1. **Dosya kurtarma**' yı seçtikten sonra **geri** yükleme bölmesi, seçili dosya paylaşımında kullanılabilen geri yükleme noktalarının listesini görüntüleyen bir **geri yükleme noktası** menüsüyle açılır.

1. Geri yükleme işlemini gerçekleştirmek için kullanmak istediğiniz geri yükleme noktasını seçin ve **Tamam**' ı seçin.

    ![Geri yükleme noktası seç](./media/restore-afs/restore-point.png)

1. **Tamam**' ı seçtikten sonra geri yükleme bölmesi menüsü **geri yükleme konumuna**geçer. **Geri yükleme konumu**' nda, verilerin nereye veya nasıl geri yükleneceğini belirtin. Aşağıdaki iki seçenekten birini seçin:

    * **Özgün konum**: seçili dosya veya klasörleri özgün kaynakla aynı dosya paylaşımında geri yükleyin.
    * **Alternatif konum**: seçili dosya veya klasörleri alternatif bir konuma geri yükleyin ve özgün dosya paylaşma içeriğini olduğu gibi tutun.

#### <a name="restore-to-the-original-location"></a>Özgün konuma geri yükleme

1. **Kurtarma hedefi**olarak **özgün konum** ' u seçin ve çakışmalar varsa, atlayıp atlayamayacağını veya üzerine yaz ' ı seçin.

    ![Öğe düzeyinde kurtarma için özgün konum](./media/restore-afs/original-location-item-level.png)

1. Geri yüklemek istediğiniz dosya veya klasörleri seçmek için **Dosya Seç** ' i seçin.

    ![Dosya Seç ' i seçin](./media/restore-afs/select-file.png)

1. **Dosya Seç**' i seçtikten sonra bir dosya paylaşma bölmesi, geri yükleme için seçtiğiniz dosya paylaşma kurtarma noktasının içeriğini görüntüler.

1. Geri yüklemek istediğiniz dosya veya klasöre karşılık gelen onay kutusunu seçin ve **Seç**' i seçin.

    ![Dosya veya klasör seçin](./media/restore-afs/select-file-folder.png)

1. Geri yükleme için birden çok dosya veya klasör seçmek üzere 2 ile 4 arasındaki adımları tekrarlayın.
1. Geri yüklemek istediğiniz tüm öğeleri seçtikten sonra **Tamam**' ı seçin.

    ![Geri yüklenecek tüm öğeleri seçtikten sonra Tamam ' ı seçin.](./media/restore-afs/after-selecting-items.png)

1. Geri yükleme işlemini başlatmak için **geri yükle ' yi seçin.**

    ![Başlamak için geri yükle 'yi seçin](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. **Kurtarma hedefi**olarak **alternatif konum** ' u seçin.
1. Yedeklenen içeriği **depolama hesabı** açılır listesinden geri yüklemek istediğiniz hedef depolama hesabını seçin.
1. **Dosya Paylaşımı Seç** açılan listesi, adım 2 ' de seçtiğiniz depolama hesabında bulunan dosya paylaşımlarını görüntüler. Yedeklenen içeriği geri yüklemek istediğiniz dosya payını seçin.
1. **Klasör adı** kutusunda, geri yüklenen içerikle hedef dosya paylaşımında oluşturmak istediğiniz bir klasör adı belirtin.
1. Çakışmalar varsa atlayın veya üzerine yaz seçeneğini belirleyin.
1. Geri yüklemek istediğiniz dosya veya klasörleri seçmek için **Dosya Seç** ' i seçin.

    ![Alternatif konuma geri yüklenecek öğeleri seçin](./media/restore-afs/restore-to-alternate-location.png)

1. **Dosya Seç**' i seçtiğinizde, bir dosya paylaşma bölmesi, geri yükleme için seçtiğiniz dosya paylaşma kurtarma noktasının içeriğini görüntüler.
1. Geri yüklemek istediğiniz dosya veya klasöre karşılık gelen onay kutusunu seçin ve **Seç**' i seçin.

    ![Kurtarma hedefini seçin](./media/restore-afs/recovery-destination.png)

1. Geri yükleme için birden çok dosya veya klasör seçmek üzere 6 ile 8 arasındaki adımları yineleyin.
1. Geri yüklemek istediğiniz tüm öğeleri seçtikten sonra **Tamam**' ı seçin.

    ![Tüm dosyaları seçtikten sonra Tamam ' ı seçin](./media/restore-afs/after-selecting-all-items.png)

1. Geri yükleme işlemini başlatmak için **geri yükle ' yi seçin.**

## <a name="track-a-restore-operation"></a>Geri yükleme işlemini izleme

Geri yükleme işlemini tetikledikten sonra, yedekleme hizmeti izleme için bir iş oluşturur. Azure Backup portalda iş hakkında bildirimleri görüntüler. İşle ilgili işlemleri görüntülemek için bildirimler köprüsünü seçin.

![Bildirimleri Seç köprü](./media/restore-afs/notifications-link.png)

Kurtarma Hizmetleri kasasından geri yükleme ilerlemesini de izleyebilirsiniz:

1. Kurtarma Hizmetleri kasasını geri yükleme işlemini tetiklediğiniz yerden açın.
1. Farklı iş yüklerinde çalışan işlemlerin durumunu görmek için genel bakış bölmesinde **izleme** bölümü altında **yedekleme işleri** ' ni seçin.

    ![Yedekleme Işlerini seçin](./media/restore-afs/backup-jobs.png)

1. Geri yükleme işlemi hakkında, **aktarılan veriler** ve **geri yüklenen dosya sayısı**gibi daha fazla ayrıntı görüntülemek için dosya paylaşımınıza karşılık gelen iş yükü adını seçin.

    ![Geri yüklenen Ayrıntıları gör](./media/restore-afs/restore-details.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşma yedeklemelerini yönetmeyi](manage-afs-backup.md)öğrenin.
