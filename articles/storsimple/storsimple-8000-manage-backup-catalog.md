---
title: StorSimple yedekleme kataloğunuzu yönetme | Microsoft Docs
description: Yedek kümelerini listelemek, seçmek ve silmek için StorSimple Aygıt Yöneticisi hizmet yedekleme kataloğu sayfasının nasıl kullanılacağını açıklar.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60319692"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a>StorSimple Aygıt Yöneticisi hizmetini kullanarak yedekleme kataloğunuzu yönetin
## <a name="overview"></a>Genel Bakış
StorSimple Aygıt Yöneticisi hizmeti **Yedekleme kataloğu** dikey penceresi, el ile veya zamanlanmış yedeklemeler çekilirken oluşturulan tüm yedekleme kümelerini görüntüler. Bu sayfayı bir yedekleme ilkesi veya birimi için tüm yedeklemeleri listelemek, yedeklemeleri seçmek veya silmek ya da bir birimi geri yüklemek ya da kopyalamak için bir yedekleme kullanmak için kullanabilirsiniz.

Bu öğreticide, bir yedekleme kümesini listeleme, seçme ve silme işlemlerinin nasıl yapılacağı açıklanmaktadır. Cihazınızı yedekten geri yükleme hakkında bilgi edinmek için, [cihazınızı bir yedekleme kümesinden geri yükleme](storsimple-8000-restore-from-backup-set-u2.md)bölümüne gidin. Bir birimi nasıl klonleyeceğinizi öğrenmek için, [StorSimple birimini kopyalama](storsimple-8000-clone-volume-u2.md)bölümüne gidin.

![Yedekleme kataloğu](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

**Yedekleme kataloğu** dikey penceresi, yedekleme kümesi seçiminizi daraltmak için bir sorgu sağlar. Aşağıdaki parametrelere göre alınan yedekleme kümelerini filtreleyebilirsiniz:

* **Cihaz** : yedekleme kümesinin oluşturulduğu cihaz.
* Yedekleme **ilkesi veya birimi** : Bu yedekleme kümesiyle ilişkili yedekleme ilkesi veya birim.
* **Ve** ile: yedekleme kümesinin oluşturulduğu tarih ve saat aralığı.

Filtrelenmiş yedekleme kümeleri aşağıdaki özniteliklere göre tablo olarak ayarlanır:

* **Name** : yedekleme ilkesi veya yedekleme kümesiyle ilişkili birimin adı.
* **Size** : yedekleme kümesinin gerçek boyutu.
* **Oluşturulma** tarihi: yedeklemelerin oluşturulduğu tarih ve saat. 
* **Tür** – yedekleme kümeleri yerel anlık görüntüler veya bulut anlık görüntüleri olabilir. Yerel anlık görüntü, cihazda yerel olarak depolanan tüm birim verilerinizin yedeğine karşın bir bulut anlık görüntüsü, bulutta bulunan birim verilerinin yedeklemesini ifade eder. Yerel anlık görüntüler, veri dayanıklılığı için bulut anlık görüntüleri seçildiği halde daha hızlı erişim sağlar.
* **Başlatan** : yedeklemeler, bir zamanlama veya bir kullanıcı tarafından el ile başlatılabilir. Yedeklemeleri zamanlamak için bir yedekleme İlkesi kullanabilirsiniz. Alternatif olarak, el ile yedekleme gerçekleştirmek için **yedekleme al** seçeneğini kullanabilirsiniz.

## <a name="list-backup-sets-for-a-backup-policy"></a>Yedekleme ilkesi için yedekleme kümelerini listeleyin
Bir yedekleme ilkesi için tüm yedeklemeleri listelemek üzere aşağıdaki adımları izleyin.

#### <a name="to-list-backup-sets"></a>Yedekleme kümelerini listelemek için
1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve **Yedekleme kataloğu**' na tıklayın.

2. Seçimleri aşağıdaki şekilde filtreleyin:
   
   1. Zaman aralığını belirtin.
   2. Uygun aygıtı seçin.
   3. İlgili yedeklemeleri görüntülemek için **yedekleme ilkesine** göre filtreleyin.
   3. Seçilen cihazdaki tüm yedeklemeleri görüntülemek için yedekleme İlkesi açılan listesinden **Tümü** ' ne tıklayın.
   4. Bu sorguyu yürütmek için **Uygula** ' ya tıklayın.
      
      Seçilen yedekleme ilkesiyle ilişkili yedeklemeler, yedekleme kümeleri listesinde görünmelidir.

      ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Bir yedekleme kümesi seçin
Bir birim veya yedekleme ilkesi için bir yedekleme kümesi seçmek üzere aşağıdaki adımları izleyin.

#### <a name="to-select-a-backup-set"></a>Bir yedekleme kümesi seçmek için
1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve **Yedekleme kataloğu**' na tıklayın.
2. Seçimleri aşağıdaki şekilde filtreleyin:
   
   1. Zaman aralığını belirtin. 
   2. Uygun aygıtı seçin. 
   3. Seçmek istediğiniz yedekleme için birim veya yedekleme ilkesine göre filtreleyin.
   4. Bu sorguyu yürütmek için **Uygula** ' ya tıklayın.
      
      Seçilen birim veya yedekleme ilkesiyle ilişkili yedeklemeler, yedekleme kümeleri listesinde görünmelidir.

      ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Bir yedekleme kümesi seçin ve genişletin. Artık içerdiği birimler tarafından ayrılmış yedekleme kümelerini görebilirsiniz. **Geri yükleme** ve **silme** seçenekleri, yedekleme kümesi için bağlam menüsü (sağ tıklama) yoluyla kullanılabilir. Seçtiğiniz yedekleme kümesinde bu eylemlerden birini gerçekleştirebilirsiniz.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Bir yedekleme kümesini silme
Artık onunla ilişkili verileri sürdürmek istemediğiniz zaman bir yedeği silin. Bir yedekleme kümesini silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-backup-set"></a>Bir yedekleme kümesini silmek için
 StorSimple Aygıt Yöneticisi hizmetinize gidin ve **Yedekleme kataloğu**' na tıklayın.
1. Seçimleri aşağıdaki şekilde filtreleyin:
   
   1. Zaman aralığını belirtin. 
   2. Uygun aygıtı seçin. 
   3. Seçmek istediğiniz yedekleme için birim veya yedekleme ilkesine göre filtreleyin.
   4. Bu sorguyu yürütmek için **Uygula** ' ya tıklayın.
      
      Seçilen birim veya yedekleme ilkesiyle ilişkili yedeklemeler, yedekleme kümeleri listesinde görünmelidir.

      ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

1. Bir yedekleme kümesi seçin ve genişletin. Artık içerdiği birimler tarafından ayrılmış yedekleme kümelerini görebilirsiniz. **Geri yükleme** ve **silme** seçenekleri, yedekleme kümesi için bağlam menüsü (sağ tıklama) yoluyla kullanılabilir. Seçili yedekleme kümesine sağ tıklayın ve bağlam menüsünden **Sil**' i seçin.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

1. Onay istendiğinde, görünen bilgileri gözden geçirin ve **Sil**' e tıklayın. Seçilen yedekleme kalıcı olarak silindi.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

1. Silme işlemi devam ederken ve başarıyla tamamlandığında size bildirilir. Silme işlemi tamamlandıktan sonra, bu sayfadaki sorguyu yenileyin. Silinen yedekleme kümesi artık yedekleme kümeleri listesinde gözükmeyecektir.

    ![Yedekleme kataloğuna git](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Sonraki adımlar
* Yedekleme [kataloğunu kullanarak cihazınızı bir yedekleme kümesinden geri yükleme](storsimple-8000-restore-from-backup-set-u2.md)hakkında bilgi edinin.
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini nasıl kullanacağınızı](storsimple-8000-manager-service-administration.md)öğrenin.

