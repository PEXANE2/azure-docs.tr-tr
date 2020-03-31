---
title: StorSimple 8000 serisi donanım bileşenleri ve durumu | Microsoft Dokümanlar
description: StorSimple Device Manager hizmeti aracılığıyla StorSimple cihazınızın donanım bileşenlerini nasıl izleyeceğinizi öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: alkohli
ms.openlocfilehash: a987239669e7437a179f5f24034f4dbe45535663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254748"
---
# <a name="use-the-storsimple-device-manager-service-to-monitor-hardware-components-and-status"></a>Donanım bileşenlerini ve durumunu izlemek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış
Bu makalede, şirket içi StorSimple 8000 serisi aygıtınızdaki çeşitli fiziksel ve mantıksal bileşenler açıklanmaktadır. Ayrıca, StorSimple Device Manager hizmetinde **Durum ve donanım sağlık** bıçağını kullanarak aygıt bileşeni durumunun nasıl izlendiğini de açıklar.

**Durum ve donanım sağlık** bıçağı, tüm StorSimple aygıt bileşenlerinin donanım durumunu gösterir.

8100 için bileşenler listesi altında, açıklayan üç bölüm vardır:

* **Paylaşılan Bileşenler** – Bunlar disk sürücüleri, muhafaza, Güç ve Soğutma Modülü (PCM) bileşenleri ve PCM sıcaklığı, hat gerilimi ve hat akım sensörleri gibi denetleyicilerin bir parçası değildir.
* **Denetleyici 0 Bileşenleri** – Denetleyici 0'da yer alan denetleyici, SAS genişletici ve konektörü, denetleyici sıcaklık sensörleri ve çeşitli ağ arabirimleri gibi bileşenler.
* **Denetleyici 1 Bileşenleri** – Denetleyici 1'i oluşturan bileşenler, Denetleyici 0 için ayrıntılı olanlara benzer.

8600 aygıtı, Genişletilmiş Disk Demeti (EBOD) muhafazasına karşılık gelen ek bileşenlere sahiptir. Bileşenler listesinin altında beş bölüm vardır. Bunlardan, birincil kasadaki bileşenleri içeren ve 8100 için açıklananbileşenlerle aynı üç bölüm vardır. EBOD muhafazası için aşağıdakileri açıklayan iki ek bölüm vardır:

* **EBOD Controller 0 Bileşenleri** – EBOD denetleyici, SAS genişletici ve konektörü ve kontrol sıcaklık sensörleri gibi EBOD muhafaza 0'da yer alan bileşenler.
* **EBOD Controller 1 Bileşenleri** – EBOD muhafaza 1'i oluşturan bileşenler, EBOD muhafaza0 için ayrıntılı olanlara benzer.
* **EBOD muhafazaSı Paylaşılan Bileşenler** – EBOD muhafazasında ve PCM'de bulunan ve EBOD denetleyicisinin parçası olmayan bileşenler.

> [!NOTE]
> **Donanım durumu StorSimple Cloud Appliance (8010/8020) için kullanılamaz.**


## <a name="monitor-the-hardware-status"></a>Donanım durumunu izleme
Aygıt bileşeninin donanım durumunu görüntülemek için aşağıdaki adımları gerçekleştirin:

1. **Aygıtlara**gidin, belirli bir StorSimple aygıtını seçin. Donanım **>'na**gidin.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health1.png)

2. Donanım **bileşenleri** bölümünü bulun ve kullanılabilir bileşenlerarasından seçim yapın. Listeyi genişletmek ve çeşitli aygıt bileşenlerinin durumunu görüntülemek için bileşen etiketini tıklatmanız yeterlidir. Birincil [muhafaza için ayrıntılı bileşen listesine](#component-list-for-primary-enclosure-of-storsimple-device) ve [EBOD muhafazası için ayrıntılı bileşen listesine](#component-list-for-ebod-enclosure-of-storsimple-device)bakın.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health2.png)

3. Bileşen durumunu yorumlamak için aşağıdaki renk kodlama düzenini kullanın:
   
   * **Yeşil onay** – **Ok** durumu olan sağlıklı bir bileşeni gösterir.
   * **Sarı** – **Uyarı** durumunda bozulmuş bir bileşeni gösterir.
   * **Kırmızı ünlem** – **Hata** durumu olan başarısız bir bileşeni belirtir.
   * **Siyah metinli beyaz** – Var olmayan bir bileşeni gösterir.
   
   Aşağıdaki ekran **görüntüsü, Tamam,** **Uyarı**ve **Hata** durumunda bileşenleri olan bir aygıtı gösterir.
       
   ![](./media/storsimple-8000-monitor-hardware-status/hw-health3.png)

   **Paylaşılan bileşenler listesini**genişletilerken, NVRAM ve kümenin bozulduğunu görebiliriz.

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health5.png)

   **Denetleyici 1 bileşenleri** listesini genişletinin, küme düğümünün başarısız olduğunu görebiliriz.  

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health4.png)  

4. **Sağlıklı** durumda olmayan bir bileşenle karşılaşırsanız, Microsoft Destek'e başvurun. Cihazınızda uyarılar etkinse, bir e-posta uyarısı alırsınız. Başarısız bir donanım bileşenini değiştirmeniz gerekiyorsa, [Bkz. StorSimple donanım bileşeni değiştirme.](storsimple-hardware-component-replacement.md)

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>StorSimple cihazının birincil muhafazası için bileşen listesi
Aşağıdaki tablo, şirket içi StorSimple cihazınızın birincil kasasında (hem 8100 hem de 8600'de mevcut) bulunan fiziksel ve mantıksal bileşenleri özetlemektedir.

| Bileşen | Modül | Tür | Konum | Alan değiştirilebilir birim (FRU)? | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Yuvada sürücü [0-11] |Disk Sürücüleri |Fiziksel |Paylaşımlı |Evet |Birincil kasadaki SSD veya HDD sürücülerin her biri için bir satır sunulur. |
| Ortam sıcaklığı sensörü |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Şasi içindeki sıcaklığı ölçer. |
| Orta düzlem sıcaklık sensörü |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Orta uçağın sıcaklığını ölçer. |
| Sesli alarm |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Şasi içindeki sesli alarm alt sisteminin işlevsel olup olmadığını gösterir. |
| Muhafaza |Muhafaza |Fiziksel |Paylaşımlı |Evet |Bir şasinin varlığını gösterir. |
| Kasa ayarları |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Şasinin ön panelianlamına gelir. |
| Hat gerilim sensörleri |PCM |Fiziksel |Paylaşımlı |Hayır |Çok sayıda hat gerilim icüllerinin durumları görüntülenir, bu da ölçülen gerilimin tolerans içinde olup olmadığını gösterir. |
| Hat akım sensörleri |PCM |Fiziksel |Paylaşımlı |Hayır |Çok sayıda hat akım sensörleri, ölçülen akımın tolerans içinde olup olmadığını gösteren durumları görüntülenir. |
| PCM'de sıcaklık sensörleri |PCM |Fiziksel |Paylaşımlı |Hayır |Giriş ve Hotspot sensörleri gibi çok sayıda sıcaklık sensörü, ölçülen sıcaklığın tolerans dahilinde olup olmadığını gösteren bir durum sergilenebilmektedir. |
| Güç kaynağı [0-1] |PCM |Fiziksel |Paylaşımlı |Evet |Aygıtın arka yerinde bulunan iki PCM'deki güç kaynaklarının her biri için bir satır sunulur. |
| Soğutma [0-1] |PCM |Fiziksel |Paylaşımlı |Evet |İki PCM'de bulunan dört soğutma fanının her biri için bir satır sunulur. |
| Pil [0-1] |PCM |Fiziksel |Paylaşımlı |Evet |PCM'de bulunan yedek pil modüllerinin her biri için bir satır sunulur. |
| Metis |Yok |Mantıksal |Paylaşımlı |Yok |Pillerin durumunu görüntüler: şarj gerek olup olmadığı ve ömrünün sonuna yaklaşıp yaklaşmadıkları. |
| Küme |Yok |Mantıksal |Paylaşımlı |Yok |İki tümleşik denetleyici modülü arasında oluşturulan kümenin durumunu görüntüler. |
| Küme düğümü |Yok |Mantıksal |Paylaşımlı |Yok |Denetleyicinin kümenin bir parçası olarak durumunu gösterir. |
| Küme çoğunluğu |Yok |Mantıksal | |Yok |HDD depolama havuzunda çoğunluk disk üyeliğinin varlığını gösterir. |
| HDD veri alanı |Yok |Mantıksal |Paylaşımlı |Yok |Sabit disk sürücüsü (HDD) depolama havuzundaki veriler için kullanılan depolama alanı. |
| HDD yönetim alanı |Yok |Mantıksal |Paylaşımlı |Yok |Yönetim görevleri için HDD depolama havuzunda ayrılmış alan. |
| HDD çoğunluk alanı |Yok |Mantıksal |Paylaşımlı |Yok |Küme çoğunluğu için HDD depolama havuzunda ayrılmış alan. |
| HDD değiştirme alanı |Yok |Mantıksal |Paylaşımlı |Yok |Denetleyici değişimi için HDD depolama havuzunda ayrılmış alan. |
| SSD veri alanı |Yok |Mantıksal |Paylaşımlı |Yok |Katı hal sürücüsü (SSD) depolama havuzunda veri için kullanılan depolama alanı. |
| SSD NVRAM alanı |Yok |Mantıksal |Paylaşımlı |Yok |NVRAM mantığına adanmış SSD depolama havuzundaki depolama alanı. |
| HDD depolama havuzu |Yok |Mantıksal |Paylaşımlı |Yok |Aygıt HDD'lerinden oluşturulan mantıksal depolama havuzunun durumunu görüntüler. |
| SSD depolama havuzu |Yok |Mantıksal |Paylaşımlı |Yok |Aygıt SSD'lerinden oluşturulan mantıksal depolama havuzunun durumunu görüntüler. |
| Denetleyici [0-1] [durum] |G/Ç |Fiziksel |Denetleyicisi |Evet |Denetleyicinin durumunu ve şasi içinde etkin veya bekleme modunda olup olmadığını görüntüler. |
| Denetleyicideki sıcaklık sensörleri |G/Ç |Fiziksel |Denetleyicisi |Hayır |G/Ç modülü, CPU sıcaklığı, DIMM ve PCIe sensörleri gibi çok sayıda sıcaklık sensörü, karşılaşılan sıcaklığın tolerans dahilinde olup olmadığını gösteren durumları görüntülenmiştir. |
| SAS genişletici |G/Ç |Fiziksel |Denetleyicisi |Hayır |Entegre depolamayı denetleyiciye bağlamak için kullanılan seri ekli SCSI (SAS) genişleticinin durumunu gösterir. |
| SAS konektörü [0-1] |G/Ç |Fiziksel |Denetleyicisi |Hayır |Tümleşik depolamayı SAS genişleticisine bağlamak için kullanılan her SAS konektörünün durumunu gösterir. |
| SBB orta düzlem arası bağlantı |G/Ç |Fiziksel |Denetleyicisi |Hayır |Her denetleyiciyi orta düzleme bağlamak için kullanılan orta düzlem konektörünün durumunu gösterir. |
| İşlemci çekirdeği |G/Ç |Fiziksel |Denetleyicisi |Hayır |Her denetleyicideki işlemci çekirdeklerinin durumunu gösterir. |
| Muhafaza elektroniği gücü |G/Ç |Fiziksel |Denetleyicisi |Hayır |Kasa tarafından kullanılan güç sisteminin durumunu gösterir. |
| Muhafaza elektronik tanılama |G/Ç |Fiziksel |Denetleyicisi |Hayır |Denetleyici tarafından sağlanan tanılama alt sistemlerinin durumunu gösterir. |
| Temel Kart Yönetim Denetleyicisi (BMC) |G/Ç |Fiziksel |Denetleyicisi |Hayır |Donanım aygıtını sensörler aracılığıyla izleyen ve bağımsız bir bağlantı üzerinden sistem yöneticisiyle iletişim kuran özel bir servis işlemcisi olan taban tahtası yönetim denetleyicisinin (BMC) durumunu gösterir. |
| Ethernet |G/Ç |Fiziksel |Denetleyicisi |Hayır |Ağ arabirimlerinin her birinin durumunu, yani denetleyicide sağlanan yönetim ve veri bağlantı noktalarını gösterir. |
| Nvram |G/Ç |Fiziksel |Denetleyicisi |Hayır |Güç kesintisi durumunda uygulama açısından kritik bilgileri korumaya hizmet eden pil tarafından yedeklenen geçici olmayan rasgele erişim belleği NVRAM'ın durumunu gösterir. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>StorSimple cihazının EBOD muhafazası için bileşen listesi
Aşağıdaki tablo, şirket içi StorSimple cihazınızın EBOD kasasında bulunan fiziksel ve mantıksal bileşenleri (yalnızca 8600 modelde bulunur) özetlemektedir.

| Bileşen | Modül | Tür | Konum | Fru? | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Yuvada sürücü [0-11] |Disk Sürücüleri |Fiziksel |Paylaşımlı |Evet |EBOD kasasının önündeki HDD sürücülerin her biri için bir satır sunulur. |
| Ortam sıcaklığı sensörü |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Şasi içindeki sıcaklığı ölçer. |
| Orta düzlem sıcaklık sensörü |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Orta uçağın sıcaklığını ölçer. |
| Sesli alarm |Muhafaza |Fiziksel |Paylaşımlı |Hayır |Şasi içindeki sesli alarm alt sisteminin işlevsel olup olmadığını gösterir. |
| Muhafaza |Muhafaza |Fiziksel |Paylaşımlı |Evet |Bir şasinin varlığını gösterir. |
| Kasa ayarları |Muhafaza |Fiziksel |Paylaşımlı |Hayır |OPS veya şasinin ön paneli anlamına gelir. |
| Hat gerilim sensörleri |PCM |Fiziksel |Paylaşımlı |Hayır |Çok sayıda hat gerilim icüllerinin durumları görüntülenir, bu da ölçülen gerilimin tolerans içinde olup olmadığını gösterir. |
| Hat akım sensörleri |PCM |Fiziksel |Paylaşımlı |Hayır |Çok sayıda hat akım sensörleri, ölçülen akımın tolerans içinde olup olmadığını gösteren durumları görüntülenir. |
| PCM'de sıcaklık sensörleri |PCM |Fiziksel |Paylaşımlı |Hayır |Giriş ve Hotspot sensörleri gibi çok sayıda sıcaklık sensörü, ölçülen sıcaklığın tolerans dahilinde olup olmadığını gösteren durumları görüntülenmiştir. |
| Güç kaynağı [0-1] |PCM |Fiziksel |Paylaşımlı |Evet |Aygıtın arka yerinde bulunan iki PCM'deki güç kaynaklarının her biri için bir satır sunulur. |
| Soğutma [0-1] |PCM |Fiziksel |Paylaşımlı |Evet |İki PCM'de bulunan dört soğutma fanının her biri için bir satır sunulur. |
| Yerel depolama [HDD] |Yok |Mantıksal |Paylaşımlı |Yok |Aygıt HDD'lerinden oluşturulan mantıksal depolama havuzunun durumunu görüntüler. |
| Denetleyici [0-1] [durum] |G/Ç |Fiziksel |Denetleyicisi |Evet |EBOD modülündeki denetleyicilerin durumunu görüntüler. |
| EBOD'da sıcaklık sensörleri |G/Ç |Fiziksel |Denetleyicisi |Hayır |Her denetleyiciden gelen çok sayıda sıcaklık sensörü, karşılaşılan sıcaklığın tolerans dahilinde olup olmadığını gösteren bir duruma sahiptir. |
| SAS genişletici |G/Ç |Fiziksel |Denetleyicisi |Hayır |Tümleşik depolamayı denetleyiciye bağlamak için kullanılan SAS genişleticinin durumunu gösterir. |
| SAS konektörü [0-2] |G/Ç |Fiziksel |Denetleyicisi |Hayır |Tümleşik depolamayı SAS genişleticisine bağlamak için kullanılan her SAS konektörünün durumunu gösterir. |
| SBB orta düzlem arası bağlantı |G/Ç |Fiziksel |Denetleyicisi |Hayır |Her denetleyiciyi orta düzleme bağlamak için kullanılan orta düzlem konektörünün durumunu gösterir. |
| Muhafaza elektroniği gücü |G/Ç |Fiziksel |Denetleyicisi |Hayır |Kasa tarafından kullanılan güç sisteminin durumunu gösterir. |
| Muhafaza elektronik tanılama |G/Ç |Fiziksel |Denetleyicisi |Hayır |Denetleyici tarafından sağlanan tanılama alt sistemlerinin durumunu gösterir. |
| Aygıt denetleyicisine bağlantı |G/Ç |Fiziksel |Denetleyicisi |Hayır |EBOD G/Ç modülü ile aygıt denetleyicisi arasındaki bağlantının durumunu gösterir. |

## <a name="next-steps"></a>Sonraki adımlar
* Cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanmak için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)
* Bozulmuş veya başarısız durumu olan bir aygıt bileşenini gidermeniz gerekiyorsa, [StorSimple izleme göstergelerine](storsimple-monitoring-indicators.md)bakın.
* Başarısız bir donanım bileşenini değiştirmek için [Bkz. StorSimple donanım bileşeni değiştirme.](storsimple-hardware-component-replacement.md)
* Aygıt sorunlarıyla yaşamaya devam ederseniz, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

