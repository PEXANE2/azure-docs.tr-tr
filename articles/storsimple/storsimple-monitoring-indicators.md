---
title: StorBasit izleme göstergeleri | Microsoft Dokümanlar
description: StorSimple cihazının durumunu izlemek için kullanılan ışık yayan diyotlar (LED'ler) ve sesli alarmları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: ef8acf1c3c9211168ebacc8d62647f6789c745a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60630657"
---
# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Cihazınızı yönetmek için StorBasit izleme göstergelerini kullanın


## <a name="overview"></a>Genel Bakış
StorSimple cihazınız, StorSimple cihazının modüllerini ve genel durumunu izlemek için kullanabileceğiniz ışık yayan diyotlar (LED'ler) ve alarmlar içerir. İzleme göstergeleri, cihazın birincil kasasının ve EBOD kasasının donanım bileşenlerinde bulunabilir. İzleme göstergeleri LED veya duyulabilir alarmlar olabilir.

Bir modülün durumunu belirtmek için kullanılan üç LED durumu vardır: yeşil, yanıp sönen yeşilden kırmızı kehribara veya kırmızı kehribara.  

* Yeşil LED'ler sağlıklı bir çalışma durumunu temsil eder.  
* Yeşilden kırmızı kehribar LED'lere yanıp sönen, kullanıcı müdahalesi gerektirebilecek kritik olmayan koşulların varlığını temsil eder.  
* Kırmızı-kehribar LED'ler modül içinde kritik bir hata mevcut olduğunu gösterir.  

Bu makalenin geri kalanı çeşitli izleme göstergesi LED'leri, StorSimple aygıtındaki konumlarını, LED durumlarına dayalı aygıt durumunu ve ilişkili sesli alarmları açıklar.

## <a name="front-panel-indicator-leds"></a>Ön panel göstergesi LED'ler
*Operasyon paneli* veya *operasyon paneli*olarak da bilinen ön panel, sistemdeki tüm modüllerin toplam durumunu görüntüler. Ön panel StorSimple birincil ve EBOD muhafaza aynıdır ve aşağıda gösterilmiştir.  

   ![Cihaz ön paneli][1]

Ön panel aşağıdaki göstergeleri içerir:  

1. Sessiz düğmesi
2. Güç göstergesi LED (yeşil/kırmızı-kehribar)
3. Modül arıza göstergesi LED (ACI kırmızı-amber/OFF)
4. Mantıksal hata göstergesi LED (ABEYAZRİ/KAPALI
5. Birim kimlik ekranı  

Cihaz için ön panel LED'leri ile EBOD muhafazası arasındaki en büyük fark, LED ekranda gösterilen **Sistem Birimi Tanımlama Numarasıdır.** Cihazda görüntülenen varsayılan birim kimliği **00'dır,** EBOD kasası üzerinde görüntülenen varsayılan birim kimliği **ise 01'dir.** Bu, cihaz açıkken cihaz ve EBOD muhafazası arasında hızlı bir şekilde ayrım yapmanızı sağlar. Aygıtınız kapalıysa, Cihazı EBOD kasasından ayırt etmek için [Yeni bir cihazı Aç'ta](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) verilen bilgileri kullanın.  

## <a name="front-panel-led-status"></a>Ön panel LED durumu
Cihazın veya EBOD kasasının ön panelindeki LED'ler tarafından belirtilen durumu belirlemek için aşağıdaki tabloyu kullanın.  

| Sistem gücü | Modül hatası | Mantıksal hata | Alarm | Durum |
| --- | --- | --- | --- | --- |
| Kırmızı-kehribar |KAPALI |KAPALI |Yok |AC güç kaybı, yedek güç veya AC güç ON ve denetleyici modülleri üzerinde çalışan kaldırıldı. |
| Yeşil |AÇIK |AÇIK |Yok |Ops paneli gücü (5s) test durumu |
| Yeşil |KAPALI |KAPALI |Yok |Güç, tüm fonksiyonlar iyi |
| Yeşil |AÇIK |Yok |PCM arıza LED'leri, fan arızası LED'leri |Herhangi bir PCM arızası, fan arızası, aşırı veya düşük sıcaklık |
| Yeşil |AÇIK |Yok |G/Ç modül LED'leri |Herhangi bir denetleyici modül hatası |
| Yeşil |AÇIK |Yok |Yok |Muhafaza mantık hatası |
| Yeşil |Flash |Yok |Denetleyici modülünde modül durumu LED. PCM arıza LED'leri, fan arızası LED'leri |Bilinmeyen denetleyici modül tipi yüklü, I2C veri otobüsü arızası, denetleyici modülü hayati ürün verileri (VPD) yapılandırma hatası |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Güç soğutma modülü (PCM) gösterge LED'leri
Güç soğutma modülü (PCM) gösterge LED'leri her PCM modülünde birincil kasanın veya EBOD kasasının arka tarafında bulunabilir. Bu konu, StorSimple cihazınızın durumunu izlemek için aşağıdaki LED'lerin nasıl kullanılacağını tartışır.  

* Birincil muhafaza için PCM LED'ler
* EBOD muhafazası için PCM LED'ler

## <a name="pcm-leds-for-the-primary-enclosure"></a>Birincil muhafaza için PCM LED'ler
StorSimple cihazı, ek bir pil içeren 764W PCM modülüne sahiptir. Aşağıdaki resimde cihazın LED paneli gösterilmektedir.  

   ![Birincil kasadaki PCM LED'leri][2]

LED efsanesi:

1. AC güç kesintisi
2. Fan arızası
3. Pil arızası
4. PCM TAMAM
5. DC hatası
6. Pil iyi  

PCM'nin durumu LED panelinde gösterilir. Cihaz PCM LED paneli altı LED vardır. Bu LED'lerden dördü güç kaynağının ve fanın durumunu gösterir. Geri kalan iki LED, PCM'deki yedek pil modülünün durumunu gösterir. PCM'nin durumunu belirlemek için aşağıdaki tabloları kullanabilirsiniz.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>Güç kaynağı ve fan için PCM gösterge LED'leri
| Durum | PCM OK (yeşil) | AC başarısız (kehribar) | Fan başarısız (kehribar) | DC başarısız (kehribar) |
| --- | --- | --- | --- | --- |
| AC gücü yok (kasaya) |KAPALI |KAPALI |KAPALI |KAPALI |
| AC gücü yok (yalnızca bu PCM) |KAPALI |AÇIK |KAPALI |AÇIK |
| AC mevcut PCM AYI - Tamam |AÇIK |KAPALI |KAPALI |KAPALI |
| PCM başarısız (fan başarısız) |KAPALI |KAPALI |AÇIK |Yok |
| PCM arızası (amp üzerinde, voltaj üzerinde, akım üzerinde) |KAPALI |AÇIK |AÇIK |AÇIK |
| PCM (fan tolerans dışında) |AÇIK |KAPALI |KAPALI |AÇIK |
| Bekleme modu |Yanıp sönen |KAPALI |KAPALI |KAPALI |
| PCM firmware indir |KAPALI |Yanıp sönen |Yanıp sönen |Yanıp sönen |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>YEDEK pil için PCM göstergesi LED'leri
| Durum | Pil iyi (yeşil) | Pil arızası (kehribar) |
| --- | --- | --- |
| Pil yok |KAPALI |KAPALI |
| Pil mevcut ve şarj |AÇIK |KAPALI |
| Akü şarj ı veya bakım deşarjı |Yanıp sönen |KAPALI |
| Pil "yumuşak" hatası (kurtarılabilir) |KAPALI |Yanıp sönen |
| Pil "sert" hatası (kurtarılamaz) |KAPALI |AÇIK |
| Pil etkisiz hale getirildi |Yanıp sönen |KAPALI |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>EBOD muhafazası için PCM LED'ler
EBOD kasası 580W PCM'ye sahiptir ve ek pil yoktur. EBOD muhafazası için PCM paneli, yalnızca güç kaynakları ve fan için gösterge LED'lere sahiptir. Aşağıdaki resimde bu LED'ler gösterilmektedir.

   ![EBOD kasası üzerindeki PCM LED'leri][3] 

PCM'nin durumunu belirlemek için aşağıdaki tabloyu kullanabilirsiniz.  

| Durum | PCM OK (yeşil) | AC başarısız (kehribar) | Fan başarısız (kehribar) | DC başarısız (kehribar) |
| --- | --- | --- | --- | --- |
| AC gücü yok (kasaya) |KAPALI |KAPALI |KAPALI |KAPALI |
| AC gücü yok (yalnızca bu PCM) |KAPALI |AÇIK |KAPALI |AÇIK |
| AC mevcut PCM AYI – Tamam |AÇIK |KAPALI |KAPALI |KAPALI |
| PCM başarısız (fan başarısız) |KAPALI |KAPALI |AÇIK |X |
| PCM arızası (amp üzerinde, voltaj üzerinde, akım üzerinde |KAPALI |AÇIK |AÇIK |AÇIK |
| PCM (fan tolerans dışında) |AÇIK |KAPALI |KAPALI |AÇIK |
| Bekleme modeli |Yanıp sönen |KAPALI |KAPALI |KAPALI |
| PCM firmware indir |KAPALI |Yanıp sönen |Yanıp sönen |Yanıp sönen |

## <a name="controller-module-indicator-leds"></a>Denetleyici modül göstergesi LED'ler
StorSimple cihazı birincil denetleyici ve EBOD denetleyici modülleri için LED'ler içerir.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Birincil denetleyici için LED izleme
Aşağıdaki resimde birincil denetleyici üzerinde LED'ler belirlemenize yardımcı olur. (Tüm bileşenler yönlendirmeye yardımcı olmak için listelenmiştir.)  

   ![Led izleme - birincil denetleyici][4]

Denetleyici modülünün doğru çalışıp çalışmadığını belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="controller-indicator-leds"></a>Denetleyici göstergesi LED'leri
| Led | Açıklama |
| --- | --- |
| KIMLIK LEDI (mavi) |Modülün tanımlandığını gösterir. Mavi LED çalışan bir denetleyicide yanıp sönüyorsa, denetleyici etkin denetleyici, diğeri ise bekleme denetleyicisidir. Daha fazla bilgi için [bkz.](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device) |
| Arıza LED (kehribar) |Denetleyicideki bir hatayı gösterir. |
| TAMAM LED (yeşil) |Sabit yeşil denetleyicinin tamam olduğunu gösterir. Yanıp sönen yeşil bir denetleyici VPD yapılandırma hatası gösterir. |
| SAS aktivite LED'leri (yeşil) |Sabit yeşil, geçerli bir faaliyeti olmayan bir bağlantıyı gösterir. Yanıp sönen yeşil bağlantının devam eden bir faaliyeti olduğunu gösterir. |
| Ethernet durumu LED'leri |Sağ tarafta bağlantı/ağ etkinliği gösterir: (sabit yeşil) bağlantı etkin, (yanıp sönen yeşil) ağ etkinliği. Sol taraf ağ hızını gösterir: (sarı) 1000 Mb/s, (yeşil) 100 Mb/s, ve (KAPALI) 10 Mb/s. Bileşen modeline bağlı olarak, ağ arabirimi etkin olmasa bile bu ışık yanıp sönebilir. |
| POSTA LED'LERI |Denetleyici açık olduğunda önyükleme ilerlemesini gösterir. StorSimple aygıtı önyükleme de başarısız olursa, bu LED Microsoft Destek hata oluştu önyükleme işleminde noktası belirlemenize yardımcı olur. |

> [!IMPORTANT]
> Hata LED'i yanıyorsa, denetleyici modülünde denetleyiciyi yeniden başlatarak çözülebilecek bir sorun vardır. Denetleyiciyi yeniden başlatmak bu sorunu çözmüyorsa lütfen Microsoft Destek'e başvurun.  
> 
> 

### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>EBOD (EBOD muhafazası) için LED'lerin izlenmesi
6 Gb/s SAS EBOD denetleyicilerinin her birinde aşağıdaki resimde gösterildiği gibi durumunu gösteren LED'ler bulunur.  

  ![İzleme LED'leri - EBOD muhafazası][5]

EBOD denetleyici modülünün normal çalışıp çalışmadığını belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD denetleyici modül göstergesi LED'ler
| Durum | G/Ç modülü Tamam (yeşil) | G/Ç modül arızası (kehribar) | Ana bağlantı noktası etkinliği (yeşil) |
| --- | --- | --- | --- |
| Denetleyici modülü Tamam |AÇIK |KAPALI |- |
| Denetleyici modül hatası |KAPALI |AÇIK |- |
| Harici ana bilgisayar bağlantı bağlantısı yok |- |- |KAPALI |
| Dış ana bilgisayar bağlantı noktası bağlantısı – etkinlik yok |- |- |AÇIK |
| Dış ana bilgisayar bağlantı noktası bağlantısı - etkinlik |- |- |Yanıp sönen |
| Denetleyici modülü meta veri hatası |Yanıp sönen |- |- |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Birincil kasa ve EBOD muhafazası için disk sürücü göstergesi LED'leri
StorSimple aygıtında hem birincil kasada hem de EBOD kasasında bulunan disk sürücüleri bulunur. Her disk sürücüsü, bu bölümde açıklandığı gibi izleme göstergesi LED'leri içerir. 

Disk sürücüleri için sürücü durumu, her sürücü taşıyıcı modülünün önüne monte edilmiş yeşil bir LED ve kırmızı kehribar LED ile gösterilir. Aşağıdaki resimde bu LED'ler gösterilmektedir.

  ![Disk sürücü LED'leri][6]

Sırayla genel ön panel LED durumunu etkileyen her disk sürücüsünün durumunu belirlemek için aşağıdaki tabloyu kullanın.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>EBOD muhafazası için disk sürücü göstergesi LED'leri
| Durum | Etkinlik Tamam LED (yeşil) | Arıza LED (kırmızı-kehribar) | İlişkili ops paneli LED |
| --- | --- | --- | --- |
| Sürücü yüklü değil |KAPALI |KAPALI |None |
| Sürücü yüklü ve çalışır durumda |Etkinlikle yanıp sönme/kapatma |X |None |
| SCSI Muhafaza Hizmetleri (SES) cihaz kimlik seti |AÇIK |Yanıp sönen 1 saniye/1 saniye kapalı |None |
| SES cihazı arıza biti seti |AÇIK |AÇIK |Mantıksal hata (kırmızı) |
| Güç kontrol devresi arızası |KAPALI |AÇIK |Modül arızası (kırmızı) |

## <a name="audible-alarms"></a>Sesli alarmlar
StorSimple cihazı, hem birincil kasa hem de EBOD muhafazasıyla ilişkili sesli alarmlar içerir. Her iki muhafazanın ön panelinde (operasyon paneli olarak da bilinir) sesli bir alarm bulunur. Sesli alarm, bir hata koşulunun ne zaman olduğunu gösterir. Aşağıdaki koşullar alarmı etkinleştirecektir:  

* Fan hatası veya arıza
* Voltaj menzil dışında
* Sıcaklık koşullarında veya üzerinde
* Termal taşma
* Sistem hatası
* Mantıksal hata
* Güç kaynağı arızası
* Güç soğutma modülünün (PCM) kaldırılması  

Aşağıdaki tabloda çeşitli alarm durumları açıklanmaktadır.  

### <a name="alarm-states"></a>Alarm durumları
| Alarm durumu | Eylem | Sessiz düğmesine basılarak eylem |
| --- | --- | --- |
| S0 |Normal mod: sessiz |Bip iki kez |
| S1 |Hata modu: 1 saniye kapalı/1 saniye |S2 veya S3'e geçiş (notlara bakın) |
| S2 |Hatırlatma modu: aralıklı bip sesi |None |
| S3 |Sessiz mod: sessiz |None |
| S4 |Kritik hata modu: sürekli alarm |Kullanılamıyor: sessiz etkin değil |

> [!NOTE]
> * Alarm durumunda S1, 2 dakika içinde sessize tuşuna basmazsanız, durum otomatik olarak S2 veya S3'e geçiş yapar.  
> * Alarm, hata durumu temizlendikten sonra S1'den S4'e geri döndüğünü belirtir.  
> * Kritik hata durumu S4 başka bir durumdan girilebilir.  


Operasyon panelindeki sessiz düğmesine basarak sesli alarmı kapatabilirsiniz. Sessiz anahtarı manuel olarak çalıştırılamazsa, otomatik sessize alma işlemi iki dakika sonra gerçekleşir. Alarm kapatıldığında, bir sorunun hala var olduğunu belirtmek için kısa aralıklı arı sesleri ile çalmaya devam edecektir. Tüm sorunlar temizlendiğinde alarm sessiz olacak.

Aşağıdaki tabloda çeşitli alarm koşulları açıklanmaktadır.

### <a name="alarm-conditions"></a>Alarm koşulları
| Durum | Severity | Alarm | Ops paneli LED |
| --- | --- | --- | --- |
| PCM uyarısı – tek bir PCM'den DC güç kaybı |Hata – fazlalık kaybı yok |S1 |Modül hatası |
| PCM uyarısı – tek bir PCM'den DC güç kaybı |Hata – fazlalık kaybı |S1 |Modül hatası |
| PCM fan başarısız |Hata – fazlalık kaybı |S1 |Modül hatası |
| SBB modülü PCM arızası algılandı |Hatam |S1 |Modül hatası |
| PCM kaldırıldı |Yapılandırma hatası |None |Modül hatası |
| Kasa yapılandırma hatası |Hata – kritik |S1 |Modül hatası |
| Düşük uyarı sıcaklığı uyarısı |Uyarı |S1 |Modül hatası |
| Yüksek uyarı sıcaklığı uyarısı |Uyarı |S1 |Modül hatası |
| Sıcaklık alarmı üzerinde |Hata – kritik |S1 |Modül hatası |
| I2C veri otobüsü arızası |Hata – fazlalık kaybı |S1 |Modül hatası |
| Ops paneli iletişim hatası (I2C) |Hata – kritik |S1 |Modül hatası |
| Denetleyici hatası |Hata – kritik |S1 |Modül hatası |
| SBB arayüz modülü hatası |Hata – kritik |S1 |Modül hatası |
| SBB arabirim modülü hatası – çalışan modüller kalmaz |Hata – kritik |S4 |Modül hatası |
| SBB arayüz modülü kaldırıldı |Uyarı |None |Modül hatası |
| Sürücü güç kontrol hatası |Uyarı – sürücü gücü kaybı yok |S1 |Modül hatası |
| Sürücü güç kontrol hatası |Hata – kritik; sürücü gücü kaybı |S1 |Modül hatası |
| Sürücü kaldırıldı |Uyarı |None |Modül hatası |
| Yetersiz güç kullanılabilir |Uyarı |yok |Modül hatası |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşenleri ve durumu](storsimple-8000-monitor-hardware-status.md)hakkında daha fazla bilgi edinin.

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


