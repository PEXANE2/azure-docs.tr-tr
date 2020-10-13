---
title: Video analizlerini dağıtma-nesne ve hareket algılama Azure IoT Central uygulama şablonu
description: Bu kılavuzda, video analizi-nesne ve hareket algılama uygulama şablonu kullanılarak bir Azure IoT Central uygulaması dağıtma adımları özetlenmektedir.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: how-to
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: decfa7020be7778e8ca64a9fb0cb4aac1657da27
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91873345"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Video analizi-nesne ve hareket algılama uygulama şablonunu kullanarak IoT Central uygulaması dağıtma

Temel *video analizi-nesne ve hareket algılama* uygulaması bileşenlerine genel bakış için bkz. [nesne ve hareket algılama video analizi uygulama mimarisi](architecture-video-analytics.md).

Aşağıdaki videoda, bir IoT Central çözümü dağıtmak için _video Analytics-Object ve Motion Detection uygulama şablonunun_ nasıl kullanılacağına ilişkin bir anlatım verilmiştir:

> [!VIDEO https://www.youtube.com/embed/Bo3FziU9bSA]

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Video analizi uygulama şablonunu kullanarak bir IoT Central uygulamasını dağıtmak için aşağıdaki adımları izleyin:

1. [Azure IoT Central bir video Analytics uygulaması oluşturma (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) veya [Azure 'Da video analizi oluşturma IoT Central (openvino &trade; )](tutorial-video-analytics-create-app-openvino.md) öğreticisini şunları yapın:
    - Azure Media Services hesabı oluşturun.
    - Video analizi-nesne ve hareket algılama uygulama şablonundan IoT Central uygulamayı oluşturun.
    - IoT Central uygulamasında bir ağ geçidi cihazı yapılandırın. Ağ Geçidi, kamera cihazlarının uygulamaya bağlanmasına olanak sağlar.

1. [Video analizi için IoT Edge örneğini oluşturma (LINUX VM)](tutorial-video-analytics-iot-edge-vm.md) veya [öğretici: video analizi için bir IoT Edge örneği oluşturma (Intel nuc)](tutorial-video-analytics-iot-edge-nuc.md) öğreticisini şu şekilde yapın:
    - Azure IoT Edge çalışma zamanı yüklü bir Azure VM oluşturun.-IoT Edge yüklemeyi video analizi modülünü barındıracak şekilde hazırlayın.
    - IoT Edge cihazını IoT Central uygulamanıza bağlayın.

1. [İzleme ve video analizi uygulamasını yönetme](tutorial-video-analytics-manage.md) öğreticisini doldurun ve şunları yapın:
    - IoT Central uygulamanızdaki ağ geçidine nesne ve hareket algılama kameraları ekleyin.
    - Kamera işlemesini başlatın.
    - Yakalanan Videoyu AMS 'de görüntülemek için yerel bir medya oynatıcı yükler.
    - Algılanan nesneleri gösteren yakalanan videoyu görüntüleyin.
    - Tidy.

## <a name="next-steps"></a>Sonraki adımlar

Artık, video analizi uygulama şablonunu dağıtma ve kullanma adımlarına genel bir bakış için bkz. başlamak için Azure 'da [bir video analytics IoT Central uygulaması oluşturma (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) veya [Azure IoT Central (openvino &trade; ) üzerinde bir video analizi oluşturma](tutorial-video-analytics-create-app-openvino.md) .
