---
title: StorSimple 8000 serisi cihazı devre dışı bırakma ve silme | Microsoft Dokümanlar
description: StorSimple aygıtının önce devre dışı bırakılarak ve sonra silerek hizmetten nasıl kaldırılış yapılacağını açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61481975"
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>StorSimple cihazını devre dışı bırakma ve silme

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Device Manager hizmetine bağlı bir StorSimple aygıtının nasıl devre dışı bırakılabildiğini ve silinir. Bu makaledeki kılavuz yalnızca StorSimple Bulut Cihazları da dahil olmak üzere StorSimple 8000 serisi cihazlar için geçerlidir. Bir StorSimple Sanal Dizi kullanıyorsanız, devre [dışı bırak'a gidin ve StorSimple Sanal Dizisini silin.](storsimple-virtual-array-deactivate-and-delete-device.md)

Devre dışı bırakma, aygıt ile ilgili StorSimple Device Manager hizmeti arasındaki bağlantıyı keser. Bir StorSimple cihazını hizmet dışı almak isteyebilirsiniz (örneğin, aygıtınızı değiştiriyor veya yükseltiyorsanız veya storSimple kullanmıyorsanız). Bu nedenle, aygıtı silmeden önce devre dışı bırakmanız gerekir.

Bir aygıtı devre dışı bıraktığınızda, aygıtta yerel olarak depolanan verilere artık erişilemez. Yalnızca bulutta depolanan aygıtla ilişkili veriler kurtarılabilir.

> [!WARNING]
> Devre dışı bırakma kalıcı bir işlemdir ve geri alınamaz. Devre dışı bırakılan aygıt, fabrika varsayılanlarına sıfırlamadığı sürece StorSimple Device Manager hizmetine kaydedilemez.
>
> Fabrika sıfırlama işlemi cihazınızda yerel olarak depolanan tüm verileri siler. Bu nedenle, bir aygıtı devre dışı bırakmadan önce tüm verilerinizin bulut anlık görüntüsünü almanız gerekir. Bu bulut anlık görüntüsü, tüm verileri daha sonraki bir aşamada kurtarmanızı sağlar.

Bu öğretici okuduktan sonra, mümkün olacak:

* Aygıtı devre dışı bırakın ve verileri silin.
* Aygıtı devre dışı bırakın ve verileri koruyun.

> [!NOTE]
> StorSimple fiziksel aygıtını veya bulut cihazını devre dışı bırakmadan önce, bu cihaza bağlı olan istemcileri ve ana bilgisayarları durdurun veya silin.


## <a name="deactivate-and-delete-data"></a>Verileri devre dışı bırakma ve silme

Aygıtı tamamen silerken ilgileniyorsanız ve aygıttaki verileri saklamak istemiyorsanız, aşağıdaki adımları tamamlayın.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Aygıtı devre dışı bırakmak ve verileri silmek için

1. Aygıtı devre dışı bırakmadan önce, aygıtla ilişkili tüm birim kapsayıcılarını (ve birimleri) silmeniz gerekir. Birim kapsayıcılarını yalnızca ilişkili yedeklemeleri sildikten sonra silebilirsiniz.

    > [!NOTE]
    > StorSimple fiziksel aygıtını veya bulut cihazını devre dışı bırakmadan önce, silinen ses kapsayıcısından gelen verilerin aygıttan gerçekten silindiğinden emin olun. Bulut tüketim grafiklerini izleyebilir ve sildiğiniz yedeklemeler nedeniyle bulut kullanımının düştüğünü gördüğünüzde aygıtı devre dışı bırakmaya devam edebilirsiniz. Bu düşüş oluşmadan önce aygıtı devre dışı bırakırsanız, veriler depolama hesabında mahsur kalır ve ücret tahakkuk eder.

2. Aygıtı aşağıdaki gibi devre dışı bırakın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** bıçağında, devre dışı bırakmak istediğiniz aygıtı seçin, sağ tıklatın ve sonra **Devre dışı bırak'ı**tıklatın.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. Devre **Dışı Bırak** bıçağında, onaylamak için aygıt adını yazın ve sonra **Devre Dışı'yı**tıklatın. Devre dışı bırakma işlemi başlar ve tamamlanması birkaç dakika sürer.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Devre dışı bırakmadan sonra aygıtı tamamen silebilirsiniz. Aygıtı silerse, aygıtı hizmete bağlı aygıtlar listesinden kaldırır. Hizmet daha sonra silinen aygıtı artık yönetemez. Aygıtı silmek için aşağıdaki adımları kullanın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** bıçağında, silmek istediğiniz devre dışı bırakılan aygıtı seçin, sağ tıklatın ve sonra **Sil'i**tıklatın.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. **Sil** çubuğuna, onaylamak için aygıt adını yazın ve sonra **Sil'i**tıklatın. Silme işleminin tamamlanması birkaç dakika sürer.

        ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandıktan sonra size bildirilir. Aygıt listesi, silme işlemini yansıtacak şekilde de güncellenir.

## <a name="deactivate-and-retain-data"></a>Verileri devre dışı bırakma ve saklama

Aygıtı silerken ilgileniyorsanız ancak verileri saklamak istiyorsanız, aşağıdaki adımları tamamlayın:

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Bir aygıtı devre dışı bırakmak ve verileri korumak için
1. Aygıtı devre dışı bırakın. Tüm ses kapları ve aygıtın anlık görüntüleri kalır.
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** bıçağında, devre dışı bırakmak istediğiniz aygıtı seçin, sağ tıklatın ve sonra **Devre dışı bırak'ı**tıklatın.

         ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. Devre **Dışı Bırak** bıçağında, onaylamak için aygıt adını yazın ve sonra **Devre Dışı'yı**tıklatın. Devre dışı bırakma işlemi başlar ve tamamlanması birkaç dakika sürer.

         ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Artık birim kapsayıcıları ve ilişkili anlık görüntüler üzerinde başarısız olabilirsiniz. Yordamlar [için, Failover ve StorSimple cihazınız için olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md)gidin.
3. Devre dışı bırakma ve devre dışı bırakma sonrasında aygıtı tamamen silebilirsiniz. Aygıtı silerse, aygıtı hizmete bağlı aygıtlar listesinden kaldırır. Hizmet daha sonra silinen aygıtı artık yönetemez. Aygıtı silmek için aşağıdaki adımları tamamlayın:
   
   1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. **Cihazlar** bıçağında, silmek istediğiniz devre dışı bırakılan aygıtı seçin, sağ tıklatın ve sonra **Sil'i**tıklatın.

       ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. **Sil** çubuğuna, onaylamak için aygıt adını yazın ve sonra **Sil'i**tıklatın. Silme işleminin tamamlanması birkaç dakika sürer.

       ![StorSimple cihazını devre dışı bırakma](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Silme işlemi başarıyla tamamlandıktan sonra size bildirilir. Aygıt listesi, silme işlemini yansıtacak şekilde de güncellenir.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Bulut cihazını devre dışı bırakma ve silme

StorSimple Cloud Appliance için, portaldan devre dışı bırakma, sanal makineyi ve sağlandığında oluşturulan kaynakları yer alır ve siler. Bulut gereci devre dışı bırakıldıktan sonra, önceki durumuna geri yüklenemez.

![StorSimple Cloud Cihazıdevre Dışı Bırak](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Devre dışı bırakma aşağıdaki eylemlerle sonuçlanır:

* StorSimple Cloud Appliance hizmetten kaldırılır.
* StorSimple Cloud Appliance'ın sanal makinesi silinir.
* StorSimple Cloud Appliance için oluşturulan işletim sistemi diski ve veri diskleri korunur. Bu varlıkları kullanmıyorsanız, bunları el ile silmeniz gerekir.
* Sağlama sırasında oluşturulan barındırılan hizmet ve Sanal Ağ korunur. Bu varlıkları kullanmıyorsanız, bunları el ile silmeniz gerekir.
* StorSimple Cloud Appliance tarafından oluşturulan bulut anlık görüntüleri korunur.

Bulut cihazı devre dışı bırakıldıktan sonra, aygıtlar listesinden silebilirsiniz. Devre dışı bırakılan aygıtı seçin, sağ tıklatın ve sonra **Sil'i**tıklatın. StorSimple, aygıt silindikten ve aygıtların listesi güncelleştirildikten sonra sizi haber verir.

## <a name="next-steps"></a>Sonraki adımlar

* Devre dışı bırakılan aygıtı fabrika varsayılanlarına geri yüklemek için aygıtı [fabrika varsayılan ayarlarına sıfırla'ya](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings)gidin.
* Teknik yardım için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
* StorSimple Aygıt Yöneticisi hizmetini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

