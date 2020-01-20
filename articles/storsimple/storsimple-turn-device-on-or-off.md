---
title: StorSimple 8000 serisi cihazınızı açın veya kapatın
description: Yeni bir StorSimple cihazını nasıl kullanacağınızı, kapatılmış veya kapalı bir cihazı açma ve çalışan bir cihazı kapatma hakkında bilgiler sağlar.
author: alkohli
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1319583569a8abc619ad902a87ee551b476f88eb
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76276890"
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınızı açma veya kapatma

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple cihazının kapatılması, normal sistem işleminin bir parçası olarak gerekli değildir. Ancak, yeni bir cihaz veya kapatılması gereken bir cihaz açmanız gerekebilir. Genellikle, başarısız olan donanımı değiştirmek, bir birimi fiziksel olarak taşımak veya hizmet dışı bir cihaz almak zorunda olduğu durumlarda bir kapatma gerekir. Bu öğreticide, StorSimple cihazınızı farklı senaryolarda açma ve kapatma için gereken yordam açıklanmaktadır.

## <a name="turn-on-a-new-device"></a>Yeni bir cihazı aç
StorSimple cihazını ilk kez açmak için gereken adımlar, cihazın 8100 veya 8600 modeli olmasına bağlı olarak farklılık gösterir. 8100 tek bir birincil muhafaza içerir, ancak 8600 birincil bir kutu ve EBOD Kasası olan çift taraflı bir cihazdır. Her iki modelin ayrıntılı adımları aşağıdaki bölümlerde ele alınmıştır.

* [Yalnızca birincil Kasası olan yeni cihaz](#new-device-with-primary-enclosure-only)
* [EBOD kasası ile yeni cihaz](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Yalnızca birincil Kasası olan yeni cihaz
StorSimple 8100 modeli tek bir muhafaza aygıtıdır. Cihazınız yedekli güç ve soğutma modülleri (PCMs) içerir. Yüksek kullanılabilirlik sağlamak için her iki PCMs 'nin yüklü ve farklı güç kaynaklarına bağlanması gerekir.

Cihazınızın güç bağlantısını yapmak için aşağıdaki adımları uygulayın.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Tüm cihaz kurulumu ve kablolama yönergeleri için, [StorSimple 8100 cihazınızı yükleme](storsimple-8100-hardware-installation.md)bölümüne gidin. Yönergeleri tam olarak izlediğinizden emin olun.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>EBOD kasası ile yeni cihaz
StorSimple 8600 modelinde hem birincil bir kutu hem de bir EBOD kasası vardır. Bu, birimlerin seri bağlantılı SCSI (SAS) bağlantısı ve gücü için bir araya gruplanmalıdır.

Bu cihazı ilk kez ayarlarken, önce SAS kablolaması için adımları gerçekleştirin ve ardından güç kabloları adımlarını doldurun.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Tüm cihaz kurulumu ve kablolama yönergeleri için, [StorSimple 8600 cihazınızı yükleme](storsimple-8600-hardware-installation.md)bölümüne gidin. Yönergeleri tam olarak izlediğinizden emin olun.

## <a name="turn-on-a-device-after-shutdown"></a>Kapatmadan sonra bir cihazı açma
Bir StorSimple cihazını kapatduktan sonra açma adımları, cihazın bir 8100 veya 8600 modeli olmasına bağlı olarak farklılık belirtir. 8100 tek bir birincil muhafaza içerir, ancak 8600 birincil bir kutu ve EBOD Kasası olan çift taraflı bir cihazdır.

* [Yalnızca birincil Kasası olan cihaz](#device-with-primary-enclosure-only)
* [EBOD kasası ile cihaz](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Yalnızca birincil Kasası olan cihaz
Bir kapatmadan sonra, bir StorSimple cihazını birincil bir muhafaza ve EBOD Kasası olmadan açmak için aşağıdaki yordamı kullanın.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Bir cihazı yalnızca birincil bir kasa ile açmak için
1. Güç ve soğutma modüllerindeki (PCMs) güç anahtarlarının kapalı konumda olduğundan emin olun. Anahtarlar kapalı konumda değilse, bunları kapalı konumuna çevirin ve ışıklarının bitmesini bekleyin.
2. Her iki PCMs üzerindeki güç anahtarlarını açık konuma getirerek cihazı açın. Cihazın açık olması gerekir.
3. Cihazın tam açık olduğunu doğrulamak için aşağıdakileri denetleyin:
   
   1. Her iki PCM modül üzerindeki Tamam LED 'Leri yeşildir.
   2. Her iki denetleyicde durum LED 'Leri düz yeşil.
   3. Denetleyicilerden birindeki mavi ışığı yanıp söndüğü için denetleyicinin etkin olduğunu gösterir.
      
      Bu koşullardan herhangi biri karşılanmazsa, cihazınız sağlıklı değildir. Lütfen [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>EBOD kasası ile cihaz
Bir kapatmadan sonra, bir StorSimple cihazını birincil bir kutu ve EBOD kasası ile açmak için aşağıdaki yordamı kullanın. Her bir adımı tam olarak açıklandığı gibi sırayla gerçekleştirin. Bunun başarısız olması, veri kaybına neden olabilir.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Bir cihazı birincil ve EBOD kasası ile açmak için
1. EBOD Kasası 'nın birincil muhafaza ile bağlı olduğundan emin olun. Daha fazla bilgi için bkz. [StorSimple 8600 cihazınızı yüklemeyi](storsimple-8600-hardware-installation.md).
2. Hem EBOD hem de birincil kasaları üzerindeki güç ve soğutma modüllerinin (PCMs) kapalı konumunda olduğundan emin olun. Anahtarlar kapalı konumda değilse, bunları kapalı konumuna çevirin ve ışıklarının bitmesini bekleyin.
3. Her iki PCMs üzerindeki güç anahtarlarını açık konuma getirerek EBOD Kasası 'nı etkinleştirin. PCM LED 'Leri yeşil olmalıdır. Bu birim üzerinde yeşil bir EBOD denetleyicisi, EBOD Kasası 'nın açık olduğunu gösterir.
4. Her iki PCMs üzerindeki güç anahtarlarını açık konuma getirerek birincil Kasası açın. Tüm sistem artık açık olmalıdır.
5. SAS LED 'lerini yeşil olduğunu doğrulayın. Bu, EBOD kasası ile birincil kasa arasındaki bağlantının iyi olmasını sağlar.

## <a name="turn-on-a-device-after-a-power-loss"></a>Güç kaybından sonra bir cihazı açma
Güç kesintisi veya kesinti, StorSimple cihazını kapatabilir. Güç kesintisi güç kaynağı veya güç kaynaklarının birinde gerçekleşebilir. Kurtarma adımları, cihazın bir 8100 veya 8600 modeli olmasına bağlı olarak farklılık belirtir. 8100 tek bir birincil muhafaza içerir, ancak 8600 birincil bir kutu ve EBOD Kasası olan çift taraflı bir cihazdır. Bu bölümde her senaryonun kurtarma yordamı açıklanmaktadır.

* [Yalnızca birincil Kasası olan cihaz](#8100)
* [EBOD kasası ile cihaz](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Yalnızca birincil kasa <a name="8100"> cihaz
Güç kaynaklarının birine güç kaybı olursa sistem normal işlemine devam edebilir. Ancak, cihazın yüksek oranda kullanılabilirliğini sağlamak için güç kaynağına mümkün olan en kısa sürede yeniden yükleyin.

Her iki güç kaynağı üzerinde güç kesintisi veya güç kesintisi varsa, sistem düzenli olarak ve denetimli bir şekilde kapanır. Güç geri yüklendiğinde sistem otomatik olarak açılır.

### <a name="device-with-ebod-enclosure-a-name8600"></a>EBOD Kasası <a name="8600"> cihaz
#### <a name="power-loss-on-one-power-supply"></a>Bir güç kaynağında güç kaybı
Birincil kasadan veya EBOD kasasında güç kaynaklarının birine güç kaybı varsa, Sistem normal işlemine devam edebilir. Ancak, cihazın yüksek oranda kullanılabilirliğini sağlamak için lütfen güç kaynağına en kısa sürede gücü geri yükleyin.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Birincil ve EBOD kasaları üzerinde hem güç kaynaklarında güç kaybı
Her iki güç kaynağı üzerinde güç kesintisi veya güç kesintisi varsa, EBOD Kasası hemen kapanır ve birincil kasa düzenli olarak ve denetimli bir şekilde kapanır. Güç geri yüklendiğinde, Gereç otomatik olarak başlatılır.

Güç el ile kapatılmışsa, sisteme güç geri yüklemek için aşağıdaki adımları uygulayın.

1. EBOD Kasası 'nı açın.
2. EBOD Kasası açıldıktan sonra, birincil Kasası açın.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>EBOD Kasası 'ndaki güç kaynaklarında güç kaybı
Kablolarınızı ayarlarken, EBOD 'nin hiç bağımsız olarak ayrı bir PDU 'ya bağlı olduğundan emin olmanız gerekir. EBOD ve birincil kutu aynı anda başarısız olursa sistem kurtarılır.

Her iki güç kaynağı üzerinde yalnızca EBOD Kasası başarısız olursa, sistem otomatik olarak kurtarılmaz. Sistemi açmak ve sağlıklı bir duruma geri yüklemek için aşağıdaki adımları uygulayın:

1. Birincil kutu açıksa hem güç hem de soğutma modüllerini (PCMs) kapatın.
2. Sistemin kapanması için birkaç dakika bekleyin.
3. EBOD Kasası 'nı açın.
4. EBOD Kasası açıldıktan sonra, birincil Kasası açın.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Birincil ve EBOD Kasası bağlantısı kaybolduktan sonra cihazı aç
Bekleme denetleyicisi ile buna karşılık gelen EBOD denetleyicisi arasında bağlantı kaybolursa cihaz çalışmaya devam eder. Sistem etkin denetleyicisi ile buna karşılık gelen EBOD denetleyicisi arasındaki bağlantı kaybolursa, yük devretme gerçekleşmelidir ve cihazın normal olarak çalışmaya devam etmesi gerekir.

Hem seri bağlı SCSI (SAS) kabloları kaldırıldığında ya da EBOD kasası ile birincil kasa arasındaki bağlantı çıkarıldığında, cihaz çalışmayı durdurur. Bu noktada, aşağıdaki adımları gerçekleştirin.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Bağlantı kaybolduktan sonra cihazı açmak için
1. Cihazın arkasına erişin.
2. EBOD kasası ve birincil kasa arasındaki SAS kablosu bağlantısı bozulur, EBOD Kasası 'ndaki tüm SAS Lane LED 'Leri kapalı olur.
3. EBOD kasası ve birincil kasa üzerinde hem güç hem de soğutma modüllerini (PCMs) kapatın.
4. Her iki muhafazaın arkasında bulunan tüm ışıklar kapanana kadar bekleyin.
5. SAS kablolarını yeniden takın ve EBOD kasası ile birincil kasa arasında iyi bir bağlantı olduğundan emin olun.
6. Her iki PCM öğesini de açık konuma getirerek EBOD Kasası 'nı etkinleştirin.
7. Yeşil LED 'in açık olduğunu kontrol ederek EBOD Kasası ' nın açık olduğundan emin olun.
8. Birincil Kasası açın.
9. Denetleyicinin yeşil LED 'in açık olduğunu kontrol ederek birincil kutunun açık olduğundan emin olun.
10. SAS Lane LED 'Leri (EBOD denetleyicisi başına dört) açık olduğunu kontrol ederek, birincil muhafaza ile EBOD Kasası bağlantısının iyi olduğunu doğrulayın.

> [!IMPORTANT]
> SAS kabloları kusurlu veya EBOD kasası ile birincil kasa arasındaki bağlantı iyi değilse, sistemi açtığınızda kurtarma moduna geçer. Lütfen bu durumda [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md) .


## <a name="turn-off-a-running-device"></a>Çalışan bir cihazı kapatma
Çalışan bir StorSimple cihazının taşınmakta olması, hizmetten alınması veya değiştirilmesini gerektiren hatalı çalışan bir bileşene sahip olması gerekebilir. Bu adımlar, StorSimple cihazının bir 8100 veya 8600 modeli olmasına bağlı olarak farklılık belirtir. 8100 tek bir birincil muhafaza içerir, ancak 8600 birincil bir kutu ve EBOD Kasası olan çift taraflı bir cihazdır. Bu bölümde çalışan bir cihazı kapatma adımları ayrıntılı olarak yapılır.

* [Birincil Kasası olan cihaz](#8100a)
* [EBOD kasası ile cihaz](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Birincil kasa <a name="8100a"> cihaz
Cihazı düzenli ve denetimli bir şekilde kapatmak için Azure portal veya StorSimple için Windows PowerShell aracılığıyla yapabilirsiniz. 

> [!IMPORTANT]
> Çalışan bir cihazı cihazın arkasında bulunan güç düğmesini kullanarak kapatmayın.
> 
> Cihazı kapatmadan önce tüm cihaz bileşenlerinin sağlıklı olduğundan emin olun. Azure portal, > **Donanım durumunu** **izlemek** > **cihazlar** ' a gidin ve tüm bileşenlerin durumunun yeşil olduğunu doğrulayın. Bu, yalnızca sağlıklı bir sistem için geçerlidir. Sistem hatalı çalışan bir bileşeni değiştirmek için kapalıysa, **donanım durumundaki**ilgili bileşen için başarısız (kırmızı) veya düşürülmüş (sarı) durumu görürsünüz.
> 
> 

StorSimple için Windows PowerShell veya Azure portal erişduktan sonra, [StorSimple cihazını kapatma](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device)bölümündeki adımları izleyin. 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>EBOD Kasası <a name="8600a"> cihaz
> [!IMPORTANT]
> Birincil kasası ve EBOD Kasası 'nı kapatmadan önce tüm cihaz bileşenlerinin sağlıklı olduğundan emin olun. Azure portal, > **donanım sistem durumunu** **izlemek** > **cihazlar** ' a gidin ve tüm bileşenlerin sağlıklı olduğunu doğrulayın.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Çalışan bir cihazı EBOD kasası ile kapatmak için
1. Birincil kasa için [bir StorSimple cihazını kapatma](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) bölümünde listelenen tüm adımları izleyin.
2. Birincil kutu kapatıldıktan sonra, güç ve soğutma modülü (PCM) anahtarlarını kapatarak EBOD 'yi kapatın.
3. EBOD 'nin kapatılmakta olduğunu doğrulamak için, EBOD Kasası 'nın arkasında bulunan tüm ışıklarının kapalı olduğunu kontrol edin.

> [!NOTE]
> EBOD kasasının birincil Kasası 'na bağlanması için kullanılan SAS kabloları, sistem kapanıncaya kadar kaldırılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar
StorSimple cihazını açarken veya kapatırken sorunlarla karşılaşırsanız [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md) .

