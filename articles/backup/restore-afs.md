---
title: Azure dosya paylaşımlarını geri yükleme
description: Dosya paylaşımının tamamını veya belirli dosyaları Azure Backup hizmeti tarafından oluşturulan bir geri yükleme noktasından geri yüklemek için Azure portal nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 01/12/2020
ms.openlocfilehash: b16eb4120ff2d269135ae8ae6555ef4fdbdbda5d
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294402"
---
# <a name="restore-azure-file-shares"></a>Azure dosya paylaşımlarını geri yükleme

Bu makalede, bir dosya paylaşımının tamamını veya belirli dosyaları [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından oluşturulan bir geri yükleme noktasından geri yüklemek için Azure Portal nasıl kullanılacağı açıklanmaktadır.

Bu kılavuzda şunları yapmayı öğreneceksiniz:

* Tam Azure dosya paylaşımının geri yüklenmesi
* Tek tek dosya veya klasörleri geri yükleme
* Geri yükleme Işlemi durumunu izleme

## <a name="steps-to-perform-restore"></a>Geri yüklemeyi gerçekleştirme adımları

### <a name="select-the-file-share-to-restore"></a>Geri yüklenecek dosya payını seçin

1. [Azure Portal](https://portal.azure.com/), dosya paylaşımının yedeklemesini yapılandırmak Için kullandığınız kurtarma hizmetleri kasasını açın.

2. **Genel bakış** dikey penceresinin **korumalı öğeler** bölümü altında **yedekleme öğeleri** ' ne tıklayın.

    ![Yedekleme öğeleri ' ne tıklayın](./media/restore-afs/backup-items.png)

3. **Yedekleme öğeleri**' ne tıkladığınızda, aşağıda görüldüğü gibi **genel bakış** dikey penceresinin yanında tüm yedekleme yönetimi türlerini listelemek için yeni bir dikey pencere görüntülenir:

    ![Yedekleme yönetim türleri](./media/restore-afs/backup-management.png)

4. **Yedekleme öğeleri**' nde, **yedekleme yönetimi türü**altında **Azure depolama (Azure dosyaları)** öğesini seçin. Tüm dosya paylaşımlarının ve bunlara karşılık gelen depolama hesaplarının bu kasa kullanılarak yedeklenme listesini görürsünüz.

    ![Tüm dosya paylaşımlarının listesi](./media/restore-afs/file-shares.png)

5. Azure dosya paylaşımları listesinden, geri yükleme işlemini gerçekleştirmek istediğiniz dosya paylaşımını seçin.

### <a name="full-share-recovery"></a>Tam paylaşma kurtarması

Özgün dosya paylaşımının tamamını veya alternatif bir konuma geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

1. Geri [yüklenecek dosya paylaşımının](#select-the-file-share-to-restore) 5. adımında geri yüklenecek istenen dosya paylaşımından sonra görüntülenen **yedekleme öğesi** dikey penceresinden **paylaşma içeriğini geri yükle** seçeneğini seçin.

   ![Paylaşma geri yükle seçeneğini belirleyin](./media/restore-afs/restore-share.png)

2. **Paylaşma 'Yı geri**Yükle ' ye tıkladığınızda, **geri yükleme** dikey penceresi seçili dosya paylaşımında kullanılabilen geri yükleme noktalarının listesini görüntüleyen bir **geri yükleme noktası** menüsüyle açılır.

3. Geri yükleme işlemini gerçekleştirmek için kullanmak istediğiniz geri yükleme noktasını seçin ve **Tamam**' a tıklayın.

    ![Geri yükleme noktası seç](./media/restore-afs/restore-point.png)

4. Tamam ' a tıkladığınızda geri yükleme dikey penceresi, **geri yükleme konumuna**geçer. **Geri yükleme konumu**' nda, verilerin nereye (veya nasıl) geri yükleneceğini belirtin. Aşağıdaki iki seçenekten birini seçebilirsiniz:

    * **Özgün konum**: dosya paylaşımının tamamını özgün kaynakla aynı konuma geri yükleyin.
    * **Alternatif konum**: dosya paylaşımının tamamını alternatif bir konuma geri yükleyin ve özgün dosya paylaşımının **olduğu gibi**tutun.

#### <a name="restore-to-original-location"></a>Özgün konuma geri yükle

1. **Kurtarma hedefi** olarak **özgün konum** ' u seçin ve çakışmalar varsa, Skip veya üzerine yaz ' ı seçin. Uygun seçimi yaptıktan sonra **Tamam** ' ı tıklatın.

    ![Özgün konum seç](./media/restore-afs/original-location.png)

2. Geri yükleme işlemini başlatmak için **geri yükle** ' ye tıklayın.

    ![Başlamak için geri yükle 'ye tıklayın](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. Kurtarma hedefi olarak **alternatif konum** ' ı seçin.
2. **Depolama hesabı** alanının açılan menüsünde, yedeklenen içeriği geri yüklemek istediğiniz hedef depolama hesabını seçin.
3. Adım 2 ' de seçtiğiniz depolama hesabına bağlı olarak, **Dosya Paylaşımı Seç** açılan menüsünde, seçilen depolama hesabında mevcut olan dosya paylaşımlarının listesi görüntülenir. Yedeklenen içeriği geri yüklemek istediğiniz dosya payını seçin.
4. **Klasör adı** alanında, geri yüklenen içerikle hedef dosya paylaşımında oluşturmak istediğiniz bir klasör adı belirtin.
5. Çakışmalar varsa atlayın veya üzerine yaz seçeneğini belirleyin.
6. Tüm alanlara uygun değerleri girdikten sonra **Tamam** ' ı tıklatın.

    ![Alternatif Konum Seç](./media/restore-afs/alternate-location.png)

7. Geri yükleme işlemini başlatmak için geri yükle ' ye tıklayın.

    ![Başlamak için geri yükle 'ye tıklayın](./media/restore-afs/click-restore.png)

### <a name="item-level-recovery"></a>Öğe düzeyinde kurtarma

Özgün veya alternatif konumdaki tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

1. Geri yüklenecek [dosya paylaşımının 5](#select-the-file-share-to-restore) . adımında geri yüklenecek istenen dosya paylaşımından sonra görüntülenen **yedekleme öğesi** dikey penceresinden **dosya kurtarma** seçeneğini belirleyin.

    ![Dosya kurtarma seçin](./media/restore-afs/file-recovery.png)

2. **Dosya kurtarma**' ya tıkladığınızda **, geri yükleme dikey penceresi** seçili dosya paylaşımında kullanılabilen geri yükleme noktalarının listesini görüntüleyen bir geri yükleme **noktası** menüsüyle açılır.

3. Geri yükleme işlemini gerçekleştirmek için kullanmak istediğiniz geri yükleme noktasını seçin ve **Tamam**' a tıklayın.

    ![Geri yükleme noktası seç](./media/restore-afs/restore-point.png)

4. Tamam ' a tıkladığınızda geri yükleme dikey penceresi, **geri yükleme konumuna**geçer. **Geri yükleme konumu**' nda, verilerin nereye (veya nasıl) geri yükleneceğini belirtin. Aşağıdaki iki seçenekten birini seçebilirsiniz:

    * **Özgün konum**: seçili dosya/klasörleri özgün kaynakla aynı dosya paylaşımında geri yükleyin.
    * **Alternatif konum**: seçili dosya/klasörleri alternatif bir konuma geri yükleyin ve özgün dosya paylaşma içeriğini **olduğu gibi**tutun.

#### <a name="restore-to-original-location"></a>Özgün konuma geri yükleme

1. **Kurtarma hedefi** olarak **özgün konum** ' u seçin ve çakışmalar varsa, Skip veya üzerine yaz ' ı seçin.

    ![Öğe düzeyinde kurtarma için özgün konum](./media/restore-afs/original-location-item-level.png)

2. Geri yüklemek istediğiniz dosya/klasörleri seçmek için **Dosya Seç** ' e tıklayın.

    ![Dosya Seç ' e tıklayın](./media/restore-afs/select-file.png)

3. **Dosya Seç**' e tıkladığınızda, geri yükleme için seçtiğiniz dosya paylaşma kurtarma noktasının içeriğini görüntüleyen bir dosya paylaşma dikey penceresi görüntülenir.

4. Geri yüklemek istediğiniz dosya/klasöre karşılık gelen kutuyu işaretleyin ve **Seç**' e tıklayın.

    ![Dosya veya klasör seçin](./media/restore-afs/select-file-folder.png)

5. Geri yükleme için birden çok dosya/klasör seçmek üzere 2-4 arasındaki adımları yineleyin.
6. Geri yüklemek istediğiniz tüm öğeleri seçtikten sonra **Tamam**' a tıklayın.

    ![Geri yüklenecek tüm öğeleri seçtikten sonra Tamam ' a tıklayın.](./media/restore-afs/after-selecting-items.png)

7. Geri yükleme işlemini başlatmak için geri yükle ' ye tıklayın.

    ![Başlamak için geri yükle 'ye tıklayın](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. Kurtarma hedefi olarak **alternatif konum** ' ı seçin.
2. **Depolama hesabı** alanının açılan menüsünde, yedeklenen içeriği geri yüklemek istediğiniz hedef depolama hesabını seçin.
3. Adım 2 ' de seçtiğiniz depolama hesabına bağlı olarak, **Dosya Paylaşımı Seç** açılan menüsünde, seçilen depolama hesabında mevcut dosya paylaşımlarının listesi görüntülenir. Yedeklenen içeriği geri yüklemek istediğiniz dosya payını seçin.
4. **Klasör adı** alanında, geri yüklenen içerikle hedef dosya paylaşımında oluşturmak istediğiniz bir klasör adı belirtin.
5. Çakışmalar varsa atlayın veya üzerine yaz seçeneğini belirleyin.
6. Geri yüklemek istediğiniz dosya/klasörleri seçmek için **Dosya Seç** ' e tıklayın.

    ![Alternatif konuma geri yüklenecek öğeleri seçin](./media/restore-afs/restore-to-alternate-location.png)

7. **Dosya Seç**' e tıkladığınızda, geri yükleme için seçtiğiniz dosya paylaşma kurtarma noktasının içeriğini görüntüleyen bir dosya paylaşma dikey penceresi görüntülenir.
8. Geri yüklemek istediğiniz dosya/klasöre karşılık gelen kutuyu işaretleyin ve **Seç**' e tıklayın.

    ![Kurtarma hedefini seçin](./media/restore-afs/recovery-destination.png)

9. Geri yükleme için birden çok dosya/klasör seçmek üzere 6-8 arasındaki adımları yineleyin.
10. Geri yüklemek istediğiniz tüm öğeleri seçtikten sonra **Tamam**' a tıklayın.

    [Tüm dosyaları seçtikten sonra Tamam 'a tıklayın](./media/restore-afs/after-selecting-all-items.png)

11. Geri yükleme işlemini başlatmak için **geri yükle** ' ye tıklayın.

## <a name="track-restore-operation"></a>Geri yükleme Işlemini izle

Geri yükleme işlemini tetikledikten sonra, yedekleme hizmeti izleme için bir iş oluşturur. Azure Backup portalda iş hakkında bildirimleri görüntüler. İşle ilgili işlemleri görüntülemek için bildirimler köprüye tıklayın.

![Tıklama bildirimleri Köprüsü](./media/restore-afs/notifications-link.png)

Kurtarma Hizmetleri kasasından geri yükleme ilerlemesini de izleyebilirsiniz. Geri yükleme işlemi durumunu denetlemek için adımlar aşağıda verilmiştir:

1. Kurtarma Hizmetleri kasasını geri yükleme işlemini tetiklediğiniz yerden açın.
2. Farklı iş yüklerine karşı çalışan işlemlerin durumunu görmek için **genel bakış** dikey penceresinin **Izleme bölümü** altında **yedekleme işleri** ' ne tıklayın.

    ![Yedekleme Işleri ' ne tıklayın](./media/restore-afs/backup-jobs.png)

3. Geri yükleme işlemi hakkında, aktarılan veriler, geri yüklenen dosya sayısı gibi daha fazla ayrıntı görüntülemek için dosya paylaşımınıza karşılık gelen iş yükü adına tıklayın.

    ![Geri yüklenen Ayrıntıları gör](./media/restore-afs/restore-details.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşımının yedeklerini yönetme](manage-afs-backup.md) hakkında bilgi edinin
