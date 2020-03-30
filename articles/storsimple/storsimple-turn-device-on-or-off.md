---
title: StorSimple 8000 serisi cihazınızı açma veya kapatma
description: Yeni bir StorSimple aygıtının nasıl açılacağını, kapatılmış veya güç kaybeden bir aygıtı nasıl açacağını ve çalışan bir aygıtı nasıl kapatacağını açıklar.
author: alkohli
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1319583569a8abc619ad902a87ee551b476f88eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254631"
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınızı açın veya kapatın

## <a name="overview"></a>Genel Bakış
Normal sistem çalışmasının bir parçası olarak bir Microsoft Azure StorSimple aygıtının kapatılması gerekli değildir. Ancak, yeni bir aygıtı veya kapatılması gereken bir aygıtı açmanız gerekebilir. Genellikle, başarısız donanımı değiştirmeniz, bir birimi fiziksel olarak taşımanız veya aygıtı hizmet dışı götürmeniz gereken durumlarda kapatma gerekir. Bu öğretici, StorSimple cihazınızı farklı senaryolarda açmak ve kapatmak için gerekli yordamı açıklar.

## <a name="turn-on-a-new-device"></a>Yeni bir cihazı açma
StorSimple aygıtını ilk kez açma adımları, aygıtın 8100 veya 8600 modeli olup olmadığına bağlı olarak değişir. 8100 tek bir birincil kasaya sahipken, 8600 birincil kasası ve EBOD muhafazası olan çift kasalı bir cihazdır. Her iki model için ayrıntılı adımlar aşağıdaki bölümlerde ele alınmıştır.

* [Yalnızca birincil kasaya sahip yeni aygıt](#new-device-with-primary-enclosure-only)
* [EBOD muhafazalı yeni cihaz](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Yalnızca birincil kasaya sahip yeni aygıt
StorSimple 8100 modeli tek bir kasa cihazıdır. Cihazınızyedek Güç ve Soğutma Modülleri (PCM) içerir. Yüksek kullanılabilirlik sağlamak için her iki PCM'nin de yüklü ve farklı güç kaynaklarına bağlanması gerekir.

Cihazınızı güç için kablolamak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Tam cihaz kurulumu ve kablolama yönergeleri için [StorSimple 8100 cihazınızı yükleyin.](storsimple-8100-hardware-installation.md) Talimatları tam olarak uyguladığınıza emin olun.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>EBOD muhafazalı yeni cihaz
StorSimple 8600 modeli hem birincil kasaya hem de EBOD kasaya sahiptir. Bu, birimlerin Seri Bağlı SCSI (SAS) bağlantısı ve gücü için birbirine bağlanmasını gerektirir.

Bu aygıtı ilk kez ayarlarken, önce SAS kablolama için adımları gerçekleştirin ve ardından güç kablolama için adımları tamamlayın.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Tam cihaz kurulumu ve kablolama yönergeleri için [StorSimple 8600 cihazınızı yükleyin.](storsimple-8600-hardware-installation.md) Talimatları tam olarak uyguladığınıza emin olun.

## <a name="turn-on-a-device-after-shutdown"></a>Kapatma dan sonra aygıtı açma
Bir StorSimple aygıtı kapatıldıktan sonra açma adımları, aygıtın 8100 veya 8600 model olmasına bağlı olarak farklıdır. 8100 tek bir birincil kasaya sahipken, 8600 birincil kasası ve EBOD muhafazası olan çift kasalı bir cihazdır.

* [Yalnızca birincil kasaya sahip aygıt](#device-with-primary-enclosure-only)
* [EBOD muhafazalı cihaz](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Yalnızca birincil kasaya sahip aygıt
Kapatma dan sonra, birincil kasası olan ve EBOD muhafazası olmayan bir StorSimple cihazını açmak için aşağıdaki yordamı kullanın.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Yalnızca birincil kasası olan bir aygıtı açmak için
1. Hem Güç hem de Soğutma Modülleri'ndeki (PCM) güç anahtarlarının KAPALı konumda olduğundan emin olun. Anahtarlar KAPALI konumda değilse, bunları KAPALI konuma getirin ve ışıkların sönmesini bekleyin.
2. Her iki PCM'deki güç anahtarlarını ANDAKI konuma çevirerek cihazı açın. Cihaz açılmalıdır.
3. Aygıtın tam olarak açıktık ını doğrulamak için aşağıdakileri kontrol edin:
   
   1. Her iki PCM modülündeki OK LED'ler yeşildir.
   2. Her iki denetleyicide durum LED'leri düz yeşildir.
   3. Denetleyicilerden birinin mavi LED'i yanıp sönüyor, bu da denetleyicinin etkin olduğunu gösteriyor.
      
      Bu koşullardan herhangi biri karşılanmazsa, cihazınız sağlıklı değildir. Lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

### <a name="device-with-ebod-enclosure"></a>EBOD muhafazalı cihaz
Kapatmadan sonra, birincil kasası ve EBOD muhafazası olan bir StorSimple aygıtını açmak için aşağıdaki yordamı kullanın. Her adımı açıklandığı gibi sırayla gerçekleştirin. Aksi takdirde veri kaybına neden olabilir.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Birincil ve EBOD muhafazası olan bir cihazı açmak için
1. EBOD kasasının birincil kasaya bağlı olduğundan emin olun. Daha fazla bilgi için [StorSimple 8600 cihazınızı yükleyin.](storsimple-8600-hardware-installation.md)
2. Hem EBOD hem de birincil kasalarda bulunan Güç ve Soğutma Modüllerinin (PCM) KAPALı konumda olduğundan emin olun. Anahtarlar KAPALI konumda değilse, bunları KAPALI konuma getirin ve ışıkların sönmesini bekleyin.
3. Her iki PCM'deki güç anahtarlarını andaki konuma çevirerek önce EBOD kasasını açın. PCM LED'ler yeşil olmalıdır. Bu ünitedeki yeşil EBOD denetleyici LED,EBOD kasasının ait olduğunu gösterir.
4. Her iki PCM'deki güç anahtarlarını ANDAKI konuma çevirerek birincil muhafazayı açın. Tüm sistem şu anda avelmemeli.
5. SAS LED'lerinin yeşil olduğunu doğrulayın, bu da EBOD kasası ile birincil kasa arasındaki bağlantının iyi olmasını sağlar.

## <a name="turn-on-a-device-after-a-power-loss"></a>Güç kaybından sonra cihazı açma
Elektrik kesintisi veya kesinti, StorSimple aygıtını kapatabilir. Elektrik kesintisi, güç kaynaklarının birinde veya her iki güç kaynağında da olabilir. Kurtarma adımları, aygıtın 8100 veya 8600 modeli olup olmadığına bağlı olarak farklıdır. 8100 tek bir birincil kasaya sahipken, 8600 birincil kasası ve EBOD muhafazası olan çift kasalı bir cihazdır. Bu bölümde, her senaryo için kurtarma yordamı açıklanır.

* [Yalnızca birincil kasaya sahip aygıt](#8100)
* [EBOD muhafazalı cihaz](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Yalnızca birincil kasaya sahip aygıt<a name="8100">
Güç kaynaklarının birinde güç kaybı olursa sistem normal işleyişine devam edebilir. Ancak, cihazın yüksek kullanılabilirliğini sağlamak için, güç kaynağına mümkün olan en kısa sürede güç geri yükleyin.

Her iki güç kaynağında da elektrik kesintisi veya elektrik kesintisi varsa, sistem düzenli ve kontrollü bir şekilde kapanır. Güç geri yüklendiğinde, sistem otomatik olarak açılacaktır.

### <a name="device-with-ebod-enclosure-a-name8600"></a>EBOD muhafazalı cihaz<a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Bir güç kaynağında güç kaybı
Sistem, birincil kasada veya EBOD muhafazasında güç kaynaklarından birinde güç kesintisi olursa normal çağrısına devam edebilir. Ancak, cihazın yüksek kullanılabilirliğini sağlamak için, lütfen mümkün olan en kısa sürede güç kaynağına güç geri yükleyin.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Birincil ve EBOD muhafazalarında hem güç kaynaklarında güç kaybı
Her iki güç kaynağında da elektrik kesintisi veya elektrik kesintisi olursa, EBOD muhafazası derhal kapanır ve birincil kasa düzenli ve kontrollü bir şekilde kapanır. Güç geri yüklendiğinde, cihaz otomatik olarak çalışmaya başlar.

Güç el ile kapatılırsa, sisteme güç geri yüklemek için aşağıdaki adımları izleyin.

1. EBOD kasasını açın.
2. EBOD muhafazası açOlduktan sonra, birincil muhafazayı açın.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>EBOD kasası üzerindeki her iki güç kaynağında da güç kaybı
Kablolarınızı kurduğunuzda, EBOD'un hiçbir zaman ayrı bir PDU'ya bağlı olmadığından emin olmalısınız. EBOD ve birincil kasa aynı anda başarısız olursa, sistem düzelir.

Her iki güç kaynağında da yalnızca EBOD muhafazası arızalanırsa, sistem otomatik olarak iyileşmez. Sistemi açmak ve sağlıklı bir duruma geri yüklemek için aşağıdaki adımları izleyin:

1. Birincil kasa açıksa, hem Güç hem de Soğutma Modüllerini (PCM) kapatın.
2. Sistemin kapanması için birkaç dakika bekleyin.
3. EBOD kasasını açın.
4. EBOD muhafazası açOlduktan sonra, birincil muhafazayı açın.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Birincil ve EBOD muhafaza bağlantısı kaybolduktan sonra cihazı açın
Bekleme denetleyicisi ile ilgili EBOD denetleyicisi arasındaki bağlantı kaybolursa, aygıt çalışmaya devam ediyor. Sistem etkin denetleyicisi ile ilgili EBOD denetleyicisi arasındaki bağlantı kaybolursa, arıza meydana gelmeli ve cihaz normal şekilde çalışmaya devam etmelidir.

Her iki Seri Bağlı SCSI (SAS) kablosu kaldırıldığında veya EBOD kasası ile birincil kasa arasındaki bağlantı kesildiğinde, aygıt çalışmayı durdurur. Bu noktada, aşağıdaki adımları gerçekleştirin.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Bağlantı kaybolduktan sonra cihazı açmak için
1. Aygıtın arkasına erişin.
2. EBOD kasası ile birincil kasa arasındaki SAS kablo bağlantısı kırılırsa, EBOD kasası üzerindeki tüm SAS şeritLI LED'ler kapalı olacaktır.
3. EBOD muhafazası ve birincil kasada hem Güç hem de Soğutma Modüllerini (PCM'leri) kapatın.
4. Her iki kasanın arka tarafındaki tüm ışıklar kapanana kadar bekleyin.
5. SAS kablolarını yeniden takın ve EBOD muhafazası ile birincil kasa arasında iyi bir bağlantı olduğundan emin olun.
6. Her iki PCM anahtarını da ANDAKI konuma getirerek önce EBOD kasasını açın.
7. Yeşil LED'in A.B.D. olup olmadığını kontrol ederek EBOD kasasının üzerinde olduğundan emin olun.
8. Birincil muhafazayı açın.
9. Denetleyici yeşil LED'inin AÇ OLUP Olmadığını kontrol ederek birincil muhafazanın üzerinde olduğundan emin olun.
10. Birincil kasaile EBOD muhafaza bağlantısının, SAS şeritLI LED'lerin (EBOD denetleyicibaşına dört) tüm AÇI'da olup olmadığını kontrol ederek iyi olduğunu doğrulayın.

> [!IMPORTANT]
> SAS kabloları arızalıysa veya EBOD muhafazası ile birincil kasa arasındaki bağlantı iyi değilse, sistemi açtığınızda kurtarma moduna geçer. Bu durumda lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)


## <a name="turn-off-a-running-device"></a>Çalışan bir cihazı kapatma
Çalışan bir StorSimple aygıtıtaşınıyorsa, hizmet dışı ediliyorsa veya değiştirilmesi gereken arızalı bir bileşeni varsa kapatılması gerekebilir. Adımlar, StorSimple aygıtının 8100 veya 8600 modeli olup olmadığına bağlı olarak farklıdır. 8100 tek bir birincil kasaya sahipken, 8600 birincil kasası ve EBOD muhafazası olan çift kasalı bir cihazdır. Bu bölümde çalışan bir aygıtı kapatmak için adımlar ayrıntıları.

* [Birincil muhafazalı aygıt](#8100a)
* [EBOD muhafazalı cihaz](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Birincil muhafazalı aygıt<a name="8100a">
Aygıtı düzenli ve kontrollü bir şekilde kapatmak için, bunu Azure portalı üzerinden veya StorSimple için Windows PowerShell üzerinden yapabilirsiniz. 

> [!IMPORTANT]
> Cihazın arka sideki güç düğmesini kullanarak çalışan bir aygıtı kapatmayın.
> 
> Aygıtı kapatmadan önce, tüm aygıt bileşenlerinin sağlıklı olduğundan emin olun. Azure portalında **Aygıtlar** > **Donanımını İzle** > **durumuna**gidin ve tüm bileşenlerin durumunun yeşil olduğunu doğrulayın. Bu sadece sağlıklı bir sistem için geçerlidir. Sistem arızalı bir bileşeni değiştirmek için kapatılıyorsa, **Donanım Durumu'ndaki**ilgili bileşen için başarısız (kırmızı) veya bozulmuş (sarı) bir durum görürsünüz.
> 
> 

StorSimple için Windows PowerShell'e veya Azure portalına eriştinkten sonra, [bir StorSimple aygıtını kapatma](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device)adımlarını izleyin. 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>EBOD muhafazalı cihaz<a name="8600a">
> [!IMPORTANT]
> Birincil muhafazayı ve EBOD muhafazasını kapatmadan önce, tüm cihaz bileşenlerinin sağlıklı olduğundan emin olun. Azure portalında **Aygıtlar** > **Donanımını İzle'ye** > **Hardware health**gidin ve tüm bileşenlerin sağlıklı olduğunu doğrulayın.


#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>EBOD muhafazalı çalışan bir aygıtı kapatmak için
1. Birincil kasa için [bir StorSimple aygıtını kapatın'da](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) listelenen tüm adımları izleyin.
2. Birincil kasa kapatıldıktan sonra, Hem Güç hem de Soğutma Modülü (PCM) anahtarlarını ters çevirerek EBOD'u kapatın.
3. EBOD'un kapandığını doğrulamak için, EBOD kasasının arka yerindeki tüm ışıkların kapalı olup olmadığını kontrol edin.

> [!NOTE]
> EBOD kasasını birincil kasaya bağlamak için kullanılan SAS kabloları, sistem kapatılana kadar çıkarılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar
StorSimple aygıtını açarken veya kapatırken sorunlarla karşılaşırsanız [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

