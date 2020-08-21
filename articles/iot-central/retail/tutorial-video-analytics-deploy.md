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
ms.openlocfilehash: d661df57e4409c1d7fe196c7f136965191421bd4
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719749"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Video analizi-nesne ve hareket algılama uygulama şablonunu kullanarak IoT Central uygulaması dağıtma

Temel *video analizi-nesne ve hareket algılama* uygulaması bileşenlerine genel bakış için bkz. [nesne ve hareket algılama video analizi uygulama mimarisi](architecture-video-analytics.md).

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Video analizi uygulama şablonunu kullanarak bir IoT Central uygulamasını dağıtmak için aşağıdaki adımları izleyin:

1. [Azure 'da bir video Analytics uygulaması oluşturma IoT Central](tutorial-video-analytics-create-app.md) öğreticisini doldurun:
    - Azure Media Services hesabı oluşturun.
    - Video analizi-nesne ve hareket algılama uygulama şablonundan IoT Central uygulamayı oluşturun.
    - IoT Central uygulamasında bir ağ geçidi cihazı yapılandırın. Ağ Geçidi, kamera cihazlarının uygulamaya bağlanmasına olanak sağlar.

1. [Video analizi için IoT Edge örneği oluşturma (LINUX VM)](tutorial-video-analytics-iot-edge-vm.md) öğreticisini şu şekilde doldurun:
    - Azure IoT Edge çalışma zamanı yüklü bir Azure VM oluşturun.-IoT Edge yüklemeyi video analizi modülünü barındıracak şekilde hazırlayın.
    - IoT Edge cihazını IoT Central uygulamanıza bağlayın.

1. [İzleme ve video analizi uygulamasını yönetme](tutorial-video-analytics-manage.md) öğreticisini doldurun ve şunları yapın:
    - IoT Central uygulamanızdaki ağ geçidine nesne ve hareket algılama kameraları ekleyin.
    - Kamera işlemesini başlatın.
    - Yakalanan Videoyu AMS 'de görüntülemek için yerel bir medya oynatıcı yükler.
    - Algılanan nesneleri gösteren yakalanan videoyu görüntüleyin.
    - Tidy.

## <a name="next-steps"></a>Sonraki adımlar

Artık, video analizi uygulama şablonunu dağıtma ve kullanma adımlarına genel bir bakış sunulmaktadır. başlamak için bkz. [Azure IoT Central bir video Analytics uygulaması oluşturma](tutorial-video-analytics-create-app.md) .
