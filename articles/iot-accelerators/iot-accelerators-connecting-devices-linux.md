---
title: Linux cihazlarını C-Azure 'da uzaktan Izlemeye sağlama | Microsoft Docs
description: Linux üzerinde çalışan C 'de yazılmış bir uygulamayı kullanarak bir cihazın uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağını açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: dobett
ms.openlocfilehash: 91d4eda566c8b534daa10c62637db28ccb01bbb6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61454511"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-linux"></a>Cihazınızı uzaktan Izleme çözümü hızlandırıcısına bağlama (Linux)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihazın uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağı gösterilmektedir.

Kısıtlanmış cihazlarda çalışan birçok katıştırılmış uygulamada olduğu gibi, cihaz uygulaması için istemci kodu C dilinde yazılır. Bu öğreticide, Ubuntu (Linux) çalıştıran bir makinede uygulamayı oluşturacaksınız.

Bir cihazın benzetimini yapmayı tercih ediyorsanız, bkz. [Yeni bir sanal cihaz oluşturma ve test](iot-accelerators-remote-monitoring-create-simulated-device.md)etme.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için Ubuntu sürüm 15,04 veya üstünü çalıştıran bir cihaza ihtiyacınız vardır. Devam etmeden önce, [Linux geliştirme ortamınızı ayarlayın](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux).

## <a name="view-the-code"></a>Kodu görüntüleyin

Bu kılavuzda kullanılan [örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) Azure IoT C SDK GitHub deposunda bulunur.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirin ve projeyi hazırlayın

Projeyi hazırlamak için GitHub 'dan [Azure IoT C SDK deposunu](https://github.com/Azure/azure-iot-sdk-c) kopyalayın veya indirin.

Örnek, **Samples/Solutions/remote_monitoring_client** klasöründe bulunur.

Bir metin düzenleyicisinde **Samples/Solutions/remote_monitoring_client** klasöründe **remote_monitoring. c** dosyasını açın.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Aşağıdaki adımlarda, *CMake* 'in istemci uygulamasını oluşturmak için nasıl kullanılacağı açıklanır. Uzaktan izleme istemci uygulaması, SDK 'nın derleme sürecinin bir parçası olarak oluşturulur.

1. Çözüm hızlandırıcısına bir cihaz eklediğinizde, `<connectionstring>` bu nasıl yapılır kılavuzunun başlangıcında not ettiğiniz cihaz bağlantı dizesiyle değiştirilecek **remote_monitoring. c** dosyasını düzenleyin.

1. [Azure IoT C SDK 'sı depo](https://github.com/Azure/azure-iot-sdk-c) deposunun kopyalanmış kopyasının köküne gidin ve istemci uygulamasını derlemek için aşağıdaki komutları çalıştırın:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. İstemci uygulamasını çalıştırın ve IoT Hub telemetri gönderin:

    ```sh
    ./samples/solutions/remote_monitoring_client/remote_monitoring_client
    ```

    Konsol şu şekilde iletileri görüntüler:

    - Uygulama, çözüm hızlandırıcısına örnek telemetri gönderir.
    - Çözüm panosundan çağrılan yöntemlere yanıt verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
