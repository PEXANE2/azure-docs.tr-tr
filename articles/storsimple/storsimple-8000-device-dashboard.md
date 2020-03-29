---
title: StorSimple 8000 serisi cihaz özetini kullanın | Microsoft Dokümanlar
description: StorSimple Device Manager servis aygıtı özetini ve depolama ölçümlerini ve bağlı başlatıcıları görüntülemek ve seri numarasını ve IQN'yi bulmak için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 1d88af2c0739c30b2562bad7660015b890e8159c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60578326"
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a>StorSimple Device Manager hizmetinde cihaz özetini kullanma

## <a name="overview"></a>Genel Bakış
StorSimple aygıt özet bıçağı, Microsoft Azure StorSimple çözümünüzde yer alan tüm aygıtlar hakkında bilgi veren hizmet özeti bıçağının aksine, belirli bir StorSimple aygıtına ilişkin bilgilere genel bir bakış sağlar.

Aygıt özeti bıçak, belirli bir StorSimple Aygıt Yöneticisi'ne kayıtlı bir StorSimple 8000 serisi aygıtın özet görünümünü sağlayarak sistem yöneticisinin dikkatine ihtiyaç duyan aygıt sorunlarını vurgular. Bu öğretici, aygıt özeti bıçağını tanıtır, içeriği ve işlevi açıklar ve bu bıçaktan gerçekleştirebileceğiniz görevleri açıklar.

Cihaz özeti bıçak aşağıdaki bilgileri görüntüler:

![Cihaz özet bıçağı](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Yönetim komut çubuğu

StorSimple aygıt bıçaklarında, StorSimple cihazınızı yönetme seçeneklerini görürsünüz. Yönetimin komutlarını bıçağın üstündeve sol tarafında görüyorsunuz. Bu seçenekleri, paylaşım lar veya birimler eklemek veya aygıtınızı güncellemek veya başarısız olmak için kullanın.

![Yönetim komut çubuğu](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Temel Bileşenler

Temel alan, durum, model, hedef IQN ve yazılım sürümü gibi bazı önemli özellikleri yakalar. 

![Cihaz temelleri](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>İzleme

* **Uyarılar** döşemesi, cihazınız için uyarı şiddetine göre gruplanmış tüm etkin uyarıların anlık görüntüsünü sağlar.

    ![Uyarı döşemesi](./media/storsimple-8000-device-dashboard/device-summary4.png)

    **Uyarılar** bıçağını açmak için döşemeyi tıklatın ve ardından önerilen eylemler de dahil olmak üzere bu uyarıyla ilgili ek ayrıntıları görüntülemek için tek bir uyarıyı tıklatın. Sorun çözülmüşse uyarıyı da temizleyebilirsiniz.

    ![Uyarı döşemesi'ni tıklatın](./media/storsimple-8000-device-dashboard/device-summary10.png)

* **Durum ve sistem durumu** döşemesi, aygıt durumunu da içeren bir aygıt için donanım bileşeni durumu hakkında öngörüler sağlar. Aygıt durumu çevrimdışı, çevrimiçi, devre dışı bırakılmış veya ayarlanacak hazır olabilir.

    ![Durum ve sağlık karosu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* **Birimler** döşemesi, cihazınızda duruma göre gruplanan birim sayısının bir özetini sağlar.

    ![Hacimler döşeme](./media/storsimple-8000-device-dashboard/device-summary6.png)

    **Birimler** listesi bıçağını açmak için döşemeyi tıklatın ve sonra özelliklerini görüntülemek veya değiştirmek için tek bir birimin üzerine tıklayın.
    
    ![Birim döşemeyi tıklatın](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Daha fazla bilgi için birimleri nasıl [yönetirene](storsimple-8000-manage-volumes-u2.md)bakın.

* **Kullanım** grafiğinde, cihazınızda kullanılan birincil depolama alanını ve varsayılan süre olan son 7 gün içinde tüketilen bulut depolama alanını görüntüleyebilirsiniz.

     ![Kullanım döşemesi](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Edit** seçeneğini kullanın.

     ![Kullanım grafiğini edin](./media/storsimple-8000-device-dashboard/device-summary12.png)

     Bu grafikte, toplam birincil depolama alanı (ana bilgisayarlar tarafından cihazınıza yazılan veri miktarı) ve cihazınız tarafından belirli bir süre boyunca tüketilen toplam bulut depolama alanı nın ölçümlerini görüntüleyebilirsiniz.
  
     Bu bağlamda, *birincil depolama* ana bilgisayar tarafından yazılmış toplam veri miktarını ifade eder ve birim türüne göre bölünebilir: birincil katmanlı *depolama* hem yerel olarak depolanan verileri hem de buluta katmanlı verileri içerir. *Birincil yerel olarak sabitlenmiş depolama* yalnızca yerel olarak depolanan verileri içerir. *Bulut depolama,* diğer taraftan, bulutta depolanan toplam veri miktarının bir ölçüsüdür. Bu depolama katmanlı verileri ve yedeklemeleri içerir. Bulutta depolanan veriler çoğaltılır ve sıkıştırılır, birincil depolama ise veriler çoğaltılmadan ve sıkıştırılmadan önce kullanılan depolama miktarını gösterir. (Sıkıştırma oranı hakkında bir fikir elde etmek için bu iki sayıyı karşılaştırabilirsiniz.) Hem birincil hem de bulut depolama için gösterilen tutarlar, yapılandırdığınız izleme sıklığını temel alar. Örneğin, bir haftalık bir sıklık seçerseniz, grafik önceki haftadaki her güne ait verileri gösterir.

     Zaman içinde tüketilen bulut depolama miktarını görmek için **BULUT DEPOLAMA KULLANILI seçeneğini** belirleyin. Ana bilgisayar tarafından yazılmış toplam depolama alanını görmek için, **KULLANıLAN BIRINCIL KATMANLı DEPOLAMA** ve BIRINCIL YEREL OLARAK **SABITLENMIŞ DEPOLAMA KULLANIseçeneklerini** seçin. 
     Daha fazla bilgi için [StorSimple cihazınızı izlemek için StorSimple Device Manager hizmetini kullanın.](storsimple-monitor-device.md)


* **Kapasite** döşemesi, aynı için kullanılabilir toplam depolama alanına göre aygıt genelinde sağlanan ve aygıt genelinde kalan birincil depolamayı görüntüler. **Sağlanan,** kullanılmak üzere hazırlanan ve ayrılan depolama miktarını ifade eder, **Kalan** bu aygıt ta sağlanabilir kalan kapasiteyi ifade eder. 

    ![Kullanım döşemesi](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Kapasitenin katmanlı ve yerel olarak sabitlenmiş birimler arasında nasıl sağlanmış olduğunu görmek için bu döşemeyi tıklatın. **Kalan Katmanlı** kapasite bulut da dahil olmak üzere sağlanabilir kullanılabilir kapasitedir, **Kalan Yerel** ise bu aygıta bağlı disklerde kalan kapasitedir.

    ![Kullanım grafiğini tıklatın](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple hizmet özeti bıçak](storsimple-8000-service-dashboard.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

