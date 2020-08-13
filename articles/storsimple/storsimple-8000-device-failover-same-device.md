---
title: Öğretici-StorSimple fiziksel cihazını aynı cihaza devreder
description: Bir olağanüstü durum varsa StorSimple 8000 serisi fiziksel cihazının kendine yük devri için gereken adımları öğrenin.
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: 17c116194aa52a82246bcee9114824e8a918ebbd
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88184338"
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a>StorSimple fiziksel cihazınızı aynı cihaza devreder

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir olağanüstü durum varsa StorSimple 8000 serisi fiziksel cihazının kendine yük devri için gereken adımlar açıklanmaktadır. StorSimple, veri merkezindeki kaynak fiziksel cihazından başka bir fiziksel cihaza veri geçirmek için cihaz yük devretme özelliğini kullanır. Bu öğreticideki kılavuz, güncelleştirme 3 ve üzeri yazılım sürümlerini çalıştıran StorSimple 8000 serisi fiziksel cihazları için geçerlidir.

Cihaz yük devretmesi ve bir olağanüstü durumdan kurtarmak için nasıl kullanıldığı hakkında daha fazla bilgi edinmek için, [StorSimple 8000 Serisi Cihazlar Için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md)bölümüne gidin.

Fiziksel bir cihazı başka bir fiziksel cihaza devretmek için [aynı StorSimple fiziksel cihazına yük devretmek](storsimple-8000-device-failover-physical-device.md)üzere gidin. StorSimple fiziksel cihazını bir StorSimple Cloud Appliance yük devretmek için, [StorSimple Cloud Appliance yük devretme](storsimple-8000-device-failover-cloud-appliance.md)sayfasına gidin.


## <a name="prerequisites"></a>Önkoşullar

- Cihaz yük devretmesi ile ilgili dikkat edilmesi gereken noktaları gözden geçirdiğinizden emin olun. Daha fazla bilgi için [cihaz yük devretmesi Için sık karşılaşılan hususlar](storsimple-8000-device-failover-disaster-recovery.md)bölümüne bakın.


## <a name="steps-to-fail-over-to-the-same-device"></a>Aynı cihaza yük devretme adımları

Aynı cihaza yük devri yapmanız gerekiyorsa aşağıdaki adımları gerçekleştirin.

1. Cihazınızdaki tüm birimlerin bulut anlık görüntülerini alın. Daha fazla bilgi için bkz. [yedek oluşturmak Için StorSimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).
2. Cihazınızı fabrika ayarlarına sıfırlayın. [StorSimple cihazını fabrika varsayılan ayarlarına sıfırlama](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings)konusundaki ayrıntılı yönergeleri izleyin.
3. StorSimple Aygıt Yöneticisi hizmetine gidin ve ardından **cihazlar**' ı seçin. **Cihazlar** dikey penceresinde eski cihaz **çevrimdışı**olarak gösterilmelidir.

    ![Kaynak cihaz çevrimdışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Cihazınızı yapılandırın ve StorSimple Aygıt Yöneticisi hizmetinize yeniden kaydedin. Yeni kaydedilen cihaz, **ayarlanmaya hazırlık**olarak gösterilmelidir. Yeni cihaz için cihaz adı eski cihazla aynıdır, ancak cihazın fabrika varsayılanına sıfırlandığını ve yeniden kaydedildiğini göstermek için bir sayıyla birlikte eklenir.

    ![Yeni kayıtlı cihaz ayarlamaya hazırlanıyor](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Yeni cihaz için, cihaz kurulumunu doldurun. Daha fazla bilgi için 4. [Adım: en düşük cihaz kurulumunu tamamlayın](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup)bölümüne gidin. **Cihazlar** dikey penceresinde, cihazın durumu **çevrimiçi**olarak değişir.

   > [!IMPORTANT]
   > **Önce en düşük yapılandırmayı tamamladıktan sonra, DR başarısız olabilir.**

    ![Yeni kayıtlı cihaz çevrimiçi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Eski cihazı (çevrimdışı durum) seçin ve komut çubuğundan yük **devretme**' ye tıklayın. Yük **devretme** dikey penceresinde, kaynak olarak eski cihaz ' ı seçin ve hedef cihazı yeni kayıtlı cihaz olarak belirtin.

    ![Yük devretme Özeti](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Ayrıntılı yönergeler için başka bir fiziksel cihaza yük devretme bölümüne bakın.

7. **İşler** dikey penceresinden izleyebilmeniz için bir cihaz geri yükleme işi oluşturulur.

8. İş başarıyla tamamlandıktan sonra yeni cihaza erişin ve **birim kapsayıcıları** dikey penceresine gidin. Eski cihazdaki tüm birim kapsayıcılarının yeni cihaza geçirildiğini doğrulayın.

   ![Birim kapsayıcıları geçirildi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. Yük devretme işlemi tamamlandıktan sonra, eski cihazı portaldan devre dışı bırakabilir ve silebilirsiniz. Eski cihazı (çevrimdışı) seçin, sağ tıklayın ve ardından **devre dışı bırak**' ı seçin. Cihaz devre dışı bırakıldıktan sonra cihazın durumu güncelleştirilir.

     ![Kaynak cihaz devre dışı](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Devre dışı bırakılmış cihazı seçin, sağ tıklayın ve ardından **Sil**' i seçin. Bu, cihazı cihaz listesinden siler.

    ![Kaynak cihaz silindi](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Sonraki adımlar

* Yük devretme gerçekleştirdikten sonra [StorSimple cihazınızı devre dışı bırakmanız veya silmeniz](storsimple-8000-deactivate-and-delete-device.md)gerekebilir.
* StorSimple Aygıt Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için, StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)bölümüne gidin.

