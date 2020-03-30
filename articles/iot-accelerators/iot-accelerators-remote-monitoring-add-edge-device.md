---
title: Uzaktan İzleme çözümü Edge cihazı eklemek - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcısına Bir IoT Edge aygıtının nasıl eklendiği açıklanmaktadır
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/09/2018
ms.topic: conceptual
ms.openlocfilehash: 0a42763ff47cccfa506acbbbd95d20d41eb0827f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72965384"
---
# <a name="add-an-iot-edge-device-to-your-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcınıza bir IoT Edge cihazı ekleme

Çözüm hızlandırıcınıza bir [IoT Edge](../iot-edge/about-iot-edge.md) aygıtı eklemek için aşağıdaki iki adımı tamamlayın:

1. Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde **Aygıt Gezgini** sayfasındaki Edge aygıtını ekleyin.
1. Edge cihazınıza IoT Edge çalışma süresini yükleyin.

## <a name="add-the-iot-edge-device"></a>IoT Edge aygıtını ekleme

Uzaktan İzleme çözüm hızlandırıcısına bir IoT Edge aygıtı eklemek için web Kullanıcı Arabirimi'ndeki **Aygıt Gezgini** sayfasına gidin ve **+ Yeni aygıtı**tıklatın.

Yeni **aygıt** panelinde, **IoT Edge aygıtını**seçin. Varsayılan değerleri diğer ayarlar için bırakabilirsiniz. Ardından **Apply** (Uygula) öğesine tıklayın:

![IoT Edge cihazı ekle](media/iot-accelerators-remote-monitoring-add-edge-device/addedgedevice.png)

### <a name="alternative-ways-to-add-an-iot-edge-device"></a>IoT Edge aygıtı eklemenin alternatif yolları

Ayrıca, çözüm hızlandırıcınızda doğrudan IoT Hub örneğine bir IoT Edge cihazı kaydettirebilirsiniz. Bu nasıl yapIleTilebilme kılavuzlarından herhangi birini izlemeden önce çözüm hızlandırıcınızdaki IoT hub'ının adını bilmeniz gerekir:

- [Azure portalından yeni bir Azure IoT Edge aygıtı kaydetme](../iot-edge/how-to-register-device.md#register-in-the-azure-portal)
- [Azure CLI ile yeni bir Azure IoT Edge aygıtı kaydetme](../iot-edge/how-to-register-device.md#register-with-the-azure-cli)
- [Visual Studio Kodu'ndan yeni bir Azure IoT Edge aygıtı kaydetme](../iot-edge/how-to-register-device.md#register-with-visual-studio-code)

Bir aygıtı uzaktan izleme çözüm hızlandırıcısında doğrudan IoT hub'ına kaydettirdiğinizde, bu aygıt web Kullanıcı Arabirimi'ndeki **Aygıt Gezgini** sayfasında listelenir.

## <a name="install-the-iot-edge-runtime"></a>IoT Edge çalışma süresini yükleme

Modülleri Edge cihazınıza dağıtmadan önce, IoT Edge çalışma süresini gerçek aygıta yüklemeniz gerekir. Aşağıdaki nasıl yapılacağını gösteren kılavuzlar, çalışma zamanının ortak aygıt platformlarına nasıl yüklenirolduğunu gösterir:

- [Azure IoT Edge çalışma süresini Linux'a yükleme (x64)](../iot-edge/how-to-install-iot-edge-linux.md)
- [Azure IoT Edge çalışma süresini Linux'a yükleme (ARM32v7/armhf)](../iot-edge/how-to-install-iot-edge-linux-arm.md)
- [Windows kapsayıcılarıyla kullanmak üzere Windows'da Azure IoT Edge çalışma süresini yükleme](../iot-edge/how-to-install-iot-edge-windows-with-windows.md)
- [Linux kapsayıcılarıyla kullanmak üzere Windows'da Azure IoT Edge çalışma süresini yükleme](../iot-edge/how-to-install-iot-edge-windows-with-linux.md)
- [Windows IoT Core'da IoT Edge çalışma süresini yükleme](../iot-edge/how-to-install-iot-core.md)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi IoT Edge cihazınızı hazırladınız, bir sonraki adım modülleri ona dağıtmaktır. Uzaktan [İzleme çözüm hızlandırıcınıza bir IoT Edge paketi alma](iot-accelerators-remote-monitoring-import-edge-package.md)
