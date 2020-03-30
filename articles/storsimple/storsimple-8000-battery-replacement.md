---
title: Microsoft Azure StorSimple 8000 serisi cihazda pili değiştirme
description: StorSimple cihazınızdaki yedek pil modülünüzün nasıl kaldırılıp değiştirilmeye ve korunup korunabildiğini açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: f21bbf4777aa74e84ffb8c1af903f90608d5551f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255021"
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>StorSimple cihazınızın yedek pil modülünü değiştirme

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple cihazınızdaki birincil kasa Güç ve Soğutma Modülü (PCM) ek bir pil paketine sahiptir. Bu paket, birincil kasada AC güç kaybı olduğunda StorSimple aygıtının veri kaydedilebilmeleri için güç sağlar. Bu pil paketi yedek *pil modülü*olarak adlandırılır. Yedek pil modülü yalnızca StorSimple cihazınızdaki birincil kasa için vardır (EBOD muhafazası yedek pil modülü içermez).

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Yedek pil modüllerini kaldırma
* Yeni bir yedek pil modülü yükleme
* Yedek pil modüllerini koruyun

> [!IMPORTANT]
> Yedek pil modüllerini çıkarmadan ve değiştirmeden önce, [StorSimple donanım bileşeni nesle girişteki](storsimple-8000-hardware-component-replacement.md)güvenlik bilgilerini gözden geçirin.


## <a name="remove-the-backup-battery-module"></a>Yedek pil modüllerini kaldırma
StorSimple cihazınızın yedek pil modülü alan değiştirilebilir bir birimdir. PCM'ye takılmadan önce pil modülü orijinal ambalajında saklanmalıdır. Yedek pili kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-the-backup-battery-module"></a>Yedek pil modüllerini kaldırmak için
1. Azure portalında StorSimple Device Manager servis bıçağınıza gidin. **Cihazlara** gidin ve ardından aygıtlar listesinden cihazınızı seçin. **Donanım durumunu** **İzle'ye** > gidin. **Paylaşılan Bileşenler**altında, pilin durumuna bakın.
2. Pilin arızalandığı PCM'yi tanımlayın. Şekil 1 StorSimple cihazının arkasını gösterir.
   
    ![Cihazın Arka Düzlemi Birincil Muhafaza Modülleri](./media/storsimple-battery-replacement/IC740994.png)
   
    **Şekil 1** PCM ve denetleyici modüllerini gösteren birincil aygıtın arkası
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
   
    Şekil 2'de 3 numarada gösterildiği gibi, PCM 0'de **Pil Arızası'na** karşılık gelen izleme göstergesi LED'in yanması gerekir.
   
    ![Cihazın Arka Düzlemi PCM İzleme Gösterge LED'leri](./media/storsimple-battery-replacement/IC740992.png)
   
    **Şekil 2** İzleme göstergesi LED'leri gösteren PCM'nin arkası
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |AC güç kesintisi |
   | 2 |Fan arızası |
   | 3 |Pil arızası |
   | 4 |PCM TAMAM |
   | 5 |DC güç kesintisi |
   | 6 |Pil sağlıklı |
3. PCM'yi başarısız bir pil ile kaldırmak [için, PCM Kaldır'daki](storsimple-8000-power-cooling-module-replacement.md#remove-a-pcm)adımları izleyin.
4. PCM'nin kaldırılmasıyla, pil modül tutamacını aşağıdaki şekilde belirtildiği gibi yukarı doğru kaldırın ve döndürün ve pili çıkarmak için yukarı çekin.
   
    ![PCM'den Pil Çıkarma](./media/storsimple-battery-replacement/IC741019.png)
   
    **Şekil 3** Pili PCM'den çıkarma
5. Modülü alan değiştirilebilir ünite ambalajına yerleştirin.
6. Arızalı birimi uygun servis ve kullanım için Microsoft'a iade edin.

## <a name="install-a-new-backup-battery-module"></a>Yeni bir yedek pil modülü yükleme
Değiştirilen pil modülünün PCM'deki yerine gelen pil modülünün StorSimple cihazınızın birincil kasasına yüklenmesi için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-battery-module"></a>Pil modüllerini yüklemek için
1. Yedek pil modüllerini PCM'de uygun yönlendirmeye yerleştirin.
2. Konektörü oturtmak için pil modülü koluna kadar bastırın.
3. [StorSimple cihazınızdaki Güç ve Soğutma Modüllerini Değiştir'deki](storsimple-8000-power-cooling-module-replacement.md)yönergeleri izleyerek birincil kasadaki PCM'yi değiştirin.
4. Değiştirme tamamlandıktan sonra cihazınıza gidin ve ardından Azure portalındaki**Donanım durumunu** **izleyin.** >  Yüklemenin başarılı olduğundan emin olmak için pilin durumunu doğrulayın. Yeşil durum pilin sağlıklı olduğunu gösterir.

## <a name="maintain-the-backup-battery-module"></a>Yedek pil modüllerini koruyun
StorSimple cihazınızda, yedek pil modülü bir güç kaybı olayı sırasında kumandaya güç sağlar. StorSimple cihazının kontrollü bir şekilde kapanmadan önce kritik verileri kaydetmesine olanak tanır. PCM'lerde tam şarjlı iki pil ile sistem art arda iki kayıp olayını işleyebilir.

Azure portalında, **Monitör** bıçağının altındaki **Donanım durumu** pilin arızalı mı yoksa ömrünün sonuna doğru mı yaklaştığını gösterir. Pil durumu, **Paylaşılan Bileşenler**altında **PCM 0'daki Pil** veya **PCM 1'deki Pil** ile gösterilir. Bu bıçak, yaşam sonu yaklaşması için **bozulmuş** bir durum gösterecektir ve yaşam sonu için **başarısız** olacaktır.

> [!NOTE]
> Pil, yalnızca şarj edilmesi gerektiğinde **FAILED'i** bildirebilir.


**BOZULMUŞ** durum görünüyorsa, aşağıdaki eylem yolunu öneririz:

* Sistem yakın zamanda güç kaybı yaşamış olabilir veya piller periyodik bakımdan geçiyor olabilir. Devam etmeden önce 12 saat boyunca sistemi gözlemleyin.
  
  * Denetleyicileri ve **DEGRADED** PCM'ler çalışırken AC gücüne 12 saatlik sürekli bağlantıdan sonra durum hala bozulmuşsa, pilin değiştirilmesi gerekir. Yedek yedek pil modülü için lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
  * Durum 12 saat sonra iyileşirse, pil çalışır durumda ve sadece bir bakım şarjı gerekir.
* Ac güç ilişkili bir kayıp olmamıştır ve PCM açık ve AC gücüne bağlı ise, pil değiştirilmesi gerekir. Yedek bir yedek pil modülü sipariş etmek için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

> [!IMPORTANT]
> Ulusal ve bölgesel düzenlemelere göre başarısız pil ilerler.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşen değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

