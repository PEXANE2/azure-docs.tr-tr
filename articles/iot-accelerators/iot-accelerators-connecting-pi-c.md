---
title: C-Azure kullanarak uzaktan Izlemeye Raspberry PI sağlama | Microsoft Docs
description: C dilinde yazılmış bir uygulama kullanılarak Raspberry PI cihazının uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağını açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 3331db51f4d141cf142d1bd0578043ca6681f3cd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61454515"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Raspberry PI cihazınızı uzaktan Izleme çözüm hızlandırıcısına bağlama (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihazın uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağı gösterilmektedir. Kısıtlanmış cihazlarda çalışan birçok katıştırılmış uygulamada olduğu gibi, Raspberry PI cihaz uygulaması için istemci kodu C dilinde yazılır. Bu öğreticide, uygulamayı Raspbian OS çalıştıran bir Raspberry Pi üzerinde oluşturacaksınız.

Bir cihazın benzetimini yapmayı tercih ediyorsanız, bkz. [Yeni bir sanal cihaz oluşturma ve test](iot-accelerators-remote-monitoring-create-simulated-device.md)etme.

### <a name="required-hardware"></a>Gerekli donanım

Raspberry PI 'deki komut satırına uzaktan bağlanmanızı sağlayan bir masaüstü bilgisayar.

[Raspberry PI 3 veya eşdeğer bileşenler Için Microsoft IoT başlangıç seti](https://azure.microsoft.com/develop/iot/starter-kits/) . Bu öğretici, Kit 'teki aşağıdaki öğeleri kullanır:

- Raspberry Pi 3
- Mikro SD kartı (NOOBS ile)
- Bir USB Mini kablosu
- Bir Ethernet kablosu

### <a name="required-desktop-software"></a>Gerekli masaüstü yazılımı

Raspberry PI 'deki komut satırına uzaktan erişmenizi sağlamak için masaüstü makinenizde SSH istemcisi gerekir.

- Windows bir SSH istemcisi içermez. [Putty](https://www.putty.org/)kullanmanızı öneririz.
- Çoğu Linux dağıtımları ve Mac OS, komut satırı SSH yardımcı programını içerir. Daha fazla bilgi için bkz. [Linux veya Mac OS kullanarak SSH](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Gerekli Raspberry PI yazılımı

Bu makalede, Raspbian OS 'ın en son sürümünü [Raspberry Pi 'nize](https://www.raspberrypi.org/learning/software-guide/quickstart/)yüklediğinizi varsayılmaktadır.

Aşağıdaki adımlarda çözüm hızlandırıcısına bağlanan bir C uygulaması oluşturmak için Raspberry PI 'nizi nasıl hazırlayacağınız gösterilmektedir:

1. **SSH**kullanarak Raspberry PI 'nize bağlanın. Daha fazla bilgi için [Raspberry PI Web sitesinde](https://www.raspberrypi.org/) [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) bölümüne bakın.

1. Raspberry PI 'nizi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için, Raspberry PI 'nize gerekli geliştirme araçları ve kitaplıklarını eklemek üzere [Linux geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) bölümündeki adımları izleyin.

## <a name="view-the-code"></a>Kodu görüntüleyin

Bu kılavuzda kullanılan [örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) Azure IoT C SDK GitHub deposunda bulunur.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirin ve projeyi hazırlayın

Projeyi hazırlamak için GitHub 'dan [Azure IoT C SDK deposunu](https://github.com/Azure/azure-iot-sdk-c) kopyalayın veya indirin.

Örnek, **Samples/Solutions/remote_monitoring_client** klasöründe bulunur.

Bir metin düzenleyicisinde **Samples/Solutions/remote_monitoring_client** klasöründe **remote_monitoring. c** dosyasını açın.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Aşağıdaki adımlarda, *CMake* 'in istemci uygulamasını oluşturmak için nasıl kullanılacağı açıklanır. Uzaktan izleme istemci uygulaması, SDK 'nın derleme sürecinin bir parçası olarak oluşturulur.

1. **remote_monitoring.c** `<connectionstring>` Çözüm hızlandırıcısına bir cihaz eklediğinizde, bu nasıl yapılır kılavuzunun başlangıcında not ettiğiniz cihaz bağlantı dizesiyle değiştirilecek remote_monitoring. c dosyasını düzenleyin.

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
