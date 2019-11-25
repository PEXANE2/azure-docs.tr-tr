---
title: Translate modbus protocols with gateways - Azure IoT Edge | Microsoft Docs
description: IoT Edge ağ geçidi cihazı oluşturarak Modbus TCP kullanan cihazların Azure IoT Hub ile iletişim kurmasına imkan sağlama
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: kgremban
ms.openlocfilehash: 649c7f620b83464d1bb56cf4b8191b0747105f01
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457204"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Connect Modbus TCP devices through an IoT Edge device gateway

Modbus TCP veya RTU protokollerini kullanan IoT cihazlarını bir Azure IoT hub’ına bağlamak istiyorsanız ağ geçidi olarak bir IoT Edge cihazı kullanın. Ağ geçidi cihazı Modbus cihazlarınızdan verileri okur, sonra desteklenen bir protokolü kullanarak bu verileri buluta iletir.

![Modbus devices connect to IoT Hub through IoT Edge gateway](./media/deploy-modbus-gateway/diagram.png)

Bu makalede, bir Modbus modülü için kendi kapsayıcı görüntünüzü oluşturma (dilerseniz önceden oluşturulmuş bir örneği de kullanabilirsiniz) ve bu görüntüyü ağ geçidi olarak kullanacağınız IoT Edge cihazına dağıtma işlemleri açıklanmaktadır.

Bu makalede Modbus TCP protokolünü kullandığınız varsayılır. For more information about how to configure the module to support Modbus RTU, see the [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) project on GitHub.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure IoT Edge cihazı. For a walkthrough on how to set up one, see [Deploy Azure IoT Edge on Windows](quickstart.md) or [Linux](quickstart-linux.md).
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.
* Modbus TCP’yi destekleyen fiziksel veya sanal bir cihaz.

## <a name="prepare-a-modbus-container"></a>Modbus kapsayıcısı hazırlama

Modbus ağ geçidinin işlevselliğini test etmek istiyorsanız Microsoft tarafından sağlanan örnek modülü kullanabilirsiniz. You can access the module from the Azure Marketplace, [Modbus](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview), or with the image URI, **mcr.microsoft.com/azureiotedge/modbus:1.0**.

If you want to create your own module and customize it for your environment, there is an open-source [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) project on GitHub. Kendi kapsayıcı görüntünüzü oluşturmak için bu projedeki yönergeleri izleyin. To create a container image, refer to [Develop C# modules in Visual Studio](how-to-visual-studio-develop-csharp-module.md) or [Develop modules in Visual Studio Code](how-to-vs-code-develop-module.md). Those articles provide instructions on creating new modules and publishing container images to a registry.

## <a name="try-the-solution"></a>Try the solution

This section walks through deploying Microsoft's sample Modbus module to your IoT Edge device.

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınıza gidin.

2. Go to **IoT Edge** and click on your IoT Edge device.

3. **Modül ayarla**’yı seçin.

4. Modbus modülünü ekleyin:

   1. Click **Add** and select **IoT Edge module**.

   2. **Ad** alanına "modbus" yazın.

   3. **Resim** alanına örnek kapsayıcının resim URI’sini girin: `mcr.microsoft.com/azureiotedge/modbus:1.0`.

   4. Modül ikizinin istenen özelliklerini güncelleştirmek için **Etkinleştir** kutusunu işaretleyin.

   5. Aşağıdaki JSON’u metin kutusuna kopyalayın. **SlaveConnection** değerini Modbus cihazınızın IPv4 adresi olarak değiştirin.

      ```JSON
      {
        "properties.desired":{
          "PublishInterval":"2000",
          "SlaveConfigs":{
            "Slave01":{
              "SlaveConnection":"<IPV4 address>","HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{
                "Op01":{
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"40001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. **Kaydet**’i seçin.

5. **Modül Ekle** adımına dönüp **İleri**’yi seçin.

7. **Yolları Belirtin** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Bu yol, Modbus modülü tarafından toplanan tüm iletileri IoT Hub’a gönderir. In this route, **modbusOutput** is the endpoint that Modbus module uses to output data and **$upstream** is a special destination that tells IoT Edge hub to send messages to IoT Hub.

   ```JSON
   {
     "routes": {
       "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
     }
   }
   ```

8. **İleri**’yi seçin.

9. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.

10. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. You should see the new **modbus** module running along with the IoT Edge runtime.

## <a name="view-data"></a>Verileri görüntüleme
Modbus modülü üzerinden gelen verileri görüntüleyin:
```cmd/sh
iotedge logs modbus
```

You can also view the telemetry the device is sending by using the [Azure IoT Hub Toolkit extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (formerly Azure IoT Toolkit extension).

## <a name="next-steps"></a>Sonraki adımlar

- To learn more about how IoT Edge devices can act as gateways, see [Create an IoT Edge device that acts as a transparent gateway](./how-to-create-transparent-gateway.md).
- For more information about how IoT Edge modules work, see [Understand Azure IoT Edge modules](iot-edge-modules.md).
