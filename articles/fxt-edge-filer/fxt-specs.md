---
title: Microsoft Azure FXT Edge Filer özellikleri | Microsoft Dokümanlar
description: Azure FXT Edge Filer donanımı için fiziksel ve çevresel özellikler
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: rohogue
ms.openlocfilehash: c06b0c79e01257eebf566b9752269cb88c072d8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264732"
---
# <a name="azure-fxt-edge-filer-specifications"></a>Azure FXT Edge Filer özellikleri

Bu makalede, Azure FXT Edge Filer donanım düğümleri için donanım özellikleri açıklanmaktadır. Uygulamada, kümelenmiş önbellek sistemini sağlamak için üç veya daha fazla düğüm birlikte yapılandırılır.

## <a name="hardware-specifications"></a>Donanım belirtimleri

| Bileşen | FXT 6600 | FXT 6400 |
|----------|-----------|-----------|
| CPU çekirdekleri |  16 | 16 |
| Dramı  | 1536 GB | 768 GB |
| Ağ bağlantı noktaları | 6 x 25/10 Gb + 2 x 1 Gb | 6 x 25/10 Gb + 2 x 1 Gb |
| NVMe SSD kapasitesi | 25.6 TB | 12.8 TB |

## <a name="drive-specifications"></a>Sürücü özellikleri

Sistemin önden erişilebilen on tane sürücü yuvası var. Doldurulan her sürücü, kapasite bilgileriyle birlikte sağda etiketlenir. 

Sürücü numaraları sürücüler arasındaki boşluğa yazdırılır. Azure FXT Edge Filer'de sürücü 0 sol üst sürücüdür ve sürücü 1 doğrudan altındadır.

![sürücü numaralarını ve kapasite etiketlerini gösteren, FXT kasasında ki bir sabit sürücü yuvasının fotoğrafı](media/fxt-drives-photo.png)

| Sürücü numaraları    |  Kullanım   |  Belirtimler |
|------------------|--------|-----------------|
| 0, 1             | İşletim Sistemi     | 480 GB SATA SSD |
| 2, 3, 4, 5, 6, 7, 8, 9 | Veri   | FXT 6600: 3.2 TB NVMe SSD <br> FXT 6400: 1.6 TB NVMe SSD |


## <a name="dimensions-and-weight"></a>Boyutlar ve ağırlık

Azure FXT Edge Filer standart 19" ekipman rafına uyacak şekilde tasarlanmıştır ve bir raf ünitesi yüksekliğindedir (1U). 

<!-- 10x2.5 inches version -->

| Filer boyutları           |                          |
|-----------------------------|--------------------------|
| Height                      | 42,8 mm (1,68 inç)    |
| Genişlik (raf kulakları dahil) | 482,0 mm (18,97 inç)  |
| Genişlik - ana kasa      | 434,0 mm (17,08 inç) |
| Derinlik - ana kasanın arkasına kadar raf kulakları                   | 733,82 mm (29,61 inç) |
| Derinlik - en uzak arka çıkıntıya raf kulakları                 | 772,67 mm (30,42 inç) |
| Derinlik - çerçeve olmadan, en uzak ön çıkıntı için raf kulakları | 22,0 mm (0,87 inç)  |
| Derinlik - çerçeve ile en uzak ön çıkıntı için raf kulakları    | 35,84 mm (1,41 inç) |

| Ağırlık | |
|-----------------|----------------------|
| Düğüm ağırlığı (ambalajsız, aksesuarsız) | 40 lbs (18,1 kg) |
| Net ağırlık (ambalajsız, aksesuarlar dahil) | 51 lbs (23,1 kg)|
| Brüt ağırlık (sevk edilirken, tüm ambalajları içerir) |  64 lbs (29,0 kg) |

### <a name="shipping-dimensions"></a>Sevkiyat boyutları

| Paket boyutu | Milimetre | Inç |
|-------------------|-------------|--------|
| Height            | 311.2       | 12.25" |
| Genişlik             | 642.8       | 25.31" |
| Uzunluk            | 1,051.1     | 41.38" |

## <a name="power-and-thermal-specifications"></a>Güç ve Termal Özellikleri

Bu bölümde, Azure FXT Edge Filer için güç derecelendirmeleri ve ölçümleri veremeleri yer almaktadır.

### <a name="nameplate-ratings"></a>Ad levhası derecelendirmeleri

| FXT 6000 serisi modeller için ad plakası derecelendirmeleri |
|----------------|
| 100 - 240V~    |
| 10A - 5A (X2)  |
| 50/60Hz         |

<!-- matches the Dell regulatory label exactly -->

### <a name="power-and-thermal-measurements"></a>Güç ve ısı ölçümleri 

Azure FXT Edge Filer düğümleri değişken hız fanları kullanır, bu nedenle güç sıcaklık ve yüke bağlıdır. Maksimum fan hızlarına yüksek yük ve yüksek ortam sıcaklıklarının belirli kombinasyonlarında ulaşılabilir. 

Bu grafikler, yaygın olarak kullanılan voltaj-frekans kombinasyonları için güç tüketimi ve ısı çıkış ölçümleri sağlar. 

| Oda sıcaklığında FXT 6600 güç <br />(22° C, 71.6° F) | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Gerilim (V) | 100 | 120 | 208 | 230 | 240 | 
| Frekans (Hz) | 60 | 60 | 60 | 50 | 50 |
| Akım (A) | 5.02 | 4.16 |2.40 | 2.20 | 2.16 |
| Görünen Güç (VA) | 502 | 499 | 499 | 506 | 518|
| Güç Faktörü | 0.99 | 0.99 |0.98 | 0.98 | 0.98 |
| Gerçek Güç (W) | 497 |494 | 489 | 496 | 508 |
| Termal Dağılım (BTU/Hr) |1696 | 1686 | 1669 | 1692 | 1733 |

| Maksimum fan hızlarında FXT 6600 güç | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Gerilim (V) | 100 |120 | 208 | 230 | 240| 
| Frekans (Hz) | 60 | 60 | 60 | 50 | 50 |
| Akım (A) | 5.98 | 5.01 | 2.81 | 2.55 | 2.48 |
| Görünen Güç (VA) | 598 | 601 | 584 | 587 | 595 |
| Güç Faktörü | 0.99 | 0.99 | 0.98 | 0.98 | 0.98 |
| Gerçek Güç (W) | 592 | 595 | 573 | 575 | 583 |
| Termal Dağılım (BTU/Hr) | 2020 |2031 | 1954 | 1961 | 1990 |

| Oda sıcaklığında FXT 6400 güç <br />(22° C, 71.6° F) | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Gerilim (V) | 100 | 120 | 208 | 230 | 240 |
| Frekans (Hz) |60 | 60 | 60 | 50 | 50 |
| Akım (A) | 4,63 | 3.86 | 2.24 | 2.04 | 1.94 |
| Görünen Güç (VA) | 463 | 463 | 466 | 469 | 466 |
| Güç Faktörü | 0.99 | 0.99 | 0.98 | 0.98 | 0.98 | 
| Gerçek Güç (W) | 458 | 459 | 457 | 460 | 456 |
| Termal Dağılım (BTU/Hr) | 1564 | 1565 | 1558 | 1569 | 1557 |

| Maksimum fan hızlarında FXT 6400 güç | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz |
|---------|---|---|---|---|---|
| Gerilim (V) | 100 | 120 | 208 | 230 | 240 |
| Frekans (Hz) | 60 | 60 | 60 | 50 | 50 |
| Akım (A) | 5.15 | 4.28 | 2.48 | 2.28 | 2.13 |
| Görünen Güç (VA) | 515 | 514 | 516 | 524 | 511 |
| Güç Faktörü | 0.99 | 0.99 | 0.98 | 0.98 | 0.98 |
| Gerçek Güç (W) | 510 | 508 | 506 | 514 | 501 |
| Termal Dağılım (BTU/Hr) | 1740 | 1735 | 1725 | 1753 | 1709 |

## <a name="environmental-requirements"></a>Çevresel gereksinimler

Bu bölümde donanımın ortam ortamı için teknik özellikler verir.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem

| Çevresel özellik   | Çalışma aralığı                   | Çalışma dışı aralık         |
|---------------------------|-----------------------------------|-----------------------------|
| Ortam sıcaklığı aralığı | 10°C ila 35°C (50 - 86°F)          | -40°C ila 65°C (-40 - 149°F) |
| Ortam bağıl nem | %10 - %80 yoğuşmasız          | %5 - %95 yoğuşmasız     |
| Maksimum çiy noktası         | 29°C (84°F)                       | 33°C (91°F)                 |
| Irtifa                  | 3048 metreye (10.000 fit) kadar, aşağıda belirtilen sıcaklık de-rating tabi | 12.000 metreye kadar (39.370 fit) |

> [!NOTE] 
> **Yükseklik sıcaklığı de-rating:** Maksimum sıcaklık 950 m'nin (3,117 ft) üzerinde 1°C/300 m (1°F/547 ft) azalır.

### <a name="airflow-shock-and-vibration"></a>Hava akımı, şok ve titreşim 

| Öznitelik         | Belirtim |
|-------------------|---------------|
| Hava akımı                    | Sistem hava akımı önden arkaya doğru. Sistem düşük basınçlı arka egzoz tesisatı ile çalıştırılmalıdır. |
| Şok, operasyonel         | 11 milisaniye için 6 G (6 yönde test edilmiştir) |
| Şok, çalışmayan     | 2 milisaniye için 71 G (6 yönde test edilmiştir) |
| Titreşim, operasyonel     | 0,26 G<sub>RMS</sub> 5 Hz - 350 Hz rastgele         |
| Titreşim, operasyonel olmayan | 15 dakika boyunca 1,88 G<sub>RMS</sub> 10 Hz - 500 Hz (altı taraf da test edildi)  |

## <a name="safety-regulation-compliance"></a>Güvenlik yönetmeliği uyumluluğu 

Azure FXT Edge Filer listelenen yönetmeliklere uyar. 

| Kategori       | Mevzuat şartnamesi | 
|----------------|--------------------------|
| Genel güvenlik | EN 60950-1:2006 +A1:2010 +A2:2013 +A11:2009 +A12:2011/IEC 60950-1:2005 ed2 +A1:2009 +A2:2013 <br>EN 62311:2008 | 
| Emc            | FCC A, ICES-003  <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 +A1:2015/CISPR 24:2010 +A1:2015  <br>EN 61000-3-2:2014/IEC 61000-3-2:2014 (D Sınıfı)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013 |
| Enerji         | Komisyon Yönetmeliği (AB) No. 617/2013  |
| Rohs           |    EN 50581:2012   |