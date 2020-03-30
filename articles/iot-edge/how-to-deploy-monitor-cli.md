---
title: Azure CLI - Azure IoT Edge kullanarak modülleri ölçekte dağıtın
description: IoT Edge aygıt grupları için otomatik dağıtımlar oluşturmak için Azure CLI için IoT uzantısını kullanın
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/20/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9152b38a0155b610f39f7de239bcc377ad96be5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271479"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Azure CLI'yi kullanarak IoT Edge modüllerini ölçekte dağıtın ve izleyin

Birçok cihaz için aynı anda devam eden dağıtımları yönetmek için Azure komut satırı arabirimini kullanarak bir **IoT Edge otomatik dağıtım** oluşturun. IoT Edge için otomatik dağıtımlar, IoT Hub'ın [otomatik aygıt yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar, birden çok cihaza birden fazla modül dağıtmanızı, modüllerin durumunu ve sistem durumunu izlemenizi ve gerektiğinde değişiklik yapmanızı sağlayan dinamik işlemlerdir.

Daha fazla bilgi için bkz: [Tek aygıtlar için veya ölçekte IoT Edge otomatik dağıtımları anlayın.](module-deployment-monitoring.md)

Bu makalede, Azure CLI ve IoT uzantısını ayarlarsınız. Daha sonra modülleri bir ioT Edge aygıtı kümesine nasıl dağıtabileceğinizi ve kullanılabilir CLI komutlarını kullanarak ilerlemeyi izlemeyi öğrenirsiniz.

## <a name="cli-prerequisites"></a>CLI Ön Koşulları

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-using-cli.md)
* [IoT Edge](how-to-register-device.md#prerequisites-for-the-azure-cli) çalışma zamanı yüklü Olan IoT Edge aygıtları.
* Ortamınızda [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI sürümünüz en az 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI için IoT uzantısı.](https://github.com/Azure/azure-iot-cli-extension)

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Daha fazla bilgi için [bkz.](module-composition.md)

Azure CLI kullanarak modülleri dağıtmak için dağıtım bildirimini yerel olarak .txt dosyası olarak kaydedin. Yapılandırmayı cihazınıza uygulamak için komutu çalıştırdığınızda bir sonraki bölümdeki dosya yolunu kullanırsınız.

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

## <a name="layered-deployment"></a>Katmanlı dağıtım

Katmanlı dağıtımlar, üst üste yığılabilen bir otomatik dağıtım türüdür. Katmanlı dağıtımlar hakkında daha fazla bilgi için bkz: [Tek aygıtlar için veya ölçekte IoT Edge otomatik dağıtımları anlayın.](module-deployment-monitoring.md)

Katmanlı dağıtımlar, herhangi bir otomatik dağıtım gibi Azure CLI ile oluşturulabilir ve yönetilebilir ve sadece birkaç farklılık la birlikte. Katmanlı bir dağıtım oluşturulduktan sonra, katmanlı dağıtımlar için aynı Azure CLI çalışır. Katmanlı dağıtım oluşturmak için, `--layered` oluşturma komutuna bayrağı ekleyin.

İkinci fark dağıtım manifestosunun yapımındadır. Standart otomatik dağıtım, herhangi bir kullanıcı modülüne ek olarak sistem çalışma zamanı modüllerini içermelidir, ancak katmanlı dağıtımlar yalnızca kullanıcı modülleri içerebilir. Bunun yerine, katmanlı dağıtımların, sistem çalışma zamanı modülleri gibi her IoT Edge aygıtının gerekli bileşenlerini sağlamak için standart bir otomatik dağıtımın da bir aygıtta olması gerekir.

Aşağıda, örnek olarak bir modüle sahip temel katmanlı dağıtım bildirimi verilmiştir:

```json
{
  "content": {
    "modulesContent": {
      "$edgeAgent": {
        "properties.desired.modules.SimulatedTemperatureSensor": {
          "settings": {
            "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
              "createOptions": ""
          },
          "type": "docker",
          "status": "running",
          "restartPolicy": "always",
          "version": "1.0"
        }
      },
      "$edgeHub": {
        "properties.desired.routes.upstream": "FROM /messages/* INTO $upstream"
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

Önceki örnekte, bir modül `properties.desired` için ayar lanan katmanlı bir dağıtım gösterilmiştir. Bu katmanlı dağıtım, aynı modülün zaten uygulandığı bir aygıtı hedefliyorsa, varolan tüm istenen özelliklerin üzerine yazar. Güncelleştirmek için, üzerine yazmak yerine, istenen özellikleri, yeni bir alt bölüm tanımlayabilirsiniz. Örnek:

```json
"SimulatedTEmperatureSensor": {
  "properties.desired.layeredProperties": {
    "SendData": true,
    "SendInterval": 5
  }
}
```

Katmanlı dağıtımlarda modül ikizlerini yapılandırma hakkında daha fazla bilgi için katmanlı [dağıtım](module-deployment-monitoring.md#layered-deployment)

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları tanımlama

Dağıtım oluşturmadan önce, hangi aygıtları etkilemek istediğinizi belirtemeniz gerekir. Azure IoT Edge, aygıt ikizinde **etiketleri** kullanan aygıtları tanımlar. Her aygıtın, çözümünüz için anlamlı olan herhangi bir şekilde tanımladığınız birden çok etiketi olabilir. Örneğin, akıllı binalardan oluşan bir kampüsü yönetiyorsanız, bir aygıta aşağıdaki etiketleri ekleyebilirsiniz:

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

Aygıt ikizleri ve etiketleri hakkında daha fazla bilgi [için, IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)

## <a name="create-a-deployment"></a>Dağıtım oluşturma

Dağıtım bildiriminin yanı sıra diğer parametrelerden oluşan bir dağıtım oluşturarak modülleri hedef aygıtlarınıza dağılarsınız.

Bir dağıtım oluşturmak için [az iot kenar dağıtım oluşturma](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-create) komutunu kullanın:

```cli
az iot edge deployment create --deployment-id [deployment id] --hub-name [hub name] --content [file path] --labels "[labels]" --target-condition "[target query]" --priority [int]
```

Katmanlı bir dağıtım `--layered` met oluşturmak için bayrakla aynı komutu kullanın.

Dağıtım oluşturma komutu aşağıdaki parametreleri alır:

* **--katmanlı** - Dağıtımı katmanlı dağıtım olarak tanımlamak için isteğe bağlı bir bayrak.
* **--deployment-id** - IoT hub'ında oluşturulacak dağıtımın adı. Dağıtımınıza en fazla 128 küçük harf içeren benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`. Gerekli parametre.
* **--içerik** - Dağıtım bildirimi JSON için Filepath. Gerekli parametre.
* **--hub-name** - Dağıtımın oluşturulacağı IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutla geçerli aboneliğinizi değiştirin. `az account set -s [subscription name]`
* **--etiketler** - Dağıtımlarınızı izlemenize yardımcı olmak için etiket ekleyin. Etiketler, dağıtımınızı açıklayan Ad, Değer çiftleridir. Etiketler, adlar ve değerler için JSON biçimlendirmesini alır. Örneğin, `{"HostPlatform":"Linux", "Version:"3.0.1"}`
* **--hedef koşulu** - Bu dağıtımda hangi aygıtların hedefleneceğini belirlemek için bir hedef koşulu girin.Koşul, aygıt ikiz etiketlerine veya aygıt ikizi bildirilen özelliklere dayanır ve ifade biçimiyle eşleşmelidir.Örneğin, `tags.environment='test' and properties.reported.devicemodel='4000x'`.
* **--öncelik** - Pozitif tamsayı. İki veya daha fazla dağıtımın aynı aygıtta hedeflanması durumunda, Öncelik için en yüksek sayısal değere sahip dağıtım uygulanır.
* **--ölçümler** - Dağıtım durumunu izlemek için edgeHub bildirilen özellikleri sorgulayan ölçümler oluşturun. Ölçümler JSON girişi veya bir dosya yolu alır. Örneğin, `'{"queries": {"mymetric": "SELECT deviceId FROM devices WHERE properties.reported.lastDesiredStatus.code = 200"}}'`.

## <a name="monitor-a-deployment"></a>Dağıtımı izleme

Tek bir dağıtımın ayrıntılarını görüntülemek için [az iot edge dağıtım göster](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show) komutunu kullanın:

```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
```

Dağıtım gösterisi komutu aşağıdaki parametreleri alır:

* **--deployment-id** - IoT hub'ında bulunan dağıtımın adı. Gerekli parametre.
* **--hub-name** - Dağıtımın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutile istenilen aboneye geçme`az account set -s [subscription name]`

Komut penceresindeki dağıtımı inceleyin.**Ölçümler** özelliği, her merkez tarafından değerlendirilen her metrik için bir sayım listeler:

* **targetedCount** - IoT Hub'daki aygıt ikizlerinin sayısını belirleyen ve hedefleme koşuluyla eşleşen bir sistem ölçümü.
* **appliedCount** - Sistem ölçümü, IoT Hub'da modüllerine dağıtım içeriği uygulanan aygıt sayısını belirtir.
* **reportedSuccessfulCount** - Dağıtımdaki IoT Edge aygıtlarının sayısını ioT Edge istemci çalışma zamanından bildiren bir aygıt ölçümü.
* **reportedFailedCount** - IoT Edge istemci çalışma zamanından dağıtım raporlama hatasındaki IoT Edge aygıtlarının sayısını belirten bir aygıt ölçümü.

[Az iot edge deployment show-metric](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show-metric) komutunu kullanarak ölçümlerin her biri için aygıt kimliklerinin veya nesnelerin listesini gösterebilirsiniz:

```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name]
```

Dağıtım gösteri metrik komutu aşağıdaki parametreleri alır:

* **--deployment-id** - IoT hub'ında bulunan dağıtımın adı.
* **--metric-id** - Örneğin, `reportedFailedCount`aygıt kimliklerinin listesini görmek istediğiniz metnin adı.
* **--hub-name** - Dağıtımın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutu `az account set -s [subscription name]`ile istenilen aboneye geçin.

## <a name="modify-a-deployment"></a>Dağıtımı değiştirme

Bir dağıtımı değiştirdiğinizde, değişiklikler hemen tüm hedeflenen aygıtlara çoğalır.

Hedef koşulu güncelleştirirseniz, aşağıdaki güncelleştirmeler oluşur:

* Bir aygıt eski hedef koşulu karşılamadı, ancak yeni hedef koşulu karşılıyorsa ve bu dağıtım bu aygıt için en yüksek öncelikse, bu dağıtım aygıta uygulanır.
* Şu anda bu dağıtımı çalıştıran bir aygıt artık hedef koşulu karşılamazsa, bu dağıtımı kaldırır ve bir sonraki en yüksek öncelikli dağıtımı alır.
* Şu anda bu dağıtımı çalıştıran bir aygıt artık hedef koşulu karşılamıyorsa ve diğer dağıtımların hedef koşulunu karşılamıyorsa, aygıtta değişiklik olmaz. Aygıt geçerli modüllerini geçerli durumunda çalıştırmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtımın hedef koşulunu karşıladıktan sonra, bu dağıtımı yükler ve yenisini alır.

Dağıtım bildiriminde tanımlanan modülleri ve yolları içeren bir dağıtımın içeriğini güncelleştiremezsiniz. Bir dağıtımın içeriğini güncelleştirmek istiyorsanız, bunu daha yüksek önceliğe sahip aynı aygıtları hedefleyen yeni bir dağıtım oluşturarak yaparsınız. Hedef koşul, etiketler, ölçümler ve öncelik de dahil olmak üzere varolan bir modülün belirli özelliklerini değiştirebilirsiniz.

Dağıtımı güncelleştirmek için [az iot edge dağıtım güncelleştirme](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-update) komutunu kullanın:

```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
```

Dağıtım güncelleştirme komutu aşağıdaki parametreleri alır:

* **--deployment-id** - IoT hub'ında bulunan dağıtımın adı.
* **--hub-name** - Dağıtımın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutile istenilen aboneye geçme`az account set -s [subscription name]`
* **--set** - Dağıtımdaki bir özelliği güncelleştirin. Aşağıdaki özellikleri güncelleştirebilirsiniz:
  * targetCondition - örneğin`targetCondition=tags.location.state='Oregon'`
  * Etiket
  * Öncelik
* **--ekle** - Hedef koşullar veya etiketler de dahil olmak üzere dağıtıma yeni bir özellik ekleyin.
* **--remove** - Hedef koşullar veya etiketler de dahil olmak üzere varolan bir özelliği kaldırın.

## <a name="delete-a-deployment"></a>Dağıtımı silme

Bir dağıtımı sildiğinizde, tüm aygıtlar bir sonraki en yüksek öncelikli dağıtımını alır. Aygıtlarınız başka bir dağıtımın hedef koşuluna uymuyorsa, dağıtım silindiğinde modüller kaldırılmaz.

Bir dağıtımı silmek için [az iot edge deployment delete](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-delete) komutunu kullanın:

```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name]
```

Dağıtım silme komutu aşağıdaki parametreleri alır:

* **--deployment-id** - IoT hub'ında bulunan dağıtımın adı.
* **--hub-name** - Dağıtımın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutile istenilen aboneye geçme`az account set -s [subscription name]`

## <a name="next-steps"></a>Sonraki adımlar

[Modülleri IoT Edge aygıtlarına dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
