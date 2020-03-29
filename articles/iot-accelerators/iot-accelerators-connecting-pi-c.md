---
title: C kullanarak Uzaktan İzleme için Prove Raspberry Pi - Azure | Microsoft Dokümanlar
description: C ile yazılmış bir uygulamayı kullanarak Bir Raspberry Pi cihazının Uzaktan İzleme çözüm hızlandırıcısına nasıl bağlanıştırılabildiğini açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 3331db51f4d141cf142d1bd0578043ca6681f3cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61454515"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Raspberry Pi cihazınızı Uzaktan İzleme çözüm hızlandırıcısına (C) bağlayın

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, gerçek bir aygıtı Uzaktan İzleme çözüm hızlandırıcısına nasıl bağlayabileceğinizi gösterir. Kısıtlı aygıtlarda çalışan çoğu gömülü uygulamada olduğu gibi, Raspberry Pi aygıt uygulamasının istemci kodu C olarak yazılır. Bu eğitimde, Raspbian işletim sistemi çalıştıran bir Raspberry Pi üzerinde uygulama oluşturmak.

Bir aygıtı simüle etmeyi tercih ederseniz, [bkz.](iot-accelerators-remote-monitoring-create-simulated-device.md)

### <a name="required-hardware"></a>Gerekli donanım

Raspberry Pi'deki komut satırına uzaktan bağlanmanızı sağlayan bir masaüstü bilgisayar.

Raspberry Pi 3 veya eşdeğer bileşenler [için Microsoft IoT Başlangıç Kiti.](https://azure.microsoft.com/develop/iot/starter-kits/) Bu öğretici, kitte aşağıdaki öğeleri kullanır:

- Ahududu Pi 3
- MicroSD Kart (NOOBS ile)
- USB Mini kablo
- Ethernet kablosu

### <a name="required-desktop-software"></a>Gerekli masaüstü yazılımı

Raspberry Pi'deki komut satırına uzaktan erişebilmeniz için masaüstü makinenizde SSH istemcisine ihtiyacınız vardır.

- Windows bir SSH istemcisi içermez. [Biz PuTTY](https://www.putty.org/)kullanmanızı öneririz.
- Çoğu Linux dağıtımı ve Mac OS komut satırı SSH yardımcı programı içerir. Daha fazla bilgi için Linux [veya Mac OS kullanarak SSH'ye](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md)bakın.

### <a name="required-raspberry-pi-software"></a>Gerekli Raspberry Pi yazılımı

Bu makalede, [Ahududu Pi üzerinde Raspbian işletim sistemi](https://www.raspberrypi.org/learning/software-guide/quickstart/)en son sürümünü yüklediğiniz varsayar.

Aşağıdaki adımlar, çözüm hızlandırıcısına bağlanan bir C uygulaması oluşturmak için Raspberry Pi'nizi nasıl hazırlayacağınızı gösterir:

1. **Ssh**kullanarak Raspberry Pi'nize bağlanın. Daha fazla bilgi için [Raspberry Pi web](https://www.raspberrypi.org/) [sitesindessh (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) bakın.

1. Raspberry Pi'nizi güncellemek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Bu nasıl yap'la ilgili kılavuzların adımlarını tamamlamak için, Raspberry Pi'nize gerekli geliştirme araçlarını ve kitaplıklarını eklemek için [Linux geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) adımlarını izleyin.

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
