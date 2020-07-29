---
title: StorSimple 8000 serisi cihaz için hizmet Özeti dikey penceresini kullanma
description: StorSimple hizmet Özeti dikey penceresini açıklar ve StorSimple çözümünüzün sistem durumunu izlemek için nasıl kullanılacağını açıklar.
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
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 7b4b697b3d27b57212fc59396e1f8111e297d6cf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514947"
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihaz için hizmet Özeti dikey penceresini kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Aygıt Yöneticisi hizmeti Özeti dikey penceresi, StorSimple Aygıt Yöneticisi hizmetine bağlı tüm cihazların bir Özet görünümünü sağlar ve bu da bir sistem yöneticisinin dikkatini çekmesi gereken cihazları vurgular. Bu öğretici, hizmet Özeti dikey penceresini tanıtır, Pano içeriğini ve işlevini açıklar ve bu sayfadan gerçekleştirebileceğiniz görevleri açıklar.

![Hizmet Özeti](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Yönetim komutları

StorSimple hizmeti Özeti dikey penceresinde, StorSimple Aygıt Yöneticisi hizmetinizi ve bu hizmete kayıtlı StorSimple 8000 serisi cihazlarını yönetme seçeneklerini görürsünüz. Yönetim komutlarını dikey pencerenin üst kısmında ve sol tarafta görürsünüz.

![Komut çubuğu](./media/storsimple-8000-service-dashboard/service-summary2.png)

Paylaşım veya birim ekleme gibi çeşitli işlemleri gerçekleştirmek veya StorSimple cihazlarında çalışan çeşitli işleri izlemek için bu seçenekleri kullanın.


## <a name="essentials"></a>Temel Bileşenler

Essentials alanı, StorSimple Aygıt Yöneticisi oluşturulduğu kaynak grubu, konum ve abonelik gibi bazı önemli özelliklerden bazılarını yakalar.

![Temel Bileşenler](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>StorSimple Aygıt Yöneticisi hizmeti Özeti

* **Uyarılar** kutucuğu, uyarı önem derecesine göre gruplandırılan tüm cihazlarda etkin uyarıların tümünün anlık görüntüsünü sağlar.

    ![Uyarılar kutucuğu](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Kutucuğa tıkladığınızda, önerilen eylemler de dahil olmak üzere uyarı hakkında ek ayrıntıları görüntülemek için tek bir uyarıya tıklabileceğiniz **Uyarılar** dikey penceresi açılır. Sorun çözümlenirse uyarıyı da temizleyebilirsiniz.

    ![Uyarılar kutucuğuna tıklayın](./media/storsimple-8000-service-dashboard/service-summary8.png)

* **Kapasite** kutucuğu, tüm cihazlarda bulunan toplam depolamaya göre tüm cihazlarda sağlanan ve kalan birincil depolamayı gösterir. **Sağlanan** , kullanıma hazır ve kullanım için ayrılan depolama miktarına başvuruyorsa, **kalan** , tüm cihazlarda sağlanabilen kalan kapasiteyi ifade eder.

    ![Kapasite kutucuğu](./media/storsimple-8000-service-dashboard/service-summary6.png)

    **Kalan katmanlı** kapasite, bulut dahil olmak üzere sağlanabilen kullanılabilir kapasitedir, **kalan yerel** , StorSimple 8000 serisi cihazlara eklenen diskler üzerinde kalan kapasiteye sahip olur.


* **Kullanım** grafiğinde, cihazlarınızla ilgili ölçümleri görebilirsiniz. Tüm cihazlarda kullanılan birincil depolama alanını ve son 7 gün boyunca cihazlar tarafından tüketilen bulut depolama alanını, varsayılan zaman aralığını görüntüleyebilirsiniz. 

    ![Kullanım kutucuğu](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Düzenle** seçeneğini kullanın.

     ![Kullanım Kutucuğu ' na tıklayın](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Grafik verilerini dışarı aktar](./media/storsimple-8000-service-dashboard/service-summary11.png)

* **Cihazlar** kutucuğu, storsimple Aygıt Yöneticisi cihaz durumuna göre gruplanan storsimple 8000 serisi cihazların bir özetini sağlar. 

    ![Cihazlar kutucuğu](./media/storsimple-8000-service-dashboard/service-summary5.png)

    Bu kutucuğa tıklayarak **cihazlar** listesi dikey penceresini açın ve cihaza özgü cihaz özetine gitmek için tek bir cihaza tıklayın. Ayrıca, belirli bir cihaz Özeti dikey penceresinden cihaza özgü eylemler gerçekleştirebilirsiniz. Cihaz Özeti dikey penceresi hakkında daha fazla bilgi için [cihaz Özeti dikey penceresine](storsimple-8000-device-dashboard.md)gidin.

    ![Cihazlar kutucuğuna tıklayın](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a>Etkinlik günlüklerini görüntüleme

StorSimple Aygıt Yöneticisi içinde yürütülen çeşitli işlemleri görüntülemek için, StorSimple hizmet Özeti dikey pencerenizin sol tarafındaki **etkinlik günlükleri** bağlantısına tıklayın. Bu, sizi gerçekleştirilen son işlemlerin özetini görebileceğiniz **etkinlik günlükleri** dikey penceresine götürür.

![Etkinlik Günlükleri](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Sonraki adımlar

* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

