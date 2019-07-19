---
title: Azure için OPC Ikizi modülünü sıfırdan dağıtma | Microsoft Docs
description: OPC Ikizi 'i sıfırdan dağıtma.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: df1dd45d58baf82710b5e362afaf055aad140b98
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302635"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>OPC Ikizi modülünü ve bağımlılıklarını sıfırdan dağıtma

OPC Ikizi modülü IoT Edge üzerinde çalışır ve OPC cihaz ikizi ve kayıt defteri Hizmetleri için birkaç Edge hizmeti sağlar. 

[Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) ağ geçidinize modül dağıtmak için çeşitli seçenekler vardır

- [Azure portal IoT Edge dikey penceresinden dağıtma](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [AZ CLı kullanarak dağıtma](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Dağıtım ayrıntıları ve yönergeleri hakkında daha fazla bilgi için bkz. GitHub [deposu](https://github.com/Azure/azure-iiot-components).

## <a name="deployment-manifest"></a>Dağıtım bildirimi

Tüm modüller bir dağıtım bildirimi kullanılarak dağıtılır.  Aşağıdaki [OPC yayımcısı](https://github.com/Azure/iot-edge-opc-publisher) ve [OPC ikizi](https://github.com/Azure/azure-iiot-opc-twin-module) dağıtmak için örnek bir bildirim aşağıda gösterilmiştir.

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
                "createOptions": ""
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "opctwin": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-twin:latest",
                "createOptions": "{\"NetworkingConfig\": {\"EndpointsConfig\": {\"host\": {}}}, \"HostConfig\": {\"NetworkMode\": \"host\" }}"
              }
            },
            "opcpublisher": {
              "version": "2.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-publisher:latest",
                "createOptions": "{\"Hostname\":\"publisher\",\"Cmd\":[\"publisher\",\"--pf=./pn.json\",\"--di=60\",\"--to\",\"--aa\",\"--si=0\",\"--ms=0\"],\"ExposedPorts\":{\"62222/tcp\":{}},\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"PortBindings\":{\"62222/tcp\":[{\"HostPort\":\"62222\"}]}}}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
            "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      }
    }
  }
}
```

## <a name="deploying-from-azure-portal"></a>Azure portal dağıtma

Azure IoT Edge ağ geçidi cihazına modülleri dağıtmanın en kolay yolu, Azure portal.  

### <a name="prerequisites"></a>Önkoşullar

1. OPC ikizi [bağımlılıklarını](howto-opc-twin-deploy-dependencies.md) dağıtın ve elde edilen `.env` dosyayı elde edin. `PCS_IOTHUBREACT_HUB_NAME` Değişkenin elde edilen `hub name`dosyadadağıtıldığınıaklınızdaedin `.env` .

2. Bir [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) veya [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows) IoT Edge ağ geçidini kaydedin ve başlatın ve konumunu `device id`aklınızda yapın.

### <a name="deploy-to-an-edge-device"></a>Sınır cihazına dağıtma

1. Oturum [Azure portalında](https://portal.azure.com/) ve IOT hub'ınıza gidin.

2. Sol taraftaki menüden **IoT Edge** ' yi seçin.

3. Hedef cihazın cihazlar listesinden numarasını tıklayın.

4. **Modülleri Ayarlama**'yı seçin.

5. Sayfanın **dağıtım modülleri** bölümünde, **Ekle** ve **IoT Edge modülünü seçin.**

6. **IoT Edge özel modül** iletişim kutusunda modül için `opctwin` ad olarak kullanın, sonra kapsayıcı *görüntüsü URI* 'sini şu şekilde belirtin

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   *Kapsayıcı oluşturma seçenekleri*olarak aşağıdaki JSON 'u kullanın:

   ```json
   {"NetworkingConfig": {"EndpointsConfig": {"host": {}}}, "HostConfig": {"NetworkMode": "host" }}
   ```

   Gerekirse, isteğe bağlı alanları doldurun. Kapsayıcı hakkında daha fazla bilgi seçenekleri, yeniden başlatma ilkesi oluşturabilir ve istenen durumunu görmek için [EdgeAgent istenen özelliklerini](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties). Modül ikizi hakkında daha fazla bilgi için bkz. [tanımlayın veya güncelleştirme istenen özelliklerini](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

7. **Kaydet** ' i seçin ve **5**. adımı yineleyin.  

8. IoT Edge özel modül iletişim kutusunda modül için as `opcpublisher` adı ve kapsayıcı *görüntüsü URI 'si* olarak kullanın 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   *Kapsayıcı oluşturma seçenekleri*olarak aşağıdaki JSON 'u kullanın:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. **Kaydet** ' i ve ardından yollar bölümüne devam etmek için **İleri** ' yi seçin.

10. Rotalar sekmesinde, aşağıdakileri yapıştırın 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
      }
    }
    ```

    ve **İleri ' yi** seçin

11. Dağıtım bilgilerinizi ve bildirimini gözden geçirin.  Yukarıdaki dağıtım bildirimi gibi görünmelidir.  Seçin **gönderme**.

12. Cihazınıza modülleri dağıttıktan sonra bunların tümünün görüntüleyebilirsiniz **cihaz ayrıntıları** portal sayfası. Bu sayfa, her dağıtılan modülü yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgiler adını görüntüler.

## <a name="deploying-using-azure-cli"></a>Azure CLı kullanarak dağıtma

### <a name="prerequisites"></a>Önkoşullar

1. [Azure komut satırı arabirimi 'nin (az)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) en son sürümünü [buradan](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yüklersiniz.

### <a name="quickstart"></a>Hızlı Başlangıç

1. Yukarıdaki dağıtım bildirimini bir `deployment.json` dosyaya kaydedin.  

2. IOT Edge cihazına yapılandırmayı uygulamak için aşağıdaki komutu kullanın:

   ```bash
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   `device id` Parametresi, büyük/küçük harfe duyarlıdır. İçerik parametresi dağıtım noktalarına bildirim kaydettiğiniz dosyası. 
    ![az IoT Edge set-modules çıktısı](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Cihazınıza modülleri dağıttıktan sonra tüm bunları aşağıdaki komutla görebilirsiniz:

   ```bash
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   Cihaz KIMLIĞI parametresi, büyük/küçük harfe duyarlıdır. ![az IOT hub kimlik modülü liste çıkışı](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Ikizi 'in sıfırdan nasıl dağıtılacağını öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC Ikizi 'yi mevcut bir projeye dağıtma](howto-opc-twin-deploy-existing.md)
