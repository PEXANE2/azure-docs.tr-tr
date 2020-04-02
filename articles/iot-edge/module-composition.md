---
title: Dağıtım bildirimleriyle modülü & rotaları dağıtma - Azure IoT Edge
description: Dağıtım bildiriminin hangi modüllerin dağıtılanabildiğini nasıl açıklaştırdığını, nasıl dağıtılabildiğini ve aralarında ileti yolları nasıl oluşturulacaklarını öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/26/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6a4b90d8b6fe67de26c8e652e0dc5b62cc27023f
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80545622"
---
# <a name="learn-how-to-deploy-modules-and-establish-routes-in-iot-edge"></a>IoT Edge'de modülleri dağıtmayı ve yolları oluşturmayı öğrenin

Her IoT Edge cihazı en az iki modül çalıştırıyor: $edgeAgent ve $edgeHub, IoT Edge çalışma zamanının bir parçası. IoT Edge aygıtı, herhangi bir sayıda işlem için birden çok ek modül çalıştırabilir. Cihazınıza hangi modülleri yükleyeceğini ve birlikte çalışacak şekilde nasıl yapılandırılabildiğini söylemek için bir dağıtım bildirimi kullanın.

*Dağıtım bildirimi,* aşağıdakileri açıklayan bir JSON belgesidir:

* Üç bileşeniçeren **IoT Edge aracı** modülü ikizi:
  * Aygıtta çalışan her modül için kapsayıcı görüntüsü.
  * Modül görüntüleri içeren özel kapsayıcı kayıtlarına erişmek için kimlik bilgileri.
  * Her modülün nasıl oluşturulup yönetilmesi gerektiğine yönelik talimatlar.
* İletilerin modüller arasında ve sonunda IoT Hub'a nasıl aktığını içeren **IoT Edge hub** modülü ikizi.
* Herhangi bir ek modül ikizler (isteğe bağlı) istenilen özellikleri.

Tüm IoT Edge aygıtları bir dağıtım bildirimi ile yapılandırılmalıdır. Yeni yüklenen IoT Edge çalışma zamanı, geçerli bir bildirimle yapılandırılıncaya kadar bir hata kodu bildirir.

Azure IoT Edge öğreticilerinde, Azure IoT Edge portalındaki bir sihirbazdan geçerek bir dağıtım bildirimi oluşturursunuz. REST veya IoT Hub Service SDK'yı kullanarak bir dağıtım bildirimini programlı olarak da uygulayabilirsiniz. Daha fazla bilgi için [Bkz. IoT Edge dağıtımlarını anlayın.](module-deployment-monitoring.md)

## <a name="create-a-deployment-manifest"></a>Dağıtım bildirimi oluşturma

Yüksek düzeyde, dağıtım bildirimi, istenen özelliklere göre yapılandırılan modül ikizlerinin listesidir. Dağıtım bildirimi, bir IoT Edge aygıtına (veya bir aygıt grubuna) hangi modüllerin yüklenmesini ve nasıl yapılandırılabildiğini söyler. Dağıtım bildirimleri, her modül ikizi için *istenen özellikleri* içerir. IoT Edge aygıtları her modül için *bildirilen özellikleri* geri bildirir.

Her dağıtım bildiriminde iki modül `$edgeAgent`gereklidir: ve `$edgeHub`. Bu modüller, IoT Edge aygıtını ve üzerinde çalışan modülleri yöneten IoT Edge çalışma zamanının bir parçasıdır. Bu modüller hakkında daha fazla bilgi için Bkz. [IoT Edge çalışma süresini ve mimarisini anlayın.](iot-edge-runtime.md)

İki çalışma zamanı modülüne ek olarak, bir IoT Edge aygıtında çalıştırmak için size ait en fazla 20 modül ekleyebilirsiniz.

Yalnızca IoT Edge çalışma süresini (edgeAgent ve edgeHub) içeren bir dağıtım bildirimi geçerlidir.

Dağıtım bildirimleri aşağıdaki yapıyı izler:

```json
{
    "modulesContent": {
        "$edgeAgent": { // required
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
            }
        },
        "$edgeHub": { //required
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "module1": {  // optional
            "properties.desired": {
                // desired properties of module1
            }
        },
        "module2": {  // optional
            "properties.desired": {
                // desired properties of module2
            }
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Modülleri yapılandırma

IoT Edge çalışma zamanının modülleri dağıtımınızda nasıl yüklediğinizi tanımlayın. IoT Edge aracısı, bir IoT Edge aygıtının yüklemesini, güncelleştirmelerini ve durum raporlamasını yöneten çalışma zamanı bileşenidir. Bu nedenle, $edgeAgent modül ikiztüm modüller için yapılandırma ve yönetim bilgilerini içerir. Bu bilgiler, IoT Edge aracısının kendisi için yapılandırma parametrelerini içerir.

Dahil edilebilen veya eklenmesi gereken özelliklerin tam listesi için Bkz. [IoT Edge aracısı ve IoT Edge hub'ının özellikleri.](module-edgeagent-edgehub.md)

$edgeAgent özellikleri aşağıdaki yapıyı izler:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
            "settings":{
                "registryCredentials":{ // give the edge agent access to container images that aren't public
                    }
                }
            }
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "module1": { // optional
                // configuration and management details
            },
            "module2": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Rotaları bildir

IoT Edge hub'ı modüller, IoT Hub ve tüm yaprak aygıtları arasındaki iletişimi yönetir. Bu nedenle, $edgeHub modül ikizi, iletilerin dağıtım içinde nasıl geçirildiğini bildiren *yollar* adı verilen istenen bir özellik içerir. Aynı dağıtım içinde birden çok rotanız olabilir.

Rotalar aşağıdaki sözdizimi ile istenilen **$edgeHub** özellikleri bildirilir:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "route1": "FROM <source> WHERE <condition> INTO <sink>",
            "route2": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Her rotanın bir kaynağa ve bir lavaboya ihtiyacı vardır, ancak koşul iletileri filtrelemek için kullanabileceğiniz isteğe bağlı bir parçadır.

### <a name="source"></a>Kaynak

Kaynak, iletilerin nereden geldiğini belirtir. IoT Edge, modüllerden veya yaprak aygıtlardan iletileri yönlendirebilir.

IoT SDK'ları kullanarak, modüller ModuleClient sınıfını kullanarak iletileri için belirli çıktı kuyruklarını bildirebilir. Çıktı kuyrukları gerekli değildir, ancak birden çok rotayı yönetmek için yararlıdır. Yaprak aygıtlar, IoT Edge ağ geçidi aygıtlarına ileti göndermede, IoT Hub'a ileti gönderecekleri gibi IoT SDK'ların DeviceClient sınıfını kullanabilir. Daha fazla bilgi için Azure [IoT Hub SDK'larını anlayın ve kullanın.](../iot-hub/iot-hub-devguide-sdks.md)

Kaynak özelliği aşağıdaki değerlerden biri olabilir:

| Kaynak | Açıklama |
| ------ | ----------- |
| `/*` | Herhangi bir modül veya yaprak cihazdan tüm aygıttan buluta iletiler veya çift değiştirme bildirimleri |
| `/twinChangeNotifications` | Herhangi bir modül veya yaprak cihazdan gelen herhangi bir ikiz değişiklik (bildirilen özellikler) |
| `/messages/*` | Bir modül tarafından bir çıktı veya hiç çıktı olmadan veya bir yaprak aygıt aracılığıyla gönderilen aygıttan buluta ileti |
| `/messages/modules/*` | Bir modül tarafından bazı veya hiç çıktı üzerinden gönderilen aygıttan buluta ileti |
| `/messages/modules/<moduleId>/*` | Belirli bir modül tarafından bazı veya hiç çıktı olmadan gönderilen aygıttan buluta ileti |
| `/messages/modules/<moduleId>/outputs/*` | Belirli bir modül tarafından bazı çıktılar aracılığıyla gönderilen aygıttan buluta ileti |
| `/messages/modules/<moduleId>/outputs/<output>` | Belirli bir çıktı aracılığıyla belirli bir modül tarafından gönderilen aygıttan buluta ileti |

### <a name="condition"></a>Koşul

Koşul, bir rota bildiriminde isteğe bağlıdır. Kaynaktan lavaboya tüm iletileri aktarmak istiyorsanız, **WHERE** yan tümcesini tamamen dışarıda bırakmanız gerekir. Veya durumu karşılayan belirli iletilere veya ileti türlerine filtre uygulama yapmak için [IoT Hub sorgu dilini](../iot-hub/iot-hub-devguide-routing-query-syntax.md) kullanabilirsiniz. IoT Edge yolları, iletileri ikiz etiketlere veya özelliklere göre filtreleme özelliğini desteklemez.

IoT Edge'deki modüller arasında geçen iletiler, aygıtlarınız ile Azure IoT Hub'ınız arasında geçen iletilerle aynı biçimlendirilir. Tüm iletiler JSON olarak biçimlendirilir ve **systemProperties,** **appProperties**ve **gövde** parametreleri vardır.

Aşağıdaki sözdizimi ile üç parametreden herhangi biri etrafında sorgular oluşturabilirsiniz:

* Sistem özellikleri: `$<propertyName>` veya`{$<propertyName>}`
* Uygulama özellikleri:`<propertyName>`
* Gövde özellikleri:`$body.<propertyName>`

İleti özellikleri için sorguların nasıl oluşturulacığına ilişkin örnekler [için](../iot-hub/iot-hub-devguide-routing-query-syntax.md)bkz.

IoT Edge'e özgü bir örnek, bir yaprak aygıtından ağ geçidi aygıtına gelen iletilere filtre uygulama yapmak istediğinizde elde edilir. Modüllerden gelen iletiler **bağlantıModuleId**adı verilen bir sistem özelliği içerir. Bu nedenle, yaprak aygıtlardan gelen iletileri doğrudan IoT Hub'ına yönlendirmek istiyorsanız, modül iletilerini hariç tutmak için aşağıdaki rotayı kullanın:

```query
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Havuz

Lavabo, iletilerin nereye gönderildiğini tanımlar. Yalnızca modüller ve IoT Hub ileti alabilir. İletiler diğer aygıtlara yönlendirilemiyor. Lavabo tesisinde joker karakter seçeneği yoktur.

Lavabo özelliği aşağıdaki değerlerden herhangi biri olabilir:

| Havuz | Açıklama |
| ---- | ----------- |
| `$upstream` | İletiyi IoT Hub'ına gönderme |
| `BrokeredEndpoint("/modules/<moduleId>/inputs/<input>")` | İletiyi belirli bir modülün belirli bir girişine gönderme |

IoT Edge en az bir kez garanti sağlar. IoT Edge hub'ı, bir rotanın iletiyi lavabosuna teslim edeme ihtimaline karşı iletileri yerel olarak depolar. Örneğin, IoT Edge hub'ı IoT Hub'ına bağlanamıyorsa veya hedef modül bağlı değilse.

IoT Edge hub'ı iletileri, `storeAndForwardConfiguration.timeToLiveSecs` [IoT Edge hub'ının](module-edgeagent-edgehub.md)özelliğinde belirtilen zamana kadar depolar.

## <a name="define-or-update-desired-properties"></a>İstenilen özellikleri tanımlama veya güncelleştirme

Dağıtım bildirimi, IoT Edge aygıtına dağıtılan her modül için istenen özellikleri belirtir. Dağıtım bildiriminde istenen özellikler, modül ikizinde şu anda istenen özelliklerin üzerine yazılır.

Dağıtım bildiriminde bir modül ikizinin istenen özelliklerini belirtmezseniz, IoT Hub modül ikizini hiçbir şekilde değiştirmez. Bunun yerine, istenen özellikleri programlı olarak ayarlayabilirsiniz.

Cihaz ikizleri değiştirmek için izin aynı mekanizmalar modül ikizler değiştirmek için kullanılır. Daha fazla bilgi için [modül ikiz geliştirici kılavuzuna](../iot-hub/iot-hub-devguide-module-twins.md)bakın.

## <a name="deployment-manifest-example"></a>Dağıtım bildirimi örneği

Aşağıdaki örnek, geçerli bir dağıtım bildirimi belgesinin nasıl görünebileceğini gösterir.

```json
{
  "modulesContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "ContosoRegistry": {
                "username": "myacr",
                "password": "<password>",
                "address": "myacr.azurecr.io"
              }
            }
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
              "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
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
          },
          "filtermodule": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "env": {
              "tempLimit": {"value": "100"}
            },
            "settings": {
              "image": "myacr.azurecr.io/filtermodule:latest",
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
          "sensorToFilter": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 10
        }
      }
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* $edgeAgent ve $edgeHub dahil edilebilen veya eklenmesi gereken özelliklerin tam listesi için [IoT Edge aracısının ve IoT Edge hub'ının özelliklerine](module-edgeagent-edgehub.md)bakın.

* Artık IoT Edge modüllerinin nasıl kullanıldığını bildiğinize [göre, IoT Edge modüllerini geliştirmek için gereksinimleri ve araçları anlayın.](module-development.md)
