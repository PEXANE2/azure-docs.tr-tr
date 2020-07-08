---
title: StorSimple Aygıt Yöneticisi hizmetinde cihaz Özeti kullanma
description: StorSimple Aygıt Yöneticisi hizmeti cihaz özetini ve depolama ölçümlerini ve bağlı başlatıcıları görüntülemek ve seri numarasını ve ıQN 'yi bulmak için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: ed624fd5fb86b95e0b79c1c7ed4de50749ec7046
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512041"
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a>StorSimple Aygıt Yöneticisi hizmetinde cihaz özetini kullanma

## <a name="overview"></a>Genel Bakış
StorSimple cihaz Özeti dikey penceresi, belirli bir StorSimple cihazının bilgilerine bir genel bakış sağlar. bu sayede, Microsoft Azure StorSimple çözümünüze dahil olan tüm cihazlar hakkında bilgi sağlanır.

Cihaz Özeti dikey penceresi, belirli bir StorSimple Aygıt Yöneticisi kayıtlı olan bir StorSimple 8000 serisi cihazının Özet görünümünü sağlar ve sistem yöneticisinin dikkatini çekmesi gereken cihaz sorunlarını vurgular. Bu öğretici, cihaz Özeti dikey penceresini tanıtır, içerik ve işlevi açıklar ve bu dikey pencereden gerçekleştirebileceğiniz görevleri açıklar.

Cihaz Özeti dikey penceresinde aşağıdaki bilgiler görüntülenir:

![Cihaz Özeti dikey penceresi](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Yönetim komut çubuğu

StorSimple cihaz dikey penceresinde, StorSimple cihazınızı yönetme seçeneklerini görürsünüz. Yönetim komutlarını dikey pencerenin üst kısmında ve sol tarafta görürsünüz. Paylaşımları veya birimleri eklemek ya da cihazınızı güncelleştirmek veya yükünü devretmek için bu seçenekleri kullanın.

![Yönetim komut çubuğu](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Temel Bileşenler

Essentials alanı, durum, model, hedef ıQN ve yazılım sürümü gibi önemli özelliklerden bazılarını yakalar. 

![Cihaz temelleri](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>İzleme

* **Uyarılar** kutucuğu, cihazınızın uyarı önem derecesine göre gruplanan tüm etkin uyarıların anlık görüntüsünü sağlar.

    ![Uyarı kutucuğu](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Kutucuğa tıklayarak **Uyarılar** dikey penceresini açın ve ardından bu uyarıya ilişkin ek ayrıntıları görüntülemek için önerilen eylemler dahil olmak üzere tek bir uyarıya tıklayın. Sorun çözümlenirse uyarıyı da temizleyebilirsiniz.

    ![Uyarı kutucuğu ' na tıklayın](./media/storsimple-8000-device-dashboard/device-summary10.png)

* **Durum ve sistem** durumu kutucuğu, cihaz durumu da dahil olmak üzere bir cihazın donanım bileşeni sistem durumu hakkında öngörüler sağlar. Cihaz durumu çevrimdışı, çevrimiçi, devre dışı veya kurulum için kullanılabilir durumda olabilir.

    ![Durum ve sistem durumu kutucuğu](./media/storsimple-8000-device-dashboard/device-summary5.png)

* **Birimler** kutucuğu, cihazınızdaki duruma göre gruplanmış birim sayısı özetini sağlar.

    ![Birimler kutucuğu](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Kutucuğa tıklayarak **birimler** listesi dikey penceresini açın ve ardından ayrı bir birime tıklayarak özelliklerini görüntüleyin veya değiştirin.
    
    ![Birimler bölmesi ' ne tıklayın](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Daha fazla bilgi için bkz. [birimleri yönetme](storsimple-8000-manage-volumes-u2.md).

* **Kullanım** grafiğinde, cihazınız genelinde kullanılan birincil depolama alanını ve son 7 gün içinde tüketilen bulut depolama alanını, varsayılan zaman aralığını görüntüleyebilirsiniz.

     ![Kullanım kutucuğu](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Düzenle** seçeneğini kullanın.

     ![Kullanım Grafiğini Düzenle](./media/storsimple-8000-device-dashboard/device-summary12.png)

     Bu grafikte, toplam birincil depolama için ölçümleri (konaklar tarafından yazılan veri miktarı) ve cihazınız tarafından bir süre boyunca tüketilen toplam bulut depolama alanı için görüntüleyebilirsiniz.
  
     Bu bağlamda, *birincil depolama* , ana bilgisayar tarafından yazılan toplam veri miktarını ifade eder ve birim türüne göre ayrılabilir: *birincil katmanlı depolama* , hem yerel olarak depolanmış verileri hem de buluta katmanlanmış verileri içerir. *Birincil yerel olarak sabitlenmiş depolama* yalnızca yerel olarak depolanan verileri içerir. *Bulut depolama*, diğer yandan bulutta depolanan toplam veri miktarının ölçümüdür. Bu depolama alanı, katmanlı verileri ve yedeklemeleri içerir. Bulutta depolanan veriler yinelenenleri kaldırılmış ve sıkıştırılır, ancak birincil depolama, veriler silinmeden ve sıkıştırmadan önce kullanılan depolama alanı miktarını gösterir. (Sıkıştırma hızının bir fikrini almak için bu iki sayıyı karşılaştırabilirsiniz.) Birincil ve bulut depolama için, gösterilen tutarlar yapılandırdığınız izleme sıklığıyla hesaplanır. Örneğin, bir hafta sıklığı seçerseniz, grafik önceki haftadaki her bir güne ait verileri gösterir.

     Zaman içinde tüketilen bulut depolama miktarını görmek için, **kullanılan bulut depolama alanını** seçin. Konak tarafından yazılan toplam depolama alanını görmek için, **kullanılan BIRINCIL KATMANLı depolama** ALANıNı ve **yerel olarak SABITLENMIŞ depolama alanı kullanılan** seçenekleri seçin. 
     Daha fazla bilgi için bkz. StorSimple [cihazınızı izlemek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-monitor-device.md).


* **Kapasite** kutucuğu, cihaz genelinde sağlanan ve kalan toplam depolama alanı ile ilişkili birincil depolamayı görüntüler. **Sağlanan** , kullanıma hazır ve kullanım için ayrılan depolama miktarına başvuruyorsa, **kalan** , bu cihaz genelinde sağlanabilen kalan kapasiteyi ifade eder. 

    ![Kullanım kutucuğu](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Kapasitenin katmanlı ve yerel olarak sabitlenmiş birimlerde nasıl sağlandığını görüntülemek için bu kutucuğa tıklayın. **Kalan katmanlı** kapasite, bulut dahil olmak üzere sağlanan kapasiteye, **kalan yerel** ise bu cihaza bağlı diskler üzerinde kalan kapasiteye sahip olur.

    ![Kullanım grafiği ' ne tıklayın](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple hizmeti Özet dikey](storsimple-8000-service-dashboard.md)penceresi hakkında daha fazla bilgi edinin.
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

