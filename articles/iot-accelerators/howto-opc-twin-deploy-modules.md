---
title: Azure için OPC Twin modülü sıfırdan nasıl dağıtılır | Microsoft Dokümanlar
description: Bu makalede, Azure portalının IoT Edge bıçağını kullanarak ve ayrıca AZ CLI'yi kullanarak OPC Twin'in sıfırdan nasıl dağıtılanınca açıklanmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 6c8ceeaf49d8ebfa15a83118e8b518190f6ff85e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241067"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>OPC Twin modülve bağımlılıkları sıfırdan dağıtın

OPC Twin modülü IoT Edge üzerinde çalışır ve OPC aygıt ikiz ve kayıt hizmetleri için çeşitli kenar hizmetleri sağlar. 

Modülleri [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) Ağ Geçidinize dağıtmak için çeşitli seçenekler vardır:

- [Azure portalının IoT Edge bıçağından dağıtım](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [AZ CLI kullanarak dağıtma](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Dağıtım ayrıntıları ve yönergeleri hakkında daha fazla bilgi için GitHub [deposuna](https://github.com/Azure/azure-iiot-components)bakın.

## <a name="deployment-manifest"></a>Dağıtım bildirimi

Tüm modüller bir dağıtım bildirimi kullanılarak dağıtılır.  Hem [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) hem de [OPC Twin](https://github.com/Azure/azure-iiot-opc-twin-module) dağıtmak için bir örnek bildirim aşağıda gösterilmiştir.

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

## <a name="deploying-from-azure-portal"></a>Azure portalından dağıtım

Modülleri bir Azure IoT Edge ağ geçidi aygıtına dağıtmanın en kolay yolu Azure portalından geçer.  

### <a name="prerequisites"></a>Ön koşullar

1. OPC Twin [bağımlılıklarını](howto-opc-twin-deploy-dependencies.md) dağıtın ve `.env` ortaya çıkan dosyayı elde edin. Ortaya çıkan `hub name` `.env` dosyadaki `PCS_IOTHUBREACT_HUB_NAME` değişkenin dağıtıldığını unutmayın.

2. Bir [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) veya [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows) IoT Edge ağ `device id`geçidine kaydolun ve başlatın ve onun .

### <a name="deploy-to-an-edge-device"></a>Kenar aygıtına dağıtma

1. [Azure portalında](https://portal.azure.com/) oturum açın ve IoT hub'ınıza gidin.

2. Sol menüden **IoT Edge'i** seçin.

3. Aygıtlar listesinden hedef aygıtın kimliğine tıklayın.

4. **Modülleri Ayarlama**'yı seçin.

5. Sayfanın **Dağıtım modülleri** bölümünde **Ekle** ve **IoT Edge Modülü'nü seçin.**

6. **IoT Edge Özel Modül** iletişim `opctwin` kutusunda modül için ad olarak *Image URI* kullanın, ardından

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   *Kapsayıcı Oluşturma Seçenekleri*olarak, aşağıdaki JSON kullanın:

   ```json
   {"NetworkingConfig": {"EndpointsConfig": {"host": {}}}, "HostConfig": {"NetworkMode": "host" }}
   ```

   Gerekirse isteğe bağlı alanları doldurun. Kapsayıcı oluşturma seçenekleri hakkında daha fazla bilgi için, ilkeyi yeniden başlatın ve istenen durum [edgeagent istenen özelliklere](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties)bakın. Modül ikizi hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties)bkz.

7. **Kaydet** ve adımı **5'i**tekrarla'yı seçin.  

8. IoT Edge Özel Modül iletişim `opcpublisher` kutusunda, modül için ad olarak kullanın ve konteyner *görüntüsü URI* 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   *Kapsayıcı Oluşturma Seçenekleri*olarak, aşağıdaki JSON kullanın:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. Rotalar bölümüne devam etmek için **Kaydet'i** ve ardından **Sonraki'ni** seçin.

10. Rotalar sekmesine aşağıdaki leri yapıştırın 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
      }
    }
    ```

    ve **Sonraki'ni** seçin

11. Dağıtım bilgilerinizi gözden geçirin ve bildiriminizi.  Yukarıdaki dağıtım bildirimi gibi görünmelidir.  **Gönder**’i seçin.

12. Modülleri cihazınıza dağıttıktan sonra, tüm modülleri portalın **Aygıt ayrıntıları** sayfasında görüntüleyebilirsiniz. Bu sayfa, dağıtılan her modülün adının yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgileri görüntüler.

## <a name="deploying-using-azure-cli"></a>Azure CLI kullanarak dağıtma

### <a name="prerequisites"></a>Ön koşullar

1. Azure komut satırı [arabiriminin (AZ)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) en son sürümünü [buradan](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yükleyin.

### <a name="quickstart"></a>Hızlı Başlangıç

1. Yukarıdaki dağıtım bildirimini `deployment.json` bir dosyaya kaydedin.  

2. Yapılandırmayı bir IoT Edge aygıtına uygulamak için aşağıdaki komutu kullanın:

   ```azurecli
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   Parametre `device id` büyük/küçük harf duyarlıdır. İçerik parametresi, kaydettiğiniz dağıtım bildirimi dosyasına işaret eder. 
    ![az IoT Edge set-modülleri çıkışı](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Modülleri cihazınıza dağıttıktan sonra, aşağıdaki komutla tümmodülleri görüntüleyebilirsiniz:

   ```azurecli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   Aygıt kimliği parametresi büyük/küçük harf duyarlıdır. ![az iot hub modülü-kimlik listesi çıktı](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

OPC Twin'i sıfırdan nasıl dağıtabileceğinizi öğrendiğiniz için önerilen bir sonraki adım şunlardır:

> [!div class="nextstepaction"]
> [OPC Twin'i varolan bir projeye dağıtma](howto-opc-twin-deploy-existing.md)
