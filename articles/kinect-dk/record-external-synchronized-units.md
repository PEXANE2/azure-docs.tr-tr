---
title: Dış eşitlenmiş cihazlarla Azure Kinect Kaydedicisini kullanma
description: Azure Kinect Kaydedicisi kullanılarak dış eşitleme için yapılandırılmış cihazlardan verileri nasıl kaydedeceğinizi öğrenin.
author: tesych
ms.author: tesych
ms.reviewer: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, algılayıcı, Görüntüleyici, dış eşitleme, aşama gecikmesi, derinlik, RGB, kamera, ses kablosu, kaydedici
ms.openlocfilehash: 052f6f1ac9f90e764de25d1d4d1b25b3d50a848d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277654"
---
# <a name="use-azure-kinect-recorder-with-external-synchronized-devices"></a>Dış eşitlenmiş cihazlarla Azure Kinect Kaydedicisini kullanma

Bu makalede, [Azure Kinect kaydedicisinin](azure-kinect-recorder.md) veri dış eşitleme yapılandırılmış cihazlarını nasıl kaydedebileceği hakkında rehberlik sunulmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- [Dış eşitleme için birden çok Azure Kinect dk birimi ayarlayın](https://support.microsoft.com/help/4494429).

## <a name="external-synchronization-constraints"></a>Dış eşitleme kısıtlamaları

- Ana cihazda, kablo bağlantılı olarak EŞITLEME olamaz.
- Eşitlemeyi etkinleştirmek için ana cihazın RGB kamerasını akışı gerekir.
- Tüm birimlerin aynı kamera yapılandırmasını kullanması gerekir (kare hızı ve çözüm).
- Tüm birimlerin aynı cihaz üretici yazılımını çalıştırması gerekir ([bellenim](update-device-firmware.md) talimatlarını güncelleştirin).
- Tüm bağımlı cihazların ana cihazdan önce başlatılması gerekir.
- Aynı Pozlandırma değeri tüm cihazlarda ayarlanmalıdır.
- Her bir alt öğenin *bekleme* ayarı ana cihazla ilişkilidir.

## <a name="record-when-each-unit-has-a-host-pc"></a>Her birimde bir ana bilgisayar olduğunda Kaydet

Aşağıdaki örnekte, her bir cihazın kendi adanmış ana bilgisayar bilgisayarı vardır.
USB bant genişliği ve CPU/GPU kullanımıyla ilgili sorunları engellemek için cihazları adanmış bilgisayarlara bağlamanız önerilir.

### <a name="subordinate-1"></a>Alt-1

1. İlk birim için kaydediciyi ayarla

      `k4arecorder.exe --external-sync sub -e -8 -r 5 -l 10 sub1.mkv`

2. Cihaz beklemeye başladı

    ```console
    Device serial number: 000011590212
    Device version: Rel; C: 1.5.78; D: 1.5.60[6109.6109]; A: 1.5.13
    Device started
    [subordinate mode] Waiting for signal from master
    ```

### <a name="subordinate-2"></a>Alt-2

1. İkinci birim için kaydediciyi ayarla

    `k4arecorder.exe --external-sync sub -e -8 -r 5 -l 10 sub2.mkv`

2. Cihaz beklemeye başladı

    ```console
    Device serial number: 000011590212
    Device version: Rel; C: 1.5.78; D: 1.5.60[6109.6109]; A: 1.5.13
    Device started
    [subordinate mode] Waiting for signal from master
    ```

### <a name="master"></a>Ana

1. Ana sunucuda kayıt başlatma

    `>k4arecorder.exe --external-sync master -e -8 -r 5 -l 10 master.mkv`

2. Kayıt bitene kadar bekle

## <a name="recording-when-multiple-units-connected-to-single-host-pc"></a>Tek bir ana bilgisayara birden çok birim bağlandığında kaydetme

Tek bir ana bilgisayara bağlı birden çok Azure ınect bağlantısı olabilir. Ancak bu, USB bant genişliği ve ana bilgisayar işlem için çok güç olabilir. Talebi azaltmak için:

- Her cihazı kendi USB ana bilgisayar denetleyicisine bağlayın.
- Her cihaz için derinlik altyapısını işleyebilen güçlü bir GPU 'SU vardır.
- Yalnızca gerekli algılayıcıları kaydedin ve düşük kare hızını kullanın.

Önce her zaman alt cihazları ve ana son ' ı başlatın.

## <a name="subordinate-1"></a>Alt-1

1. Bağımlı üzerinde kaydediciyi Başlat

    `>k4arecorder.exe --device 1 --external-sync subordinate --imu OFF -e -8 -r 5 -l 5 output-2.mkv`

2. Cihaz bekleme durumuna geçiyor

## <a name="master"></a>Ana

1. Ana cihazı Başlat

    `>k4arecorder.exe --device 0 --external-sync master --imu OFF -e -8 -r 5 -l 5 output-1.mkv`

2. Kaydın bitmesini bekle

## <a name="playing-recording"></a>Kayıt yürütülüyor

Kaydı kayıttan yürütmek için [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md) 'ni kullanabilirsiniz.



## <a name="tips"></a>İpuçları

- Eşitlenmiş kameraları kaydetmek için el ile pozlamayı kullanın. RGB Kamerası otomatik pozlaması, zaman eşitlemesini etkileyebilir.
- Alt cihazın yeniden başlatılması eşitlemenin kaybolmasına neden olur.
- Bazı [Kamera modlarında](hardware-specification.md#depth-camera-supported-operating-modes) 15 fps maksimum değeri desteklenir. Cihazlar arasında modlar/çerçeve oranları karıştırmayın
- Birden çok birimi tek bir BILGISAYARA bağlamak, USB bant genişliğinden kolayca doygunluğu sağlar ve cihaz başına ayrı bir ana bilgisayar BILGISAYARı kullanmayı düşünün. CPU/GPU işlemlerine dikkat edin.
- Güvenilirliği artırmak için gerekmiyorsa mikrofonu ve ıMU 'yi devre dışı bırakın.

Herhangi bir sorun için bkz. [sorun giderme](troubleshooting.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Dış eşitleme ayarlama](https://support.microsoft.com/help/4494429/sync-multiple-devices)
- Kaydedici ayarları ve ek bilgiler için [Azure Kinect Kaydedicisi](azure-kinect-recorder.md) .
- Kayıt çalmak için [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md) ya da kayıt aracılığıyla kullanılamayan RGB kamera özelliklerini ayarlama.
- Cihaz bellenimini güncelleştirmek için [Azure Kinect üretici yazılımı aracı](azure-kinect-firmware-tool.md) .
