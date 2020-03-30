---
title: StorBasit teknik özellikler | Microsoft Dokümanlar
description: StorSimple donanım bileşenleri için teknik özellikleri ve mevzuat standartlarına uygunluk bilgilerini açıklar.
services: storsimple
documentationcenter: NA
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
ms.openlocfilehash: 061194422a8c1bc449dbef0c4f04bb8e1db10dea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68965282"
---
# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>StorSimple cihazı için teknik özellikler ve uyumluluk

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

Microsoft Azure StorSimple cihazınızın donanım bileşenleri, bu makalede belirtilen teknik belirtimlere ve düzenleyici standartlara bağlıdır. Teknik özellikler Güç ve Soğutma Modüllerini (PCM'leri), disk sürücülerini, depolama kapasitesini ve muhafazaları tanımlar. Uyumluluk bilgileri uluslararası standartlar, güvenlik ve emisyonlar ve kablolama gibi şeyleri kapsar.

## <a name="power-and-cooling-module-specifications"></a>Güç ve Soğutma Modülü özellikleri

StorSimple cihazı iki adet 100-240 V çift fanlı, SBB uyumlu Güç Soğutma Modülleri (PCM) vardır. Bu, gereksiz bir güç yapılandırması sağlar. Bir PCM başarısız olursa, başarısız modül değiştirilene kadar aygıt diğer PCM'de normal şekilde çalışmaya devam eder.

EBOD kasası 580 W PCM, birincil kasa ise 764 W PCM kullanır. Aşağıdaki tablolarda PCM'lerle ilişkili teknik özellikler listelenebilmiştir.

| Belirtim | 580 W PCM (EBOD) | 764 W PCM (Birincil) |
| --- | --- | --- |
| Maksimum çıkış gücü |580 W |764 |
| Frequency |50/60 Hz |50/60 Hz |
| Gerilim aralığı seçimi |Otomatik değişen: 90 - 264 V AC, 47/63 Hz |Otomatik değişen: 90- 264 V AC, 47/63 Hz |
| Maksimum inrush akımı |20 A |20 A |
| Güç faktörü düzeltme |%95 nominal giriş gerilimi > |%95 nominal giriş gerilimi > |
| Harmonik |EN61000-3-2 ile tanışır |EN61000-3-2 ile tanışır |
| Çıktı |5V Bekleme \@ gerilimi 2.0 A |5V Bekleme \@ gerilimi 2.7 A |
| +5V \@ 42 A |+5V \@ 40 A | |
| +12V \@ 38 A |+12V \@ 38 A | |
| Sıcak takılabilir |Evet |Evet |
| Anahtarlar ve LED'ler |AC AÇI/KAPALI anahtar ve dört durum göstergesi LED'leri |AC AÇI-KAPALI anahtar ve altı durum göstergesi LED'leri |
| Kasa soğutma |Değişken fan hız kontrolüne sahip eksenel soğutma fanları |Değişken fan hız kontrolüne sahip eksenel soğutma fanları |

## <a name="power-consumption-statistics"></a>Güç tüketimi istatistikleri

Aşağıdaki tabloda, StorSimple aygıtının çeşitli modelleri için tipik güç tüketimi verileri (gerçek değerler yayımlanandan farklı olabilir) listelenir.

| Koşullar | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Fanlar yavaş, sürücüler boşta |1.45 A |0,31 kW |1057.76 BTU/saat |3.19 A |0,34 kW |1160.13 BTU/saat |
|  Fanlar yavaş, erişim sürücüler |1.54 A |0,33 kW |1126.01 BTU/saat |3.27 A |0,36 kW |1228.37 BTU/saat |
|  Fanlar hızlı, boşta sürücüler, iki PSU destekli |2.14 A |0,49 kW |1671.95 BTU/saat |4.99 A |0,54 kW |1842.56 BTU/saat |
|  Fanlar hızlı, boşta sürücüler, bir PSU boşta bir güç |2.05 A |0,48 kW |1637.83 BTU/saat |4.58 A |0,50 kW |1706.07 BTU/saat |
|  Fanlar hızlı, sürücüler erişim, iki PSU destekli |2.26 A |0,51 kW |1740.19 BTU/saat |4.95 A |0,54 kW |1842.56 BTU/saat |
|  Fanlar hızlı, sürücüler erişim, bir PSU bir boşta güç |2.14 A |0,49 kW |1671.95 BTU/saat |4.81 A |0,53 kW |1808.44 BTU/saat |

## <a name="disk-drive-specifications"></a>Disk sürücüsü özellikleri

StorSimple cihazınız 12 3,5 inç'e kadar form faktörlü Seri Bağlı SCSI (SAS) disk sürücülerini destekler. Gerçek sürücüler, ürün yapılandırmasına bağlı olarak katı hal sürücülerin (SSD'ler) veya sabit disk sürücülerin (HDD'ler) bir karışımı olabilir. 12 disk sürücü yuvası kasanın önünde 3'e 4 yapılandırmada yer alır. EBOD muhafazası, başka bir 12 disk sürücüsü için ek depolama alanı sağlar. Bunlar her zaman HDD'lerdir.

## <a name="storage-specifications"></a>Depolama özellikleri

StorSimple aygıtları, hem 8100 hem de 8600 için sabit disk sürücüleri ve katı hal sürücülerinin bir karışımına sahiptir. 8100 ve 8600 için toplam kullanılabilir kapasite sırasıyla yaklaşık 15 TB ve 38 TB vardır. Aşağıdaki tabloda StorSimple çözüm kapasitesi bağlamında SSD, HDD ve bulut kapasitesinin ayrıntıları belgelenebilme yatmaktadır.

| Cihaz modeli / Kapasite | 8100 | 8600 |
| --- | --- | --- |
| Sabit disk sürücüsü sayısı (HDD) |8 |19 |
| Katı hal sürücülerinin (SSD) sayısı |4 |5 |
| Tek HDD kapasitesi |4 TB |4 TB |
| Tek SSD kapasitesi |400 GB |800 GB |
| Yedek kapasite |4 TB |4 TB |
| Kullanılabilir HDD kapasitesi |14 TB |36 TB |
| Kullanılabilir SSD kapasitesi |800 GB |2 TB |
| Toplam kullanılabilir kapasite* |~ 15 TB |~ 38 TB |
| Maksimum çözüm kapasitesi (bulut dahil) |200 TB |500 TB |

<sup>* </sup>- *Toplam kullanılabilir kapasite, veri, meta veri ve arabellek için kullanılabilir kapasiteyi içerir. 8100 aygıtında 8,5 TB'a kadar yerel olarak sabitlenmiş hacimler veya daha büyük 8600 aygıtında 22,5 TB'a kadar sağlayabilirsiniz. Daha fazla bilgi için [StorSimple yerel olarak sabitlenmiş ciltlere](storsimple-8000-local-volume-faq.md)gidin.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Muhafaza boyutları ve ağırlık özellikleri

Aşağıdaki tablolar, boyutlar ve ağırlık için çeşitli muhafaza belirtimlerini listelemektedir.

### <a name="enclosure-dimensions"></a>Muhafaza boyutları

Aşağıdaki tabloda kasanın milimetre ve inç boyutları listelenir.

| Muhafaza | Milimetre | Inç |
| --- | --- | --- |
| Height |87.9 |3.46 |
| Montaj flanşları boyunca genişlik |483 |19.02 |
| Muhafaza gövdesi boyunca genişlik |443 |17.44 |
| Ön montaj flanşından muhafaza gövdesinin ekstremitesine kadar derinlik |577 |22.72 |
| Operasyon panelinden muhafazanın en uzak ekstremitesine kadar derinlik |630.5 |24.82 |
| Montaj flanşından muhafazanın en uzak ekstremitesine kadar derinlik |603 |23.74 |

### <a name="enclosure-weight"></a>Muhafaza ağırlığı

Yapılandırmaya bağlı olarak, tam dolu bir birincil kasa 21 ila 33 kg ağırlığında olabilir ve bunu iki kişinin işlemesini gerektirir.

| Muhafaza | Ağırlık |
| --- | --- |
| Maksimum ağırlık (yapılandırmaya bağlıdır) |30 kg - 33 kg |
| Boş (sürücü takılı değil) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Muhafaza ortamı özellikleri

Bu bölümde muhafaza ortamıyla ilgili teknik özellikler listelemektedir. Sıcaklık, nem, yükseklik, şok, titreşim, yönlendirme, güvenlik ve Elektromanyetik Uyumluluk (EMC) bu kategoriye dahildir.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem

| Muhafaza | Ortam sıcaklığı aralığı | Ortam bağıl nem | Maksimum ıslak ampul |
| --- | --- | --- | --- |
| Operasyonel |5°C - 35°C(41°F - 95°F) |%20 - %80 yoğuşma- |28°C (82°F) |
| Operasyonel olmayan |-40°C - 70°C(40°F - 158°F) |%5 - %100 yoğuşmasız |29°C (84°F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Hava akımı, irtifa, şok, titreşim, yönlendirme, güvenlik ve EMC

| Muhafaza | Operasyonel özellikler |
| --- | --- |
| Hava akımı |Sistem hava akımı önden arkaya doğru. Sistem düşük basınçlı, arka egzoz tesisatı ile çalıştırılmalıdır. Raf kapıları ve engellertarafından oluşturulan arka basınç 5 pascal'ı (0,5 mm su göstergesi) geçmemelidir. |
| Yükseklik, operasyonel |-30 metre 3045 metre (-100 feet ila 10.000 feet) maksimum çalışma sıcaklığı 7000 feet üzerinde 5 ° C tarafından de-rated ile. |
| İrtifa, operasyonel olmayan |-305 metre 12.192 metre (-1.000 feet için 40.000 feet) |
| Şok, operasyonel |5g 10 ms 1/2 sinüs |
| Şok, çalışmayan |30g 10 ms 1/2 sinüs |
| Titreşim, operasyonel |0.21g RMS 5-500 Hz rastgele |
| Titreşim, operasyonel olmayan |1.04g RMS 2-200 Hz rastgele |
| Titreşim, tehcir |3g 2-200 Hz sinüs |
| Oryantasyon ve montaj |19" raf montajı (2 ÇED ünitesi) |
| Raf rayları |IEC 297 ile uyumlu en az 700 mm (31,50 inç) derinlik raflarını sığdırmak için |
| Güvenlik ve onaylar |CE ve UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| Emc |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Uluslararası standartlara uygunluk

Microsoft Azure StorSimple cihazınız aşağıdaki uluslararası standartlara uygundur:  

* CE - EN 60950 - 1
* Cb Raporu IEC 60950 - 1
* UL ve UL 60950 için cUL - 1

## <a name="safety-compliance"></a>Güvenlik uyumluluğu

Microsoft Azure StorSimple cihazınız aşağıdaki güvenlik derecelendirmelerini karşılar:

* Sistem ürün türü onayı: UL, cUL, CE
* Güvenlik uygunluğu: UL 60950, IEC 60950, EN 60950

## <a name="emc-compliance"></a>EMC uyumluluğu

Microsoft Azure StorSimple aygıtınız aşağıdaki EMC derecelendirmelerini karşılar.

### <a name="emissions"></a>Emisyon

Cihaz, emc uyumluolup, emisyon seviyeleri ile uyumludur.

* Yürütülen emisyon sınır seviyeleri: CFR 47 Bölüm 15B Sınıf A EN55022 Sınıf A CISPR Sınıf A
* Yayılan emisyon sınır seviyeleri: CFR 47 Part 15B Class A EN55022 Sınıf A CISPR Sınıf A

### <a name="harmonics-and-flicker"></a>Harmonik ve titreme

Cihaz EN61000-3-2/3 ile uyumludur.

### <a name="immunity-limit-levels"></a>Bağışıklık limit düzeyleri

Cihaz EN55024 ile uyumludur.

## <a name="ac-power-cord-compliance"></a>AC güç kablosu uyumluluğu

Fiş ve tam güç kablosu montajı, cihazın kullanıldığı ülke/bölge için uygun standartları karşılamalı ve o ülkede/bölgede kabul edilebilir güvenlik onaylarına sahip olmalıdır. Aşağıdaki tablolarda ABD ve Avrupa standartları listeleilmektedir.

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC güç kabloları - ABD (NRTL listelenmiş olmalıdır)

| Bileşen | Belirtim |
| --- | --- |
| Kablo tipi |SV veya SVT, 18 AWG minimum, 3 iletken, 2.0 metre maksimum uzunluk |
| Fiş |NEMA 5-15P topraklama tipi eki fişi 120 V, 10 A; veya IEC 320 C14, 250 V, 10 A |
| Soket |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>AC güç kabloları - Avrupa

| Bileşen | Belirtim |
| --- | --- |
| Kablo tipi |Uyumlu, H05-VVF-3G1.0 |
| Soket |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Desteklenen ağ kabloları

10 GbE ağ arabirimleri, DATA 2 ve DATA 3 için [desteklenen ağ kabloları ve modülleri listesine](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Artık veri merkezinizde bir StorSimple aygıtı dağıtmaya hazırsınız. Daha fazla bilgi için [bkz.](storsimple-8000-deployment-walkthrough-u2.md)

