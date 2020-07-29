---
title: StorSimple 8000 serisi cihazındaki PCM 'yi değiştirme | Microsoft Docs
description: StorSimple cihazınızda güç ve soğutma modülünün (PCM) nasıl kaldırılacağını ve değiştirileceğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 045cec85174a88d1d608a4adc679461008852768
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514598"
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>StorSimple cihazınızın güç ve soğutma modülünü değiştirme
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple cihazınızdaki güç ve soğutma modülü (PCM), birincil ve EBOD kasaları aracılığıyla denetlenen bir güç kaynağı ve soğutma fanından oluşur. Her bir kutu için sertifikalı yalnızca bir adet PCM modeli vardır. Birincil kutu 764 W PCM için sertifikalıdır ve EBOD Kasası 580 W PCM için sertifikalıdır. Birincil kutu ve EBOD Kasası için PCMs, farklı olsa da, değiştirme yordamı aynıdır.

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* PCM 'yi kaldırma
* Yeni bir PCM yüklemesi

> [!IMPORTANT]
> PCM 'yi kaldırmadan ve değiştirmeden önce, [StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md)içindeki güvenlik bilgilerini gözden geçirin.


## <a name="before-you-replace-a-pcm"></a>PCM 'yi değiştirmeden önce
PCM 'nizi değiştirmeden önce aşağıdaki önemli sorunları göz önünde bulundurun:

* PCM 'nin güç kaynağı başarısız olursa, hatalı modülünü yüklü bırakın, ancak güç kablosunu kaldırın. Fan, kasadan güç almaya ve uygun soğutma sağlamaya devam edecektir. Fan başarısız olursa, PCM 'nin hemen değiştirilmeleri gerekir.
* PCM 'yi kaldırmadan önce, ana anahtarı (varsa) kapatarak veya güç kablosunu fiziksel olarak kaldırarak PCM 'den gücün bağlantısını kesin. Bu, sisteminize bir güç kapatma işlemi olduğunu belirten bir uyarı sağlar.
* Hatalı PCM 'yi değiştirmeden önce, devam eden sistem işlemi için diğer PCM 'nin işlevsel olduğundan emin olun. Hatalı PCM 'nin, mümkün olan en kısa sürede tamamen işlemsel PCM ile değiştirilmeleri gerekir.
* PCM modülü değiştirme işleminin tamamlanması yalnızca birkaç dakika sürer, ancak aşırı Isıtma oluşmasını engellemek için başarısız PCM 'yi kaldırmanın 10 dakika içinde tamamlanması gerekir.
* Fabrikadan gelen değiştirme 764 W PCM modüllerinin yedekleme pil modülünü içermediğini unutmayın. Pili hatalı PCM 'den kaldırmanız ve ardından değişikliği yapmadan önce bunu değiştirme modülüne eklemeniz gerekir. Daha fazla bilgi için bkz. [Yedekleme Pil modülünü kaldırma ve ekleme](storsimple-8000-battery-replacement.md).

## <a name="remove-a-pcm"></a>PCM 'yi kaldırma
Microsoft Azure StorSimple cihazınızdan bir güç ve soğutma modülünü (PCM) kaldırmaya hazırsanız bu yönergeleri izleyin.

> [!NOTE]
> PCM 'nizi kaldırmadan önce, doğru bir değiştirme (birincil kutu için 764 W veya EBOD Kasası için 580 W) olduğunu doğrulayın.

#### <a name="to-remove-a-pcm"></a>PCM 'yi kaldırmak için
1. Klasik Azure portalında **ayarlar > > donanım durumunu izle**' ye tıklayın. Hangi PCM 'nin başarısız olduğunu belirlemek için, **paylaşılan bileşenler** altındaki PCM bileşenlerinin durumunu kontrol edin:
   
   * PCM 0 ' daki bir güç kaynağı başarısız olduysa, **PCM 0 ' daki güç kaynağı** durumu kırmızı olur.
   * PCM 1 ' deki bir güç kaynağı başarısız olduysa, **PCM 1 ' deki güç kaynağı** durumu kırmızı olacaktır.
   * PCM 1 ' deki fan başarısız olduysa, PCM 0 **Için soğutma 0** ya da **PCM 0 için soğutma 1** durumu kırmızı olacaktır.
2. Birincil kutunun arkasında başarısız olan PCM 'yi bulun. 8600 modeli çalıştırıyorsanız, ön panel LED ' te gösterilen sistem birimi kimlik numarasına bakarak birincil Kasası tanımlama. Birincil kasada görünen varsayılan birim KIMLIĞI **00**' dır, ancak EBOD muhafazasında görünen varsayılan birim kimliği **01**' dir. Aşağıdaki diyagramda ve tabloda, LED görüntüsüne ait ön panel açıklanmaktadır.
   
    ![Ön OPS panelinde sistem KIMLIĞI](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Şekil 1** Cihazın ön bölmesi  
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |Sessiz düğme |
   | 2 |Sistem gücü |
   | 3 |Modül hatası |
   | 4 |Mantıksal hata |
   | 5 |Birim KIMLIĞI görüntüleme |
3. Birincil kutunun arkasında bulunan izleme göstergesi LED 'Leri hatalı PCM 'yi belirlemek için de kullanılabilir. Hatalı PCM 'yi bulmak için LED 'Leri nasıl kullanacağınızı anlamak için aşağıdaki diyagrama ve tabloya bakın. Örneğin, **fanı** 'a KARŞıLıK gelen LED, fanı ise, fan başarısız olur. Benzer şekilde, AC 'ye karşılık gelen LED 'in **başarısız** olması durumunda güç kaynağı başarısız olur. 
   
    ![Cihaz PCM izleme göstergesi LED 'Leri arka düzlemi](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Şekil 2** Gösterge LED 'Leri ile PCM 'nin geri dönmesi
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |AC güç hatası |
   | 2 |Fan hatası |
   | 3 |Pil hatası |
   | 4 |PCM TAMAM |
   | 5 |DC güç hatası |
   | 6 |Pil sağlıklı |
4. Başarısız PCM modülünü bulmak için StorSimple cihazının geri doğru olan aşağıdaki diyagrama bakın. PCM 0 solda ve PCM 1 sağda. Aşağıdaki tabloda modüller açıklanmaktadır.
   
     ![Cihaz birincil kutu modüllerinin geri düzlemi](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Şekil 3** Eklenti modülleriyle cihaz geri dön 
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
5. Hatalı PCM 'yi kapatın ve güç kaynağı kablosunun bağlantısını kesin. Artık PCM 'yi kaldırabilirsiniz.
6. Parmak izi ve küçük resim arasında PCM tutamacının mandalı ve tarafının yanı sıra tanıtıcıyı açmak için bunları birbirine sıkıştırın.
   
    ![PCM tanıtıcısı açılıyor](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Şekil 4** PCM tanıtıcısı açılıyor
7. Tutamacı idare edin ve PCM 'yi kaldırın.
   
    ![Cihaz PCM 'yi kaldırma](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Şekil 5** PCM 'yi kaldırma

## <a name="install-a-replacement-pcm"></a>Yeni bir PCM yüklemesi
StorSimple cihazınıza bir PCM yüklemek için bu yönergeleri izleyin. Değiştirme PCM 'yi yüklemeden önce yedekleme pil modülünü yerleştirdiğinizden emin olun (yalnızca 764 W PCMs için geçerlidir). Daha fazla bilgi için bkz. [Yedekleme Pil modülünü kaldırma ve ekleme](storsimple-8000-battery-replacement.md).

#### <a name="to-install-a-pcm"></a>PCM 'yi yüklemek için
1. Bu kutu için doğru değişiklik PCM 'ye sahip olduğunuzu doğrulayın. Birincil kutunun bir 764 W PCM olması gerekir ve EBOD Kasası 580 W PCM gerektirir. Birincil kasada 580 W PCM veya EBOD kasasında 764 W PCM kullanmayı denememelisiniz. Aşağıdaki görüntüde, PCM 'ye yapıştırılmış etikette bu bilgilerin nerede tanımlanacağına ilişkin bilgiler gösterilmektedir.
   
    ![Cihaz PCM etiketi](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Şekil 6** PCM etiketi
2. Bağlayıcılara dikkat edin ve bağlayıcılar için dikkat edin. 
   
   > [!NOTE]
   > **Herhangi bir bağlayıcı PIN 'i Bükülü ise modülü yüklemeyin.**
   > 
   > 
3. Açık konumdaki PCM tutamacı ile modülü, kutunun içine kaydırın.
   
    ![Cihaz PCM 'yi yükleme](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Şekil 7** PCM 'yi yükleme
4. PCM tanıtıcısını el ile kapatın. Tanıtıcı mandalı olarak bir tıklama duymalısınız.
   
   > [!NOTE]
   > Bağlayıcının PIN 'lerinin sahip olduğundan emin olmak için, mandal serbest kalmadan tutamacı bir şekilde ayarlayabilirsiniz. PCM tarafından slaytlar kapalıysa, mandal bağlayıcılardan önce kapalı hale gelir.
   
5. Güç kablolarını güç kaynağına ve PCM 'ye bağlayın.
6. Güvenli hale getirme rahatları.
7. PCM 'yi açın.
8. Değiştirme işleminin başarılı olduğunu doğrulayın: StorSimple Aygıt Yöneticisi hizmetinizin Azure portal, cihazınıza gidin ve ardından **ayarlar > > donanım sistem durumunu izleyin**. **Paylaşılan bileşenler**altında, PCM 'nin durumu yeşil olmalıdır.
   
   > [!NOTE]
   > Değiştirme PCM 'nin tamamen başlatılması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

