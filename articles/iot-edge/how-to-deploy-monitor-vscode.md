---
title: Visual Studio Code - Azure IoT Edge kullanarak modülleri ölçekte dağıtın
description: IoT Edge aygıtgrupları için otomatik dağıtımlar oluşturmak için Visual Studio Code için IoT uzantısını kullanın.
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 1/8/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 08299a589dc6e8f768cba7ef976e109ef1fb69d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774139"
---
# <a name="deploy-iot-edge-modules-at-scale-using-visual-studio-code"></a>Visual Studio Code kullanarak IoT Edge modüllerini ölçekte dağıtın

Aynı anda birçok cihaz için devam eden dağıtımları yönetmek için Visual Studio Code'u kullanarak **bir IoT Edge otomatik dağıtım** oluşturabilirsiniz. IoT Edge için otomatik dağıtımlar, IoT Hub'ın [otomatik aygıt yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar, birden çok modüle birden fazla modül dağıtmanızı sağlayan dinamik işlemlerdir. Ayrıca modüllerin durumunu ve sistem durumunu izleyebilir ve gerektiğinde değişiklik yapabilirsiniz.

Daha fazla bilgi için bkz: [Tek aygıtlar için veya ölçekte IoT Edge otomatik dağıtımları anlayın.](module-deployment-monitoring.md)

Bu makalede, Visual Studio Code ve IoT uzantısını ayarlarsınız. Daha sonra modülleri bir dizi IoT Edge aygıtına nasıl dağıtabileceğinizi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-through-portal.md)
* IoT Edge çalışma zamanı yüklü bir [IoT Edge aygıtı.](how-to-register-device.md#register-with-visual-studio-code)
* [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
* Visual Studio Code için [Azure IoT Araçları.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools#overview)

## <a name="sign-in-to-access-your-iot-hub"></a>IoT hub'ınıza erişmek için oturum açın

Hub'ınızla işlem yapmak için Visual Studio Code için Azure IoT uzantılarını kullanabilirsiniz. Bu işlemlerin işe yaraması için Azure hesabınızda oturum açmanız ve üzerinde çalıştığınız IoT merkezini seçmeniz gerekir.

1. Visual Studio Code'da **Explorer** görünümünü açın.

1. Explorer'ın alt kısmında **Azure IoT Hub** bölümünü genişletin.

1. **Azure IoT Hub** **bölümündeki ...** bölümünü tıklatın. Elipsleri görmüyorsanız, başlığın üzerine gidin.

1. **IoT Hub'ı seçin.**

1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için istemleri izleyin.

1. Azure aboneliğinizi seçin.

1. IoT hub'ınızı seçin.

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılanmasını açıklayan bir JSON belgesidir. Ayrıca, modüller arasında veri akışını ve modülün istenilen özelliklerini de açıklar. Daha fazla bilgi için [bkz.](module-composition.md)

Visual Studio Code kullanarak modülleri dağıtmak için, dağıtım bildirimini yerel olarak kaydedin. JSON dosyası. Yapılandırmayı cihazınıza uygulamak için komutu çalıştırdığınızda konumunu sağlamanız gerekir.

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

Şu anda yapılandırabileceğiniz Hangi IoT Edge aygıtlarını belirlemeniz gerekiyorsa, **IoT Edge: Get Device Info** komutunu çalıştırın.

## <a name="identify-devices-with-target-conditions"></a>Hedef koşullara sahip cihazları tanımlama

Dağıtımı alacak Olan IoT Edge aygıtlarını tanımlamak için bir hedef koşulu belirtmeniz gerekir. Belirtilen ölçütler deviceId, etiket değeri veya bildirilen özellik değeriyle eşleştiğinde hedef koşul karşılanır.

Aygıt ikizindeki etiketleri yapılandırırsınız. Etiketleri olan bir aygıt ikizi örneği aşağıda verilmiştir:

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

Dağıtım için hedef koşul etiketin değerlerinden biriyle eşleşen bir ifade içeriyorsa, bu `tag.location.building = '20'`aygıt bir dağıtım alır.

Belirli bir aygıtı, etiketleri veya diğer değerlerinden bağımsız olarak `deviceId` hedeflemek istiyorsanız, hedef koşulu belirtmeniz gerekir.

Aşağıda daha fazla örnek verilmiştir:

* deviceId ='linuxprod1'
* deviceId = 'linuxprod1' OR deviceId = 'linuxprod2' OR deviceId = 'linuxprod3'
* tags.environment ='prod'
* tags.environment = 'prod' VE tags.location = 'westus2'
* tags.environment = 'prod' OR tags.location = 'westus2'
* tags.operator = 'John' VE tags.environment = 'prod' VE NOT deviceId = 'linuxprod1'

Ayrıntılar için [hedef koşula](module-deployment-monitoring.md#target-condition) bakın. Aygıt ikizleri ve etiketleri hakkında daha fazla bilgi [için, IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)

### <a name="edit-the-device-twin"></a>Aygıtı ikizi olarak edin

Etiketleri yapılandırmak için aygıt ikizini Visual Studio Code'da ayarlayabilirsiniz. **Görünüm** menüsünden **Komut Paleti'ni** seçin ve **IoT Edge: Aygıt İkiz** komutunu edin. IoT Edge cihazınızı seçin ve aygıt ikizi görünür.

Bu örnekte, hiçbir etiket tanımlanmamıştır. Geçerli boş bölümü `"tags": {}` kendi etiket tanımınızla değiştirin.

```json
{
    "deviceId": "myEdgeDevice",
    "etag": "AAAAAAAAAAE=",
    "deviceEtag": "NTgwMDg5MDAz",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00Z",
    "connectionState": "Disconnected",
    "lastActivityTime": "0001-01-01T00:00:00Z",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "version": 2,
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2019-12-29T00:58:49.9315265Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2019-12-29T00:58:49.9315265Z"
            },
            "$version": 1
        }
    },
    "capabilities": {
        "iotEdge": true
    },
    "deviceScope": "ms-azure-iot-edge://myEdgeDevice-637131779299315265",
    "tags": {}
}
```

Yerel dosyayı kurtardıktan sonra **IoT Edge: Update Device Twin** komutunu çalıştırın.

## <a name="create-deployment-at-scale"></a>Ölçekte dağıtım oluşturma

Dağıtım bildirimini ve aygıt ikizindeki etiketleri yapılandırdıktan sonra dağıtmaya hazırsınız.

1. **Görünüm** menüsünden **Komut Paleti'ni** seçin ve **Azure IoT Edge: Ölçek komutunda Dağıtım Oluştur'u** seçin.

1. Kullanmak istediğiniz dağıtım bildirimi JSON dosyasına gidin ve **Kenar Dağıtım Bildirimi'ni seçin'i**tıklatın.

1. **Dağıtım kimliğinden**başlayarak, istendiği gibi değerler sağlayın.

   ![Dağıtım kimliği belirtin](./media/how-to-deploy-monitor-vscode/create-deployment-at-scale.png)

   Bu parametreler için değerleri belirtin:

  | Parametre | Açıklama |
  | --- | --- |
  | Dağıtım kimliği | IoT hub'ında oluşturulacak dağıtımın adı. Dağıtımınıza en fazla 128 küçük harf içeren benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`. |
  | Hedef koşul | Bu dağıtımda hangi aygıtların hedefleneceğini belirlemek için bir hedef koşulu girin.Koşul, aygıt ikiz etiketlerine veya aygıt ikizi bildirilen özelliklere dayanır ve ifade biçimiyle eşleşmelidir.Örneğin, `tags.environment='test' and properties.reported.devicemodel='4000x'`. |
  | Öncelik |  Pozitif bir tamsayı. İki veya daha fazla dağıtım aynı aygıtta hedeflenirse, Öncelik için en yüksek sayısal değere sahip dağıtım uygulanır. |

  Önceliği belirttikten sonra, terminal aşağıdaki tasvire benzer çıktı göstermelidir:

   ```cmd
   [Edge] Start deployment with deployment id [{specified-value}] and target condition [{specified-value}]
   [Edge] Deployment with deployment id [{specified-value}] succeeded.
   ```

## <a name="monitoring-and-modifying-deployments"></a>Dağıtımları izleme ve değiştirme

Dağıtımları izlemek, değiştirmek ve silmek için [Azure CLI'yi](how-to-deploy-monitor-cli.md#monitor-a-deployment) veya [Azure portalını](how-to-deploy-monitor.md#monitor-a-deployment) kullanın. Her ikisi de dağıtımlarınızda ölçümler sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Modülleri IoT Edge aygıtlarına dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
