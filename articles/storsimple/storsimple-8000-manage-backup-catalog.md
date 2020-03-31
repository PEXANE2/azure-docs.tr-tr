---
title: StorSimple yedekleme kataloğunuzu yönetin | Microsoft Dokümanlar
description: Yedekleme kümelerini listelemek, seçmek ve silmek için StorSimple Device Manager hizmet yedekleme kataloğu sayfasını nasıl kullanacağımı açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 07d9e03f1631ebce88a7a7c2e33be62f21dda522
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60319692"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a>Yedekleme kataloğunuzu yönetmek için StorSimple Device Manager hizmetini kullanın
## <a name="overview"></a>Genel Bakış
StorSimple Device Manager hizmeti **Yedekleme Kataloğu** bıçak, manuel veya zamanlanmış yedeklemealındığında oluşturulan tüm yedekleme kümelerini görüntüler. Bu sayfayı, bir yedekleme ilkesinin veya birimin tüm yedeklemelerini listelemek, yedeklemeleri seçmek veya silmek veya bir birimi geri yüklemek veya klonlamak için yedekleme kullanmak için kullanabilirsiniz.

Bu öğretici, yedekleme kümesini nasıl listeleyecek, seçip sileceklerini açıklar. Aygıtınızı yedeklemeden nasıl geri yükleyin, [aygıtınızı yedek kümesinden geri yükleme'ye](storsimple-8000-restore-from-backup-set-u2.md)gidin. Bir birimi nasıl klonlamayı öğrenmek için [StorSimple birimini klonla'ya](storsimple-8000-clone-volume-u2.md)gidin.

![Yedek katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

**Yedek Katalog** bıçağı, yedek ayar seçiminizi daraltmak için bir sorgu sağlar. Alınan yedekleme kümelerini aşağıdaki parametrelere göre filtreleyebilirsiniz:

* **Aygıt** – Yedekleme kümesinin oluşturulduğu aygıt.
* **Yedekleme ilkesi veya Birim** - Bu yedekleme kümesiyle ilişkili yedekleme ilkesi veya birim.
* **Gönderen ve Gönderen** – Yedekleme kümesinin oluşturulduğu tarih ve saat aralığı.

Filtre uygulanmış yedekleme kümeleri aşağıdaki özniteliklere göre tablolanır:

* **Ad** – Yedekleme kümesiyle ilişkili yedekleme ilkesinin veya biriminin adı.
* **Boyut** – Yedekleme kümesinin gerçek boyutu.
* **Oluşturuldu** – Yedeklemelerin oluşturulduğu tarih ve saat. 
* **Tür** - Yedekleme kümeleri yerel anlık görüntüler veya bulut anlık görüntüleri olabilir. Yerel anlık görüntü, aygıtta yerel olarak depolanan tüm birim verilerinizin yedeği olurken, bulut anlık görüntüsü bulutta bulunan birim verilerinin yedeğini ifade eder. Yerel anlık görüntüler daha hızlı erişim sağlarken, veri esnekliği için bulut anlık görüntüleri seçilir.
* **Tarafından Başlatılan** – Yedeklemeler bir zamanlama tarafından veya bir kullanıcı tarafından el ile otomatik olarak başlatılabilir. Yedeklemeleri zamanlamak için bir yedekleme ilkesi kullanabilirsiniz. Alternatif olarak, el ile yedekleme yapmak için **Yedeklemeyi Al** seçeneğini kullanabilirsiniz.

## <a name="list-backup-sets-for-a-backup-policy"></a>Yedekleme ilkesi için yedekleme kümelerini listele
Yedekleme ilkesinin tüm yedeklemelerini listelemek için aşağıdaki adımları tamamlayın.

#### <a name="to-list-backup-sets"></a>Yedekleme kümelerini listelemek için
1. StorSimple Device Manager hizmetinize gidin ve **Yedekleme kataloğunu**tıklatın.

2. Seçimleri aşağıdaki gibi filtreleyin:
   
   1. Zaman aralığını belirtin.
   2. Uygun aygıtı seçin.
   3. İlgili yedeklemeleri görüntülemek için **Yedekleme ilkesine** göre filtre uygulayın.
   3. Yedekleme ilkesi açılır listesinden, seçili aygıttaki tüm yedeklemeleri görüntülemek için **Tümü'ni** seçin.
   4. Bu sorguyu yürütmek için **Uygula'yı** tıklatın.
      
      Seçili yedekleme ilkesiyle ilişkili yedeklemeler yedekleme kümeleri listesinde görünmelidir.

      ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Yedek set seçin
Bir birim veya yedekleme ilkesi için bir yedekleme kümesi seçmek için aşağıdaki adımları tamamlayın.

#### <a name="to-select-a-backup-set"></a>Yedek kümesi seçmek için
1. StorSimple Device Manager hizmetinize gidin ve **Yedekleme kataloğunu**tıklatın.
2. Seçimleri aşağıdaki gibi filtreleyin:
   
   1. Zaman aralığını belirtin. 
   2. Uygun aygıtı seçin. 
   3. Seçmek istediğiniz yedekleme için ses düzeyine veya yedekleme ilkesine göre filtre uygulayın.
   4. Bu sorguyu yürütmek için **Uygula'yı** tıklatın.
      
      Seçili birim veya yedekleme ilkesiyle ilişkili yedeklemeler yedekleme kümeleri listesinde görünmelidir.

      ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Bir yedekleme kümesiseçin ve genişletin. Artık yedek kümelerin içerdiği birimlere göre ayrıldığını görebilirsiniz. **Geri Yükle** ve **Sil** seçenekleri, yedekleme kümesi için bağlam menüsünden (sağ tıklatma) kullanılabilir. Bu eylemlerden birini seçtiğiniz yedekleme kümesinde gerçekleştirebilirsiniz.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Yedekleme kümesini silme
Artık ilişkili verileri korumak istemediğiniz bir yedeklemeyi silin. Yedekleme kümesini silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-backup-set"></a>Yedekleme kümesini silmek için
 StorSimple Device Manager hizmetinize gidin ve **Yedekleme kataloğunu**tıklatın.
1. Seçimleri aşağıdaki gibi filtreleyin:
   
   1. Zaman aralığını belirtin. 
   2. Uygun aygıtı seçin. 
   3. Seçmek istediğiniz yedekleme için ses düzeyine veya yedekleme ilkesine göre filtre uygulayın.
   4. Bu sorguyu yürütmek için **Uygula'yı** tıklatın.
      
      Seçili birim veya yedekleme ilkesiyle ilişkili yedeklemeler yedekleme kümeleri listesinde görünmelidir.

      ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

1. Bir yedekleme kümesiseçin ve genişletin. Artık yedek kümelerin içerdiği birimlere göre ayrıldığını görebilirsiniz. **Geri Yükle** ve **Sil** seçenekleri, yedekleme kümesi için bağlam menüsünden (sağ tıklatma) kullanılabilir. Seçili yedekleme kümesini sağ tıklatın ve bağlam menüsünden **Sil'i**seçin.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

1. Onay istendiğinde, görüntülenen bilgileri gözden geçirin ve **Sil'i**tıklatın. Seçili yedekleme kalıcı olarak silinir.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

1. Silme işlemi tamamlandığında ve başarılı bir şekilde tamamlandığında size bildirilir. Silme işlemi bittikten sonra, bu sayfadaki sorguyu yenileyin. Silinen yedekleme kümesi artık yedekleme kümeleri listesinde görünmez.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Cihazınızı yedekleme kümesinden geri yüklemek için yedekleme kataloğunu](storsimple-8000-restore-from-backup-set-u2.md)nasıl kullanacağınızı öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

