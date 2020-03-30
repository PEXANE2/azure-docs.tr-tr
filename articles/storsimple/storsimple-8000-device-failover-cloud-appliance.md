---
title: StorSimple Cloud Appliance'da arıza ve olağanüstü durum kurtarma
description: StorSimple 8000 serisi fiziksel cihazınızda bir bulut cihazında nasıl başarısız oylanın olduğunu öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 347b899608d4322a7873b9f80f38ca1c767194d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77468754"
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a>StorSimple Cloud Cihazınızda başarısız

## <a name="overview"></a>Genel Bakış

Bu öğretici, bir felaket olduğunda StorSimple 8000 serisi fiziksel aygıtın StorSimple Cloud Appliance'a göre başarısız olması için gereken adımları açıklar. StorSimple, veri merkezindeki bir kaynak fiziksel aygıttan Azure'da çalışan bir bulut cihazına veri aktarmak için aygıt hata özelliğini kullanır. Bu öğreticideki kılavuz, StorSimple 8000 serisi fiziksel aygıtlar ve yazılım sürümlerini çalıştıran bulut cihazları için geçerlidir Güncelleştirme 3 ve sonrası.

Aygıt arızası ve bir felaketten kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi edinmek [için, StorSimple 8000 serisi cihazlar için Failover ve olağanüstü durum kurtarma'ya](storsimple-8000-device-failover-disaster-recovery.md)gidin.

Bir StorSimple fiziksel aygıtı üzerinde başka bir fiziksel aygıtta başarısız olmak [için, Bir StorSimple fiziksel aygıta Başarısız 'a](storsimple-8000-device-failover-physical-device.md)gidin. Bir aygıtın kendisi üzerinde başarısız olmak için, [aynı StorSimple fiziksel aygıtına Başarısız 'a](storsimple-8000-device-failover-same-device.md)gidin.

## <a name="prerequisites"></a>Ön koşullar

- Aygıt arızası ile ilgili hususları gözden aldığınızdan emin olun. Daha fazla bilgi için, [aygıt failover için ortak hususlar](storsimple-8000-device-failover-disaster-recovery.md)gidin.

- Bu yordamı çalıştırmadan önce bir StorSimple Cloud Appliance oluşturulmalı ve yapılandırılmalıdır. Update 3 yazılım sürümünü çalıştırıyorsanız veya daha sonra, DR için bir 8020 bulut cihazı kullanmayı düşünün. 8020 modeli 64 TB'ye sahiptir ve Premium Depolama kullanır. Daha fazla bilgi için [Bir StorSimple Cloud Appliance'ı dağıt'a gidin ve yönetin.](storsimple-8000-cloud-appliance-u2.md)

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a>Bulut cihazında başarısız olacak adımlar

Aygıtı bir hedefe geri yüklemek için aşağıdaki adımları gerçekleştirin StorSimple Cloud Appliance.

1.  Başarısız olmak istediğiniz birim kapsayıcının ilişkili bulut anlık görüntüleri olduğunu doğrulayın. Daha fazla bilgi [için, yedekleme oluşturmak için StorSimple Device Manager hizmetini kullanın' a](storsimple-8000-manage-backup-policies-u2.md)gidin.
2. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** bıçağında, hizmetinizle bağlantılı aygıtlar listesine gidin.
    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Kaynak cihazınızı seçin ve tıklatın. Kaynak aygıtta başarısız olmak istediğiniz birim kapsayıcıları vardır. Ayarlar **> Birim Kapsayıcıları'na**gidin.

    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Başka bir aygıtta başarısız olmak istediğiniz bir birim kapsayıcı seçin. Bu kapsayıcının içindeki birimlerin listesini görüntülemek için birim kapsayıcısını tıklatın. Bir birim seçin, sağ tıklatın ve birimi çevrimdışı hale getirmek için **Çevrimdışı Kaldır'ı** tıklatın.

    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Bu işlemi, birim kapsayıcıdaki tüm birimler için yineleyin.

     ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Başka bir aygıtta başarısız olmak istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.

7. **Cihazlar** bıçağına geri dön. Komut çubuğundan Başarısız'ı **tıklatın.**

    ![Başarısız'ı tıklatın](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. Fail **over** blade'de aşağıdaki adımları gerçekleştirin:
   
    1. **Kaynak'ı**tıklatın. Üzerinde başarısız olmak için birim kapsayıcıları seçin. **Yalnızca ilişkili bulut anlık görüntüleri ve çevrimdışı birimlerin yer alan birim kapsayıcıları görüntülenir.**
        ![Kaynak seçme](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. **Hedef'i**tıklatın. Kullanılabilir cihazların açılır listesinden bir hedef bulut cihazı seçin. **Listede yalnızca kaynak birim kapsayıcılarını barındıracak kapasiteye sahip aygıtlar görüntülenir.**

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. **Özet** altındaki arıza ayarlarını gözden geçirin ve seçili birim kapsayıcılarında ki birimlerin çevrimdışı olduğunu belirten onay kutusunu seçin. 

        ![Başarısız ayarları gözden geçirme](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Bir başarısız iş oluşturulur. Başarısız işi izlemek için iş bildirimini tıklatın.

    ![Başarısız işi izleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Arıza tamamlandıktan **sonra, Cihazlar** bıçağına geri dön.

    1. Başarısız olmak için hedef olarak kullanılan aygıtı seçin.

       ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. **Birim Kapsayıcılar'ı**tıklatın. Tüm birim kapsayıcıları, eski aygıttan gelen hacimlerle birlikte listelenmelidir.

       Üzerinde başarısız olduğunuz birim kapsayıcısı yerel olarak sabitlenmiş birimlere sahipse, bu birimler katmanlı birimler olarak başarısız olur. StorSimple Cloud Appliance'ta yerel olarak sabitlenmiş birimler desteklenmez.

       ![Hedef hacim kaplarını görüntüleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bir hata yaptıktan sonra [StorSimple cihazınızı devre dışı bırakmanız veya silmeniz](storsimple-8000-deactivate-and-delete-device.md)gerekebilir.

* StorSimple Aygıt Yöneticisi hizmetini nasıl kullanacağınız hakkında bilgi için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

