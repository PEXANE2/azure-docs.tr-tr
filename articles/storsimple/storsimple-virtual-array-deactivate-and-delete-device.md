---
title: Microsoft Azure StorBasit Sanal Diziyi devre dışı bırakma ve silme | Microsoft Dokümanlar
description: StorSimple aygıtının önce devre dışı bırakılarak ve sonra silerek hizmetten nasıl kaldırılış yapılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: bb1a56d204a46f89213f20e317494120f0ea565e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60580637"
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>StorSimple Sanal Dizisini devre dışı bırakma ve silme

## <a name="overview"></a>Genel Bakış

Bir StorSimple Virtual Array'i devre dışı bıraktığınızda, aygıt la ilgili StorSimple Device Manager hizmeti arasındaki bağlantıyı koparAbilirsiniz. Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Aygıtı devre dışı bırakma 
* Devre dışı bırakılmış bir aygıtı silme

Bu makaledeki bilgiler yalnızca StorSimple Virtual Arrays için geçerlidir. 8000 serisi hakkında bilgi için, bir aygıtı nasıl [devre dışı bırakıp silerken](storsimple-deactivate-and-delete-device.md)eve gidin.

## <a name="when-to-deactivate"></a>Ne zaman devre dışı bırakılalı?

Devre dışı bırakma kalıcı bir işlemdir ve geri alınamaz. Devre dışı bırakılmış bir aygıtı Tekrar StorSimple Device Manager hizmetine kaydedemezsiniz. Aşağıdaki senaryolarda bir StorSimple Sanal Dizisini devre dışı bırakmanız ve silmeniz gerekebilir:

* **Planlı arıza :** Cihazınız çevrimiçi ve cihazınız üzerinde başarısız olmayı planlıyorsunuz. Daha büyük bir cihaza yükseltme yapmayı planlıyorsanız, cihazınız üzerinde başarısız olmanız gerekebilir. Veri sahipliği aktarıldıktan ve hata tamamlandıktan sonra kaynak aygıt otomatik olarak silinir.
* **Planlanmamış arıza :** Cihazınız çevrimdışıve aygıt üzerinde başarısız olmanız gerekir. Bu senaryo, veri merkezinde bir kesinti olduğunda ve birincil aygıtınız çöktüğinde bir felaket sırasında oluşabilir. İkincil bir aygıta aygıtın üzerinde başarısız olmayı planlıyorsunuz. Veri sahipliği aktarıldıktan ve hata tamamlandıktan sonra kaynak aygıt otomatik olarak silinir.
* **Kullanımdan Kaldırma** : Aygıtı devre dışı bırakmak istiyorsunuz. Bu, önce aygıtı devre dışı bırakmanızı ve sonra silmenizi gerektirir. Bir aygıtı devre dışı bıraktığınızda, artık yerel olarak depolanan verilere erişemezsiniz. Yalnızca bulutta depolanan verilere erişebilir ve kurtarabilirsiniz. Devre dışı bırakma dan sonra aygıt verilerini tutmayı planlıyorsanız, aygıtı devre dışı bırakmadan önce tüm verilerinizin bulut anlık görüntüsünü almanız gerekir. Bu bulut anlık görüntüsü, tüm verileri daha sonraki bir aşamada kurtarmanızı sağlar.

## <a name="deactivate-a-device"></a>Aygıtı devre dışı bırakma

Cihazınızı devre dışı bırakmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-deactivate-the-device"></a>Aygıtı devre dışı bırakmak için

1. Hizmetinizde, Yönetim **> Cihazları'na**gidin. **Cihazlar** bıçağında, devre dışı bırakmak istediğiniz cihazı tıklatın ve seçin.
   
    ![Devre dışı bırakmak için cihazı seçin](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. **Cihazınızın pano** bıçağında... ** Daha fazla** ve listeden **Devre dışı bırak'ı**seçin.
   
    ![Devre dışı bırak'ı tıklatın](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. Devre **Dışı Bırak** çubuğunda, aygıt adını yazın ve sonra **Devre dışı bırak'ı**tıklatın. 
   
    ![Devre dışı bırak'ı onaylayın](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    Devre dışı bırakma işlemi başlar ve tamamlanması birkaç dakika sürer.
   
    ![Devam ediyor devre dışı bırakma](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Devre dışı bırakmadan sonra aygıtların listesi yenilenir.
   
    ![Tamamlandı](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Artık bu aygıtı silebilirsiniz.

## <a name="delete-the-device"></a>Cihazı silme

Aygıtısilmek için önce bir aygıtın devre dışı bırakılması gerekir. Aygıtı silerse, aygıtı hizmete bağlı aygıtlar listesinden kaldırır. Hizmet daha sonra silinen aygıtı artık yönetemez. Ancak aygıtla ilişkili veriler bulutta kalır. Bu veriler daha sonra ücret tahakkuk eder.

Aygıtı silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-the-device"></a>Cihazı silmek için

1. StorSimple Aygıt Yöneticinizde, **Yönetim > Cihazları'na**gidin. **Cihazlar** bıçağında, silmek istediğiniz devre dışı bırakılmış bir aygıt seçin.
2. Cihaz **pano** bıçağında, **... Daha fazla** ve sonra **Sil'i**tıklatın.
   
   ![Silmek için cihazı seçin](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. **Sil** bıçağına, silme işlemini onaylamak için cihazınızın adını yazın ve sonra **Sil'i**tıklatın. Aygıtın silinmesi, aygıtla ilişkili bulut verilerini silmez. 
   
   ![Silmeyi onayla](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. Silme işlemi başlar ve tamamlanması birkaç dakika sürer.
   
   ![Silme devam ediyor](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    Aygıt silindikten sonra güncelleştirilmiş aygıt listesini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl başarısız olduğu hakkında bilgi için, [Failover ve StorSimple Sanal Dizi olağanüstü kurtarma](storsimple-virtual-array-failover-dr.md)gidin.

* StorSimple Device Manager hizmetini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [StorSimple Virtual Array'inizi yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-virtual-array-manager-service-administration.md) 

