---
title: C - Azure' da uzaktan izleme için Windows aygıtlarını sağlama | Microsoft Dokümanlar
description: Windows'da çalışan C ile yazılmış bir uygulamayı kullanarak aygıtın Uzaktan İzleme çözüm hızlandırıcısına nasıl bağlanıştırılabildiğini açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 2a8a0bf1e63f06bbe6b6a073af6b3da8904dcaeb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61450230"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Cihazınızı Uzaktan İzleme çözüm hızlandırıcısına (Windows) bağlayın

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, gerçek bir aygıtı Uzaktan İzleme çözüm hızlandırıcısına nasıl bağlayabileceğinizi gösterir.

Kısıtlı aygıtlarda çalışan gömülü uygulamaların çoğunda olduğu gibi, aygıt uygulamasının istemci kodu C olarak yazılır. Bu öğreticide, Windows çalıştıran bir makineüzerinde aygıt istemcisi uygulamasını oluşturursunuz.

Bir aygıtı simüle etmeyi tercih ederseniz, [bkz.](iot-accelerators-remote-monitoring-create-simulated-device.md)

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzları tamamlamak için, Windows makinenize gerekli geliştirme araçlarını ve kitaplıklarını eklemek için [Windows geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) adımlarını izleyin.

## <a name="view-the-code"></a>Kodu görüntüleme

Bu kılavuzda kullanılan [örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) Azure IoT C SDKs GitHub deposunda kullanılabilir.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirin ve projeyi hazırlayın

Projeyi hazırlamak için, [GitHub'dan Azure IoT C SDK'lar deposunu kopyalayın.](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment)

Örnek, **örnekler/çözümler/remote_monitoring_client** klasöründe bulunur.

**remote_monitoring.c** dosyasını bir metin düzenleyicisinde **örnekler/çözümler/remote_monitoring_client** klasöründeki açın.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Çözüm hızlandırıcısına bir aygıt `<connectionstring>` eklediğinizde bu nasıl yapılacağını zedelerken belirttiğiniz aygıt bağlantı dizesiyle değiştirmek için **remote_monitoring.c** dosyasını edin.

1. SDK ve uzaktan izleme istemcisi uygulamasını oluşturmak için [Windows'da C SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#build-the-c-sdk-in-windows) oluştur'daki adımları izleyin.

1. Çözümü oluşturmak için kullandığınız komut isteminde çalıştırın:

    ```cmd
    samples\solutions\remote_monitoring_client\Release\remote_monitoring_client.exe
    ```

    Konsol iletileri aşağıdaki gibi görüntüler:

    - Uygulama çözüm hızlandırıcısına örnek telemetri gönderir.
    - Çözüm panosundan çağrılan yöntemlere yanıt verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
