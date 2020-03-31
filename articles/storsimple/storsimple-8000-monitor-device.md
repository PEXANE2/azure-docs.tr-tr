---
title: StorSimple 8000 serisi cihazınızı izleyin
description: Kullanımı, G/Ç performansını ve kapasite kullanımını izlemek için StorSimple Device Manager hizmetinin nasıl kullanılacağını açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: alkohli
ms.openlocfilehash: b3b77024606c5cdb02ff7bdd357c2d14a2415efa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277050"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-your-storsimple-device"></a>StorSimple cihazınızı izlemek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış
StorSimple çözümünüzdeki belirli cihazları izlemek için StorSimple Device Manager hizmetini kullanabilirsiniz. G/Ç performansı, kapasite kullanımı, ağ oluşturma ve aygıt performansı ölçümlerine dayalı özel grafikler oluşturabilir ve bunları panoya sabitleyebilirsiniz. Daha fazla bilgi için [portal panonuzu özelleştirmeye](../azure-portal/azure-portal-dashboards.md)gidin.

Azure portalında belirli bir aygıtın izleme bilgilerini görüntülemek için StorSimple Device Manager hizmetini seçin. Aygıtlar listesinden cihazınızı seçin ve ardından **Monitor'a**gidin. Daha sonra seçili aygıtın **Kapasite,** **Kullanım**ve **Performans** grafiklerini görebilirsiniz.

## <a name="capacity"></a>Kapasite
**Kapasite,** aygıtta kalan alanı ve verilen alanı izler. Kalan kapasite daha sonra yerel olarak sabitlenmiş veya katmanlı olarak görüntülenir.

Sağlanan ve kalan kapasite, katmanlı ve yerel olarak sabitlenmiş hacimler tarafından daha da bölünur. Her birim için, verilen kapasite ve aygıtta kalan kapasite görüntülenir.

![IO kapasitesi](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Kullanım
**Kullanım,** birimler, birim kapsayıcılar veya aygıt tarafından kullanılan veri depolama alanı miktarıyla ilgili ölçümleri izler. Birincil depolamanızın, bulut depolamanızın veya aygıt depolamanızın kapasite kullanımına dayalı raporlar oluşturabilirsiniz. Kapasite kullanımı belirli bir hacim, belirli bir hacim kapsayıcısı veya tüm hacim kapları üzerinde ölçülebilir.
Varsayılan olarak, son 24 saat için kullanım bildirilir. Aşağıdakilerden birini seçerek, kullanımın raporlandığı süreyi değiştirmek için grafiği düzenleme yapabilirsiniz:
* Son 24 saat
* Son 7 gün
* Son 30 gün
* Son 90 gün
* Geçen yıl

Kullanım grafikleri için iki temel ölçüm, büyüme ve aralık bildirilir. Aralık, seçilen süre boyunca bildirilen kullanımın en büyük değerini ve minimum değerlerini ifade eder (fo instance, Son 7 gün).

Büyüme, seçilen süre boyunca ilk günden son güne kadar kullanımdaki artışı ifade eder. 

Büyüme ve aralık da aşağıdaki denklemlerle temsil edilebilir:

```
Range = {Usage(minimum), Usage(maximum)}

Growth = Usage(Last day) - Usage(first day)

Growth (%) = [{Usage(last day) - Usage(first day)} X 100]/Usage(first day)
```

Kullanılan birincil, bulut ve yerel depolama aşağıdaki gibi tanımlanabilir:

### <a name="primary-storage-usage"></a>Birincil depolama alanı kullanımı
Bu grafikler, veriler çoğaltılmadan ve sıkıştırılmadan önce StorSimple birimlerine yazılan veri miktarını gösterir. Tüm birimler tarafından bir birim kapsayıcıda veya tek bir birim için kullanılan birincil depolama alanını görüntüleyebilirsiniz. Kullanılan birincil depolama, kullanılan birincil katmanlı depolama ve kullanılan birincil yerel sabitlenmiş depolama tarafından daha da bölünur.

Aşağıdaki grafikler, bir bulut anlık görüntüsü alınmadan önce ve sonra StorSimple aygıtı için kullanılan birincil depolama alanını gösterir. Bu yalnızca birim veri olduğundan, bulut anlık görüntüsü birincil depolama alanını değiştirmemelidir. Gördüğünüz gibi, grafik, bulut anlık görüntüsü alınması sonucunda kullanılan birincil katmanlı veya yerel olarak sabitlenmiş depolama alanında hiçbir fark göstermez. Bulut görüntüsü saat 23:50 sularında o cihazda başladı.

![Bulut anlık görüntüsünden sonra birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

Artık StorSimple cihazınıza bağlı ana bilgisayarda IO çalıştırırsanız, verileri yazdığınız birimlere (katmanlı veya yerel olarak sabitlenmiş) bağlı olarak kullanılan birincil katmanlı depolama da veya birincil yerel olarak sabitlenmiş depolamada bir artış görürsünüz. StorSimple aygıtının birincil depolama kullanım grafikleri aşağıda veda edilebistir. Bu cihazda, StorSimple ana bilgisayar saat 14:30 civarında cihazda katmanlı bir ses üzerinde yazma hizmet vermeye başladı. Ana bilgisayarda çalışan IO'ya karşılık gelen yazma baytlarında/s'lerde tepe noktasını görebilirsiniz.

![Katmanlı birimlerde IO çalışırken performans](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Kullanılan birincil katmanlı depolama ya da depolama ya bakarsanız, bu kadar arttı, oysa birincil yerel olarak sabitlenmiş kullanım, aygıttaki yerel olarak sabitlenmiş birimlere yazı verilmediği için değişmeden kalır.

![IO katmanlı birimlerde çalışırken birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Güncelleştirme 3 veya daha yüksek bir düzeyçalıştırıyorsanız, birincil depolama kapasitesi kullanımını tek bir birim, tüm birimler, tüm katmanlı birimler ve aşağıda gösterildiği gibi yerel olarak sabitlenmiş tüm hacimleri ayırabilirsiniz. Yerel olarak sabitlenmiş tüm birimler tarafından parçalanması, yerel katmanın ne kadarının kullanıldığını hızlı bir şekilde belirlemenize olanak sağlar.

![Tüm katmanlı birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/monitor-usage4.png)

Listedeki birimlerin her birine daha fazla tıklayabilir ve ilgili kullanımı görebilirsiniz.

![Tüm yerel olarak sabitlenmiş birimler için birincil kapasite kullanımı](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Bulut depolama alanı kullanımı
Bu grafikler, kullanılan bulut depolama miktarını gösterir. Bu veriler çoğaltılır ve sıkıştırılır. Bu tutar, herhangi bir birincil birimde yansıtılmayan ve eski veya gerekli bekletme amacıyla tutulan verileri içerebilecek bulut anlık görüntülerini içerir. Veri azaltma oranı hakkında bir fikir almak için birincil ve bulut depolama tüketim rakamlarını karşılaştırabilirsiniz, ancak sayı kesin olmayacaktır.

Aşağıdaki grafikler, bir bulut görüntüsü alındığında StorSimple aygıtının bulut depolama kullanımını gösterir.

* Bulut anlık görüntüsü o cihazda saat 11:50 civarında başladı ve bulut anlık görüntüsünden önce bulut depolama alanı nın kullanılmadığını görebilirsiniz. 
* Bulut anlık görüntüsü tamamlandıktan sonra, bulut depolama kullanımı 0,89 GB'a kadar yükseldi. 
* Bulut anlık görüntüsü devam ederken, IO'da aygıttan buluta karşılık gelen bir tepe noktası da vardır.

    ![Bulut anlık görüntüsünden önce bulut depolama kullanımı](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Bulut anlık görüntüsünden sonra bulut depolama kullanımı](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![Bulut anlık görüntüsü sırasında cihazdan buluta IO](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Yerel depolama alanı kullanımı
Bu grafikler, SSD doğrusal katmanı içerdiğinden birincil depolama kullanımından daha fazla olacak olan aygıtın toplam kullanımını gösterir. Bu katman, aygıtın diğer katmanlarında da bulunan bir veri miktarı içerir. SSD doğrusal katmanındaki kapasite, yeni veriler geldiğinde eski verilerin HDD katmanına (çoğaltılıp sıkıştırıldığı sırada) ve daha sonra buluta taşınacak şekilde döngüye girer.

Zaman içinde, kullanılan birincil depolama ve kullanılan yerel depolama büyük olasılıkla veriler buluta katmanlandırılmaya başlayana kadar birlikte artacaktır. Bu noktada, kullanılan yerel depolama büyük olasılıkla plato başlar, ancak daha fazla veri yazıldığında kullanılan birincil depolama artacaktır.

Aşağıdaki grafikler, bir bulut anlık görüntüsü alındığında StorSimple aygıtı için kullanılan birincil depolama alanını gösterir. Bulut anlık görüntüsü saat 11:50'de başladı ve yerel depolama alanı o anda azalmaya başladı. Kullanılan yerel depolama 1.445 GB'dan 1.09 GB'a düştü. Bu, doğrusal SSD katmanındaki sıkıştırılmamış verilerin büyük olasılıkla çoğaltıldığını, sıkıştırıldığını ve HDD katmanına taşındığını gösterir. Aygıtın hem SSD hem de HDD katmanlarında zaten büyük miktarda veri varsa, bu düşüşü göremeyebilirsiniz. Bu örnekte, aygıtın az miktarda verisi vardır.

![Bulut anlık görüntüsünden sonra yerel depolama kullanımı](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Performans
**Performans,** ana bilgisayar sunucusundaki iSCSI başlatıcı arabirimleri ile aygıt veya aygıt ve bulut arasındaki okuma ve yazma işlemleri sayısıyla ilgili ölçümleri izler. Bu performans, belirli bir hacim, belirli bir birim kapsayıcısı veya tüm birim kapsayıcıları için ölçülebilir. Performans ayrıca cihazınızdaki çeşitli ağ arabirimleri için CPU kullanımı ve Ağ iş çıktısı içerir.

### <a name="io-performance-for-initiator-to-device"></a>Başlatıcıdan cihaza başlatma performansı
Aşağıdaki grafikte, bir üretim aygıtının tüm birimleri için cihazınıza başlatıcının G/Ç'si gösterilmektedir. Çizilen ölçümler saniyede bayt okunur ve yazılır. Ayrıca okuma, yazma ve olağanüstü IO grafik veya okuma ve yazma gecikmeleri.

![Başlatıcıdan cihaza iO performansı](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-to-cloud"></a>Aygıtın buluta günü için G/Ç performans
Aynı aygıt için, Tüm birim kapsayıcıları için aygıttan buluta gelen veriler için G/Ç işlemleri çizilir. Bu cihazda, veriler yalnızca doğrusal katmandadır ve buluta hiçbir şey dökülmemiştir. Aygıttan buluta okuma yazma işlemi yok. Bu nedenle, grafikteki zirveler, aygıt ve hizmet arasında sinyalin kontrol edildiği frekansa karşılık gelen 5 dakikalık bir aralıktadır.

Aynı aygıt için saat 11:50'den itibaren birim verileri için bir bulut görüntüsü alındı. Bu, aygıttan buluta akan verilerle sonuçlandı. Bu süre içinde buluta yazılar sunuldu. IO grafiği, Anlık görüntünün çekildiği zamana karşılık gelen Byte/s Yaz'da bir tepe noktası gösterir.

![Bulut anlık görüntüsü sırasında cihazdan buluta IO](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Aygıt ağı arabirimleri için ağ çıktısı
**Ağ iş** letimi, ana bilgisayar sunucusunda ve aygıttaki iSCSI başlatıcı ağ arabirimlerinden ve aygıt la bulut arasındaki veri miktarıyla ilgili ölçümleri izler. Cihazınızdaki iSCSI ağ arabirimlerinin her biri için bu ölçümü izleyebilirsiniz.

Aşağıdaki grafikler, cihazınızdaki Veri 0, 1 1 GbE ağı için hem bulut etkin (varsayılan) hem de iSCSI özellikli ağ verisi sağlar. Bu cihazda 14 Haziran saat 21:00 sularında veriler buluta katmanlandı (o anda hiçbir bulut anlık görüntüsü alınmadı ve bu da katmanların verileri buluta taşıma mekanizması olduğuna işaret ediyor) ve bu da IO'nun buluta sunulmasıile sonuçlandı. Ağ iş verme grafiğinde aynı anda karşılık gelen bir tepe noktası vardır ve ağ trafiğinin çoğu buluta giden.

![Veri için ağ iş girişi 0](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Sadece iSCSI özellikli başka bir 1 GbE ağ arabirimi olan Data 1 arabirimi iş akış şemasına bakacak olursak, bu süre içinde neredeyse hiç ağ trafiği yoktu.

![Veri 1 için ağ iş birlayı](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>Cihaz için CPU kullanımı
**CPU kullanımı,** cihazınızda kullanılan CPU ile ilgili ölçümleri izler. Aşağıdaki grafik, üretimdeki bir aygıtın CPU kullanım istatistiklerini gösterir.

![Cihaz için CPU kullanımı](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Device Manager servis aygıtı panosunu](storsimple-device-dashboard.md)nasıl kullanacağınızı öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

