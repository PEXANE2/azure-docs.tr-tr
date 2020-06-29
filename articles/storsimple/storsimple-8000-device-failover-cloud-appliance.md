---
title: StorSimple Cloud Appliance yük devretme ve olağanüstü durum kurtarma
description: StorSimple 8000 serisi fiziksel cihazınızı bir bulut gerecine devretmek hakkında bilgi edinin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 198b87d0a458108ce76934217c651d613ab5d75e
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85512071"
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a>StorSimple Cloud Appliance yük devretme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir olağanüstü durum varsa StorSimple 8000 serisi fiziksel cihazının yükünü StorSimple Cloud Appliance için gereken adımlar açıklanmaktadır. StorSimple, veri merkezindeki bir kaynak fiziksel cihazdan Azure 'da çalışan bir bulut gerecine veri geçirmek için cihaz yük devretme özelliğini kullanır. Bu öğreticideki kılavuz, StorSimple 8000 serisi fiziksel cihazlara ve güncelleştirme 3 ve üzeri yazılım sürümlerini çalıştıran bulut gereçlerine yöneliktir.

Cihaz yük devretmesi ve bir olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi edinmek için, [StorSimple 8000 Serisi Cihazlar Için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md)bölümüne gidin.

StorSimple fiziksel cihazını başka bir fiziksel cihaza devretmek için, [StorSimple fiziksel cihazına yük devretme](storsimple-8000-device-failover-physical-device.md)sayfasına gidin. Bir cihazın üzerine yük devretmek için [aynı StorSimple fiziksel cihazına yük devretmek](storsimple-8000-device-failover-same-device.md)üzere gidin.

## <a name="prerequisites"></a>Ön koşullar

- Cihaz yük devretmesi ile ilgili dikkat edilmesi gereken noktaları gözden geçirdiğinizden emin olun. Daha fazla bilgi için [cihaz yük devretmesi Için sık karşılaşılan hususlar](storsimple-8000-device-failover-disaster-recovery.md)bölümüne bakın.

- Bu yordamı çalıştırmadan önce oluşturulmuş ve yapılandırılmış bir StorSimple Cloud Appliance olması gerekir. Güncelleştirme 3 yazılım sürümünü veya daha yenisini çalıştırıyorsanız, DR için 8020 bulut gereci kullanmayı düşünün. 8020 modeli 64 TB ve Premium depolama kullanır. Daha fazla bilgi için, [StorSimple Cloud Appliance dağıtma ve yönetme](storsimple-8000-cloud-appliance-u2.md)konusuna gidin.

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a>Bulut gerecine yük devretme adımları

Bir hedef StorSimple Cloud Appliance cihazı geri yüklemek için aşağıdaki adımları gerçekleştirin.

1.  Yük devretmek istediğiniz birim kapsayıcısının ilişkili bulut anlık görüntülerinin olduğunu doğrulayın. Daha fazla bilgi için bkz. [yedek oluşturmak Için StorSimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).
2. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** dikey penceresinde hizmetinize bağlı cihazların listesine gidin.
    ![Cihaz Seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Kaynak cihazınızı seçin ve tıklayın. Kaynak cihazda yük devretmek istediğiniz birim kapsayıcıları vardır. **Ayarlar > birim kapsayıcıları**' na gidin.

    ![Cihaz Seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Başka bir cihaza yük devretmek istediğiniz bir birim kapsayıcısı seçin. Bu kapsayıcıdaki birimlerin listesini göstermek için birim kapsayıcısına tıklayın. Bir birim seçin, sağ tıklayın ve **Çevrimdışına Al** ' a tıklayarak birimi çevrimdışına alın.

    ![Cihaz Seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Birim kapsayıcısındaki tüm birimler için bu işlemi tekrarlayın.

     ![Cihaz Seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Diğer bir cihaza yük devretmek istediğiniz tüm birim kapsayıcıları için önceki adımı tekrarlayın.

7. **Cihazlar** dikey penceresine geri dönün. Komut çubuğundan yük **devret**' e tıklayın.

    ![Yük devretme ' ye tıklayın](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. Yük **devretme** dikey penceresinde aşağıdaki adımları uygulayın:
   
    1. **Kaynak**seçeneğine tıklayın. Yük devredilecek birim kapsayıcılarını seçin. **Yalnızca ilişkili bulut anlık görüntüleri ve çevrimdışı birimlere sahip birim kapsayıcıları görüntülenir.**
        ![Kaynak seçme](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. **Hedef**' e tıklayın. Kullanılabilir cihazların açılan listesinden bir hedef bulut gereci seçin. **Yalnızca kaynak birim kapsayıcılarını barındırmak için yeterli kapasiteye sahip cihazlar listede görüntülenir.**

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. **Özet** altında yük devretme ayarlarını gözden geçirin ve seçilen birim kapsayıcılarındaki birimlerin çevrimdışı olduğunu belirten onay kutusunu seçin. 

        ![Yük devretme ayarlarını gözden geçirme](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Yük devretme işi oluşturulur. Yük devretme işini izlemek için iş bildirimine tıklayın.

    ![Yük devretme işini izleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. Yük devretme tamamlandıktan sonra **cihazlar** dikey penceresine geri dönün.

    1. Yük devretme hedefi olarak kullanılan cihazı seçin.

       ![Cihaz Seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. **Birim kapsayıcıları**' na tıklayın. Tüm birim kapsayıcıları, eski cihazdaki birimlerle birlikte listelenmelidir.

       Yük devretene olan birim kapsayıcısının yerel olarak sabitlenmiş birimleri varsa, bu birimler katmanlı birimler olarak yük devretmez. Yerel olarak sabitlenmiş birimler StorSimple Cloud Appliance desteklenmez.

       ![Hedef birim kapsayıcılarını görüntüle](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Sonraki adımlar

* Yük devretme gerçekleştirdikten sonra [StorSimple cihazınızı devre dışı bırakmanız veya silmeniz](storsimple-8000-deactivate-and-delete-device.md)gerekebilir.

* StorSimple Aygıt Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için, StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)bölümüne gidin.

