---
title: StorSimple 8000 serisi cihazı devre dışı bırakma ve silme | Microsoft Docs
description: İlk olarak devre dışı bırakarak StorSimple cihazını hizmetten kaldırmayı ve sonra silmeyi açıklar.
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
ms.date: 07/23/2018
ms.author: alkohli
ms.openlocfilehash: 116ac5c4efda87b5d16336dd326d516299f6955d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61481975"
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>StorSimple cihazını devre dışı bırakma ve silme

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Aygıt Yöneticisi hizmetine bağlı bir StorSimple cihazını devre dışı bırakma ve silme işlemlerinin nasıl yapılacağı açıklanır. Bu makaledeki kılavuz yalnızca StorSimple bulut gereçlerini içeren StorSimple 8000 serisi cihazlar için geçerlidir. StorSimple Sanal dizisi kullanıyorsanız, [devre dışı bırak ' a gidin ve StorSimple Sanal dizisini silin](storsimple-virtual-array-deactivate-and-delete-device.md).

Cihaz ve ilgili StorSimple Aygıt Yöneticisi hizmeti arasındaki bağlantıyı devre dışı bırakma. Bir StorSimple cihazını hizmetten alabilirsiniz (örneğin, cihazınızı değiştiriyorsanız veya yükseltiyorsanız veya artık StorSimple kullanmıyorsanız). Bu durumda, silmeden önce cihazı devre dışı bırakmanız gerekir.

Bir cihazı devre dışı bıraktıktan sonra, cihazda yerel olarak depolanan verilere artık erişilemez. Yalnızca bulutta depolanan cihazla ilişkili veriler kurtarılabilir.

> [!WARNING]
> Devre dışı bırakma kalıcı bir işlemdir ve geri alınamaz. Devre dışı bırakılmış bir cihaz, fabrika varsayılanlarına Sıfırlanmadığı takdirde StorSimple Aygıt Yöneticisi hizmetine kaydedilemez.
>
> Fabrika sıfırlama işlemi, cihazınızda yerel olarak depolanan tüm verileri siler. Bu nedenle, bir cihazı devre dışı bırakmadan önce tüm verilerinizin bulut anlık görüntüsünü almalısınız. Bu bulut anlık görüntüsü, daha sonraki bir aşamada tüm verileri kurtarmanızı sağlar.

Bu öğreticiyi okuduktan sonra şunları yapabilirsiniz:

* Bir cihazı devre dışı bırakın ve verileri silin.
* Bir cihazı devre dışı bırakın ve verileri koruyun.

> [!NOTE]
> StorSimple fiziksel cihazını veya bulut gerecini devre dışı bırakmadan önce, söz konusu cihaza bağlı istemcileri ve Konakları durdurun veya silin.


## <a name="deactivate-and-delete-data"></a>Verileri devre dışı bırakma ve silme

Cihazı tamamen silmek ve Cihazdaki verileri sürdürmek istemiyorsanız, aşağıdaki adımları uygulayın ve ardından aşağıdaki adımları izleyin.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Cihazı devre dışı bırakmak ve verileri silmek için

1. Bir cihazı devre dışı bırakmadan önce, cihazla ilişkili tüm birim kapsayıcılarını (ve birimleri) silmeniz gerekir. Birim kapsayıcılarını yalnızca ilişkili yedeklemeleri sildikten sonra silebilirsiniz.

    > [!NOTE]
    > StorSimple fiziksel cihazını veya bulut gerecini devre dışı bırakmadan önce, silinen birim kapsayıcısından gelen verilerin cihazdan gerçekten silindiğinden emin olun. Bulut tüketim grafiklerini izleyebilir ve sildiğiniz yedeklemeler nedeniyle bulut kullanımı ' nı gördüğünüzde, cihazı devre dışı bırakmaya devam edebilirsiniz. Bu bırakma yapılmadan önce cihazı devre dışı bırakırsanız, veriler depolama hesabı ve tahakkuk ücretleri ' nde kasalar.

2. Cihazı şu şekilde devre dışı bırakın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** dikey penceresinde, devre dışı bırakmak istediğiniz cihazı seçin, sağ tıklayın ve ardından **devre dışı bırak**' a tıklayın.

        ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. **Devre dışı bırak** dikey penceresinde, onaylamak için cihaz adını yazın ve **devre dışı bırak**' a tıklayın. Devre dışı bırakma işlemi başlar ve birkaç dakika sürer.

        ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Devre dışı bırakma işleminden sonra cihazı tamamen silebilirsiniz. Bir cihazın silinmesi, hizmeti hizmetine bağlı cihazların listesinden kaldırır. Hizmet bundan sonra silinen cihazı yönetemez. Cihazı silmek için aşağıdaki adımları kullanın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** dikey penceresinde silmek istediğiniz devre dışı bırakılmış cihazı seçin, sağ tıklayın ve ardından **Sil**' e tıklayın.

        ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. **Sil** dikey penceresinde, onaylamak için cihaz adını yazın ve ardından **Sil**' e tıklayın. Silmenin tamamlanabilmesi birkaç dakika sürer.

        ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandıktan sonra size bildirilir. Cihaz listesi ayrıca silme işlemini yansıtacak şekilde güncelleştirilir.

## <a name="deactivate-and-retain-data"></a>Verileri devre dışı bırakma ve koruma

Cihazı silmek, ancak verileri sürdürmek istiyorsanız aşağıdaki adımları izleyin:

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Bir cihazı devre dışı bırakmak ve verileri sürdürmek için
1. Cihazı devre dışı bırakın. Tüm birim kapsayıcıları ve cihazın anlık görüntüleri kalır.
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** dikey penceresinde, devre dışı bırakmak istediğiniz cihazı seçin, sağ tıklayın ve ardından **devre dışı bırak**' a tıklayın.

         ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. **Devre dışı bırak** dikey penceresinde, onaylamak için cihaz adını yazın ve **devre dışı bırak**' a tıklayın. Devre dışı bırakma işlemi başlar ve birkaç dakika sürer.

         ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Artık birim kapsayıcılarının ve ilişkili anlık görüntülerin yükünü devreedebilirsiniz. Yordamlar için, [StorSimple cihazınız Için yük devretme ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md)bölümüne gidin.
3. Devre dışı bırakma ve yük devretme işleminden sonra cihazı tamamen silebilirsiniz. Bir cihazın silinmesi, hizmeti hizmetine bağlı cihazların listesinden kaldırır. Hizmet bundan sonra silinen cihazı yönetemez. Cihazı silmek için aşağıdaki adımları izleyin:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** dikey penceresinde silmek istediğiniz devre dışı bırakılmış cihazı seçin, sağ tıklayın ve ardından **Sil**' e tıklayın.

       ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. **Sil** dikey penceresinde, onaylamak için cihaz adını yazın ve ardından **Sil**' e tıklayın. Silmenin tamamlanabilmesi birkaç dakika sürer.

       ![StorSimple cihazını devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandıktan sonra size bildirilir. Cihaz listesi ayrıca silme işlemini yansıtacak şekilde güncelleştirilir.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Bulut gerecini devre dışı bırakma ve silme

StorSimple Cloud Appliance için portaldan devre dışı bırakma işlemi, sanal makineyi ve sağlandığında oluşturulan kaynakları kaldırır ve siler. Bulut gereci devre dışı bırakıldıktan sonra, önceki durumuna geri yüklenemez.

![StorSimple Cloud Appliance devre dışı bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Aşağıdaki eylemlerde devre dışı bırakma sonuçları:

* StorSimple Cloud Appliance hizmetten kaldırılır.
* StorSimple Cloud Appliance sanal makinesi silindi.
* StorSimple Cloud Appliance için oluşturulan işletim sistemi diski ve veri diskleri korunur. Bu varlıkları kullanmıyorsanız, bunları el ile silmelisiniz.
* Sağlama sırasında oluşturulan barındırılan hizmet ve sanal ağ tutulur. Bu varlıkları kullanmıyorsanız, bunları el ile silmelisiniz.
* StorSimple Cloud Appliance tarafından oluşturulan bulut anlık görüntüleri korunur.

Bulut gereci devre dışı bırakıldıktan sonra cihaz listesinden silebilirsiniz. Devre dışı bırakılmış cihazı seçin, sağ tıklayın ve ardından **Sil**' e tıklayın. StorSimple, cihaz silindikten ve cihazların listesi güncelleştikten sonra size bildirir.

## <a name="next-steps"></a>Sonraki adımlar

* Devre dışı bırakılan cihazı fabrika ayarlarına geri yüklemek için, [cihazı varsayılan fabrika ayarlarına sıfırlama](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings)bölümüne gidin.
* Teknik yardım için [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md).
* StorSimple Aygıt Yöneticisi hizmetini kullanma hakkında daha fazla bilgi edinmek için, StorSimple [cihazınızı yönetmek için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)bölümüne gidin.

