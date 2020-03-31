---
title: Azure CLI komut satırından modülleri dağıtma - Azure IoT Edge
description: IoT Hub'ınızdan ioT Edge modülünüzü bir dağıtım bildirimi tarafından yapılandırılan IoT Edge aygıtınıza itmek için Azure IoT Uzantısı ile Azure CLI'yi kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/16/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 619ba7cb2d99e0137fd1834096dd5b66ffcd6ec9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240377"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli"></a>Azure CLI ile Azure IoT Edge modüllerini dağıtma

İş mantığınızla IoT Edge modülleri oluşturduktan sonra, bunları kenarda çalışması için aygıtlarınıza dağıtmak istersiniz. Verileri toplamak ve işlemek için birlikte çalışan birden çok modülüz varsa, bunları aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirebilirsiniz.

[Azure CLI,](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) IoT Edge gibi Azure kaynaklarını yönetmek için açık kaynaklı çapraz platform komut satırı aracıdır. Azure IoT Hub kaynaklarını, aygıt sağlama hizmeti örneklerini ve bağlantılı hub'ları kutudan yönetmenize olanak tanır. Yeni IoT uzantısı, Azure CLI'yi aygıt yönetimi ve tam IoT Edge özelliği gibi özelliklerle zenginleştirir.

Bu makalede, json dağıtım bildirimi nin nasıl oluşturulacak, ardından dağıtımı bir IoT Edge aygıtına itmek için bu dosyayı kullanın. Paylaşılan etiketlerine göre birden çok cihazı hedefleyen bir dağıtım oluşturma hakkında bilgi [için](how-to-deploy-monitor-cli.md) bkz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-using-cli.md)
* IoT Edge çalışma zamanı yüklü bir [IoT Edge aygıtı.](how-to-register-device.md#register-with-the-azure-cli)
* Ortamınızda [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI sürümünüz en az 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI için IoT uzantısı.](https://github.com/Azure/azure-iot-cli-extension)

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Dağıtım bildirimlerinin nasıl çalıştığı ve bunların nasıl oluşturulabileceği hakkında daha fazla bilgi için [bkz.](module-composition.md)

Azure CLI'yi kullanarak modülleri dağıtmak için dağıtım bildirimini yerel olarak .json dosyası olarak kaydedin. Yapılandırmayı cihazınıza uygulamak için komutu çalıştırdığınızda bir sonraki bölümdeki dosya yolunu kullanırsınız.

Aşağıda, örnek olarak bir modüle sahip temel bir dağıtım bildirimi verilmiştir:

```json
{
  "content": {
    "modulesContent": {
      "$edgeAgent": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "runtime": {
            "type": "docker",
            "settings": {
              "minDockerVersion": "v1.25",
              "loggingOptions": "",
              "registryCredentials": {}
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": "{}"
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "SimulatedTemperatureSensor": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                "createOptions": "{}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "upstream": "FROM /messages/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      },
      "SimulatedTemperatureSensor": {
        "properties.desired": {
          "SendData": true,
          "SendInterval": 5
        }
      }
    }
  }
}
```

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

Modül bilgileriyle yapılandırdığınız dağıtım bildirimini uygulayarak modülleri cihazınıza dağıtırsınız.

Dizinleri dağıtım bildiriminizin kaydedildiği klasöre değiştirin. VS Code IoT Edge şablonlarından birini kullandıysanız, `deployment.json` `deployment.template.json` dosyayı dosyayı değil, çözüm dizininizin **config** klasöründeki kullanın.

Yapılandırmayı bir IoT Edge aygıtına uygulamak için aşağıdaki komutu kullanın:

   ```azurecli
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content [file path]
   ```

Aygıt kimliği parametresi büyük/küçük harf duyarlıdır. İçerik parametresi, kaydettiğiniz dağıtım bildirimi dosyasına işaret eder.

   ![az iot kenar set-modülleri çıkışı](./media/how-to-deploy-cli/set-modules.png)

## <a name="view-modules-on-your-device"></a>Cihazınızdaki modülleri görüntüleme

Modülleri cihazınıza dağıttıktan sonra, aşağıdaki komutla tümmodülleri görüntüleyebilirsiniz:

IoT Edge cihazınızda modülleri görüntüleme:

   ```azurecli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

Aygıt kimliği parametresi büyük/küçük harf duyarlıdır.

   ![az iot hub modülü-kimlik listesi çıktı](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge modüllerini ölçekte nasıl dağıtılayacağım ve izleyeceğiz](how-to-deploy-monitor.md)
