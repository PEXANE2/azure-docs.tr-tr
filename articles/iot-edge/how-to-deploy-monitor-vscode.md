---
title: Visual Studio Code Azure IoT Edge kullanarak modülleri ölçekli olarak dağıtma
description: IoT Edge cihaz grupları için otomatik dağıtımlar oluşturmak üzere Visual Studio Code için IoT uzantısını kullanın.
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 1/8/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 08299a589dc6e8f768cba7ef976e109ef1fb69d7
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774139"
---
# <a name="deploy-iot-edge-modules-at-scale-using-visual-studio-code"></a>Visual Studio Code kullanarak ölçekli IoT Edge modülleri dağıtma

Aynı anda birçok cihaza yönelik devam eden dağıtımları yönetmek için Visual Studio Code kullanarak **IoT Edge otomatik dağıtım** oluşturabilirsiniz. IoT Edge için otomatik dağıtımlar IoT Hub [otomatik cihaz yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar birden çok modülü birden çok cihaza dağıtmanızı sağlayan dinamik işlemlerdir. Ayrıca, modüllerin durumunu ve sistem durumunu izleyebilir ve gerektiğinde değişiklik yapabilirsiniz.

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Bu makalede Visual Studio Code ve IoT uzantısını ayarlarsınız. Daha sonra, IoT Edge bir cihaz kümesine modül dağıtmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.
* Bir [IOT Edge cihazı](how-to-register-device.md#register-with-visual-studio-code) yüklü olan bir IOT Edge çalışma zamanı ile.
* [Visual Studio Code](https://code.visualstudio.com/).
* Visual Studio Code için [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools#overview) .

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

Hub 'ınız ile işlemler yapmak için Visual Studio Code için Azure IoT uzantıları 'nı kullanabilirsiniz. Bu işlemlerin çalışması için, Azure hesabınızda oturum açmanız ve üzerinde çalıştığınız IoT Hub 'ını seçmeniz gerekir.

1. Visual Studio Code'da açmak **Gezgini** görünümü.

1. Gezgin 'in alt kısmındaki **Azure IoT Hub** bölümünü genişletin.

1. **Azure IoT Hub** bölüm üstbilgisindeki **..** . öğesine tıklayın. Üç nokta simgesini görmüyorsanız, üst bilgisinin üzerinde gezdirin.

1. Seçin **IOT hub'ını seçin**.

1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için istemleri izleyin.

1. Azure aboneliğinizi seçin.

1. IOT hub'ınızı seçin.

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılacağını açıklayan bir JSON belgesidir. Ayrıca, modüller arasında verilerin nasıl akacağını ve modül TWINS 'in istenen özelliklerini açıklar. Daha fazla bilgi için bkz. [IoT Edge modül dağıtmayı ve yollar oluşturmayı öğrenin](module-composition.md).

Visual Studio Code kullanarak modüllerini dağıtmak için dağıtım bildirimi olarak yerel olarak kaydedin. bir. JSON dosyası. Yapılandırmayı cihazınıza uygulamak için komutunu çalıştırdığınızda konumunu belirtmeniz gerekecektir.

Örnek olarak bir modülü ile temel bir dağıtım bildirimi şöyledir:

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

Şu anda yapılandırabileceğiniz IoT Edge cihazları belirlemeniz gerekiyorsa **IoT Edge: cihaz bilgilerini al** komutunu çalıştırın.

## <a name="identify-devices-with-target-conditions"></a>Hedef koşullara sahip cihazları tanımla

Dağıtımı alacak IoT Edge cihazları tanımlamak için bir hedef koşul belirtmeniz gerekir. Belirtilen ölçüt bir DeviceID, etiket değeri veya bildirilen bir özellik değeri ile eşleştiğinde bir hedef koşul karşılanır.

Etiketleri cihaz ikizi yapılandırırsınız. Etiketlere sahip bir cihaz ikizi örneği aşağıda verilmiştir:

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

Bu cihaz, dağıtımın hedef koşulu, etiket değerlerinden biriyle eşleşen bir ifade içeriyorsa (`tag.location.building = '20'`gibi) bir dağıtım alacaktır.

Belirli bir cihazı etiketlerinden veya diğer değerlerinden bağımsız olarak hedeflemek istiyorsanız, hedef koşulun `deviceId` belirtmeniz yeterlidir.

Aşağıda bazı örnekler verilmiştir:

* DeviceID = 'linuxprod1'
* DeviceID = ' linuxprod1 ' veya DeviceID = ' linuxprod2 ' veya DeviceID = ' linuxprod3 '
* Tags.Environment = 'prod'
* Tags. Environment = ' prod ' ve Tags. Location = ' westus2 '
* Tags. Environment = ' prod ' veya Tags. Location = ' westus2 '
* Tags. operator = ' John ' ve Tags. Environment = ' prod ' ve NOT DeviceID = ' linuxprod1 '

Ayrıntılar için bkz. [hedef koşulu](module-deployment-monitoring.md#target-condition) . Cihaz ikizleri ve etiketleri hakkında daha fazla bilgi için bkz: [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).

### <a name="edit-the-device-twin"></a>Cihaz ikizi düzenleme

Etiketleri yapılandırmak için Visual Studio Code cihaz ikizi düzenleyebilirsiniz. **Görünüm** menüsünde, **komut paleti** ' ni seçin ve **IoT Edge: cihazı Düzenle ikizi** komutunu çalıştırın. IoT Edge cihazınızı seçin ve cihaz ikizi görüntülenir.

Bu örnekte, hiçbir etiket tanımlanmadı. Geçerli boş bölüm `"tags": {}` kendi etiket tanımınızla değiştirin.

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

Yerel dosyayı kaydettikten sonra **IoT Edge: Update Device ikizi** komutunu çalıştırın.

## <a name="create-deployment-at-scale"></a>Ölçekli dağıtım oluşturma

Dağıtım bildirimini yapılandırdıktan ve cihaz ikizi içinde Etiketler yapılandırdıktan sonra, dağıtmaya hazırsınız demektir.

1. **Görünüm** menüsünden **komut paleti** ' ni seçin ve **Azure IoT Edge: ölçek üzerinde dağıtım oluştur** komutunu seçin.

1. Kullanmak istediğiniz dağıtım bildirim JSON dosyasına gidin ve tıklayın **Edge dağıtım bildirimi seçin**.

1. **Dağıtım kimliğiyle**başlayarak, istenen değerleri girin.

   ![Dağıtım kimliği belirtin](./media/how-to-deploy-monitor-vscode/create-deployment-at-scale.png)

   Bu parametrelerin değerlerini belirtin:

  | Parametre | Açıklama |
  | --- | --- |
  | Dağıtım kimliği | IoT Hub 'ında oluşturulacak dağıtımın adı. Dağıtımınızı en çok 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`. |
  | Hedef koşul | Bu dağıtıma hangi cihazların hedeflenceğini belirleyen bir hedef koşul girin. Bu koşul, Device ikizi etiketlerine veya Device ikizi bildirilen özelliklerine dayalıdır ve ifade biçimiyle eşleşmelidir. Örneğin, `tags.environment='test' and properties.reported.devicemodel='4000x'`. |
  | Öncelik |  Pozitif bir tamsayı. Aynı cihaza iki veya daha fazla dağıtım hedeflenirse, öncelik için en yüksek sayısal değere sahip dağıtım uygulanır. |

  Önceliği belirttikten sonra, Terminal aşağıdaki gösterimi benzer bir çıktı görüntülemelidir:

   ```cmd
   [Edge] Start deployment with deployment id [{specified-value}] and target condition [{specified-value}]
   [Edge] Deployment with deployment id [{specified-value}] succeeded.
   ```

## <a name="monitoring-and-modifying-deployments"></a>Dağıtımları izleme ve değiştirme

Dağıtımları izlemek, değiştirmek ve silmek için [Azure CLI](how-to-deploy-monitor-cli.md#monitor-a-deployment) veya [Azure Portal](how-to-deploy-monitor.md#monitor-a-deployment) kullanın. Her ikisi de dağıtımlarınızla ilgili ölçümleri sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge cihazlara modül dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
