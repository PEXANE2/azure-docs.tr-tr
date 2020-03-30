---
title: Microsoft Azure Veri Kutusu Kenarı teknik özellikleri ve uyumluluğu| Microsoft Dokümanlar
description: Azure Veri Kutusu Edge'inizin teknik özellikleri ve uyumluluğu hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/02/2020
ms.author: alkohli
ms.openlocfilehash: b646ee9b727d5adf4ec1c8b5c769b3d8f5c0fc1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252045"
---
# <a name="azure-data-box-edge-technical-specifications"></a>Azure Veri Kutusu Kenarı teknik özellikleri

Microsoft Azure Veri Kutusu Kenarı cihazınızın donanım bileşenleri, bu makalede belirtilen teknik belirtimlere ve düzenleyici standartlara bağlıdır. Teknik özellikler Güç kaynağı ünitelerini (PSU), depolama kapasitesini, muhafazaları ve çevre standartlarını tanımlar. 

## <a name="compute-memory-specifications"></a>Hesaplama, bellek özellikleri

Veri Kutusu Kenarı aygıtı, bilgi işlem ve bellek için aşağıdaki özelliklere sahiptir:

| Belirtim           | Değer                  |
|-------------------------|----------------------------|
| CPU    | 2 X 10 çekirdekli Işlemci                     |
| Bellek              | 128 GB RAM                  |


## <a name="fpga-specifications"></a>FPGA teknik özellikleri

Makine Öğrenimi (ML) senaryolarını etkinleştiren her Veri Kutusu Kenarı cihazında Alan Programlanabilir Kapı Dizisi (FPGA) yer alır. 

| Belirtim           | Değer                  |
|-------------------------|----------------------------|
| Fpga   | Intel Arria 10 <br> Mevcut Derin Sinir Ağı (DNN) modelleri [bulut FPGA örnekleri tarafından desteklenen](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#whats-supported-on-azure)lerle aynıdır.| 


## <a name="power-supply-unit-specifications"></a>Güç kaynağı ünitesi özellikleri

Data Box Edge cihazı, yüksek performanslı fanlı iki adet 100-240 V Güç kaynağı ünitesine (PSU) sahiptir. İki PSU gereksiz güç yapılandırması sağlar. Bir PSU başarısız olursa, başarısız modül değiştirilene kadar aygıt diğer PSU üzerinde normal olarak çalışmaya devam eder. Aşağıdaki tabloda PSU'ların teknik özellikleri listelenebilmiştir.

| Belirtim           | 750 W PSU                  |
|-------------------------|----------------------------|
| Maksimum çıkış gücü    | 750 W                     |
| Frequency               | 50/60 Hz                   |
| Gerilim aralığı seçimi | Otomatik değişen: 100-240 V AC |
| Sıcak takılabilir           | Evet                        |

<!--## Power consumption statistics

The following table lists the typical power consumption data (actual values may vary from the published) for the Data Box Edge device.-->

## <a name="network-interface-specifications"></a>Ağ arabirimi özellikleri

Siz Data Box Edge cihazınızda PORT1- PORT6 olmak üzere 6 ağ arabirimi vardır.

| Belirtim           | Açıklama                 |
|-------------------------|----------------------------|
|  Ağ arabirimleri    | 2 X 1 GbE arabirimleri – 1 yönetim için, kullanıcı tarafından yapılandırılamaz, ilk kurulumda kullanılır. Diğer arabirim kullanıcı yapılandırılabilir, veri aktarımı için kullanılabilir ve varsayılan olarak DHCP'dir. <br>2 X 25 GbE arabirimleri – Bunlar 10 GbE arabirimleri olarak da çalıştırılabilir. Bu veri arabirimleri kullanıcı tarafından DHCP (varsayılan) veya statik olarak yapılandırılabilir. <br> 2 X 25 GbE arabirimleri - Bu veri arabirimleri kullanıcı tarafından DHCP (varsayılan) veya statik olarak yapılandırılabilir.                  |

## <a name="storage-specifications"></a>Depolama özellikleri

Veri Kutusu Edge aygıtlarının her biri 1,6 TB kapasiteli 9 X 2,5" NVMe SSD'si vardır. Bu SSD'lerden 1'i bir işletim sistemi diski, diğer 8'i ise veri diskidir. Cihazın toplam kullanılabilir kapasitesi kabaca 12,5 TB'dir. Aşağıdaki tabloda aygıtın depolama kapasitesi için ayrıntılar vardır.

|     Belirtim                          |     Değer             |
|--------------------------------------------|-----------------------|
|    Katı hal sürücülerinin (SSD) sayısı     |    8                  |
|    Tek SSD kapasitesi                     |    1,6 TB             |
|    Toplam kapasite                          |    12.8 TB            |
|    Toplam kullanılabilir kapasite*                  |    ~ 12.5 TB            |

**Bazı alan dahili kullanım için ayrılmıştır.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Muhafaza boyutları ve ağırlık özellikleri

Aşağıdaki tablolar, boyutlar ve ağırlık için çeşitli muhafaza belirtimlerini listelemektedir.

### <a name="enclosure-dimensions"></a>Muhafaza boyutları

Aşağıdaki tabloda kasanın milimetre ve inç boyutları listelenir.

|     Muhafaza     |     Milimetre     |     Inç     |
|-------------------|---------------------|----------------|
|    Height         |    44.45            |    1.75"          |
|    Genişlik          |    434.1           |    17.09"          |
|    Uzunluk          |    740.4           |    29.15"          |

Aşağıdaki tabloda, sevkiyat paketinin milimetre ve inç cinsinden boyutları listelenir.

|     Paket     |     Milimetre     |     Inç     |
|-------------------|---------------------|----------------|
|    Height         |    311.2            |    12.25"          |
|    Genişlik          |    642.8          |    25.31"          |
|    Uzunluk          |   1,051.1          |    41.38"          |

### <a name="enclosure-weight"></a>Muhafaza ağırlığı

Cihaz paketi 66 £ ağırlığındadır. ve bunu işlemek için iki kişi gerektirir. Aygıtın ağırlığı kasanın yapılandırmaya bağlıdır.

|     Muhafaza                                 |     Ağırlık          |
|-----------------------------------------------|---------------------|
|    Ambalaj dahil toplam ağırlık       |    61 lbs.          |
|    Cihazın ağırlığı                       |    35 lbs.          |

## <a name="enclosure-environment-specifications"></a>Muhafaza ortamı özellikleri

Bu bölümde sıcaklık, nem ve yükseklik gibi muhafaza ortamıyla ilgili teknik özellikler listelemektedir.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem

|     Muhafaza         |     Ortam sıcaklığı aralığı     |     Ortam bağıl nem     |     Maksimum çiy noktası     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    Operasyonel        |    10°C - 35°C (50°F - 86°F)         |    %10 - %80 yoğuşma sızma.         |    29°C (84°F)            |
|    Operasyonel olmayan    |    -40°C ila 65°C (-40°F - 149°F)     |    %5 - %95 yoğuşma sızma.          |    33°C (91°F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Hava akımı, irtifa, şok, titreşim, yönlendirme, güvenlik ve EMC

|     Muhafaza                           |     Operasyonel özellikler                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Hava akımı                              |    Sistem hava akımı önden arkaya doğru. Sistem düşük basınçlı, arka egzoz tesisatı ile çalıştırılmalıdır. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
|    Maksimum irtifa, operasyonel        |    3048 metre (10.000 fit) maksimum çalışma sıcaklığı ile de-rated [Çalışma sıcaklığı de-rating özellikleri](#operating-temperature-de-rating-specifications)tarafından belirlenir.                                                                                |
|    Maksimum irtifa, operasyonel olmayan    |    12.000 metre (39.370 feet)                                                                                                                                                                                         |
|    Şok, operasyonel                   |    6 yönde 11 milisaniye için 6 G                                                                                                                                                                         |
|    Şok, çalışmayan               |    6 yönde 2 milisaniye için 71 G                                                                                                                                                                           |
|    Titreşim, operasyonel               |    0,26 G<sub>RMS</sub> 5 Hz - 350 Hz rastgele                                                                                                                                                                                     |
|    Titreşim, operasyonel olmayan           |    1,88 G<sub>RMS</sub> 10 Hz ila 500 Hz 15 dakika (tüm altı taraf test.)                                                                                                                                                  |
|    Oryantasyon ve montaj             |    19" raf montaj                                                                                                                                                                                        |
|    Güvenlik ve onaylar                 |    EN 60950-1:2006 +A1:2010 +A2:2013 +A11:2009 +A12:2011/IEC 60950-1:2005 ed2 +A1:2009 +A2:2013 EN 62311:2008                                                                                                                                                                       |
|    Emc                                  |    FCC A, ICES-003 <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 +A1:2015/CISPR 24:2010 +A1:2015  <br>EN 61000-3-2:2014/IEC 61000-3-2:2014 (D Sınıfı)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013                                                                                                                                                                                         |
|    Enerji             |    Komisyon Yönetmeliği (AB) No. 617/2013                                                                                                                                                                                        |
|    Rohs           |    EN 50581:2012                                                                                                                                                                                        |


### <a name="operating-temperature-de-rating-specifications"></a>Çalışma sıcaklığı de-rating özellikleri

|     Çalışma sıcaklığı de-rating     |     Ortam sıcaklığı aralığı                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    35°C'ye kadar (95°F)                       |    Maksimum sıcaklık 950 m'nin (3,117 ft) üzerinde 1°C/300 m (1°F/547 ft) azalır.    |
|    35°C ila 40°C (95°F ila 104°F)            |    Maksimum sıcaklık 950 m'nin (3,117 ft) üzerinde 1°C/175 m (1°F/319 ft) azalır.    |
|    40°C ila 45°C (104°F ila 113°F)           |    Maksimum sıcaklık 950 m'nin (3,117 ft) üzerinde 1°C/125 m (1°F/228 ft) azalır.    |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Kutusu Kenarınızı Dağıtma](data-box-edge-deploy-prep.md)
