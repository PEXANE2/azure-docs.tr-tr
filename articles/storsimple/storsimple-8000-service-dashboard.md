---
title: StorSimple 8000 serisi cihaz özetini kullanın | Microsoft Dokümanlar
description: StorSimple hizmet özet bıçağını açıklar ve StorSimple çözümünüzün durumunu izlemek için nasıl kullanılacağını açıklar.
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
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: c174f6ce0fb3d40af953be205a7bfcca60fbfeec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267657"
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihaz için servis özet bıçağını kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Device Manager hizmet özeti bıçak, bir sistem yöneticisinin dikkatine ihtiyaç duyan aygıtları vurgulayarak StorSimple Device Manager hizmetine bağlı tüm aygıtların özet görünümünü sağlar. Bu öğretici, hizmet özeti bıçak tanıttı, pano içeriği ve işlevi açıklar ve bu sayfadan gerçekleştirebileceğiniz görevleri açıklar.

![Hizmet özeti](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Yönetim komutları

StorSimple servis özeti bıçaklarında, StorSimple Device Manager hizmetinizi ve bu hizmete kayıtlı StorSimple 8000 serisi cihazları yönetme seçeneklerini görürsünüz. Yönetimin komutlarını bıçağın üstündeve sol tarafında görüyorsunuz.

![Komut çubuğu](./media/storsimple-8000-service-dashboard/service-summary2.png)

Hisse veya birim eklemek veya StorSimple aygıtlarında çalışan çeşitli işleri izlemek gibi çeşitli işlemleri gerçekleştirmek için bu seçenekleri kullanın.


## <a name="essentials"></a>Temel Bileşenler

Temel alan, StorSimple Aygıt Yöneticinizin oluşturulduğu kaynak grubu, konum ve abonelik gibi bazı önemli özellikleri yakalar.

![Temel Bileşenler](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>StorSimple Device Manager hizmet özeti

* **Uyarılar** döşemesi, uyarı şiddetine göre gruplanmış tüm aygıtlar daki tüm etkin uyarıların anlık görüntüsünü sağlar.

    ![Uyarıları döşeme](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Döşemeyi tıklattığınızda, önerilen eylemler de dahil olmak üzere bu uyarıyla ilgili ek ayrıntıları görüntülemek için tek bir uyarıyı tıklatabileceğiniz **Uyarılar** bıçağı açılır. Sorun çözülmüşse uyarıyı da temizleyebilirsiniz.

    ![Uyarıları niçin tıklatın](./media/storsimple-8000-service-dashboard/service-summary8.png)

* **Kapasite** döşemesi ekranları, tüm aygıtlar arasında kullanılabilen toplam depolama alanına göre tüm aygıtlar arasında sağlanan ve tüm aygıtlar arasında kalan birincil depolamayı gösterir. **Sağlanan,** kullanılmak üzere hazırlanan ve ayrılan depolama miktarını ifade eder, **Kalan** tüm aygıtlar arasında sağlanabilir kalan kapasiteyi ifade eder.

    ![Kapasite döşemesi](./media/storsimple-8000-service-dashboard/service-summary6.png)

    **Kalan Katmanlı** kapasite bulut da dahil olmak üzere sağlanabilir kullanılabilir kapasitedir, **Kalan Yerel** ise StorSimple 8000 serisi aygıtlara bağlı disklerde kalan kapasitedir.


* **Kullanım** grafiğinde, aygıtlarınız için ilgili ölçümleri görebilirsiniz. Tüm aygıtlarda kullanılan birincil depolama alanını ve son 7 gün içinde aygıtlar tarafından tüketilen bulut depolama alanını, varsayılan süreyi görüntüleyebilirsiniz. 

    ![Kullanım döşemesi](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Edit** seçeneğini kullanın.

     ![Kullanım döşemesi'ni tıklatın](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Dışa aktarma grafiği verileri](./media/storsimple-8000-service-dashboard/service-summary11.png)

* **Aygıt** döşemesi, cihaz durumuna göre gruplanan StorSimple Aygıt Yöneticisi'nizdeki StorSimple 8000 serisi aygıt sayısının bir özetini sağlar. 

    ![Aygıtlar döşemesi](./media/storsimple-8000-service-dashboard/service-summary5.png)

    **Cihazlar** listesi bıçağını açmak için bu döşemeyi tıklatın ve ardından cihaza özgü aygıt özetini delmek için tek bir aygıtı tıklatın. Belirli bir aygıt özet bıçağından aygıta özel eylemler de gerçekleştirebilirsiniz. Cihaz özet bıçağı hakkında daha fazla bilgi için [Aygıt özet bıçağına](storsimple-8000-device-dashboard.md)gidin.

    ![Aygıtlar döşemesi'ni tıklatın](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a>Etkinlik günlüklerini görüntüleme

StorSimple Device Manager'ınızda gerçekleştirilen çeşitli işlemleri görüntülemek için StorSimple servis özet bıçağınızın sol tarafındaki **Etkinlik günlükleri** bağlantısını tıklatın. Bu, gerçekleştirilen son işlemlerin bir özetini görebileceğiniz **Etkinlik günlükleri** bıçağına götürür.

![Etkinlik Günlükleri](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınız hakkında daha fazla bilgi edinin.

