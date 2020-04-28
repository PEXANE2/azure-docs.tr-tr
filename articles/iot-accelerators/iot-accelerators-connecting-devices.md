---
title: C-Azure ' d e Windows cihazlarını uzaktan izlemeye sağlama | Microsoft Docs
description: Windows üzerinde çalışan C dilinde yazılmış bir uygulamayı kullanarak bir cihazın uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağını açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 2a8a0bf1e63f06bbe6b6a073af6b3da8904dcaeb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61450230"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Cihazınızı uzaktan Izleme çözüm hızlandırıcısına bağlama (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihazın uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağı gösterilmektedir.

Kısıtlanmış cihazlarda çalışan birçok katıştırılmış uygulamada olduğu gibi, cihaz uygulaması için istemci kodu C dilinde yazılır. Bu öğreticide, Windows çalıştıran bir makinede cihaz istemci uygulaması oluşturacaksınız.

Bir cihazın benzetimini yapmayı tercih ediyorsanız, bkz. [Yeni bir sanal cihaz oluşturma ve test](iot-accelerators-remote-monitoring-create-simulated-device.md)etme.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayarak, Windows makinenize gerekli geliştirme araçları ve kitaplıklarını eklemek için [Windows geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) bölümündeki adımları izleyin.

## <a name="view-the-code"></a>Kodu görüntüleyin

Bu kılavuzda kullanılan [örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) Azure IoT C SDK GitHub deposunda bulunur.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirin ve projeyi hazırlayın

Projeyi hazırlamak için [Azure IoT C SDK deposunu](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) GitHub 'dan kopyalayın.

Örnek, **Samples/Solutions/remote_monitoring_client** klasöründe bulunur.

Bir metin düzenleyicisinde **Samples/Solutions/remote_monitoring_client** klasöründe **remote_monitoring. c** dosyasını açın.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Çözüm hızlandırıcısına bir cihaz eklediğinizde, `<connectionstring>` bu nasıl yapılır kılavuzunun başlangıcında not ettiğiniz cihaz bağlantı dizesiyle değiştirilecek **remote_monitoring. c** dosyasını düzenleyin.

1. SDK 'yı ve uzaktan izleme istemci uygulamasını derlemek için [Windows 'Da C SDK oluşturma](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#build-the-c-sdk-in-windows) bölümündeki adımları izleyin.

1. Çözümü oluşturmak için kullandığınız komut isteminde şunu çalıştırın:

    ```cmd
    samples\solutions\remote_monitoring_client\Release\remote_monitoring_client.exe
    ```

    Konsol şu şekilde iletileri görüntüler:

    - Uygulama, çözüm hızlandırıcısına örnek telemetri gönderir.
    - Çözüm panosundan çağrılan yöntemlere yanıt verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
