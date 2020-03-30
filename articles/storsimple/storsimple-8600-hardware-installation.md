---
title: Microsoft Azure StorSimple 8600 cihazını yükleme
description: Yazılımı dağıtmadan ve yapılandırmadan önce StorSimple 8600 cihazınızı nasıl açabileceğinizi, rafa nasıl monte edilenleri ve kabloyla nasıl dağıtabileceğinizi açıklar.
author: alkohli
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 8d118e71b29b54b034788c37916091ad8f8b996f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254761"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>StorSimple 8600 cihazınızı açma, rafa takma ve kablolama

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple 8600'unuz çift kasa aygıtıdır ve birincil ve EBOD muhafazası ndan oluşur. Bu öğretici, StorSimple yazılımını yapılandırmadan önce StorSimple 8600 aygıt donanımını nasıl açabileceğinizi, rafa nasıl monte edilebildiğini ve kabloyla nasıl doldurabileceğinizi açıklar.

## <a name="unpack-your-storsimple-8600-device"></a>StorSimple 8600 cihazınızı boşaltın
Aşağıdaki adımlar, StorSimple 8600 depolama aygıtınızın ambalajını nasıl açacağınıza ilişkin açık ve ayrıntılı talimatlar sağlar. Bu cihaz, biri birincil kasa, diğeri EBOD muhafazası için olmak üzere iki kutu halinde sevk edilir. Bu iki kutu daha sonra tek bir kutuya yerleştirilir.

### <a name="prepare-to-unpack-your-device"></a>Cihazınızın ambalajını boşaltmaya hazırlanın
Cihazınızın ambalajını açmadan önce aşağıdaki bilgileri gözden geçirin.

![Uyarı](./media/storsimple-safety/IC740879.png)![Simgesi ağır](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) ağırlık simgesi **UYARI!**

1. Aygıtı el ile birlikte kullanıyorsanız, aygıtın ağırlığını yürütebilecek iki kişinin kullanıldığından emin olun. Tamamen yapılandırılmış bir muhafaza 32 kg'a (70 lbs.) kadar ağırolabilir.
2. Kutuyu düz ve sabit bir yüzeye yerleştirin.

Ardından, cihazınızın ambalajını açmak için aşağıdaki adımları tamamlayın.

#### <a name="to-unpack-your-device"></a>Cihazınızın ambalajını açmak için
1. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalajda ciddi hasar varsa kutuyu açmayın. Aygıtın iyi çalışıyor mu olduğunu değerlendirmenize yardımcı olmak için lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
2. Dış kutuyu açın ve birincil ve EBOD muhafazalarına karşılık gelen iki kutuyu çıkarın. Artık birincil ve EBOD muhafazalarını açabilirsiniz. Aşağıdaki şekil, muhafazalardan birinin ambalajsız görünümünü gösterir.
   
    ![Depolama aygıtınızın ambalajını boşaltın](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Depolama aygıtınızın ambalajsız görünümü**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   1 |Ambalaj kutusu |
   |   2 |SAS kabloları (aksesuar ve kablo tepsisinde) |
   |   3 |Alt köpük |
   |   4 |Cihaz |
   |   5 |Üst köpük |
   |   6 |Aksesuar kutusu |
3. İki kutuyu boşalttırdıktan sonra aşağıdakileri yaptığınızdan emin olun:
   
   * 1 adet birincil kasa (birincil kasa ve EBOD muhafazası iki ayrı kutudadır)
   * 1 EBOD muhafazası
   * 4 güç kablosu, her kutuda 2
   * 2 SAS kablosu (birincil muhafazayı EBOD kasasına bağlamak için)
   * 1 crossover Ethernet kablosu
   * 2 seri konsol kablosu
   * Seri erişim için 1 seri-USB dönüştürücü
   * 10 GbE ağ arabirimleriyle kullanım için 4 QSFP-to-SFP+ adaptörü
   * 2 raf montaj kitleri (montaj donanımı ile 4 yan raylar, birincil kasa ve EBOD muhafazası için 2'şer adet), her kutuda 1 adet
   * Dokümantasyona başlama
     
     Yukarıda listelenen öğelerden herhangi birini almadıysanız, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)  

Bir sonraki adım cihazınızı rafa yerleştirmektir.

## <a name="rack-mount-your-storsimple-8600-device"></a>StorSimple 8600 cihazınızı rafa monte edin
StorSimple 8600 depolama aygıtınızı ön ve arka direklere sahip standart 19 inç rafa yüklemek için sonraki adımları izleyin. Bu cihaz iki kasa ile birlikte gelir: birincil kasa ve bir EBOD muhafaza. Bunların her ikisinin de rafa monte edilmesi gerekir.

Yükleme, her biri aşağıdaki yordamlarda tartışılan birden çok adımdan oluşur.

> [!IMPORTANT]
> StorSimple cihazları düzgün çalışma için rafa monte edilmelidir.
> 
> 

### <a name="site-preparation"></a>Site hazırlama
Muhafazalar hem ön hem de arka direklere sahip standart 19 inç lik bir rafa monte edilmelidir. Raf kurulumuiçin hazırlanmak için aşağıdaki yordamı kullanın.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Raf kurulumu için site hazırlamak için
1. Birincil ve EBOD muhafazalarının düz, kararlı ve düz bir çalışma yüzeyinde (veya benzer) güvenle dinlendiğinden emin olun.
2. Kurmak istediğiniz sitenin bağımsız bir kaynaktan veya kesintisiz güç kaynağına (UPS) sahip bir raf Güç Dağıtım Ünitesi'nden (PDU) standart AC gücüne sahip olduğunu doğrulayın.
3. Muhafazaları monte etmeyi istediğiniz rafta bir adet 4U (2 X 2U) yuva bulunduğundan emin olun.

![Uyarı](./media/storsimple-safety/IC740879.png)![Simgesi ağır](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) ağırlık simgesi **UYARI!**

 Aygıt kurulumını el ile yönetiyorsanız, ağırlığı yönetmek için kullanılabilir iki kişi olduğundan emin olun. Tamamen yapılandırılmış bir muhafaza 32 kg'a (70 lbs.) kadar ağırolabilir.

### <a name="rack-prerequisites"></a>Raf ön koşulları
Kasalar aşağıdakilerle standart 19 inç raf lı bir kasaya montaj için tasarlanmıştır:

* Raf sonrası sonrası için raf sonrası en az 27,84 inç derinliği
* Cihaz için maksimum 32 kg ağırlık
* Maksimum geri basınç 5 Pascal (0,5 mm su ölçer)

### <a name="rack-mounting-rail-kit"></a>Raf montaj ray kiti
19 inç raf lı kabinile kullanılmak üzere bir dizi montaj rayı sağlanacaktır. Raylar maksimum muhafaza ağırlığını işlemek için test edilmiştir. Bu raylar aynı zamanda raf içinde yer kaybı olmadan birden fazla muhafazakurulumu sağlayacaktır. Önce EBOD kasasını kurun.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>EBOD kasasını raylara takmak için
1. Bu adımı yalnızca cihazınıza iç raylar yüklenmezse gerçekleştirin. Tipik olarak, iç raylar fabrikada kurulur. Raylar monte edilmezse, sol raylı ve sağ raylı kaydırakları kasa nın kenarlarına tötürün. Her iki tarafta altı metrik vida kullanarak takmak. Oryantasyon ile yardımcı olmak için, demiryolu slaytlar **LH** işaretlenir - Ön ve **RH – Ön**, ve muhafazanın arkasına doğru yapıştırılmış sonunda konik bir ucu vardır.
   
    ![Demiryolu kaydıraklarının kasa şasisine takılması](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Demiryolu kaydıraklarının muhafazanın kenarlarına takılması**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |  1 |M 3x4 düğme başlı vidalar |
   |  2 |Şasi slaytları |
2. Sol ray ve sağ ray montajlarını raf dolabı dikey üyelerine takın. Köşeli ayraçlar **LH**, **RH**ve bu tarafı doğru yönlendirme ile size rehberlik etmek **için işaretlenir.**
3. Ray tertibatının ön ve arkasındaki ray pimlerini bulun. Rayı raf direkleri arasına sığacak şekilde genişletin ve pimleri ön ve arka raf sonrası dikey üye deliklerine takın. Ray montajının düz olduğundan emin olun.
4. Sağlanan metrik vidalardan ikisini kullanarak ray tertibatını raf dikey üyelerine sabitle. Öndebir vida, arkada bir vida kullanın.
5. Diğer demiryolu montajı için bu adımları tekrarlayın.
   
     ![Demiryolu kaydıraklarının raf dolabına takılması](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Ray montajlarının rafa takılması**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   1 |Sıkma vidası |
   |   2 |Kare delikli ön raf sonrası vida |
   |   3 |Sol ön ray konum pimleri |
   |   4 |Sıkma vidası |
   |   5 |Sol arka ray konum pimleri |

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>EBOD kasasının rafa montajı
Yeni monte edilmiş raf raylarını kullanarak, EBOD kasasını rafa monte etmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-the-ebod-enclosure"></a>EBOD kasasını monte etmek için
1. Bir asistanla, muhafazayı kaldırın ve raf raylarıyla hizala.
2. Muhafazayı dikkatlice raylara takın ve raf dolabına tamamen itin.
   
    ![Rafa cihaz ekleme](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Kasanın rafa montajı**
3. Kapakları serbest bırakarak sol ve sağ ön flanş kapakları çıkarın. Flanş kapakları sadece flanşlar üzerine yapışır.
4. Her flanş, sol ve sağ aracılığıyla phillips başlı vida sağlanan bir yükleyerek raf içine muhafaza güvenli.
5. Flanş kapaklarını pozisyona bastırıp yerine koyarak yerleştirin.
   
     ![Flanş kapaklarının yüklenmesi](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Flanş kapaklarının yüklenmesi**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   1 |Kasa sabitleme vidası |

### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Birincil muhafazanın rafa montajı
EBOD muhafazasını monte etmeyi bitirdikten sonra, birincil muhafazayı aynı adımları izleyerek takmanız gerekir.

> [!NOTE]
> * Birincil kasa ile EBOD muhafazası arasında rafta birkaç boş yuva olması mümkündür.
> * Birincil muhafazayı EBOD kasasına bağlamak için sağlanan 2m SAS kablosunu kullanın.
> * Baş birimin EBOD birimine göreli yerleşimi konusunda herhangi bir kısıtlama yoktur. Bu nedenle, birincil kasa üst yuvaya ve aşağıdaki EBOD muhafazası yerleştirilebilir - ya da tam tersi.
> 
> 

Bir sonraki adım, cihazınızı güç, ağ ve seri erişim için kablolamaktır.

## <a name="cable-your-storsimple-8600-device"></a>StorSimple 8600 cihazınızın kablolarını bağlama
Aşağıdaki yordamlar, StorSimple 8600 cihazınızı güç, ağ ve seri bağlantılar için nasıl kablolayacağınızı açıklar.

### <a name="prerequisites"></a>Ön koşullar
Cihazınızı kabloya yüklemeye başlamadan önce şunları yapmanız gerekir:

* Birincil muhafazanız ve EBOD muhafazanız tamamen boşaltılmış
* Cihazınızla birlikte gelen 4 güç kablosu (birincil ve EBOD muhafazası için 2 adet)
* EBOD kasasını birincil kasaya bağlamak için cihazla birlikte verilen 2 SAS kablosu
* 2 Güç Dağıtım Ünitesine (PDA) Erişim (önerilir)
* Ağ kabloları
* Sağlanan seri kablolar
* Pc'nizde uygun sürücü yüklü seri-USB dönüştürücü (gerekirse)
* 10 GbE ağ arabirimleriyle kullanım için 4 QSFP-to-SFP+ bağdaştırıcısı sağlandı
* [StorSimple cihazınızdaki 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS ve Güç kablolama
Cihazınızda hem birincil kasa hem de EBOD muhafazası vardır. Bu, birimlerin Seri Bağlı SCSI (SAS) bağlantısı ve gücü için birbirine bağlanmasını gerektirir.

Bu aygıtı ilk kez ayarlarken, önce SAS kablolama için adımları gerçekleştirin ve ardından güç kablolama için adımları tamamlayın.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Ağ kablolama
Cihazınız etkin bekleme yapılandırmasındadır: herhangi bir zamanda, bir denetleyici modülü etkindir ve diğer denetleyici modülü beklemedeyken tüm disk ve ağ işlemlerini işler. Denetleyici hatası oluşursa, bekleme denetleyicisi hemen etkinleştirir ve tüm disk ve ağ işlemlerini sürdürür.

Bu yedek denetleyici nin başarısız olmasını desteklemek için, aygıt ağınızı aşağıdaki adımlarda gösterildiği gibi kabloyla kablolamalısınız.

#### <a name="to-cable-for-network-connection"></a>Ağ bağlantısı için kabloya
1. Cihazınızın her bir denetleyicide altı ağ arabirimi vardır: dört 1 Gbps ve iki adet 10 Gbps Ethernet bağlantı noktası. Cihazınızın arka düzlemindeki veri bağlantı noktalarını tanımlamak için aşağıdaki çizime bakın.
   
     ![8600 cihazın arka düzlemi](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Veri bağlantı noktalarını gösteren cihazınızın arkası**
   
   | Etiketle | Açıklama |
   | --- | --- |
   |   0,1,4,5 |1 GbE ağ arabirimleri |
   |   2,3 |10 GbE ağ arabirimleri |
   |   6 |Seri bağlantı noktaları |
2. Ağ kablolama için aşağıdaki diyagrama bakın. (Minimum ağ yapılandırması düz mavi çizgilerle gösterilir. Yüksek kullanılabilirlik ve performans için, gerekli ek yapılandırma noktalı çizgilerle gösterilir.)

![Ağ için 4U cihazınızı kablolayın](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Cihazınız için ağ kablolama**

| Etiketle | Açıklama |
| --- | --- |
| A |Internet erişimi olan LAN |
| B |Denetleyici 0 |
| C |PCM 0 |
| D |Denetleyici 1 |
| E |PCM 1 |
| F |EBOD denetleyici 0 |
| G |EBOD denetleyici si1 |
| H,I |Ana bilgisayarlar (örneğin, dosya sunucuları) |
| 0-5 |Ağ arabirimleri |
| 6 |Birincil muhafaza |
| 7 |EBOD muhafazası |

Aygıtı kablolarken, minimum yapılandırma şunları gerektirir:

* Her kumandada biri bulut erişimi, diğeri iSCSI olmak için en az iki ağ arabirimi bağlanır. DATA 0 bağlantı noktası otomatik olarak etkinleştirilir ve aygıtın seri konsolu üzerinden yapılandırılır. DATA 0'ın dışında, azure klasik portalı aracılığıyla başka bir veri bağlantı noktasının da yapılandırılması gerekir. Bu durumda, DATA 0 bağlantı noktasını birincil LAN'a (Internet erişimi olan ağ) bağlayın. Diğer veri bağlantı noktaları, istenen role bağlı olarak ağın SAN/iSCSI LAN (VLAN) bölümüne bağlanabilir.
* Bir denetleyici başarısız olursa kullanılabilirliği sağlamak için aynı ağa bağlı her denetleyicide aynı arabirimler. Örneğin, denetleyicilerden biri için DATA 0 ve DATA 3'ü bağlamayı seçerseniz, ilgili DATA 0 ve DATA 3'ü diğer denetleyiciye bağlamanız gerekir.

Yüksek kullanılabilirlik ve performans için aklınızda bulunsun:

* Mümkün olduğunda, bulut erişimi için bir çift ağ arabirimi (1 GbE) ve her bir denetleyicide iSCSI (10 GbE önerilir) için başka bir çift yapılandırın.
* Mümkün olduğunda, bir anahtar hatasına karşı kullanılabilirliği sağlamak için her denetleyiciden iki farklı anahtara ağ arabirimlerini bağlayın. Şekil iki farklı anahtara bağlı her denetleyiciden iki 10 GbE ağ arabirimi, DATA 2 ve DATA 3'ünü göstermektedir. Daha fazla bilgi [için, StorSimple aygıtınız için Yüksek kullanılabilirlik gereksinimleri](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple)altında **Ağ arabirimlerine** bakın.

> [!NOTE]
> 10 GbE ağ arabirimlerinizle SFP+ alıcı-vericikullanıyorsanız, sağlanan QSFP-SFP+ bağdaştırıcılarını kullanın. Daha fazla bilgi için [StorSimple cihazınızdaki 10 GbE ağ arabirimleri için Desteklenen donanıma](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)gidin.
> 
> 

### <a name="serial-port-cabling"></a>Seri bağlantı noktası kablolama
Seri bağlantı noktasınızı kablolamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-cable-for-serial-connection"></a>Seri bağlantı için kabloya
1. Cihazınızın her denetleyicide bir anahtar simgesiyle tanımlanan bir seri bağlantı noktası vardır. Seri bağlantı noktalarını bulmak için, cihazınızın arka tarafındaki veri bağlantı noktalarını gösteren çizime bakın.
2. Cihazınızın arka düzlemindeki etkin denetleyiciyi tanımlayın. Yanıp sönen mavi LED, denetleyicinin etkin olduğunu gösterir.
3. Sağlanan seri kabloyu (gerekirse dizüstü bilgisayarınız için USB seri dönüştürücü) kullanın ve konsolunuzu veya bilgisayarınızı (cihaza terminal öykünme ile) etkin denetleyicinin seri bağlantı noktasına bağlayın.
4. Seri USB sürücülerini (aygıtla birlikte gönderilen) bilgisayarınıza yükleyin.
5. Seri bağlantısını aşağıdaki gibi ayarlayın:
   
   * 115.200 baud
   * 8 veri biti
   * 1 stop biti
   * Eşitlik yok
   * Akış kontrolü **Yok** olarak ayarlandı
6. Konsolda Enter tuşuna basarak bağlantının çalıştığını doğrulayın. Seri konsol menüsü görünmelidir.

> [!NOTE]
> **Işıklar-Out Yönetimi:** Aygıt uzak bir veri merkezine veya sınırlı erişime sahip bir bilgisayar odasına yüklendiğinde, her iki denetleyiciye yapılan seri bağlantıların her zaman bir seri konsol anahtarına veya benzer donanıma bağlı olduğundan emin olun. Bu, ağ kesintisi veya beklenmeyen arızalar durumunda bant dışı uzaktan kumanda ve destek işlemlerine olanak tanır.
> 
> 

Aygıtınızı güç, ağ erişimi ve seri bağlantı için kablolamayı tamamladınız. Bir sonraki adım, cihazınızdaki yazılımı yapılandırmaktır.

## <a name="next-steps"></a>Sonraki adımlar
Artık [şirket içi StorSimple aygıtınızı dağıtmaya ve yapılandırmaya](storsimple-8000-deployment-walkthrough-u2.md)hazırsınız.

