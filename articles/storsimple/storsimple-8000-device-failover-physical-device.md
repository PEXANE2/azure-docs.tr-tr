---
title: Başka bir StorSimple 8000 cihazına yük devretme, olağanüstü durum kurtarma
description: StorSimple 8000 serisi fiziksel cihazınızı başka bir fiziksel cihaza devretmek hakkında bilgi edinin.
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 9e4e890ab5491e46ffe5ea0e1c168d168f9cc729
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77468618"
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a>StorSimple 8000 serisi fiziksel cihazına yük devretme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir olağanüstü durum varsa StorSimple 8000 serisi fiziksel cihazının başka bir StorSimple fiziksel cihazına yük devri yapmak için gereken adımlar açıklanmaktadır. StorSimple, veri merkezindeki kaynak fiziksel cihazından başka bir fiziksel cihaza veri geçirmek için cihaz yük devretme özelliğini kullanır. Bu öğreticideki kılavuz, güncelleştirme 3 ve üzeri yazılım sürümlerini çalıştıran StorSimple 8000 serisi fiziksel cihazları için geçerlidir.

Cihaz yük devretmesi ve bir olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi edinmek için, [StorSimple 8000 Serisi Cihazlar Için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md)bölümüne gidin.

StorSimple fiziksel cihazını bir StorSimple Cloud Appliance yük devretmek için, [StorSimple Cloud Appliance yük devretme](storsimple-8000-device-failover-cloud-appliance.md)sayfasına gidin. Fiziksel bir cihazın üzerine yük devretmek için [aynı StorSimple fiziksel cihazına yük devretmek](storsimple-8000-device-failover-same-device.md)üzere gidin.


## <a name="prerequisites"></a>Önkoşullar

- Cihaz yük devretmesi ile ilgili dikkat edilmesi gereken noktaları gözden geçirdiğinizden emin olun. Daha fazla bilgi için [cihaz yük devretmesi Için sık karşılaşılan hususlar](storsimple-8000-device-failover-disaster-recovery.md)bölümüne bakın.

- Veri merkezinde dağıtılmış bir StorSimple 8000 serisi fiziksel cihazınız olmalıdır. Cihazın güncelleştirme 3 veya üzeri bir yazılım sürümü çalıştırması gerekir. Daha fazla bilgi için, Şirket [Içi StorSimple cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md)konusuna gidin.


## <a name="steps-to-fail-over-to-a-physical-device"></a>Fiziksel bir cihaza yük devretme adımları

Cihazınızı bir hedef fiziksel cihaza geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Yük devretmek istediğiniz birim kapsayıcısının ilişkili bulut anlık görüntülerinin olduğunu doğrulayın. Daha fazla bilgi için bkz. [yedek oluşturmak Için StorSimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).
2. StorSimple Aygıt Yöneticisi gidin ve ardından **cihazlar**' a tıklayın. **Cihazlar** dikey penceresinde hizmetinize bağlı cihazların listesine gidin.
    cihaz](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png) ![seçin
3. Kaynak cihazınızı seçin ve tıklayın. Kaynak cihazda yük devretmek istediğiniz birim kapsayıcıları vardır. **Ayarlar > birim kapsayıcıları**' na gidin.
4. Başka bir cihaza yük devretmek istediğiniz bir birim kapsayıcısı seçin. Bu kapsayıcıdaki birimlerin listesini göstermek için birim kapsayıcısına tıklayın. Bir birim seçin, sağ tıklayın ve **Çevrimdışına Al** ' a tıklayarak birimi çevrimdışına alın. Birim kapsayıcısındaki tüm birimler için bu işlemi tekrarlayın.
5. Diğer bir cihaza yük devretmek istediğiniz tüm birim kapsayıcıları için önceki adımı tekrarlayın.
6. **Cihazlar** dikey penceresine geri dönün. Komut çubuğundan yük **devret**' e tıklayın.
    ![yük devretme seçeneğine tıklayın](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. Yük **devretme** dikey penceresinde aşağıdaki adımları uygulayın:
   
   1. **Kaynak**seçeneğine tıklayın. Bulut anlık görüntüleriyle ilişkili birimlere sahip birim kapsayıcıları görüntülenir. Yalnızca görüntülenen kapsayıcılar yük devretme için uygun değildir. Birim kapsayıcıları listesinde, yük devretmek istediğiniz birim kapsayıcılarını seçin. **Yalnızca ilişkili bulut anlık görüntüleri ve çevrimdışı birimlere sahip birim kapsayıcıları görüntülenir.**

       ![Kaynak seçme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. **Hedef**' e tıklayın. Önceki adımda seçilen birim kapsayıcıları için, kullanılabilir cihazların açılan listesinden bir hedef cihaz seçin. Yalnızca kaynak birim kapsayıcılarını barındırmak için yeterli kapasiteye sahip cihazlar listede görüntülenir.

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Son olarak, **Özet**altındaki tüm yük devretme ayarlarını gözden geçirin. Ayarları inceledikten sonra, seçili birim kapsayıcılarındaki birimlerin çevrimdışı olduğunu belirten onay kutusunu seçin. **Tamam** düğmesine tıklayın.

       ![Yük devretme ayarlarını gözden geçirme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple, bir yük devretme işi oluşturur. **İşler** dikey penceresi aracılığıyla yük devretme işini izlemek için iş bildirimine tıklayın.

    Yük devredilen birim kapsayıcısının yerel birimleri varsa, kapsayıcıda her bir yerel birim (katmanlı birimler için değil) için ayrı geri yükleme işleri görürsünüz. Bu geri yükleme işlerinin tamamlanması oldukça zaman alabilir. Yük devretme işinin daha önce tamamlanmasından kaynaklanıyor olabilir. Bu birimlerin yalnızca geri yükleme işleri tamamlandıktan sonra yerel garantisi olur.

    ![Yük devretme işini izleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Yük devretme tamamlandıktan sonra **cihazlar** dikey penceresine gidin.
   
   1. Yük devretme işlemi için hedef cihaz olarak kullanılan cihazı seçin.

       ![Cihaz Seç](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. **Birim kapsayıcıları** dikey penceresine gidin. Tüm birim kapsayıcıları, eski cihazdaki birimlerle birlikte listelenmelidir.

       ![Hedef birim kapsayıcılarını görüntüle](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Sonraki adımlar

* Yük devretme gerçekleştirdikten sonra [StorSimple cihazınızı devre dışı bırakmanız veya silmeniz](storsimple-8000-deactivate-and-delete-device.md)gerekebilir.
* StorSimple Aygıt Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için, StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)bölümüne gidin.

