---
title: StorSimple 8000 serisi cihazınızda pcm değiştirin | Microsoft Dokümanlar
description: StorSimple cihazınızdaki Güç ve Soğutma Modülü 'nün (PCM) nasıl kaldırılıp değiştirilebildiğini açıklar
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 42561570e24aec5edd33248ef1738e53175e480e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60632506"
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>StorSimple cihazınızın güç ve soğutma modülünü değiştirme
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple cihazınızdaki Güç ve Soğutma Modülü (PCM), birincil ve EBOD muhafazaları aracılığıyla kontrol edilen bir güç kaynağı ve soğutma fanlarından oluşur. Her muhafaza için onaylı pcm yalnızca bir model vardır. Birincil kasa 764 W PCM için onaylanır ve EBOD muhafazası 580 W PCM için onaylanır. Birincil kasa ve EBOD muhafazasının PC'leri farklı olsa da, değiştirme yordamı aynıdır.

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* PCM'yi kaldırma
* Yedek PCM yükleme

> [!IMPORTANT]
> Bir PCM'yi çıkarmadan ve değiştirmeden [önce, StorSimple donanım bileşeni değiştirmedeki](storsimple-8000-hardware-component-replacement.md)güvenlik bilgilerini gözden geçirin.


## <a name="before-you-replace-a-pcm"></a>Bir PCM'yi değiştirmeden önce
PCM'nizi değiştirmeden önce aşağıdaki önemli konulara dikkat edin:

* PCM'nin güç kaynağı arızalanırsa, hatalı modülü takılı bırakın, ancak güç kablosunu çıkarın. Fan kasadan güç almaya ve uygun soğutma sağlamaya devam edecektir. Fan başarısız olursa, PCM'nin derhal değiştirilmesi gerekir.
* PCM'yi çıkarmadan önce, ana anahtarı (varsa) kapatarak veya güç kablosunu fiziksel olarak çıkararak PCM'den gücü kesin. Bu, sisteminize bir güç kapatmanın yakın olduğu konusunda bir uyarı sağlar.
* Hatalı PCM'yi değiştirmeden önce diğer PCM'nin sürekli sistem çalışması için işlevsel olduğundan emin olun. Hatalı bir PCM mümkün olan en kısa sürede tam operasyonel bir PCM ile değiştirilmelidir.
* PCM modül değişiminin tamamlanması sadece birkaç dakika sürer, ancak aşırı ısınmayı önlemek için başarısız PCM'nin çıkarılmasından sonraki 10 dakika içinde tamamlanması gerekir.
* Fabrikadan gönderilen yedek 764 W PCM modüllerinin yedek pil modüllerini içermediğini unutmayın. Pili hatalı PCM'nizden çıkarmanız ve değiştirme gerçekleştirmeden önce değiştirme modülüne takmanız gerekir. Daha fazla bilgi için yedek pil modüllerini nasıl [kaldırıp takabilirsiniz.](storsimple-8000-battery-replacement.md)

## <a name="remove-a-pcm"></a>PCM'yi kaldırma
Microsoft Azure StorSimple aygıtınızdan bir Güç ve Soğutma Modülü (PCM) kaldırmaya hazır olduğunuzda bu yönergeleri izleyin.

> [!NOTE]
> PCM'nizi çıkarmadan önce, doğru bir değiştirmeniz olduğunu doğrulayın (birincil kasa için 764 W veya EBOD muhafazası için 580 W).

#### <a name="to-remove-a-pcm"></a>PCM'yi kaldırmak için
1. Azure klasik portalında **Ayarlar > Donanım sağlığını >'yı**tıklatın. Hangi PCM'nin başarısız olduğunu belirlemek için **Paylaşılan bileşenler** altındaki PCM bileşenlerinin durumunu denetleyin:
   
   * PCM 0'daki bir güç kaynağı başarısız olduysa, **PCM 0'deki Güç Kaynağı'nın** durumu kırmızı olacaktır.
   * PCM 1'deki bir güç kaynağı başarısız olduysa, **PCM 1'deki Güç Kaynağı'nın** durumu kırmızı olacaktır.
   * PCM 1'deki fan başarısız olduysa, **PCM 0 için Soğutma 0** veya **PCM 0 için Soğutma 1** durumu kırmızı olacaktır.
2. Başarısız PCM'yi birincil kasanın arkasına bulun. 8600 model çalıştırıyorsanız, ön panel LED ekranında gösterilen Sistem Birimi Kimlik Numarasına bakarak birincil muhafazayı tanımlayın. Birincil kasada görüntülenen varsayılan Birim Kimliği **00'dir,** EBOD kasası üzerinde görüntülenen varsayılan Birim Kimliği **ise 01'dir.** Aşağıdaki diyagram ve tablo LED ekranın ön panelini açıklar.
   
    ![Ön OPS panelinde sistem kimliği](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Şekil 1** Cihazın ön paneli  
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Sessiz düğmesi |
   | 2 |Sistem gücü |
   | 3 |Modül hatası |
   | 4 |Mantıksal hata |
   | 5 |Birim kimlik ekranı |
3. Birincil kasanın arka sındaki izleme göstergesi LED'leri hatalı PCM'yi tanımlamak için de kullanılabilir. Hatalı PCM'yi bulmak için LED'lerin nasıl kullanılacağını anlamak için aşağıdaki diyagrama ve tabloya bakın. Örneğin, **Fan Fail'e** karşılık gelen LED yanıyorsa, fan başarısız olur. Aynı şekilde, **AC Fail'e** karşılık gelen LED yanıyorsa, güç kaynağı başarısız olur. 
   
    ![Cihaz PCM izleme göstergesi LED'lerin arka düzlemi](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Şekil 2** Gösterge LED'li PCM'nin arkası
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |AC güç kesintisi |
   | 2 |Fan arızası |
   | 3 |Pil arızası |
   | 4 |PCM TAMAM |
   | 5 |DC güç kesintisi |
   | 6 |Pil sağlıklı |
4. Başarısız PCM modülünün yerini bulmak için StorSimple cihazının arka sının aşağıdaki diyagramına bakın. PCM 0 solda, PCM 1 sağda. Aşağıdaki tablo modülleri açıklar.
   
     ![Cihaz birincil muhafaza modüllerinin arka düzlemi](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Şekil 3** Eklenti modüllü cihazın arkası 
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
5. Hatalı PCM'yi kapatın ve güç kaynağı kablosunu kapatın. Artık PCM'yi kaldırabilirsiniz.
6. Başparmağınız ve işaret parmağınız arasındaki PCM tutucunun mandalı ve yan tarafını tutun ve tutamacı açmak için birbirine sıkın.
   
    ![PCM Kolu Açma](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Şekil 4** PCM tutamacını açma
7. Kolu tutun ve PCM'yi çıkarın.
   
    ![Aygıt PCM'yi Kaldırma](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Şekil 5** PCM'yi kaldırma

## <a name="install-a-replacement-pcm"></a>Yedek PCM yükleme
StorSimple cihazınıza bir PCM yüklemek için aşağıdaki yönergeleri izleyin. Yedek PCM'yi takmadan önce yedek pil modüllerini taktığınıza emin olun (yalnızca 764 W PCM için geçerlidir). Daha fazla bilgi için yedek pil modüllerini nasıl [kaldırıp takabilirsiniz.](storsimple-8000-battery-replacement.md)

#### <a name="to-install-a-pcm"></a>PCM yüklemek için
1. Bu muhafaza için doğru yedek PCM'ye sahip olduğunuzu doğrulayın. Birincil kasanın 764 W PCM'ye, EBOD kasasının ise 580 W PCM'ye ihtiyacı vardır. Birincil kasadaki 580 W PCM'yi veya EBOD kasasındaki 764 W PCM'yi kullanmaya çalışmamalısınız. Aşağıdaki resim, PCM'ye yapıştırılmış etiketteki bu bilgilerin nerede tanımlandığı gösterilmektedir.
   
    ![Cihaz PCM Etiketi](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Şekil 6** PCM etiketi
2. Konektörlere özellikle dikkat edin, muhafazada hasar olup olup olup yok. 
   
   > [!NOTE]
   > **Herhangi bir konektör pimi bükülürse modülü yüklemeyin.**
   > 
   > 
3. PCM tutamacı açık konumda olduğu için modülü kasaya kaydırın.
   
    ![Aygıt PCM'nin Kurulumu](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Şekil 7** PCM'nin yüklenmesi
4. PCM tutamacını manuel olarak kapatın. Tutamaç devreye girerken bir tıklama duymalısınız.
   
   > [!NOTE]
   > Konektör pimlerinin çatladığından emin olmak için, mandalını bırakmadan tutamacı hafifçe çekebilirsiniz. PCM dışarı kaydırılırsa, bu, konektörler devreye girmeden önce mandalı kapalı olduğu anlamına gelir.
   
5. Güç kablolarını güç kaynağına ve PCM'ye bağlayın.
6. Gerinim kabartma balyalarını sabitle.
7. PCM'yi açın.
8. Değiştirmenin başarılı olduğunu doğrulayın: StorSimple Device Manager hizmetinizin Azure portalında cihazınıza gidin ve ardından **Ayarlar > Donanım sağlığını >.** Paylaşılan **bileşenler**altında, PCM'nin durumu yeşil olmalıdır.
   
   > [!NOTE]
   > Değiştirilen PCM'nin tamamen başlatılması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşen değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

