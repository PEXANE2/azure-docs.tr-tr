---
title: Azure CLı 'yı kullanarak ölçeklendirmeye modül dağıtma-Azure IoT Edge
description: Cihazları IOT Edge grupları için otomatik dağıtımlar oluşturmak için Azure CLI için IOT uzantısı kullanma
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/20/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9152b38a0155b610f39f7de239bcc377ad96be5d
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78893226"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Dağıtma ve izleme uygun ölçekte Azure CLI kullanarak IOT Edge modülleri

Tek seferde birçok cihaza yönelik devam eden dağıtımları yönetmek için Azure komut satırı arabirimini kullanarak **IoT Edge otomatik dağıtım** oluşturun. IoT Edge için otomatik dağıtımlar IoT Hub [otomatik cihaz yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar birden çok modülü birden çok cihaza dağıtmanızı, modüllerin durumunu ve durumunu izlemenizi ve gerektiğinde değişiklik yapmayı sağlayan dinamik işlemlerdir.

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Bu makalede, Azure CLI ve IOT uzantısını ayarlama. Daha sonra, bir IoT Edge cihaz kümesine modül dağıtmayı ve kullanılabilir CLı komutlarını kullanarak ilerlemeyi izlemeyi öğreneceksiniz.

## <a name="cli-prerequisites"></a>CLI önkoşulları

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-using-cli.md) .
* IoT Edge çalışma zamanının yüklü olduğu [cihazlar IoT Edge](how-to-register-device.md#prerequisites-for-the-azure-cli) .
* Ortamınızdaki [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) . Azure CLı sürümünüz en azından 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az --version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI Için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Daha fazla bilgi için bkz. [IoT Edge modül dağıtmayı ve yollar oluşturmayı öğrenin](module-composition.md).

Azure CLI kullanarak modüllerini dağıtmak için dağıtım bildirimi yerel olarak bir .txt dosyası olarak kaydedin. Yapılandırmayı cihazınıza uygulamak için komutunu çalıştırdığınızda sonraki bölümde dosya yolunu kullanın.

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

## <a name="layered-deployment"></a>Katmanlı dağıtım

Katmanlı dağıtımlar, her birinin üzerine yığılmış bir otomatik dağıtım türüdür. Katmanlı dağıtımlar hakkında daha fazla bilgi için bkz. [tek cihaz veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Katmanlı dağıtımlar, her türlü otomatik dağıtım gibi Azure CLı ile oluşturulabilir ve yönetilebilir, ancak yalnızca birkaç fark vardır. Katmanlı bir dağıtım oluşturulduktan sonra aynı Azure CLı, herhangi bir dağıtımla aynı şekilde katmanlı dağıtımlar için de çalışır. Katmanlı bir dağıtım oluşturmak için Create komutuna `--layered` bayrağını ekleyin.

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

Önceki örnekte, bir modül için `properties.desired` bir katmanlı dağıtım ayarı gösteriliyordu. Bu katmanlı dağıtım, aynı modülün zaten uygulanmış olduğu bir cihazı hedeflediğinden, istenen tüm özellikler üzerine yazılır. Güncelleştirmek için, istenen özelliklerin üzerine yazmak yerine yeni bir alt bölüm tanımlayabilirsiniz. Örnek:

```json
"SimulatedTEmperatureSensor": {
  "properties.desired.layeredProperties": {
    "SendData": true,
    "SendInterval": 5
  }
}
```

Katmanlı dağıtımlarda modül TWINS yapılandırma hakkında daha fazla bilgi için bkz. [katmanlı dağıtım](module-deployment-monitoring.md#layered-deployment)

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir dağıtımı oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtebilmek sahip. Azure IoT Edge cihaz ikizi **etiketleri** kullanarak cihazları tanımlar. Her cihazda, çözümünüz için anlamlı olacak şekilde tanımladığınız birden fazla etiket olabilir. Örneğin, bir akıllı binalar, kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketler ekleyebilirsiniz:

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

## <a name="create-a-deployment"></a>Bir dağıtım oluşturun

Dağıtım bildiriminin yanı sıra diğer parametreler içeren bir dağıtım oluşturarak modülleri, hedef cihazlara dağıtın.

Dağıtım oluşturmak için [az IoT Edge Deployment Create](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-create) komutunu kullanın:

```cli
az iot edge deployment create --deployment-id [deployment id] --hub-name [hub name] --content [file path] --labels "[labels]" --target-condition "[target query]" --priority [int]
```

Katmanlı bir dağıtım oluşturmak için `--layered` bayrağıyla aynı komutu kullanın.

Dağıtım oluşturma komutu aşağıdaki parametreleri alır:

* **--katmanlı** -dağıtımı katmanlı bir dağıtım olarak tanımlamak için isteğe bağlı bir bayrak.
* **--Deployment-ID** -IoT Hub 'ında oluşturulacak dağıtımın adı. Dağıtımınızı en çok 128 küçük harf olan benzersiz bir ad verin. Boşluklardan ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`. Gerekli parametre.
* **--Content** -FilePath öğesine DAĞıTıM bildirimi JSON 'ı. Gerekli parametre.
* **--hub-adı** -dağıtımın oluşturulacağı IoT Hub 'ının adı. Hub'ın geçerli abonelikte olmalıdır. Geçerli aboneliğinizi `az account set -s [subscription name]` komutuyla değiştirin.
* **--labels** -dağıtımlarınızın izlenmesine yardımcı olmak için Etiketler ekleyin. Etiket adı, dağıtımınızı tanımlayan değer çiftleri olan. Etiketler, adlar ve değerler için JSON biçimlendirmesi alır. Örneğin, `{"HostPlatform":"Linux", "Version:"3.0.1"}`
* **--target-Condition** -Bu dağıtıma hangi cihazların hedeflenceğini belirleyen bir hedef koşul girin. Bu koşul, Device ikizi etiketlerine veya Device ikizi bildirilen özelliklerine dayalıdır ve ifade biçimiyle eşleşmelidir. Örneğin, `tags.environment='test' and properties.reported.devicemodel='4000x'`.
* **--Priority** -pozitif bir tamsayı. İki veya daha fazla dağıtım aynı cihazda hedeflenen durumunda durumunda, sayısal değeri en yüksek dağıtım önceliği geçerli olur.
* **--ölçümler** -bir dağıtımın durumunu Izlemek Için edgeHub tarafından bildirilen özellikleri sorgulayan ölçümler oluşturun. Ölçümler JSON girişi veya bir FilePath alır. Örneğin, `'{"queries": {"mymetric": "SELECT deviceId FROM devices WHERE properties.reported.lastDesiredStatus.code = 200"}}'`.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Tek bir dağıtımın ayrıntılarını görüntülemek için [az IoT Edge Deployment Show](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show) komutunu kullanın:

```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
```

Dağıtım göster komutu aşağıdaki parametreleri alır:

* **--Deployment-ID** -IoT Hub 'ında bulunan dağıtımın adı. Gerekli parametre.
* **--hub-adı** -dağıtımın bulunduğu IoT Hub 'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komut `az account set -s [subscription name]` istenen aboneliğe geçiş yapın

Komut penceresinde dağıtım inceleyin. **Ölçümler** özelliği her bir hub tarafından değerlendirilen her ölçüm için bir sayı listeler:

* **Targetedcount** -hedefleme koşuluyla eşleşen IoT Hub bir cihaz TWINS sayısını belirten bir sistem ölçümü.
* **appliedCount** -bir sistem ölçümü, dağıtım içeriğinin IoT Hub ' de modül TWINS 'lerine uyguladığı cihazların sayısını belirtir.
* **Reportedbaşarıyla Fulcount** -IoT Edge istemci çalışma zamanının başarılı olarak bildirdiği IoT Edge cihazların sayısını belirten bir cihaz ölçümü.
* **Reportedfailedcount** -IoT Edge istemci çalışma zamanından dağıtım raporlama hatası içindeki IoT Edge cihazlarının sayısını belirten bir cihaz ölçümü.

[Az IoT Edge Deployment Show-Metric](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show-metric) komutunu kullanarak her ölçüm Için cihaz kimliklerinin veya nesnelerinin bir listesini gösterebilirsiniz:

```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name]
```

Dağıtım Show-Metric komutu aşağıdaki parametreleri alır:

* **--Deployment-ID** -IoT Hub 'ında bulunan dağıtımın adı.
* **--Metric-ID** -cihaz kimliklerinin listesini görmek istediğiniz ölçümün adı, örneğin `reportedFailedCount`.
* **--hub-adı** -dağıtımın bulunduğu IoT Hub 'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komut `az account set -s [subscription name]`istenen aboneliğe geçiş yapın.

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın.

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:

* Ardından, bir cihaz, eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu dağıtım bu cihaz için en yüksek öncelikli ise, bu dağıtım cihaza uygulanır.
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelikli dağıtımı alır.
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılayan ve diğer tüm dağıtımları, hedef koşulu yerine getirmeyen, hiçbir değişiklik cihazda gerçekleşir. Cihaz, geçerli alt modüller kendi geçerli durumunda çalışmaya devam eder ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtım hedef koşulu karşılayan sonra bu dağıtım kaldırır ve yeni alır.

Dağıtım bildiriminde tanımlanan modülleri ve yolları içeren bir dağıtımın içeriğini güncelleştiremezsiniz. Bir dağıtımın içeriğini güncelleştirmek istiyorsanız, daha yüksek bir önceliğe sahip aynı cihazları hedefleyen yeni bir dağıtım oluşturarak bunu yapabilirsiniz. Hedef koşul, Etiketler, ölçümler ve öncelik dahil olmak üzere var olan bir modülün belirli özelliklerini değiştirebilirsiniz.

Bir dağıtımı güncelleştirmek için [az IoT Edge Deployment Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-update) komutunu kullanın:

```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
```

Dağıtım güncelleştirme komutu aşağıdaki parametreleri alır:

* **--Deployment-ID** -IoT Hub 'ında bulunan dağıtımın adı.
* **--hub-adı** -dağıtımın bulunduğu IoT Hub 'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komut `az account set -s [subscription name]` istenen aboneliğe geçiş yapın
* **--set** -dağıtımda bir özelliği güncelleştirin. Aşağıdaki özellikleri güncelleştirebilirsiniz:
  * targetCondition-örneğin `targetCondition=tags.location.state='Oregon'`
  * etiketler
  * öncelik
* **--Add** -hedef koşullar veya Etiketler dahil olmak üzere dağıtıma yeni bir özellik ekleyin.
* **--Remove** -hedef koşullar veya Etiketler dahil olmak üzere mevcut bir özelliği kaldırır.

## <a name="delete-a-deployment"></a>Bir dağıtımı silin

Bir dağıtım sildiğinizde, sonraki en yüksek öncelikli dağıtım üzerinde herhangi bir cihaza yararlanın. Daha sonra başka bir dağıtım hedef koşulu cihazlarınızı karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz.

Bir dağıtımı silmek için [az IoT Edge Deployment Delete](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-delete) komutunu kullanın:

```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name]
```

Dağıtım silme komutu aşağıdaki parametreleri alır:

* **--Deployment-ID** -IoT Hub 'ında bulunan dağıtımın adı.
* **--hub-adı** -dağıtımın bulunduğu IoT Hub 'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komut `az account set -s [subscription name]` istenen aboneliğe geçiş yapın

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge cihazlara modül dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
