---
title: Failover, başka bir StorSimple 8000 cihazına olağanüstü durum kurtarma
description: StorSimple 8000 serisi fiziksel cihazınızda başka bir fiziksel cihazda nasıl başarısız oylanın yapılacağını öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77468618"
---
# <a name="fail-over-to-a-storsimple-8000-series-physical-device"></a>StorSimple 8000 serisi fiziksel cihazda başarısız ol

## <a name="overview"></a>Genel Bakış

Bu öğretici, bir felaket olduğunda başka bir StorSimple fiziksel aygıta StorSimple 8000 serisi fiziksel aygıt üzerinde başarısız olmak için gereken adımları açıklar. StorSimple, veri merkezindeki bir kaynak fiziksel aygıttan başka bir fiziksel aygıta veri aktarmak için aygıt hata özelliğini kullanır. Bu öğreticideki kılavuz, yazılım sürümleri Güncelleştirme 3 ve sonrası çalışan StorSimple 8000 serisi fiziksel aygıtlar için geçerlidir.

Aygıt arızası ve bir felaketten kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi edinmek [için, StorSimple 8000 serisi cihazlar için Failover ve olağanüstü durum kurtarma'ya](storsimple-8000-device-failover-disaster-recovery.md)gidin.

StorSimple fiziksel aygıtı nda bir StorSimple Cloud Appliance'da başarısız olmak [için, Bir StorSimple Cloud Appliance'a başarısız](storsimple-8000-device-failover-cloud-appliance.md)olarak gidin. Fiziksel bir aygıt üzerinde kendisine başarısız olmak için, [aynı StorSimple fiziksel aygıta Başarısız 'a](storsimple-8000-device-failover-same-device.md)gidin.


## <a name="prerequisites"></a>Ön koşullar

- Aygıt arızası ile ilgili hususları gözden aldığınızdan emin olun. Daha fazla bilgi için, [aygıt failover için ortak hususlar](storsimple-8000-device-failover-disaster-recovery.md)gidin.

- Veri merkezinde bir StorSimple 8000 serisi fiziksel aygıtınız olmalıdır. Aygıtın Güncelleştirme 3 veya daha sonraki yazılım sürümünü çalıştırması gerekir. Daha fazla bilgi için [şirket içi StorSimple aygıtınızı dağıtın.](storsimple-8000-deployment-walkthrough-u2.md)


## <a name="steps-to-fail-over-to-a-physical-device"></a>Fiziksel bir aygıtta başarısız olacak adımlar

Cihazınızı hedef leyen fiziksel bir aygıta geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Başarısız olmak istediğiniz birim kapsayıcının ilişkili bulut anlık görüntüleri olduğunu doğrulayın. Daha fazla bilgi [için, yedekleme oluşturmak için StorSimple Device Manager hizmetini kullanın' a](storsimple-8000-manage-backup-policies-u2.md)gidin.
2. StorSimple Aygıt Yöneticisi'ne gidin ve **ardından Cihazlar'ı**tıklatın. **Cihazlar** bıçağında, hizmetinizle bağlantılı aygıtlar listesine gidin.
    ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Kaynak cihazınızı seçin ve tıklatın. Kaynak aygıtta başarısız olmak istediğiniz birim kapsayıcıları vardır. Ayarlar **> Birim Kapsayıcıları'na**gidin.
4. Başka bir aygıtta başarısız olmak istediğiniz bir birim kapsayıcı seçin. Bu kapsayıcının içindeki birimlerin listesini görüntülemek için birim kapsayıcısını tıklatın. Bir birim seçin, sağ tıklatın ve birimi çevrimdışı hale getirmek için **Çevrimdışı Kaldır'ı** tıklatın. Bu işlemi, birim kapsayıcıdaki tüm birimler için yineleyin.
5. Başka bir aygıtta başarısız olmak istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.
6. **Cihazlar** bıçağına geri dön. Komut çubuğundan Başarısız'ı **tıklatın.**
    ![Başarısız'ı tıklatın](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. Fail **over** blade'de aşağıdaki adımları gerçekleştirin:
   
   1. **Kaynak'ı**tıklatın. Bulut anlık görüntüleriyle ilişkili hacimli kapsayıcılar görüntülenir. Yalnızca görüntülenen kapsayıcılar başarısız olabilir. Toplu kapsayıcılar listesinde, başarısız olmak istediğiniz birim kapsayıcıları seçin. **Yalnızca ilişkili bulut anlık görüntüleri ve çevrimdışı birimlerin yer alan birim kapsayıcıları görüntülenir.**

       ![Kaynak seçme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. **Hedef'i**tıklatın. Önceki adımda seçilen birim kapsayıcıları için, kullanılabilir aygıtların açılır listesinden bir hedef aygıt seçin. Listede yalnızca kaynak birim kapsayıcılarını barındıracak kapasiteye sahip aygıtlar görüntülenir.

        ![Hedef seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Son olarak, **Özet**altında tüm failover ayarlarını gözden geçirin. Ayarları inceledikten sonra, seçili birim kapsayıcılarında bulunan birimlerin çevrimdışı olduğunu belirten onay kutusunu seçin. **Tamam**'a tıklayın.

       ![Başarısız ayarları gözden geçirme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple bir başarısız iş oluşturur. **İşler** bıçağı aracılığıyla başarısız işi izlemek için iş bildirimini tıklatın.

    Üzerinde başarısız olduğunuz birim kapsayıcıyerel birimlere sahipse, kapsayıcıda her yerel birim için (katmanlı birimler için değil) tek tek geri yükleme işleri görürsünüz. Bu geri yükleme işleri tamamlamak için oldukça biraz zaman alabilir. Başarısız lık işinin daha erken tamamlanması olasıdır. Bu birimler, yalnızca geri yükleme işleri tamamlandıktan sonra yerel garantilere sahip olacaktır.

    ![Başarısız işi izleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. Arıza tamamlandıktan sonra **Cihazlar** bıçağına gidin.
   
   1. Başarısızlık işlemi için hedef aygıt olarak kullanılan aygıtı seçin.

       ![Cihazı seçin](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. **Volume Containers** bıçak gidin. Tüm birim kapsayıcıları, eski aygıttan gelen hacimlerle birlikte listelenmelidir.

       ![Hedef hacim kaplarını görüntüleme](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Sonraki adımlar

* Bir hata yaptıktan sonra [StorSimple cihazınızı devre dışı bırakmanız veya silmeniz](storsimple-8000-deactivate-and-delete-device.md)gerekebilir.
* StorSimple Aygıt Yöneticisi hizmetini nasıl kullanacağınız hakkında bilgi için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

