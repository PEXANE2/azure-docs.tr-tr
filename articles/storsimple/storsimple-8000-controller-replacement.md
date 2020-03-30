---
title: StorSimple 8000 serisi cihaz denetleyicisi değiştir | Microsoft Dokümanlar
description: StorSimple 8000 serisi cihazınızdaki bir veya her iki denetleyici modülünasıl kaldırılıp değiştirilmeye başlanmıştır.
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: dd2f6fcc9b2f5d716566e91e89487969613d1005
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267930"
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>StorSimple cihazınızda bir denetleyici modüldeğiştirme
## <a name="overview"></a>Genel Bakış
Bu öğretici, StorSimple aygıtındaki bir veya her iki denetleyici modülün nasıl kaldırılıp değiştirilmeye başladığını açıklar. Ayrıca, tek ve çift denetleyici değiştirme senaryoları için temel mantığı tartışır.

> [!NOTE]
> Denetleyici değiştirme gerçekleştirmeden önce, denetleyici firmware'inizi her zaman en son sürüme güncellemenizi öneririz.
> 
> StorSimple cihazınızın zarar görmesini önlemek için LED'ler aşağıdakilerden biri olarak gösterilene kadar kumandayı çıkarmayın:
> 
> * Tüm ışıklar KAPALI.
> * LED ![3, Yeşil](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png)onay ![simgesi](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) ve Kırmızı çapraz simgesi yanıp sönüyor ve LED 0 ve LED 7 **AÇ'de.**


Aşağıdaki tablodesteklenen denetleyici değiştirme senaryolarını gösterir.

| Case (Olay) | Değiştirme senaryosu | Uygulanabilir prosedür |
|:--- |:--- |:--- |
| 1 |Denetleyicilerden biri başarısız durumda, diğer denetleyici sağlıklı ve etkin. |[Tek denetleyici değiştirme](#replace-a-single-controller), [tek bir denetleyici değiştirme arkasındaki mantığı](#single-controller-replacement-logic)açıklayan , hem de değiştirme [adımları](#single-controller-replacement-steps). |
| 2 |Her iki denetleyici de başarısız oldu ve değiştirilmesi gerekiyor. Şasi, diskler ve disk muhafazası sağlıklıdır. |[Çift denetleyici değiştirme](#replace-both-controllers), [bir çift denetleyici değiştirme arkasındaki mantığı](#dual-controller-replacement-logic)açıklayan , hem de değiştirme [adımları](#dual-controller-replacement-steps). |
| 3 |Aynı aygıttan veya farklı aygıtlardan gelen denetleyiciler değiştirilir. Şasi, diskler ve disk muhafazaları sağlıklıdır. |Yuva uyuşmazlığı uyarı iletisi görüntülenir. |
| 4 |Bir denetleyici eksik ve diğer denetleyici başarısız olur. |[Çift denetleyici değiştirme](#replace-both-controllers), [bir çift denetleyici değiştirme arkasındaki mantığı](#dual-controller-replacement-logic)açıklayan , hem de değiştirme [adımları](#dual-controller-replacement-steps). |
| 5 |Denetleyicilerden biri veya her ikisi başarısız oldu. Aygıta seri konsol veya Windows PowerShell remoting üzerinden erişemezsiniz. |El ile denetleyici değiştirme yordamı için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 6 |Denetleyicilerin farklı bir yapı sürümü vardır ve bu sürüm aşağıdakilerden kaynaklanabilir:<ul><li>Denetleyicilerin farklı bir yazılım sürümü vardır.</li><li>Denetleyicilerin farklı bir firmware sürümü vardır.</li></ul> |Denetleyici yazılım sürümleri farklıysa, değiştirme mantığı bunu algılar ve yedek denetleyicideki yazılım sürümünü güncelleştirir.<br><br>Denetleyici firmware sürümleri farklıysa ve eski firmware sürümü otomatik olarak **yükseltilemezse,** Azure portalında bir uyarı iletisi görüntülenir. Güncelleştirmeleri tarayıp firmware güncelleştirmelerini yüklemeniz gerekir.</br></br>Denetleyici firmware sürümleri farklıysa ve eski firmware sürümü otomatik olarak yükseltilebilirse, denetleyici değiştirme mantığı bunu algılar ve denetleyici başladıktan sonra firmware otomatik olarak güncelleştirilir. |

Başarısız olduysa bir denetleyici modüllerini kaldırmanız gerekir. Denetleyici modüllerinden biri veya her ikisi başarısız olabilir, bu da tek bir denetleyici nin değiştirilmesine veya çift kumanda değişimine neden olabilir. Değiştirme yordamları ve bunların arkasındaki mantık için aşağıdakilere bakın:

* [Tek bir denetleyiciyi değiştirme](#replace-a-single-controller)
* [Her iki denetleyiciyi de değiştirin](#replace-both-controllers)
* [Denetleyiciyi kaldırma](#remove-a-controller)
* [Denetleyici ekleme](#insert-a-controller)
* [Cihazınızdaki etkin denetleyiciyi belirleme](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Bir denetleyiciyi çıkarmadan ve değiştirmeden [önce, StorSimple donanım bileşeni değiştirmedeki](storsimple-8000-hardware-component-replacement.md)güvenlik bilgilerini gözden geçirin.
> 
> 

## <a name="replace-a-single-controller"></a>Tek bir denetleyiciyi değiştirme
Microsoft Azure StorSimple aygıtındaki iki denetleyiciden biri arızalandığında, arızalandığında veya eksikse, tek bir denetleyici değiştirmeniz gerekir.

### <a name="single-controller-replacement-logic"></a>Tek denetleyici değiştirme mantığı
Tek bir denetleyici değiştirmede, önce başarısız olan denetleyiciyi kaldırmanız gerekir. (Aygıtta kalan denetleyici etkin denetleyicidir.) Yedek denetleyiciyi eklediğinizde, aşağıdaki eylemler oluşur:

1. Yedek denetleyici hemen StorSimple cihazı ile iletişim kurmaya başlar.
2. Etkin denetleyici için sanal sabit diskin (VHD) anlık görüntüsü yedek denetleyiciye kopyalanır.
3. Anlık görüntü, değiştirme denetleyicisi bu VHD'den başladığında bekleme denetleyicisi olarak tanınacak şekilde değiştirilir.
4. Değişiklikler tamamlandığında, yedek denetleyici bekleme denetleyicisi olarak başlayacaktır.
5. Her iki denetleyici de çalışırken, küme çevrimiçi olarak gelir.

### <a name="single-controller-replacement-steps"></a>Tek denetleyici değiştirme adımları
Microsoft Azure StorSimple aygıtınızdaki denetleyicilerden biri başarısız olursa aşağıdaki adımları tamamlayın. (Diğer denetleyici etkin ve çalışıyor olmalıdır. Her iki denetleyici de arızalanırsa veya arızalanırsa, [çift denetleyici değiştirme adımlarına](#dual-controller-replacement-steps)gidin .)

> [!NOTE]
> Denetleyicinin yeniden başlatılması ve tek denetleyici değiştirme yordamından tamamen kurtulması 30-45 dakika sürebilir. Kabloların takılması da dahil olmak üzere tüm yordamın toplam süresi yaklaşık 2 saattir.


#### <a name="to-remove-a-single-failed-controller-module"></a>Tek bir başarısız denetleyici modüllerini kaldırmak için
1. Azure portalında StorSimple Device Manager hizmetine gidin, **Cihazlar'ı**tıklatın ve ardından izlemek istediğiniz aygıtın adını tıklatın.
2. Donanım **>'na**gidin. Denetleyici 0 veya Denetleyici 1'in durumu kırmızı olmalıdır ve bu da bir hata olduğunu gösterir.
   
   > [!NOTE]
   > Tek bir denetleyici değişiminde başarısız olan denetleyici her zaman bir bekleme denetleyicisidir.
   
3. Başarısız denetleyici modülünü bulmak için Şekil 1 ve aşağıdaki tabloyu kullanın.
   
    ![Cihaz birincil muhafaza modüllerinin arka düzlemi](./media/storsimple-controller-replacement/IC740994.png)
   
    **Şekil 1** StorSimple cihazının arkası
   
   | Etiketle | Açıklama |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Denetleyici 0 |
   | 4 |Denetleyici 1 |
4. Başarısız denetleyicide, bağlı tüm ağ kablolarını veri bağlantı noktalarından çıkarın. 8600 model kullanıyorsanız, denetleyiciyi EBOD denetleyicisine bağlayan SAS kablolarını da çıkarın.
5. Başarısız denetleyiciyi kaldırmak için [denetleyiciyi kaldırma](#remove-a-controller) adımlarını izleyin.
6. Fabrika değişimini, başarısız denetleyicinin kaldırıldığı aynı yuvaya yükleyin. Bu, tek denetleyici değiştirme mantığını tetikler. Daha fazla bilgi için [tek denetleyici değiştirme mantığına](#single-controller-replacement-logic)bakın.
7. Tek denetleyici değiştirme mantığı arka planda ilerlerken, kabloları yeniden bağlayın. Tüm kabloları değiştirmeden önce bağlandıkları şekilde bağlamaya özen tinler.
8. Denetleyici yeniden başlatıldıktan sonra, denetleyicinin sağlıklı bir duruma geri döndüğünü ve bekleme modunda olduğunu doğrulamak için Azure portalındaki **Denetleyici durumunu** ve **Küme durumunu** kontrol edin.

> [!NOTE]
> Aygıtı seri konsoldan izliyorsanız, denetleyici değiştirme yordamından kurtulurken birden çok yeniden başlatma görebilirsiniz. Seri konsol menüsü sunulduğunda, değiştirmenin tamamladığını bilirsiniz. Menü denetleyici değişimini başladından sonraki iki saat içinde görünmüyorsa, lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
>
> Güncelleştirme 4'ü başlayarak, denetleyici `Get-HCSControllerReplacementStatus` değiştirme işleminin durumunu izlemek için aygıtın Windows PowerShell arabirimindeki cmdlet'i de kullanabilirsiniz.
> 

## <a name="replace-both-controllers"></a>Her iki denetleyiciyi de değiştirin
Microsoft Azure StorSimple aygıtındaki her iki denetleyici de arızalı, arızalı veya eksikse, her iki denetleyiciyi de değiştirmeniz gerekir. 

### <a name="dual-controller-replacement-logic"></a>Çift denetleyici değiştirme mantığı
Çift denetleyici değiştirmede, önce her iki başarısız denetleyiciyi de kaldırın ve sonra değiştirmeler eklersiniz. İki yedek denetleyici takıldığında, aşağıdaki eylemler oluşur:

1. Yuva 0'daki yedek denetleyici aşağıdakileri denetler:
   
   1. Firmware ve yazılım mevcut sürümlerini kullanıyor mu?
   2. Kümenin bir parçası mı?
   3. Eş denetleyici çalışıyor ve kümelenmiş mi?
      
      Bu koşulların hiçbiri doğru değilse, denetleyici en son günlük yedeklemeyi arar (Sürücü S'deki **nonDOMstorage'da** bulunur). Denetleyici, VHD'nin en son anlık görüntüsünü yedeklemeden kopyalar.
2. Yuva 0'daki denetleyici, anlık görüntüyü görüntünün kendisini kullanır.
3. Bu arada, yuva 1'deki denetleyici, görüntülemeyi tamamlamak ve başlamak için denetleyici 0'ı bekler.
4. Denetleyici 0 başladıktan sonra, denetleyici 1 denetleyici 0 tarafından oluşturulan kümealgılar, hangi tek denetleyici değiştirme mantığı tetikler. Daha fazla bilgi için [tek denetleyici değiştirme mantığına](#single-controller-replacement-logic)bakın.
5. Daha sonra, her iki denetleyici de çalışacak ve küme çevrimiçi olacak.

> [!IMPORTANT]
> StorSimple aygıtı yapılandırıldıktan sonra çift kumanda değişiminden sonra, aygıtın manuel yedeklemesini almanız esastır. Günlük aygıt yapılandırma yedekleri, 24 saat geçmeden tetiklenmez. Cihazınızın el ile yedeklemesini yapmak için [Microsoft Destek](storsimple-8000-contact-microsoft-support.md) ile çalışın.


### <a name="dual-controller-replacement-steps"></a>Çift denetleyici değiştirme adımları
Microsoft Azure StorSimple aygıtınızdaki her iki denetleyici de başarısız olduğunda bu iş akışı gereklidir. Bu, soğutma sisteminin çalışmayı durdurduğu bir veri merkezinde gerçekleşebilir ve sonuç olarak, her iki denetleyici de kısa bir süre içinde başarısız olur. StorSimple aygıtının kapalı veya açık olup olmadığına ve 8600 veya 8100 modeli kullanıp kullanmadığınıza bağlı olarak farklı bir adım kümesi gereklidir.

> [!IMPORTANT]
> Denetleyicinin yeniden başlatılması ve çift kumanda değiştirme yordamından tamamen kurtulması 45 dakika ile 1 saat arasında sürebilir. Kabloların takılması da dahil olmak üzere tüm yordamın toplam süresi yaklaşık 2,5 saattir.

#### <a name="to-replace-both-controller-modules"></a>Her iki denetleyici modüllerini değiştirmek için
1. Aygıt kapalıysa, bu adımı atlayın ve bir sonraki adıma geçin. Aygıt açıksa, aygıtı kapatın.
   
   1. 8600 model kullanıyorsanız, önce birincil muhafazayı kapatın ve ardından EBOD kasasını kapatın.
   2. Aygıt tamamen kapanana kadar bekleyin. Cihazın arka sideki tüm LED'ler kapalı olacak.
2. Veri bağlantı noktalarına bağlı tüm ağ kablolarını kaldırın. 8600 model kullanıyorsanız, birincil muhafazayı EBOD kasasına bağlayan SAS kablolarını da çıkarın.
3. StorSimple aygıtından her iki denetleyiciyi de çıkarın. Daha fazla bilgi için [bkz.](#remove-a-controller)
4. Önce Controller 0 için fabrika yerine yerleştirin ve ardından Denetleyici 1'i takın. Daha fazla bilgi için [bir denetleyici ekleyin'](#insert-a-controller)e bakın. Bu çift denetleyici değiştirme mantığını tetikler. Daha fazla bilgi için [çift denetleyici değiştirme mantığına](#dual-controller-replacement-logic)bakın.
5. Denetleyici değiştirme mantığı arka planda ilerlerken, kabloları yeniden bağlayın. Tüm kabloları değiştirmeden önce bağlandıkları şekilde bağlamaya özen tinler. [StorSimple 8100 cihazınızı veya StorSimple](storsimple-8100-hardware-installation.md) [8600 cihazınızı yükleyin](storsimple-8600-hardware-installation.md)Kablo cihazınız bölümünde modeliniz için ayrıntılı talimatları görün.
6. StorSimple cihazını açın. 8600 model kullanıyorsanız:
   
   1. Önce EBOD kasasının açık olduğundan emin olun.
   2. EBOD muhafazası çalışana kadar bekleyin.
   3. Birincil muhafazayı açın.
   4. İlk denetleyici yeniden başlatıldıktan ve sağlıklı bir durumda olduktan sonra, sistem çalışıyor olur.
      
      > [!NOTE]
      > Aygıtı seri konsoldan izliyorsanız, denetleyici değiştirme yordamından kurtulurken birden çok yeniden başlatma görebilirsiniz. Seri konsol menüsü göründüğünde, değiştirmenin tamamladığını bilirsiniz. Menü denetleyici değişimini başladından sonraki 2,5 saat içinde görünmüyorsa, lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
     
## <a name="remove-a-controller"></a>Denetleyiciyi kaldırma
StorSimple cihazınızdan hatalı bir denetleyici modülünü kaldırmak için aşağıdaki yordamı kullanın.

> [!NOTE]
> Aşağıdaki çizimler denetleyici 0 içindir. Denetleyici 1 için bunlar tersine çevrilebilir.


#### <a name="to-remove-a-controller-module"></a>Denetleyici modüllerini kaldırmak için
1. Başparmağınız ve işaret parmağınız arasındaki modül mandalını kavrayın.
2. Kumanda mandalıserbest bırakmak için başparmağınızı ve işaret parmağınızı hafifçe sıkın.
   
    ![Serbest denetleyici mandal](./media/storsimple-controller-replacement/IC741047.png)
   
    **Şekil 2** Serbest denetleyici mandal
3. Kumandayı kasadan çıkarmak için mandal'ı tutamaç olarak kullanın.
   
    ![Şasi dışında kayan kumanda](./media/storsimple-controller-replacement/IC741048.png)
   
    **Şekil 3** Kumandayı şasiden kaydırma

## <a name="insert-a-controller"></a>Denetleyici ekleme
StorSimple cihazınızdan hatalı bir modülü çıkardıktan sonra fabrikatarafından sağlanan bir denetleyici modülünü yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-install-a-controller-module"></a>Denetleyici modülü yüklemek için
1. Arabirim konektörlerinde herhangi bir hasar olup olmadığını kontrol edin. Konektör pimlerinden herhangi biri hasar görmüş veya bükülmüşse modülü yüklemeyin.
2. Mandal tamamen serbest bırakılırken kumanda modüllerini şasiye kaydırın.
   
    ![Şasiye sürgülü kumanda](./media/storsimple-controller-replacement/IC741053.png)
   
    **Şekil 4** Şasiye sürgülü kumanda
3. Kumanda modülü takılıyken, kumanda modüllerini şasiye itmeye devam ederken mandalını kapatmaya başlayın. Mandal, kumandayı yerine getirmek için devreye girer.
   
    ![Kapatma kontrol mandalı](./media/storsimple-controller-replacement/IC741054.png)
   
    **Şekil 5** Denetleyici mandalı kapatma
4. Mandal yerine yapıştığında bitirebilirsin. **OK** LED şimdi açık olmalıdır.
   
   > [!NOTE]
   > Kumandanın ve LED'in etkinleştirilmesi 5 dakika kadar sürebilir.
  
5. Azure portalında değiştirmenin başarılı olduğunu doğrulamak için cihazınıza gidin ve ardından**Donanım sağlığını** **İzle'ye** > gidin ve hem denetleyici 0 hem de denetleyici 1'in sağlıklı olduğundan emin olun (durum yeşil).

## <a name="identify-the-active-controller-on-your-device"></a>Cihazınızdaki etkin denetleyiciyi belirleme
İlk kez cihaz kaydı veya denetleyici değiştirme gibi, etkin denetleyiciyi StorSimple aygıtında bulmanızı gerektiren birçok durum vardır. Etkin denetleyici, tüm disk firmware ve ağ işlemlerini işler. Etkin denetleyiciyi tanımlamak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* [Etkin denetleyiciyi tanımlamak için Azure portalını kullanın](#use-the-azure-portal-to-identify-the-active-controller)
* [Etkin denetleyiciyi tanımlamak için StorSimple için Windows PowerShell'i kullanın](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Etkin denetleyiciyi tanımlamak için fiziksel aygıtı kontrol edin](#check-the-physical-device-to-identify-the-active-controller)

Bu yordamların her biri sonraki açıklanır.

### <a name="use-the-azure-portal-to-identify-the-active-controller"></a>Etkin denetleyiciyi tanımlamak için Azure portalını kullanın
Azure portalında cihazınıza gidin ve ardından**Donanım durumunu** **izleyin** > ve **Denetleyiciler** bölümüne gidin. Burada hangi denetleyicinin etkin olduğunu doğrulayabilirsiniz.

![Azure portalında etkin denetleyiciyi belirleme](./media/storsimple-controller-replacement/IC752072.png)

**Şekil 6** Etkin denetleyiciyi gösteren Azure portalı

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Etkin denetleyiciyi tanımlamak için StorSimple için Windows PowerShell'i kullanın
Cihazınıza seri konsoldan erişdiğinizde, bir banner iletisi sunulur. Banner iletisi, model, ad, yüklü yazılım sürümü ve erişebildiğiniz denetleyicinin durumu gibi temel aygıt bilgilerini içerir. Aşağıdaki resim bir banner iletisi örneğini gösterir:

![Seri afiş iletisi](./media/storsimple-controller-replacement/IC741098.png)

**Şekil 7** Denetleyici 0'ı Etkin olarak gösteren banner iletisi

Bağlı olduğunuz denetleyicinin etkin veya pasif olup olmadığını belirlemek için banner iletisini kullanabilirsiniz.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Etkin denetleyiciyi tanımlamak için fiziksel aygıtı kontrol edin
Cihazınızdaki etkin denetleyiciyi tanımlamak için, birincil kasanın arka tarafındaki DATA 5 bağlantı noktasının üzerindeki mavi LED'i bulun.

Bu LED yanıp sönüyorsa, denetleyici etkindir ve diğer denetleyici bekleme modundadır. Yardım olarak aşağıdaki diyagramı ve tabloyu kullanın.

![Dataportlu aygıt birincil kasa arka düzlemi](./media/storsimple-controller-replacement/IC741055.png)

**Şekil 8** Veri bağlantı noktaları ve izleme LED'leri ile birincil kasanın arkası

| Etiketle | Açıklama |
|:--- |:--- |
| 1-6 |DATA 0 – 5 ağ bağlantı noktaları |
| 7 |Mavi LED |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşen değiştirme](storsimple-8000-hardware-component-replacement.md)hakkında daha fazla bilgi edinin.

