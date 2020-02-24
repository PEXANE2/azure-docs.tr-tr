---
title: Azure Kinect DK donanım belirtimleri
description: Azure Kinect DK’nin bileşenlerini, belirtimlerini ve özelliklerini anlayın.
author: tesych
ms.author: tesych
ms.reviewer: jarrettr
ms.prod: kinect-dk
ms.date: 02/14/2020
ms.topic: article
keywords: azure, kinect, belirtimler, donanım, DK, özellikler, derinlik, renk, RGB, IMU, mikrofon, dizi, derin
ms.custom:
- CI 114092
- CSSTroubleshooting
audience: ITPro
manager: dcscontentpm
ms.localizationpriority: high
ms.openlocfilehash: dbe4cac64e324c1188ec5a01b4da3935a5d5b479
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371297"
---
# <a name="azure-kinect-dk-hardware-specifications"></a>Azure Kinect DK donanım belirtimleri

Bu makalede Azure Kinect donanımının Microsoft’un en son algılayıcı teknolojisini USB bağlantılı tek bir aksesuarla tümleştirmesinin ayrıntılarını sağlar.

![Azure Kinect DK](./media/resources/hardware-specs-media/device-wire.png)

## <a name="terms"></a>Koşullar

Bu makale boyunca aşağıdaki terim kısaltmaları kullanılmıştır.

- NFOV (Dar görünüm alanı derinlik modu)
- WFOV (Geniş görünüm alanı derinlik modu)
- FOV (Görünüm alanı)
- FPS (Kare/saniye)
- IMU (Atalet Ölçü Birimi)
- FoI (İlgi alanı)

## <a name="product-dimensions-and-weight"></a>Ürün boyutları ve ağırlığı

Azure Kinect cihazı aşağıdaki boyutlarda ve ağırlıktadır.

- **Boyutlar**: 103 x 39 x 126 mm
- **Ağırlık**: 440 g

![Azure Kinect DK boyutları](./media/resources/hardware-specs-media/dimensions.png)

## <a name="operating-environment"></a>Çalıştırma ortamı

Azure Kinect DK aşağıdaki çevre koşullarında çalışan geliştiricilere ve ticari işletmelere yöneliktir:

- **Sıcaklık**: 10-25⁰C
- **Nem Oranı**: %8-90 (yoğuşmasız) Göreli Nem

> [!NOTE]
> Çevre koşullarının dışında kullanılması cihazın başarısız olmasına ve/veya hatalı çalışmasına neden olabilir. Bu çevre koşulları, tüm çalışma koşullarında cihazın yakın çevresindeki ortam için geçerlidir. Dış muhafazayla kullanıldığında, cihazı bu aralıklarda tutmak için etkin sıcaklık denetiminden ve/veya diğer soğutma çözümlerinden yararlanılması önerilir. Cihazın tasarımında, ön bölümle arka kılıf arasında bir soğutma kanalı vardır. Cihazı yerleştirirken bu soğutma kanalının tıkanmadığından emin olun.

Ürünle ilgili ek [güvenlik bilgilerini](https://support.microsoft.com/help/4023454/safety-information) gözden geçirin.

## <a name="depth-camera-supported-operating-modes"></a>Derinlik kamerasının desteklediği çalışma modları

Azure Kinect DK, [ISSCC 2018’de tanıtılan resim algılayıcısı](https://docs.microsoft.com/windows/mixed-reality/ISSCC-2018) kullanılarak Microsoft tasarımlı 1 Megapiksel Uçuş Zamanı (ToF) derinlik kamerasıyla tümleştirilir. Derinlik kamerası aşağıda belirtilen modları destekler:

 | Mod            | Çözünürlük | FoI       | FPS                | Çalışma aralığı* | Pozlandırma süresi |
|-----------------|------------|-----------|--------------------|------------------|---------------|
| NFOV gruplanmamış   | 640x576    | 75°x65°   | 0, 5, 15, 30       | 0,5 - 3,86 m       | 12,8 ms        |
| NFOV 2x2 gruplanmış (SW) | 320x288    | 75°x65°   | 0, 5, 15, 30       | 0,5 - 5,46 m       | 12,8 ms        |
| WFOV 2x2 gruplanmış | 512x512    | 120°x120° | 0, 5, 15, 30       | 0,25 - 2,88 m      | 12,8 ms        |
| WFOV gruplanmamış   | 1024x1024  | 120°x120° | 0, 5, 15           | 0,25 - 2,21 m      | 20,3 ms        |
| Pasif IR      | 1024x1024  | Yok       | 0, 5, 15, 30       | Yok              | 1,6 ms         |

\*850 nm’de %15 ile %95 arası yansıtma, 2,2 μW/cm<sup>2</sup>/nm, rastgele hata standart sapması ≤ 17 mm, tipik sistematik hata < 11 mm + çok yollu girişim olmadan uzaklığın %0,1’i. Nesnenin yansıtma düzeyine bağlı olarak belirtilen aralığın dışında derinlik sağlanır.

## <a name="color-camera-supported-operating-modes"></a>Renkli kameranın desteklediği çalışma modları

Azure Kinect DK, OV12A10 12MP CMOS jöle efekti algılayıcısı içerir. Yerel çalışma modları aşağıda listelenmiştir:

|             RGB Kamera Çözünürlüğü (HxV)  |          En Boy Oranı  |          Biçim Seçenekleri   |          Kare Hızları (FPS)  |          Nominal FOV (HxV)(işleme sonrası)  |
|------------------------------------------|------------------------|---------------------------|-----------------------------|---------------------------------------------|
|       3840x2160                          |          16:9          |          MJPEG            |          0, 5, 15, 30       |          90°x59°                              |
|       2560x1440                          |          16:9          |          MJPEG            |          0, 5, 15, 30       |          90°x59°                              |
|       1920x1080                          |          16:9          |          MJPEG            |          0, 5, 15, 30       |          90°x59°                              |
|       1280x720                           |          16:9          |          MJPEG/YUY2/NV12  |          0, 5, 15, 30       |          90°x59°                              |
|       4096x3072                          |          4:3           |          MJPEG             |          0, 5, 15           |          90°x74,3°                            |
|       2048x1536                          |          4:3           |          MJPEG             |          0, 5, 15, 30       |          90°x74,3°                            |

RGB kamerası USB Video sınıfıyla uyumludur ve Algılayıcı SDK’sı olmadan kullanılabilir. RGB kamerası renk alanı: BT.601 tam aralık [0..255]. 

> [!NOTE]
> Algılayıcı SDK’sı BGRA piksel biçiminde renkli resimler sağlayabilir. Bu, cihaz tarafından desteklenen yerel bir mod değildir ve kullanıldığında ek CPU yüküne neden olur. Cihazdan alınan MJPEG resimlerini dönüştürmek için konak CPU’su kullanılır.

## <a name="rgb-camera-exposure-time-values"></a>RGB kamerası pozlandırma süresi değerleri

Aşağıda kabul edilebilir RGB kamerası el ile pozlandırma değerleri eşlemesi verilmiştir:

| poz| 2^poz | 50 Hz   |60 Hz    |
|----|-------|--------|--------|
| -11|     488|    500|    500 |
| -10|     977|   1250|   1250 |
|  -9|    1953|   2500|   2500 |
|  -8|    3906|  10000|   8330 |
|  -7|    7813|  20000|  16670 |
|  -6|   15625|  30000|  33330 |
|  -5|   31250|  40000|  41670 |
|  -4|   62500|  50000|  50000 |
|  -3|  125000|  60000|  66670 |
|  -2|  250000|  80000|  83330 |
|  -1|  500000| 100000| 100000 |
|   0| 1000000| 120000| 116670 |
|   1| 2000000| 130000| 133330 |

## <a name="depth-sensor-raw-timing"></a>Derinlik algılayıcısı ham zamanlaması

Derinlik Modu | IR <br>Titreşimler | Titreşim <br>Genişlik  | Boş <br>Dönemler| Boş Zaman | Pozlandırma <br> Zaman
-|-|-|-|-|-
NFOV Gruplanmamış <br>  NFOV 2xx Gruplanmış <br> WFOV 2x2 Gruplanmış | 9 | 125 us | 8 | 1450 us | 12,8 ms 
WFOV Gruplanmamış                                            | 9 | 125 us | 8 | 2390 us | 20,3 ms


## <a name="camera-field-of-view"></a>Kamera görünüm alanı

Aşağıdaki resimde derinlik ve RGB kamerası görünüm alanı veya algılayıcıların “gördüğü” açılar gösterilir. Bu diyagramda 4:3 modunda RGB kamerası gösterilir.

![Kamera FOV](./media/resources/hardware-specs-media/camera-fov.png)

Bu resimde kameranın önden ve 2000 mm uzaklıktan görünüm alanı gösterilir.

![Kamera FOV Ön](./media/resources/hardware-specs-media/fov-front.png)

> [!NOTE]
> Derinlik NFOV modunda olduğunda, RGB kamerasının 4:3 çözünürlükteki piksel çakışması 16:9 çözünürlükten daha iyidir.

## <a name="motion-sensor-imu"></a>Hareket algılayıcısı (IMU)

Eklenen Atalet Ölçüm Birimi (IMU) bir LSM6DSMUS’tur ve hem ivme ölçer hem de jiroskop içerir. İvme ölçerle jiroskop eşzamanlı olarak 1,6 kHz’te örneklenir. Örnekler konağa 208 Hz hızında bildirilir.

## <a name="microphone-array"></a>Mikrofon dizisi

Azure Kinect DK’de standart USB ses sınıfı 2.0 cihazı olarak tanımlanan yüksek kaliteli, yedi mikrofonlu dairesel bir dizi yer alır. 7 kanalın hepsine erişilebilir. Performans belirtimleri:

- Duyarlılık: -22 dBFS (94 dB SPL, 1 kHz)
- Sinyal gürültü oranı > 65 dB
- Akustik aşırı yük noktası: 116 dB

![Mikrofon başlığı](./media/resources/hardware-specs-media/mic-bubble.png)

## <a name="usb"></a>USB

Azure Kinect DK, aşağıdaki donanım uç noktalarını işletim sisteminin kullanımına sunan bir USB3 bileşik cihazıdır:

Satıcı Kimliği 0x045E (Microsoft), Ürün Kimliği tablosu aşağıdadır:

|    USB Arabirimi        |    PNP IP    |     Notlar            |
|-------------------------|--------------|----------------------|
|    USB3.1 Gen1 Hub    |    0x097A    |    Ana hub    |
|    USB2.0 Hub         |    0x097B    |    HS   USB          |
|    Derinlik kamerası       |    0x097C    |    USB3.0            |
|    Renkli kamera       |    0x097D    |    USB3.0            |
|    Mikrofonlar        |    0x097E    |    HS   USB          |

## <a name="indicators"></a>Göstergeler

Cihazın ön tarafında bir kamera akış göstergesi vardır ve bu gösterge Algılayıcı SDK’sı kullanılarak program aracılığıyla devre dışı bırakılabilir.

Cihazın arkasındaki durum LED’i cihazın durumunu gösterir:

| Işığın rengi     | Anlamı                                                   |
|-----------------------|------------------------------------------------------------|
| Düz beyaz           | Cihaz açık ve düzgün çalışıyor.                         |
| Yanıp sönen beyaz        | Cihaz açık ama USB 3.0 veri bağlantısı yok.   |
| Yanıp sönen kehribar rengi        | Cihazın gücü çalışması için yeterli değil.               |
| Kehribar rengi yanıp sönen beyaz  | Üretici yazılımı güncelleştirme veya kurtarma işlemi devam ediyor                    |

## <a name="power-device"></a>Cihaza güç sağlama

Cihaza iki yolla güç sağlanabilir:

1. Birlikte sağlanan güç kaynağı kullanılarak. Veri bağlantısı ayrı bir USB Type-C tarafından Type-A kablosuna yapılır.
2. Hem güç hem veri bağlantısı için Type-C - Type-C kablosu kullanılarak.

Type-C - Type-C kablosu Azure Kinect DK’ye dahil edilmemiştir.

> [!NOTE]
> - Birlikte sağlanan güç kaynağı kablosu USB Type-A - tek namlu arkası bağlayıcısı. Bu kabloyla, sağlanan duvar güç kaynağını kullanın. Cihaz iki standart USB Type-A bağlantı noktasının sağlayabileceğinden daha fazla güç çekebilir.
> - USB kabloları önemlidir; kaliteli kablolar kullanmanızı ve birimi uzaktan dağıtmadan önce işlevselliğini doğrulamanızı öneririz.

> [!TIP]
> İyi bir Type-C - Type-C kablosu seçmek için:
> - [USB sertifikalı kablonun](https://www.usb.org/products) hem güç hem veri bağlantısını desteklemesi gerekir.
> - Pasif kablo 1,5 m’den kısa olmalıdır. Daha uzun olacaksa aktif kablo kullanın. 
> - Kablonun >1,5A desteği olmalıdır. Aksi takdirde bir dış güç kaynağına bağlamanız gerekir.

Kabloyu doğrulama:

- Cihazı kabloyla konak bilgisayara bağlayın.
- Tüm cihazların Windows cihaz yöneticisinde doğru listelendiğini doğrulayın. Derinlik ve RGB kamerasının aşağıdaki örnekte gösterildiği gibi görünmesi gerekir.

  ![Cihaz Yöneticisi’nde Azure Kinect DK](./media/resources/hardware-specs-media/device-manager.png)

- Kablonun aşağıdaki ayarlarla Azure Kinect Görüntüleyicisi’ndeki tüm algılayıcılarda güvenilir bir akış yaptığını doğrulayın:

  - Derinlik kamerası: NFOV gruplanmamış
  - RGB Kamerası: 2160p
  - Mikrofon ve IMU etkin

## <a name="what-does-the-light-mean"></a>Işık ne anlama geliyor?

Güç göstergesi Azure Kinect DK cihazınızın arkasındaki bir LED’dir. Cihazın durumuna bağlı olarak LED’in rengi değişir.

![Resimde Azure Kinect DK’nin arkası gösterilir. Numaralandırılmış üç belirtme çizgisi vardır: biri LED göstergesine, onun altındaki ikisi de kablolara işaret eder.](./media/quickstarts/azure-kinect-dk-power-indicator.png)

Bu şekilde aşağıdaki bileşenler etiketlenmiştir:

1. Güç göstergesi
1. Güç kablosu (güç kaynağına bağlı)
1. USB-C veri kablosu (bilgisayara bağlı)

Kabloların gösterildiği gibi bağlandığından emin olun. Sonra güç ışığının çeşitli durumlarının neyi gösterdiğini öğrenmek için aşağıdaki tabloyu gözden geçirin.

|Işığın rengi: |Anlamı: |Sizin yapacaklarınız: |
| ---| --- | --- |
|Düz beyaz |Cihazın gücü açık ve cihaz düzgün çalışıyor. |Cihazı kullanın. |
|Yanmıyor |Cihaz bilgisayara bağlı değil. |Yuvarlak güç bağlayıcısı kablosunun cihaza ve USB güç bağdaştırıcısına bağlı olduğundan emin olun.<br /><br />USB-C kablosunun cihaza ve bilgisayarınıza bağlı olduğundan emin olun. |
|Yanıp sönen beyaz |Cihazın gücü açık ama USB 3.0 veri bağlantısı yok. |Yuvarlak güç bağlayıcısı kablosunun cihaza ve USB güç bağdaştırıcısına bağlı olduğundan emin olun.<br /><br />USB-C kablosunun cihaza ve bilgisayarınızdaki USB 3.0 bağlantı noktasına bağlı olduğundan emin olun.<br /><br />Cihazı bilgisayarda farklı bir USB 3.0 bağlantı noktasına bağlayın.<br /><br />Bilgisayarınızda Cihaz Yöneticisi’ni açın (**Başlat** > **Denetim Masası** > **Cihaz Yöneticisi**) ve bilgisayarınızın desteklenen bir USB 3.0 konak denetleyicisi olduğunu doğrulayın. |
|Yanıp sönen kehribar rengi |Cihazın gücü çalışması için yeterli değil. |Yuvarlak güç bağlayıcısı kablosunun cihaza ve USB güç bağdaştırıcısına bağlı olduğundan emin olun.<br /><br />USB-C kablosunun cihaza ve bilgisayarınıza bağlı olduğundan emin olun. |
|Kehribar rengi, sonra yanıp sönen beyaz |Cihazın gücü açık ve cihaz üretici yazılımı güncelleştirmesi alıyor veya fabrika ayarlarını geri yüklüyor. |Güç göstergesinin düz beyaza dönüşmesini bekleyin. Daha fazla bilgi için bkz. [Azure Kinect DK’yi sıfırlama](reset-azure-kinect-dk.md). |

## <a name="power-consumption"></a>Güç tüketimi

Azure Kinect DK en çok 5,9 W tüketir; belirli güç tüketim rakamları kullanım durumuna bağlıdır.

## <a name="calibration"></a>Kalibrasyon

Azure Kinect DK’nin kalibrasyonu fabrikada yapılır. Görsel ve atalet algılayıcılarının kalibrasyon parametreleri Algılayıcı SDK’sıyla program aracılığıyla sorgulanabilir.

## <a name="device-recovery"></a>Cihaz kurtarma

Cihaz üretici yazılımı, kilit piminin altındaki düğme kullanılarak özgün üretici yazılımına sıfırlanabilir.

![Azure Kinect DK kurtarma düğmesi](./media/resources/hardware-specs-media/recovery.png)

Cihazı kurtarmak için [buradaki yönergelere](reset-azure-kinect-dk.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kinect Algılayıcı SDK’sını kullanma](about-sensor-sdk.md)
- [Donanımı kurma](set-up-azure-kinect-dk.md)
