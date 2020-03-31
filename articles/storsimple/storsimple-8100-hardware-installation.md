---
title: Microsoft Azure StorSimple 8100 cihazını yükleme
description: Yazılımı dağıtmadan ve yapılandırmadan önce StorSimple 8100 cihazınızı nasıl açabileceğinizi, rafa nasıl monte edilenleri ve kabloyla nasıl dağıtabileceğinizi açıklar.
author: alkohli
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 113b72ddf7e5d508c8a0b577d4004d4fbd83e8e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267592"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızı açma, rafa takma ve kablolama

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple 8100'unuz tek bir kasa, rafa monte edilmiş bir aygıttır. Bu öğretici, StorSimple aygıtını yapılandırmadan ve dağıtmadan önce StorSimple 8100 aygıt donanımını nasıl açabileceğinizi, rafa nasıl monte edilenleri ve kabloyla nasıl dağıtabileceğinizi açıklar.

## <a name="unpack-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızı boşaltın
Aşağıdaki adımlar, StorSimple 8100 depolama aygıtınızın ambalajını nasıl açacağınız hakkında açık ve ayrıntılı talimatlar sağlar. Cihaz tek bir kutuda gönderilir.

### <a name="prepare-to-unpack-your-device"></a>Cihazınızın ambalajını boşaltmaya hazırlanın
Cihazınızın ambalajını açmadan önce aşağıdaki bilgileri gözden geçirin.

![Uyarı](./media/storsimple-safety/IC740879.png)![Simgesi ağır](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) ağırlık simgesi **UYARI!**

1. El ile birlikte muhafazanın ağırlığını yönetmek için iki kişi olduğundan emin olun. Tamamen yapılandırılmış bir muhafaza 32 kg'a (70 lbs.) kadar ağırolabilir.
2. Kutuyu düz ve sabit bir yüzeye yerleştirin.

Ardından, cihazınızın ambalajını açmak için aşağıdaki adımları tamamlayın.

#### <a name="to-unpack-your-device"></a>Cihazınızın ambalajını açmak için
1. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalajda ciddi hasar varsa kutuyu açmayın. Aygıtın iyi çalışıyor mu olduğunu değerlendirmenize yardımcı olmak için lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
2. Kutuyu açın. Aşağıdaki resimde StorSimple cihazınızın ambalajsız görünümü gösterilmektedir.
   
     ![Depolama aygıtınızın ambalajını boşaltın](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Depolama aygıtınızın ambalajsız görünümü**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   1 |Ambalaj kutusu |
   |   2 |Alt köpük |
   |   3 |Cihaz |
   |   4 |Üst köpük |
   |   5 |Aksesuar kutusu |
3. Kutuyu açtıktan sonra aşağıdakilerin bulunduğundan emin olun:
   
   * 1 tek kasa cihazı
   * 2 güç kablosu
   * 1 crossover Ethernet kablosu
   * 2 seri konsol kablosu
   * Seri erişim için 1 seri-USB dönüştürücü
   * 1 kurcalamaya dayanıklı T10 tornavida
   * 10 GbE ağ arabirimleriyle kullanım için 4 QSFP-to-SFP+ adaptörü
   * 1 raf montaj kiti (montaj donanımlı 2 yan ray)
   * Belgelere Başlarken
     
     Yukarıda listelenen öğelerden herhangi birini almadıysanız, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

Bir sonraki adım cihazınızı rafa yerleştirmektir.

## <a name="rack-mount-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızı rafa monte edin
StorSimple 8100 depolama aygıtınızı ön ve arka direklere sahip standart 19 inç rafa yüklemek için sonraki adımları izleyin. StorSimple 8100 cihazının tek bir birincil kasası vardır.

Yükleme, her biri aşağıdaki yordamlarda tartışılan birden çok adımdan oluşur.

> [!IMPORTANT]
> StorSimple cihazları düzgün çalışma için rafa monte edilmelidir.
> 
> 

### <a name="prepare-the-site"></a>Siteyi hazırlayın
Cihaz, hem ön hem de arka direklere sahip standart 19 inç lik bir rafa monte edilmelidir. Raf kurulumuiçin hazırlanmak için aşağıdaki yordamı kullanın.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Raf kurulumu için site hazırlamak için
1. Cihazın düz, sabit ve dengeli bir çalışma yüzeyi (veya benzeri) üzerinde güvenli bir şekilde durduğundan emin olun.
2. Kurmak istediğiniz sitenin bağımsız bir kaynaktan veya kesintisiz güç kaynağına (UPS) sahip bir raf güç dağıtım ünitesinden (PDU) standart AC gücüne sahip olduğunu doğrulayın.
3. Cihazı monte etmeyi istediğiniz rafta bir adet 2U yuvası olduğundan emin olun.

![Uyarı](./media/storsimple-safety/IC740879.png)![Simgesi ağır](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) ağırlık simgesi **UYARI!**

Aygıt kurulumını el ile yönetiyorsanız, ağırlığı yönetmek için kullanılabilir iki kişi olduğundan emin olun. Tamamen yapılandırılmış bir muhafaza 32 kg'a (70 lbs.) kadar ağırolabilir.

### <a name="rack-prerequisites"></a>Raf ön koşulları
8100 kasa, aşağıdakilerle standart 19 inç raf lı kasaya montaj için tasarlanmıştır:

* Raf sonrası sonrası için raf sonrası en az 27,84 inç derinlik.
* Cihaz için maksimum 32 kg ağırlık
* Maksimum geri basınç 5 Pascal (0.5 mm su göstergesi).

### <a name="rack-mounting-rail-kit"></a>Raf montaj ray kiti
19 inç raf lı kabinile kullanılmak üzere bir dizi montaj rayı sağlanmaktadır. Raylar maksimum muhafaza ağırlığını işlemek için test edilmiştir. Bu raylar aynı zamanda raf içinde yer kaybı olmadan birden fazla muhafazakurulumu sağlayacaktır.

#### <a name="to-install-the-device-on-the-rails"></a>Cihazı raylara yüklemek için
1. Bu adımı yalnızca cihazınıza iç raylar yüklenmezse gerçekleştirin. Tipik olarak, iç raylar fabrikada kurulur. Raylar monte edilmezse, sol raylı ve sağ raylı kaydırakları kasa nın kenarlarına tötürün. Her iki tarafta altı metrik vida kullanarak takmak. Oryantasyon ile yardımcı olmak için, demiryolu slaytlar **LH** işaretlenir - Ön ve **RH – Ön**, ve muhafazanın arkasına doğru yapıştırılmış sonunda konik bir ucu vardır.<br/>
   
    ![Demiryolu kaydıraklarının kasa şasisine takılması](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **İç ray lı slaytların muhafazanın kenarlarına takılması**
   
    Etiketle | Açıklama
    ----- | -----------
    1     | M 3x4 düğme başlı vidalar
    2     | Şasi slaytları

2. Dış sol ray ve dış sağ ray montajlarını raf dolabı dikey üyelerine takın. Köşeli ayraçlar **LH**, **RH**ve bu tarafı doğru yönlendirme ile size rehberlik etmek **için işaretlenir.**
3. Ray tertibatının ön ve arkasındaki ray pimlerini bulun. Rayı raf direkleri arasına sığacak şekilde genişletin ve pimleri ön ve arka raf sonrası dikey üye deliklerine takın. Ray montajının düz olduğundan emin olun.
4. Ray tertibatını raf dikey üyelerine sabitlemek için sağlanan metrik vidalardan ikisini kullanın. Öndebir vida, arkada bir vida kullanın.
5. Diğer demiryolu montajı için bu adımları tekrarlayın.<br/>
   
     ![Demiryolu kaydıraklarının raf dolabına takılması](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Dış ray montajlarının rafa takılması**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   1 |Sıkma vidası |
   |   2 |Kare delikli ön raf sonrası vida |
   |   3 |Sol ray ön konum pimleri |
   |   4 |Sıkma vidası |
   |   5 |Sol raylı arka konum pimleri |

### <a name="mounting-the-device-in-the-rack"></a>Cihazı rafa monte etmek
Yeni yüklenen raf raylarını kullanarak, cihazı rafa monte etmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-the-device"></a>Cihazı takmak için
1. Bir asistanla, muhafazayı kaldırın ve raf raylarıyla hizala.
2. Cihazı raylara dikkatlice takın ve ardından cihazı raf dolabına tamamen itin.<br/>
   
    ![Rafa cihaz ekleme](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Cihazı rafa monte etmek**
3. Kapakları serbest bırakarak sol ve sağ ön flanş kapakları çıkarın. Flanş kapakları sadece flanşlar üzerine yapışır.
4. Her flanş, sol ve sağ aracılığıyla phillips başlı vida sağlanan bir yükleyerek raf muhafaza güvenli.
5. Flanş kapaklarını pozisyona bastırıp yerine koyarak yerleştirin.<br/>
   
     ![Flanş kapaklarının yüklenmesi](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Flanş kapaklarının yüklenmesi**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   1 |Kasa sabitleme vidası |

Bir sonraki adım, cihazınızı güç, ağ ve seri erişim için kablolamaktır.

## <a name="cable-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızın kablolarını bağlama
Aşağıdaki yordamlar, StorSimple 8100 cihazınızı güç, ağ ve seri bağlantılar için nasıl kablolayacağınızı açıklar.

### <a name="prerequisites"></a>Ön koşullar
Cihazınızın kablolarını bağlamaya başlamadan önce şunlara ihtiyacınız olacaktır:

* Depolama aygıtınız tamamen ambalajsız ve rafa monte edilmiş.
* Cihazınızla birlikte gelen 2 güç kablosu
* Erişim 2 Güç Dağıtım Birimleri (önerilir).
* Ağ kabloları
* Sağlanan seri kablolar
* Pc'nizde uygun sürücü yüklü seri USB dönüştürücü (gerekirse)
* 10 GbE ağ arabirimleriyle kullanım için 4 QSFP-to-SFP+ bağdaştırıcısı sağlandı
* [StorSimple cihazınızdaki 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Güç kablolama
Cihazınızyedek Güç ve Soğutma Modülleri (PCM) içerir. Yüksek kullanılabilirlik sağlamak için her iki PCM'nin de yüklü ve farklı güç kaynaklarına bağlanması gerekir.

Cihazınızı güç için kablolamak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Ağ kablolama
Cihazınız etkin bekleme yapılandırmasıdır: herhangi bir zamanda, bir denetleyici modülü etkindir ve diğer denetleyici modülü beklemedeyken tüm disk ve ağ işlemlerini işliyor. Denetleyici başarısız olursa, bekleme denetleyicisi hemen etkinleştirilir ve tüm disk ve ağ işlemlerine devam eder.

Bu yedek denetleyici nin başarısız olmasını desteklemek için, aygıt ağınızı aşağıdaki adımlarda açıklandığı gibi kabloyla kablolamalısınız.

#### <a name="to-cable-for-network-connection"></a>Ağ bağlantısı için kabloya
1. Cihazınızın her denetleyicide altı ağ arabirimi vardır: dört 1 Gbps ve iki adet 10 Gbps Ethernet bağlantı noktası. Cihazınızın arka düzlemindeki çeşitli veri bağlantı noktalarını tanımlayın.
   
    ![8100 cihazın arka düzlemi](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Veri bağlantı noktalarını gösteren aygıtın arkası**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   0,1,4,5 |1 GbE ağ arabirimleri |
   |   2,3 |10 GbE ağ arabirimleri |
   |   6 |Seri bağlantı noktaları |
2. Ağ kablolama için aşağıdaki diyagrama bakın. (Minimum ağ yapılandırması düz mavi çizgilerle gösterilir. Yüksek kullanılabilirlik ve performans için gereken ek yapılandırma noktalı çizgilerle gösterilir.)

    ![Ağ için 2U cihazınızı kablolayın](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Cihazınız için ağ kablolama**

   |Etiketle | Açıklama |
   |----- | ----------- |
   | A    | Internet erişimi olan LAN |
   | B    | Denetleyici 0 |
   | C    | PCM 0 |
   | D    | Denetleyici 1 |
   | E    | PCM 1 |
   | F, G | Ana bilgisayarlar |
   | 0-5  | Ağ arabirimleri |



Aygıtı kablolarken, minimum yapılandırma şunları gerektirir:

* Her kumandada biri bulut erişimi, diğeri iSCSI olmak için en az iki ağ arabirimi bağlanır. DATA 0 bağlantı noktası otomatik olarak etkinleştirilir ve aygıtın seri konsolu üzerinden yapılandırılır. DATA 0'ın dışında, azure klasik portalı aracılığıyla başka bir veri bağlantı noktasının da yapılandırılması gerekir. Bu durumda, DATA 0 bağlantı noktasını birincil LAN'a (Internet erişimi olan ağ) bağlayın. Diğer veri bağlantı noktaları, istenen role bağlı olarak ağın SAN/iSCSI LAN (VLAN) bölümüne bağlanabilir.
* Bir denetleyici başarısız olursa kullanılabilirliği sağlamak için aynı ağa bağlı her denetleyicide aynı arabirimler. Örneğin, denetleyicilerden biri için DATA 0 ve DATA 3'ü bağlamayı seçerseniz, ilgili DATA 0 ve DATA 3'ü diğer denetleyiciye bağlamanız gerekir.

Yüksek kullanılabilirlik ve performans için aklınızda bulunsun:

* Mümkün olduğunda, bulut erişimi için bir çift ağ arabirimi (1 GbE) ve her bir denetleyicide iSCSI (10 GbE önerilir) için başka bir çift yapılandırın.
* Mümkün olduğunda, bir anahtar hatasına karşı kullanılabilirliği sağlamak için her denetleyiciden iki farklı anahtara ağ arabirimlerini bağlayın. Şekil iki farklı anahtara bağlı her denetleyiciden iki 10 GbE ağ arabirimi, DATA 2 ve DATA 3'ünü göstermektedir.

Daha fazla bilgi [için, StorSimple aygıtınız için Yüksek kullanılabilirlik gereksinimleri](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple)altında **Ağ arabirimlerine** bakın.

> [!NOTE]
> 10 GbE ağ arabirimlerinizle SFP+ alıcı-vericikullanıyorsanız, sağlanan QSFP-SFP+ bağdaştırıcılarını kullanın. Daha fazla bilgi için [StorSimple cihazınızdaki 10 GbE ağ arabirimleri için Desteklenen donanıma](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)gidin.
> 
> 

### <a name="serial-port-cabling"></a>Seri bağlantı noktası kablolama
Seri bağlantı noktasınızı kablolamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-cable-for-serial-connection"></a>Seri bağlantı için kabloya
1. Cihazınızın her denetleyicide bir anahtar simgesiyle tanımlanan bir seri bağlantı noktası vardır. Cihazınızın arka düzlemindeki seri bağlantı noktalarını bulmak için [Ağ kablolama](#network-cabling) bölümündeki çizime bakın.
2. Cihazınızın arka düzlemindeki etkin denetleyiciyi tanımlayın. Yanıp sönen mavi LED, denetleyicinin etkin olduğunu gösterir.
3. Sağlanan seri kabloları (gerekirse dizüstü bilgisayarınız için USB seri dönüştürücü) kullanın ve konsolunuzu veya bilgisayarınızı (cihaza terminal öykünme ile) etkin denetleyicinin seri bağlantı noktasına bağlayın.
4. Seri USB sürücülerini (aygıtla birlikte gönderilen) bilgisayarınıza yükleyin.
5. Seri bağlantısını aşağıdaki gibi ayarlayın: 115.200 baud, 8 veri biti, 1 stop biti, eşlik yok ve akış denetimi Yok olarak ayarlandı.
6. Konsolda Enter tuşuna basarak bağlantının çalıştığını doğrulayın. Seri konsol menüsü görünmelidir.

> [!NOTE]
> **Işıksöndürme Yönetimi**: Aygıt uzak bir veri merkezine veya sınırlı erişime sahip bir bilgisayar odasına takıldığında, her iki denetleyiciye giden seri bağlantıların her zaman bir seri konsol anahtarına veya benzer donanıma bağlı olduğundan emin olun. Bu, ağ kesintileri veya beklenmeyen hatalar olduğunda bant dışı uzaktan kumanda ve destek işlemlerine izin verir.
> 
> 

Cihazınız artık güç, ağ erişimi ve seri bağlantı için kablolu. Bir sonraki adım, yazılımı yapılandırmak ve cihazınızı dağıtmaktır.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi StorSimple aygıtınızı nasıl dağıtıp yapılandırılamayı](storsimple-8000-deployment-walkthrough-u2.md)öğrenin.

