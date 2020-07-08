---
title: Azure CLı 'yı kullanarak ölçeklendirmeye modül dağıtma-Azure IoT Edge
description: IoT Edge cihaz grupları için otomatik dağıtımlar oluşturmak üzere Azure CLı için IoT uzantısını kullanma
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 4/14/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a7bb2cc23374110d447ec7526ada75f7e36a966e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83726171"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Azure CLı kullanarak IoT Edge modüllerini ölçeklendirerek dağıtma ve izleme

Tek seferde birçok cihaza yönelik devam eden dağıtımları yönetmek için Azure komut satırı arabirimini kullanarak **IoT Edge otomatik dağıtım** oluşturun. IoT Edge için otomatik dağıtımlar IoT Hub [otomatik cihaz yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar birden çok modülü birden çok cihaza dağıtmanızı, modüllerin durumunu ve durumunu izlemenizi ve gerektiğinde değişiklik yapmayı sağlayan dinamik işlemlerdir.

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Bu makalede, Azure CLı ve IoT uzantısını ayarlarsınız. Daha sonra, bir IoT Edge cihaz kümesine modül dağıtmayı ve kullanılabilir CLı komutlarını kullanarak ilerlemeyi izlemeyi öğreneceksiniz.

## <a name="cli-prerequisites"></a>CLı önkoşulları

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-using-cli.md) .
* IoT Edge çalışma zamanının yüklü olduğu [cihazlar IoT Edge](how-to-register-device.md#prerequisites-for-the-azure-cli) .
* Ortamınızdaki [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) . Azure CLı sürümünüz en azından 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimi yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılacağını, modüller arasında verilerin nasıl akacağını ve modül TWINS 'in istenen özelliklerini tanımlayan bir JSON belgesidir. Daha fazla bilgi için bkz. [IoT Edge modül dağıtmayı ve yollar oluşturmayı öğrenin](module-composition.md).

Azure CLı kullanarak modüller dağıtmak için dağıtım bildirimini yerel olarak bir. txt dosyası olarak kaydedin. Yapılandırmayı cihazınıza uygulamak için komutunu çalıştırdığınızda sonraki bölümde dosya yolunu kullanın.

Örnek olarak bir modülle birlikte temel bir dağıtım bildirimi aşağıda verilmiştir:

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

Katmanlı dağıtımlar, her birinin üzerine yığılmış bir otomatik dağıtım türüdür. Katmanlı dağıtımlar hakkında daha fazla bilgi için bkz. [tek cihaz veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Katmanlı dağıtımlar, her türlü otomatik dağıtım gibi Azure CLı ile oluşturulabilir ve yönetilebilir, ancak yalnızca birkaç fark vardır. Katmanlı bir dağıtım oluşturulduktan sonra aynı Azure CLı, herhangi bir dağıtımla aynı şekilde katmanlı dağıtımlar için de çalışır. Katmanlı bir dağıtım oluşturmak için `--layered` Oluştur komutuna bayrağı ekleyin.

İkinci fark, dağıtım bildiriminin yapıtından oluşur. Standart otomatik dağıtım, tüm Kullanıcı modüllerine ek olarak sistem çalışma zamanı modüllerini içermelidir, ancak katmanlı dağıtımlar yalnızca Kullanıcı modülleri içerebilir. Bunun yerine, katmanlı dağıtımlar, sistem çalışma zamanı modülleri gibi her IoT Edge cihazının gerekli bileşenlerini sağlamak için bir cihazda aynı zamanda standart bir Otomatik dağıtıma gerek duyar.

Aşağıda örnek olarak bir modüllü temel katmanlı dağıtım bildirimi verilmiştir:

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

Önceki örnekte, bir modül için bir katmanlı dağıtım ayarı gösteriliyordu `properties.desired` . Bu katmanlı dağıtım, aynı modülün zaten uygulanmış olduğu bir cihazı hedeflediğinden, istenen tüm özellikler üzerine yazılır. Güncelleştirmek için, istenen özelliklerin üzerine yazmak yerine yeni bir alt bölüm tanımlayabilirsiniz. Örneğin:

```json
"SimulatedTEmperatureSensor": {
  "properties.desired.layeredProperties": {
    "SendData": true,
    "SendInterval": 5
  }
}
```

Katmanlı dağıtımlarda modül TWINS yapılandırma hakkında daha fazla bilgi için bkz. [katmanlı dağıtım](module-deployment-monitoring.md#layered-deployment)

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları tanımla

Bir dağıtım oluşturabilmeniz için önce hangi cihazların etkilenmesini istediğinizi belirleyebilmelisiniz. Azure IoT Edge cihaz ikizi **etiketleri** kullanarak cihazları tanımlar. Her cihazda, çözümünüz için anlamlı olacak şekilde tanımladığınız birden fazla etiket olabilir. Örneğin, akıllı binaları bir kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketleri ekleyebilirsiniz:

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

Cihaz ikgörüti ve etiketleri hakkında daha fazla bilgi için bkz. [IoT Hub 'da cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Dağıtım oluşturma

Dağıtım bildiriminin yanı sıra diğer parametrelerden oluşan bir dağıtım oluşturarak modülleri hedef cihazlarınıza dağıtırsınız.

Dağıtım oluşturmak için [az IoT Edge Deployment Create](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/edge/deployment?view=azure-cli-latest#ext-azure-iot-az-iot-edge-deployment-create) komutunu kullanın:

```cli
az iot edge deployment create --deployment-id [deployment id] --hub-name [hub name] --content [file path] --labels "[labels]" --target-condition "[target query]" --priority [int]
```

`--layered`Katmanlı bir dağıtım oluşturmak için bayrağıyla aynı komutu kullanın.

Dağıtım oluşturma komutu aşağıdaki parametreleri alır:

* **--katmanlı** -dağıtımı katmanlı bir dağıtım olarak tanımlamak için isteğe bağlı bir bayrak.
* **--Deployment-ID** -IoT Hub 'ında oluşturulacak dağıtımın adı. Dağıtımınıza 128 küçük harflerden oluşan benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /` . Gerekli parametre.
* **--Content** -FilePath öğesine DAĞıTıM bildirimi JSON 'ı. Gerekli parametre.
* **--hub-adı** -dağıtımın oluşturulacağı IoT Hub 'ının adı. Hub geçerli abonelikte olmalıdır. Geçerli aboneliğinizi `az account set -s [subscription name]` komutla değiştirin.
* **--labels** -dağıtımlarınızın izlenmesine yardımcı olmak için Etiketler ekleyin. Etiketler, dağıtımınızı tanımlayan ad ve değer çiftleridir. Etiketler, adlar ve değerler için JSON biçimlendirmesi alır. Örneğin, `{"HostPlatform":"Linux", "Version:"3.0.1"}`
* **--target-Condition** -Bu dağıtıma hangi cihazların hedeflenceğini belirleyen bir hedef koşul girin.Bu koşul, Device ikizi etiketlerine veya Device ikizi bildirilen özelliklerine dayalıdır ve ifade biçimiyle eşleşmelidir.Örneğin, `tags.environment='test' and properties.reported.devicemodel='4000x'`.
* **--Priority** -pozitif bir tamsayı. İki veya daha fazla dağıtımın aynı cihaza hedeflenme durumunda, öncelik için en yüksek sayısal değere sahip dağıtım uygulanır.
* **--ölçümler** -bir dağıtımın durumunu Izlemek Için edgeHub tarafından bildirilen özellikleri sorgulayan ölçümler oluşturun. Ölçümler JSON girişi veya bir FilePath alır. Örneğin, `'{"queries": {"mymetric": "SELECT deviceId FROM devices WHERE properties.reported.lastDesiredStatus.code = 200"}}'`.

Azure CLı kullanarak bir dağıtımı izlemek için bkz. [IoT Edge dağıtımlarını izleme](how-to-monitor-iot-edge-deployments.md#monitor-a-deployment-with-azure-cli).

## <a name="modify-a-deployment"></a>Bir dağıtımı değiştirme

Bir dağıtımı değiştirdiğinizde, değişiklikler hedeflenen tüm cihazlara hemen çoğaltılır.

Hedef koşulu güncelleştirirseniz, aşağıdaki güncelleştirmeler gerçekleşir:

* Bir cihaz eski hedef koşulunu karşılamıyorsa, ancak yeni hedef koşulunu karşılıyorsa ve bu dağıtım bu cihaz için en yüksek önceliktir, bu dağıtım cihaza uygulanır.
* Şu anda bu dağıtımı çalıştıran bir cihaz artık hedef koşulu karşılamıyorsa, bu dağıtımı kaldırır ve sonraki en yüksek öncelikli dağıtımı alır.
* Şu anda bu dağıtımı çalıştıran bir cihaz artık hedef koşulunu karşılamıyorsa ve diğer dağıtımların hedef koşulunu karşılamıyorsa, cihazda hiçbir değişiklik gerçekleşmez. Cihaz geçerli durumunda mevcut modüllerini çalıştırmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtımın hedef koşulunu karşılıyorsa, bu dağıtımı kaldırır ve yenisini alır.

Dağıtım bildiriminde tanımlanan modülleri ve yolları içeren bir dağıtımın içeriğini güncelleştiremezsiniz. Bir dağıtımın içeriğini güncelleştirmek istiyorsanız, daha yüksek bir önceliğe sahip aynı cihazları hedefleyen yeni bir dağıtım oluşturarak bunu yapabilirsiniz. Hedef koşul, Etiketler, ölçümler ve öncelik dahil olmak üzere var olan bir modülün belirli özelliklerini değiştirebilirsiniz.

Bir dağıtımı güncelleştirmek için [az IoT Edge Deployment Update](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/edge/deployment?view=azure-cli-latest#ext-azure-iot-az-iot-edge-deployment-update) komutunu kullanın:

```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
```

Dağıtım güncelleştirme komutu aşağıdaki parametreleri alır:

* **--Deployment-ID** -IoT Hub 'ında bulunan dağıtımın adı.
* **--hub-adı** -dağıtımın bulunduğu IoT Hub 'ının adı. Hub geçerli abonelikte olmalıdır. Komutuyla istenen aboneliğe geçiş yapın`az account set -s [subscription name]`
* **--set** -dağıtımda bir özelliği güncelleştirin. Aşağıdaki özellikleri güncelleştirebilirsiniz:
  * targetCondition-Örneğin`targetCondition=tags.location.state='Oregon'`
  * etikete
  * Priority
* **--Add** -hedef koşullar veya Etiketler dahil olmak üzere dağıtıma yeni bir özellik ekleyin.
* **--Remove** -hedef koşullar veya Etiketler dahil olmak üzere mevcut bir özelliği kaldırır.

## <a name="delete-a-deployment"></a>Bir dağıtımı silme

Bir dağıtımı sildiğinizde, tüm cihazlar bir sonraki en yüksek öncelikli dağıtımı alırlar. Cihazlarınız başka bir dağıtımın hedef koşulunu karşılamıyorsa, dağıtım silindiğinde modüller kaldırılmaz.

Bir dağıtımı silmek için [az IoT Edge Deployment Delete](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/edge/deployment?view=azure-cli-latest#ext-azure-iot-az-iot-edge-deployment-delete) komutunu kullanın:

```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name]
```

Dağıtım silme komutu aşağıdaki parametreleri alır:

* **--Deployment-ID** -IoT Hub 'ında bulunan dağıtımın adı.
* **--hub-adı** -dağıtımın bulunduğu IoT Hub 'ının adı. Hub geçerli abonelikte olmalıdır. Komutuyla istenen aboneliğe geçiş yapın`az account set -s [subscription name]`

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge cihazlara modül dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
