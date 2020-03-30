---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a3ccf76b2722c04a9353fcc7020ff1387bc454c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188574"
---
#### <a name="to-install-an-update-from-the-azure-portal"></a>Azure portalından bir güncelleştirmeyi yüklemek için

1. StorSimple hizmet sayfasında cihazınızı seçin.

    ![Cihazı seçin](./media/storsimple-8000-install-update4-via-portal/update1.png)

2. Aygıt **ayarlarına** > gidin**Aygıt güncelleştirmeleri**.

    ![Cihaz güncelleştirmelerini tıklatın](./media/storsimple-8000-install-update4-via-portal/update2.png)

2. Yeni güncelleştirmeler varsa bir bildirim görüntülenir. Alternatif olarak, **Aygıt güncelleştirmeleri** **çubuğunda, Güncellemeleri Tazyir'i**tıklatın. Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. İş başarıyla tamamlandığında size bildirilir.

    ![Cihaz güncelleştirmelerini tıklatın](./media/storsimple-8000-install-update4-via-portal/update3.png)

3. Bir güncelleştirmeyi cihazınıza uygulamadan önce sürüm notlarını gözden geçirmeniz önerilir. Güncelleştirmeleri uygulamak için **güncelleştirmeleri yükle'yi**tıklatın. Düzenli **güncelleştirmeleri Onayla** bıçak bıçak, güncellemeleri uygulamadan önce tamamlamak için ön koşulları gözden geçirin. Aygıtı güncelleştirmeye hazır olduğunuzu belirtmek için onay kutusunu seçin ve ardından **Yükle'yi**tıklatın.

    ![Cihaz güncelleştirmelerini tıklatın](./media/storsimple-8000-install-update4-via-portal/update4.png)

6. Bir dizi önkoşul denetimi başlatılır. Bu denetimler şunlardır:
   
   * Her iki cihaz denetleyicisinin de sağlıklı ve çevrimiçi olduğunu doğrulamaya yönelik **denetleyici durumu denetimleri**.
   * StorSimple cihazınızdaki tüm donanım bileşenlerinin sağlıklı olduğunu doğrulamaya yönelik **donanım bileşeni durum denetimleri**.
   * DATA 0’ın cihazınızda etkin olduğunu doğrulamaya yönelik **DATA 0 denetimleri**. Bu arabirim etkin değilse etkinleştirmeniz ve sonra yeniden denemeniz gerekir.

     Güncelleştirme indirilir ve yalnızca tüm denetimler başarıyla tamamlanırsa yüklenir. Denetimler devam ederken size bildirilir. Ön denetimler başarısız olursa, o zaman başarısızlık nedenleri ile sağlanacaktır. Bu sorunları ele alın ve işlemi yeniden deneyin. Bu sorunları kendi başınıza çözemiyorsanız Microsoft Desteği’ne başvurmanız gerekebilir.

7. Ön denetimler başarıyla tamamlandıktan sonra bir güncelleştirme işi oluşturulur. Güncelleştirme işi başarıyla oluşturulduğunda size bildirilir.
   
    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update4-via-portal/update6.png)
   
    Bundan sonra güncelleştirme, cihazınıza uygulanır.

9. Güncelleştirmenin tamamlanması birkaç saat sürer. Güncelleştirme işini seçin ve **Ayrıntılar**’a tıklayarak dilediğiniz zaman işin ayrıntılarını görüntüleyin.

    ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update4-via-portal/update8.png)

     Ayrıca, Aygıt **ayarlarından > İşler'den**de güncelleme işinin ilerlemesini izleyebilirsiniz. **İşler** bıçağında, güncelleştirmenin ilerlemesini görebilirsiniz.

     ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update4-via-portal/update7.png)

10. İş tamamlandıktan sonra Aygıt **ayarları> aygıt güncelleştirmelerine**gidin. Yazılım sürümü artık güncelleştirilmelidir.

   ![Güncelleştirme işi oluşturma](./media/storsimple-8000-install-update4-via-portal/update9.png)

