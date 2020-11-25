---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 3129bbe171329ecf37f8712394cedf9b70188220
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96005834"
---
#### <a name="to-install-updates-via-the-azure-portal"></a>Azure portalı aracılığıyla güncelleştirmeleri yüklemek için

1. StorSimple Cihaz Yöneticinize gidip **Cihazlar**’ı seçin. Hizmetinize bağlı cihazların listesinden güncelleştirmek istediğiniz cihazı seçip tıklayın.

    ![Son olarak, MySSDevManager Aygıt Yöneticisi vurgulanır ve seçilir, cihazlar vurgulanır ve seçilir ve cihaz MYSSIS1103 vurgulanır ve seçilir.](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate1m.png) 

2. **Ayarlar** dikey penceresinde **Cihaz güncelleştirmeleri**'ne tıklayın.

    ![MYSSIS1103 hakkında bilgi gösterilir. Ayarlar ' da, cihaz güncelleştirmeleri vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-04/azupdate2m.png)  

3. Yazılım güncelleştirmeleri varsa bir ileti görürsünüz. Güncelleştirmeleri denetlemek için **Tara**’ya da tıklayabilirsiniz. Çalıştırdığınız yazılım sürümünü bir yere unutmayın. 

    ![Cihaz güncelleştirmeleri bölmesi "yeni güncelleştirmeler var" ve "son taranan/11/01/2017 10:56:00:00/yazılım sürümü/10.0.10289.0" diyor. Sürüm vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate3m1.png)

    Tarama başladığında ve başarıyla tamamlandığında size bildirilir.

    ![Bildirim "cihaz için güncelleştirmeleri taranıyor MySVAFS110... 10:57/işlem devam ediyor. "](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate5m.png)

4. Güncelleştirmeleri tarandıktan sonra **Güncelleştirmeleri indir**’e tıklayın.

    ![Cihaz güncelleştirmeleri bölmesinde "yeni güncelleştirmeler var" iletisi vardır. Güncelleştirmeleri Indir düğmesi vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate6m.png)

5. **Yeni güncelleştirmeler** dikey penceresinde sürüm notlarını gözden geçirin. Güncelleştirmeler indirildikten sonra, yüklemeyi onaylamanız gerektiğini de unutmayın. **Tamam** düğmesine tıklayın.

    ![Yeni güncelleştirmeler bölmesi "Güncelleştirmeler indirildikten sonra, yüklemeyi onaylamanız gerekecektir" ifadesini görürsünüz. Tamam düğmesi vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate7m.png)

6. Karşıya yükleme başlayıp başarıyla tamamlandığında size bildirilir.

     ![Bildirim, "Device hayal için güncelleştirmeler Indiriliyor... 11:07 ".](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate8m.png)

5. **Cihaz güncelleştirmeleri** dikey penceresinde **Yükle**’ye tıklayın.

     ![Cihaz güncelleştirmeleri "güncelleştirmeleri yüklemeyi Onayla" diyor. Install düğmesi vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate11m1.png)

6. **Yeni güncelleştirmeler** dikey penceresinde güncelleştirmenin kesintiye uğratacağı konusunda uyarı alırsınız. Sanal dizi tek düğümlü bir cihaz olduğundan, cihaz güncelleştirildikten sonra yeniden başlatılır. Bu işlem devam eden G/Ç işlemlerini kesintiye uğratır. Güncelleştirmeleri yüklemek için **Tamam**’a tıklayın.

    ![Yeni güncelleştirmeler bölmesindeki ileti "Bu güncelleştirmeler yüklenirken cihazınız kapalı kalma süresine sahip olacaktır". Tamam düğmesi vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate12m.png)

7. Yükleme işi başladığında size bildirilir.

    ![Bildirim "cihaz için güncelleştirmeleri yükleme MySVAFS110... 11:09 ".](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate13m.png)

8.  Yükleme işi başarıyla tamamlandıktan sonra yüklemeyi izlemek için **Cihaz güncelleştirmeleri** dikey penceresindeki **İşi Görüntüle** bağlantısına tıklayın. 

    ![Cihaz güncelleştirmeleri bölmesinde, "güncelleştirme yükleme" bağlantısı vardır. Işi görüntüle ". Install düğmesi vurgulanır.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate15m1.png)

    Bu işlem sizi **güncelleştirmeleri yükler** dikey penceresine götürür. İşle ilgili ayrıntılı bilgileri burada görüntüleyebilirsiniz.

    ![Güncelleştirmeleri yükleme bölmesinde cihaz, durum, süre, başlangıç saati ve bitiş zamanı gibi yükleme bilgileri bulunur.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate16m1.png)

9. Yazılım sürümü güncelleştirme 0,6 (10.0.10293.0) çalıştıran bir sanal dizi ile başladıysanız, artık güncelleştirme 1 ' i çalıştırıyorsunuz ve bu işlemi tamamladınız. Kalan adımları atlayabilirsiniz. Güncelleştirme 0,6 ' den (10.0.10293.0) önce yazılım sürümü çalıştıran bir sanal dizi ile başladıysanız, artık 0,6 güncelleştirme olarak güncellenmiştir. Güncelleştirmelerin kullanılabildiğini belirten başka bir ileti görürsünüz. Güncelleştirme 1 ' i yüklemek için 4-8 arasındaki adımları yineleyin.

    ![Cihaz güncelleştirmeleri bölmesi "yeni güncelleştirmeler var" ve "son taranan/11/1/2017 10:56:00:00/yazılım sürümü/10.0.10293.0" diyor.](../includes/media/storsimple-virtual-array-install-update-via-portal-1/azupdate17.png)

