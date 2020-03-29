---
title: C- Azure'da Uzaktan İzlemeye Linux cihazlarını sağlama | Microsoft Dokümanlar
description: Linux üzerinde çalışan C ile yazılmış bir uygulamayı kullanarak bir aygıtın Uzaktan İzleme çözüm hızlandırıcısına nasıl bağlanıştırılabildiğini açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: dobett
ms.openlocfilehash: 91d4eda566c8b534daa10c62637db28ccb01bbb6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61454511"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-linux"></a>Cihazınızı Uzaktan İzleme çözüm hızlandırıcısına (Linux) bağlayın

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, gerçek bir aygıtı Uzaktan İzleme çözüm hızlandırıcısına nasıl bağlayabileceğinizi gösterir.

Kısıtlı aygıtlarda çalışan gömülü uygulamaların çoğunda olduğu gibi, aygıt uygulamasının istemci kodu C olarak yazılır. Bu eğitimde, ubuntu (Linux) çalıştıran bir makine üzerinde uygulama oluşturmak.

Bir aygıtı simüle etmeyi tercih ederseniz, [bkz.](iot-accelerators-remote-monitoring-create-simulated-device.md)

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için, Ubuntu sürüm 15.04 veya sonraki sürümçalıştıran bir aygıta ihtiyacınız vardır. İşleme başlamadan [önce, Linux geliştirme ortamınızı ayarlayın.](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux)

## <a name="view-the-code"></a>Kodu görüntüleme

Bu kılavuzda kullanılan [örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) Azure IoT C SDKs GitHub deposunda kullanılabilir.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirin ve projeyi hazırlayın

Projeyi hazırlamak için, Azure [IoT C SDKs deposunu](https://github.com/Azure/azure-iot-sdk-c) GitHub'dan klonlayın veya indirin.

Örnek, **örnekler/çözümler/remote_monitoring_client** klasöründe bulunur.

**remote_monitoring.c** dosyasını bir metin düzenleyicisinde **örnekler/çözümler/remote_monitoring_client** klasöründeki açın.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Aşağıdaki adımlar, istemci uygulamasını oluşturmak için *CMake'nin* nasıl kullanılacağını açıklar. Uzaktan izleme istemcisi uygulaması SDK için yapı sürecinin bir parçası olarak oluşturulur.

1. Çözüm hızlandırıcısına bir aygıt `<connectionstring>` eklediğinizde bu nasıl yapılacağını zedelerken belirttiğiniz aygıt bağlantı dizesiyle değiştirmek için **remote_monitoring.c** dosyasını edin.

1. [Azure IoT C SDKs deposunun](https://github.com/Azure/azure-iot-sdk-c) klonlanmış kopyanızın köküne gidin ve istemci uygulamasını oluşturmak için aşağıdaki komutları çalıştırın:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. İstemci uygulamasını çalıştırın ve telemetriyi IoT Hub'a gönderin:

    ```sh
    ./samples/solutions/remote_monitoring_client/remote_monitoring_client
    ```

    Konsol iletileri aşağıdaki gibi görüntüler:

    - Uygulama çözüm hızlandırıcısına örnek telemetri gönderir.
    - Çözüm panosundan çağrılan yöntemlere yanıt verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
