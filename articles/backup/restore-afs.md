---
title: Azure dosya paylaşımlarını geri yükleme
description: Azure Yedekleme tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımını veya belirli dosyaları geri yüklemek için Azure portalını nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 01/12/2020
ms.openlocfilehash: c22078ebd89f5f6f8299e1424d4d9e21edce8b92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586960"
---
# <a name="restore-azure-file-shares"></a>Azure dosya paylaşımlarını geri yükleme

Bu makalede, [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview)tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımını veya belirli dosyaları geri yüklemek için Azure portalının nasıl kullanılacağı açıklanmaktadır.

Bu makalede, nasıl öğreneceksiniz:

* Tam bir Azure dosya paylaşımını geri yükleyin.
* Tek tek dosyaları veya klasörleri geri yükleyin.
* Geri yükleme işlemi durumunu izleyin.

## <a name="steps-to-perform-a-restore-operation"></a>Geri yükleme işlemi gerçekleştirmek için adımlar

Geri yükleme işlemi gerçekleştirmek için aşağıdaki adımları izleyin.

### <a name="select-the-file-share-to-restore"></a>Geri yüklemek için dosya paylaşımını seçin

1. Azure [portalında,](https://portal.azure.com/)dosya paylaşımı için yedeklemeyapılandırmak için kullandığınız Kurtarma Hizmetleri kasasını açın.

1. Genel bakış bölmesinde, **Korumalı öğeler** bölümünün altındaki **Yedek öğeleri** seçin.

    ![Yedek öğeleri seçin](./media/restore-afs/backup-items.png)

1. **Yedekleme öğelerini**seçtikten sonra, genel bakış bölmesinin yanında tüm yedekleme yönetim türlerini listeleyen yeni bir bölme açılır.

    ![Yedekleme yönetimi türleri](./media/restore-afs/backup-management.png)

1. Yedekleme **Öğeleri** bölmesinde, **Yedekleme Yönetim Türü**altında Azure **Depolama (Azure Dosyaları) seçeneğini**belirleyin. Bu kasayı kullanarak yedeklenen tüm dosya paylaşımlarının ve bunların karşılık gelen depolama hesaplarının bir listesini görürsünüz.

    ![Tüm dosya paylaşımları listesi](./media/restore-afs/file-shares.png)

1. Azure dosya paylaşımları listesinden, geri yükleme işlemini gerçekleştirmek istediğiniz dosya paylaşımını seçin.

### <a name="full-share-recovery"></a>Tam hisse kurtarma

Bu geri yükleme seçeneğini, özgün konumda veya alternatif bir konumda dosya paylaşımının tamamını geri yüklemek için kullanabilirsiniz.

1. Geri **yükleme** bölümüne dosya [paylaşımını](#select-the-file-share-to-restore) geri yüklemek için dosya paylaşımını seçtikten sonra görünen **Yedek Öğe** bölmesinde Geri Yükle Seçeneğini belirleyin.

   ![Paylaş'ı Geri Yükle'yi Seçin](./media/restore-afs/restore-share.png)

1. **Geri Yükle Paylaş'ı**seçtikten sonra Geri **Yükleme** bölmesi, seçili dosya paylaşımı için kullanılabilen geri yükleme noktalarının listesini görüntüleyen Geri **Yükleme Noktası** menüsüyle açılır.

1. Geri yükleme işlemini gerçekleştirmek için kullanmak istediğiniz geri yükleme noktasını seçin ve **Tamam'ı**seçin.

    ![Geri yükleme noktasını seçin](./media/restore-afs/restore-point.png)

1. **Tamam'ı**seçtikten sonra, **Geri Yükle** bölmesi menüsü Konumu **Geri Yükle'ye**geçer. **Konumu Geri Yükle'de,** verilerin nerede ve nasıl geri yüklenir olduğunu belirtin. Aşağıdaki iki seçenekten birini seçin:

    * **Özgün Konum**: Dosya paylaşımının tamamını özgün kaynakla aynı konuma geri yükleyin.
    * **Alternatif Konum**: Dosya paylaşımının tamamını alternatif bir konuma geri yükleyin ve özgün dosya paylaşımını olduğu gibi tutun.

#### <a name="restore-to-the-original-location"></a>Özgün konuma geri yükleme

1. **Kurtarma Hedefi**olarak **Özgün Konum'u** seçin ve çakışma varsa atlayıp atlanın veya üzerine yazmayacağınız konusunda seçin. Uygun seçimi yaptıktan sonra **Tamam'ı**seçin.

    ![Orijinal Konumu Seçin](./media/restore-afs/original-location.png)

1. Geri yükleme işlemini başlatmak için **Geri Yükle'yi** seçin.

    ![Başlatmak için Geri Yükle'yi seçin](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. **Kurtarma Hedefi**Olarak **Alternatif Konum'u** seçin.
1. Depo **Hesabı** açılır listesinden yedeklenen içeriği geri yüklemek istediğiniz hedef depolama hesabını seçin.
1. **Dosya Paylaşımını Seç** açılır listesini, adım 2'de seçtiğiniz depolama hesabında bulunan dosya paylaşımlarını görüntüler. Yedeklenen içeriği geri yüklemek istediğiniz dosya paylaşımını seçin.
1. Klasör **Adı** kutusunda, hedef dosya paylaşımında geri yüklenen içerikle birlikte oluşturmak istediğiniz bir klasör adı belirtin.
1. Çakışma varsa atlayıp atlamayacağımı veya üzerine yazılmasını seçin.
1. Tüm kutulara uygun değerleri girdikten sonra **Tamam'ı**seçin.

    ![Alternatif Konum'u Seçin](./media/restore-afs/alternate-location.png)

1. Geri yükleme işlemini başlatmak için **Geri Yükle'yi** seçin.

    ![Başlatmak için Geri Yükle'yi seçin](./media/restore-afs/click-restore.png)

### <a name="item-level-recovery"></a>Öğe düzeyinde kurtarma

Özgün konumda veya alternatif bir konumda tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

1. Geri yükleme bölümüne dosya [paylaşımını seç'in](#select-the-file-share-to-restore) adım 5'inde geri yüklemek için dosya paylaşımını seçtikten sonra görünen **Yedek Öğe** bölmesinde **Dosya Kurtarma** seçeneğini seçin.

    ![Dosya Kurtarma'yı seçin](./media/restore-afs/file-recovery.png)

1. **Dosya Kurtarma'yı**seçtikten sonra Geri **Yükleme** bölmesi, seçili dosya paylaşımı için kullanılabilir geri yükleme noktalarının listesini görüntüleyen geri **yükleme noktası** menüsüyle açılır.

1. Geri yükleme işlemini gerçekleştirmek için kullanmak istediğiniz geri yükleme noktasını seçin ve **Tamam'ı**seçin.

    ![Geri yükleme noktasını seçin](./media/restore-afs/restore-point.png)

1. **Tamam'ı**seçtikten sonra, geri yükleme bölmesi menüsü **Konumu Geri Yükle'ye**geçer. **Konumu Geri Yükle'de,** verilerin nerede ve nasıl geri yüklenir olduğunu belirtin. Aşağıdaki iki seçenekten birini seçin:

    * **Özgün Konum**: Seçili dosya veya klasörleri özgün kaynakla aynı dosya paylaşımına geri yükleyin.
    * **Alternatif Konum**: Seçili dosya veya klasörleri alternatif bir konuma geri yükleyin ve özgün dosya paylaşımı içeriğini olduğu gibi tutun.

#### <a name="restore-to-the-original-location"></a>Özgün konuma geri yükleme

1. **Kurtarma Hedefi**olarak **Özgün Konum'u** seçin ve çakışma varsa atlayıp atlanın veya üzerine yazmayacağınız konusunda seçin.

    ![Öğe düzeyinde kurtarma için Özgün Konum](./media/restore-afs/original-location-item-level.png)

1. Geri yüklemek istediğiniz dosyaları veya klasörleri seçmek için **Dosya'yı** seçin'i seçin.

    ![Dosya Seç'i Seçin](./media/restore-afs/select-file.png)

1. **Dosya seç**seçeneğini seçtikten sonra, dosya paylaşım bölmesi geri yüklemek için seçtiğiniz dosya paylaşımı kurtarma noktasının içeriğini görüntüler.

1. Geri yüklemek istediğiniz dosya veya klasöre karşılık gelen onay kutusunu seçin ve **Seç'i**seçin.

    ![Dosya veya klasörü seçin](./media/restore-afs/select-file-folder.png)

1. Geri yükleme için birden çok dosya veya klasör seçmek için 2 ile 4 adımlarını yineleyin.
1. Geri yüklemek istediğiniz tüm öğeleri seçtikten sonra **Tamam'ı**seçin.

    ![Geri yüklenebilmek için tüm öğeleri seçtikten sonra Tamam'ı seçin](./media/restore-afs/after-selecting-items.png)

1. Geri yükleme işlemini başlatmak için **Geri Yükle'yi** seçin.

    ![Başlatmak için Geri Yükle'yi seçin](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. **Kurtarma Hedefi**Olarak **Alternatif Konum'u** seçin.
1. Depo **Hesabı** açılır listesinden yedeklenen içeriği geri yüklemek istediğiniz hedef depolama hesabını seçin.
1. **Dosya Paylaşımını Seç** açılır listesini, adım 2'de seçtiğiniz depolama hesabında bulunan dosya paylaşımlarını görüntüler. Yedeklenen içeriği geri yüklemek istediğiniz dosya paylaşımını seçin.
1. Klasör **Adı** kutusunda, hedef dosya paylaşımında geri yüklenen içerikle birlikte oluşturmak istediğiniz bir klasör adı belirtin.
1. Çakışma varsa atlayıp atlamayacağımı veya üzerine yazılmasını seçin.
1. Geri yüklemek istediğiniz dosyaları veya klasörleri seçmek için **Dosya'yı** seçin'i seçin.

    ![Alternatif konuma geri yükecek öğeleri seçin](./media/restore-afs/restore-to-alternate-location.png)

1. **Dosya seç**seçeneğini seçtiğinizde, dosya paylaşım bölmesi geri yüklemek için seçtiğiniz dosya paylaşımı kurtarma noktasının içeriğini görüntüler.
1. Geri yüklemek istediğiniz dosya veya klasöre karşılık gelen onay kutusunu seçin ve **Seç'i**seçin.

    ![Kurtarma hedefini seçin](./media/restore-afs/recovery-destination.png)

1. Geri yükleme için birden çok dosya veya klasör seçmek için 6 ile 8 adımlarını yineleyin.
1. Geri yüklemek istediğiniz tüm öğeleri seçtikten sonra **Tamam'ı**seçin.

    ![Tüm dosyaları seçtikten sonra Tamam'ı seçin](./media/restore-afs/after-selecting-all-items.png)

1. Geri yükleme işlemini başlatmak için **Geri Yükle'yi** seçin.

## <a name="track-a-restore-operation"></a>Geri yükleme işlemini izleme

Geri yükleme işlemini tetikledikten sonra, yedekleme hizmeti izleme için bir iş oluşturur. Azure Yedekleme, portaldaki işle ilgili bildirimler görüntüler. İş işlemlerini görüntülemek için bildirimler köprüünü seçin.

![Bildirimler köprüünü seçin](./media/restore-afs/notifications-link.png)

Kurtarma Hizmetleri kasasından geri yükleme ilerlemesini de izleyebilirsiniz:

1. Geri yükleme işlemini tetiklediğiniz yerden Kurtarma Hizmetleri kasasını açın.
1. Genel bakış bölmesinde, farklı iş yüklerine karşı çalışan operasyonların durumunu görmek için **İzleme** bölümü altında **Yedek İşler'i** seçin.

    ![Yedek İşler'i Seçin](./media/restore-afs/backup-jobs.png)

1. **Veri Aktarımı** ve **Geri Yüklenen Dosya Sayısı**gibi geri yükleme işlemi hakkında daha fazla ayrıntı görüntülemek için dosya paylaşımınıza karşılık gelen iş yükü adını seçin.

    ![Geri yüklenen ayrıntılara bakın](./media/restore-afs/restore-details.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşımı yedeklemelerini](manage-afs-backup.md)nasıl yönetebilirsiniz öğrenin.
