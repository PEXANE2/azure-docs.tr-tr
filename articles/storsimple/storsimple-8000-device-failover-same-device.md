---
title: Aynı StorSimple 8000 cihazında arıza ve olağanüstü durum kurtarma
description: StorSimple cihazınızüzerinde aynı cihazda nasıl başarısız oylanın yapılacağını öğrenin.
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: c8fe2d7ec7649f47f6cb9c8ae2c83f19c15691b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471814"
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a>StorSimple fiziksel cihazınız üzerinde aynı cihazda başarısız

## <a name="overview"></a>Genel Bakış

Bu öğretici, bir felaket olduğunda kendisine bir StorSimple 8000 serisi fiziksel cihaz üzerinde başarısız olmak için gerekli adımları açıklar. StorSimple, veri merkezindeki bir kaynak fiziksel aygıttan başka bir fiziksel aygıta veri aktarmak için aygıt hata özelliğini kullanır. Bu öğreticideki kılavuz, yazılım sürümleri Güncelleştirme 3 ve sonrası çalışan StorSimple 8000 serisi fiziksel aygıtlar için geçerlidir.

Aygıt arızası ve bir felaketten kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi edinmek [için, StorSimple 8000 serisi cihazlar için Failover ve olağanüstü durum kurtarma'ya](storsimple-8000-device-failover-disaster-recovery.md)gidin.

Fiziksel bir aygıt üzerinde başka bir fiziksel aygıtta başarısız olmak için, [aynı StorSimple fiziksel aygıta Başarısız 'a](storsimple-8000-device-failover-physical-device.md)gidin. StorSimple fiziksel aygıtı nda bir StorSimple Cloud Appliance'da başarısız olmak [için, Bir StorSimple Cloud Appliance'a başarısız](storsimple-8000-device-failover-cloud-appliance.md)olarak gidin.


## <a name="prerequisites"></a>Ön koşullar

- Aygıt arızası ile ilgili hususları gözden aldığınızdan emin olun. Daha fazla bilgi için, [aygıt failover için ortak hususlar](storsimple-8000-device-failover-disaster-recovery.md)gidin.


## <a name="steps-to-fail-over-to-the-same-device"></a>Aynı aygıtta başarısız olacak adımlar

Aynı aygıtta başarısız olmanız gerekiyorsa aşağıdaki adımları gerçekleştirin.

1. Cihazınızdaki tüm birimlerin bulut anlık görüntülerini alın. Daha fazla bilgi [için, yedekleme oluşturmak için StorSimple Device Manager hizmetini kullanın' a](storsimple-8000-manage-backup-policies-u2.md)gidin.
2. Cihazınızı fabrika varsayılanlarına sıfırlayın. [StorSimple cihazını fabrika varsayılan ayarlarına sıfırlama konusunda](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings)ayrıntılı yönergeleri izleyin.
3. StorSimple Device Manager hizmetine gidin ve ardından **Aygıtlar'ı**seçin. **Aygıtlar** bıçağında, eski aygıt **Çevrimdışı**olarak gösterilmelidir.

    ![Kaynak aygıtı çevrimdışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Cihazınızı yapılandırın ve StorSimple Device Manager hizmetinize yeniden kaydedin. Yeni kaydedilmiş **cihaz, kuruluma hazır**olarak gösterilmelidir. Yeni aygıtın aygıt adı eski aygıtla aynıdır, ancak aygıtın fabrika varsayılanına sıfırlandığını ve yeniden kaydedildiğini belirtmek için bir rakamla eklenir.

    ![Yeni kaydolan aygıt kurulmaya hazır](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Yeni aygıt için aygıt kurulumuna tamamlayın. Daha fazla bilgi için [Adım 4'e gidin: Minimum aygıt kurulumuna tamamlayın.](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup) **Cihazlar** bıçağında, aygıtın durumu **Çevrimiçi**olarak değişir.

   > [!IMPORTANT]
   > **Önce minimum yapılandırmayı tamamlayın, yoksa DR'niz başarısız olabilir.**

    ![Çevrimiçi yeni kayıtlı cihaz](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Eski aygıtı (durum çevrimdışı) seçin ve komut çubuğundan **Başarısız'ı**tıklatın. Fail **over** blade'de, kaynak olarak eski aygıtı seçin ve hedef aygıtı yeni kaydedilmiş aygıt olarak belirtin.

    ![Failover özeti](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Ayrıntılı yönergeler için başka bir fiziksel aygıta başarısız bakın.

7. **İşler** bıçağından izleyebileceğiniz bir aygıt geri yükleme işi oluşturulur.

8. İş başarıyla tamamlandıktan sonra, yeni aygıta erişin ve **Volume kapsayıcıları** bıçağına gidin. Eski aygıttaki tüm birim kapsayıcıların yeni aygıta geçirilmiş olduğunu doğrulayın.

   ![Taşınan birim kapsayıcılar](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Hata tamamlandıktan sonra, eski aygıtı devre dışı bırakabilir ve portaldan silebilirsiniz. Eski aygıtı (çevrimdışı), sağ tıklatın ve sonra **Devre dışı'yı**seçin. Aygıt devre dışı bırakıldıktan sonra aygıtın durumu güncelleştirilir.

     ![Kaynak aygıt devre dışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Devre dışı bırakılan aygıtı seçin, sağ tıklatın ve sonra **Sil'i**seçin. Bu, aygıtı aygıtlar listesinden siler.

    ![Kaynak aygıt silindi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Sonraki adımlar

* Bir hata yaptıktan sonra [StorSimple cihazınızı devre dışı bırakmanız veya silmeniz](storsimple-8000-deactivate-and-delete-device.md)gerekebilir.
* StorSimple Aygıt Yöneticisi hizmetini nasıl kullanacağınız hakkında bilgi için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

