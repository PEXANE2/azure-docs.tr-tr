---
title: Hareket & ekran, uç cihazlarda video kaydetme-Azure
description: Bu hızlı başlangıçta, (sanal) bir IP kamerasından canlı video akışını analiz etmek, herhangi bir hareketin mevcut olup olmadığını algılamak ve bu durumda bir MP4 video klibini, sınır cihazında yerel dosya sistemine kaydetmek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir.
ms.topic: quickstart
ms.date: 04/27/2020
zone_pivot_groups: ams-lva-edge-programming-languages
ms.openlocfilehash: e456bb5b8d6d6658158af0c58f05ab38fe4693b8
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682347"
---
# <a name="quickstart-detect-motion-and-record-video-on-edge-devices"></a>Hızlı başlangıç: Edge cihazlarındaki hareketi algılama ve video kaydetme
 
Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir. Herhangi bir hareketin mevcut olup olmadığını nasıl tespit etmek gerektiğini gösterir. Bu durumda, sınır cihazında yerel dosya sistemine bir MP4 video klip kaydedin. Hızlı başlangıç, bir Azure VM 'yi IoT Edge bir cihaz olarak kullanır ve ayrıca sanal bir canlı video akışı kullanır. 

::: zone pivot="programming-language-csharp"
[!INCLUDE [header](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/header.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [header](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/header.md)]
::: zone-end

## <a name="prerequisites"></a>Ön koşullar

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/prerequisites.md)]
::: zone-end

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin

::: zone pivot="programming-language-csharp"
[!INCLUDE [review-sample-video](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/review-sample-video.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [review-sample-video](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/review-sample-video.md)]
::: zone-end

## <a name="overview"></a>Genel Bakış

::: zone pivot="programming-language-csharp"
[!INCLUDE [overview](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/overview.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [overview](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/overview.md)]
::: zone-end

## <a name="examine-and-edit-the-sample-files"></a>Örnek dosyaları İnceleme ve düzenleme

::: zone pivot="programming-language-csharp"
[!INCLUDE [examine-edit-sample-files](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/examine-edit-sample-files.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [examine-edit-sample-files](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/examine-edit-sample-files.md)]
::: zone-end

## <a name="review---check-the-modules-status"></a>İnceleme-modüllerin durumunu denetleme

::: zone pivot="programming-language-csharp"
[!INCLUDE [check-modules-status](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/check-modules-status.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [check-modules-status](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/check-modules-status.md)]
::: zone-end

## <a name="review---prepare-for-monitoring-events"></a>İnceleme-izleme olayları için hazırlanma

::: zone pivot="programming-language-csharp"
[!INCLUDE [prepare-monitoring-events](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/prepare-monitoring-events.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prepare-monitoring-events](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/prepare-monitoring-events.md)]
::: zone-end

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

::: zone pivot="programming-language-csharp"
[!INCLUDE [run-sample-program](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/run-sample-program.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [run-sample-program](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/run-sample-program.md)]
::: zone-end

## <a name="interpret-results"></a>Sonuçları yorumlama 

::: zone pivot="programming-language-csharp"
[!INCLUDE [interpret-results](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/interpret-results.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [interpret-results](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/interpret-results.md)]
::: zone-end

## <a name="play-the-mp4-clip"></a>MP4 klibini oynat

::: zone pivot="programming-language-csharp"
[!INCLUDE [play-mp4-clip](includes/detect-motion-record-video-clips-edge-devices-quickstart/csharp/play-mp4-clip.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [play-mp4-clip](includes/detect-motion-record-video-clips-edge-devices-quickstart/python/play-mp4-clip.md)]
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız, oluşturduğunuz kaynakları saklayın. Aksi takdirde, Azure portal kaynak gruplarınıza gidin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve ardından tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

* Canlı video akışlarına AI uygulamak için [kendi modelinizdeki canlı video analizlerini çalıştırın](use-your-model-quickstart.md) hızlı başlangıç adımlarını izleyin.
* Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

    * RTSP simülatörü kullanmak yerine RTSP 'yi destekleyen bir [IP kamera](https://en.wikipedia.org/wiki/IP_camera) kullanın. [ONVIF uyumlu ürünler](https://www.onvif.org/conformant-products) sayfasında RTSP 'YI destekleyen IP kameralarını bulabilirsiniz. Profiller G, S veya T ile uyumlu olan cihazları arayın.
    * Azure 'da Linux VM kullanmak yerine AMD64 veya x64 Linux cihazı kullanın. Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux üzerinde Azure IoT Edge çalışma zamanını Install](../../iot-edge/how-to-install-iot-edge-linux.md)içindeki yönergeleri izleyin. Ardından, cihazı Azure IoT Hub 'a kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma](../../iot-edge/quickstart-linux.md) bölümündeki yönergeleri izleyin.
