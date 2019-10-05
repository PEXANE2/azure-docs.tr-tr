---
title: Verilerinizi Azure Blob depolamaya aktarma | Microsoft Docs
description: Azure IoT Central uygulamanızdan Azure Blob depolama alanına veri aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 09/26/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 9ac650273a53da715b89e3233b30cf50710cfcfd
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973248"
---
# <a name="export-your-data-to-azure-blob-storage-preview-features"></a>Verilerinizi Azure Blob Storage 'a aktarma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede, Azure **BLOB depolama hesabınıza** veya **Azure Data Lake Storage 2. depolama hesabınıza**düzenli aralıklarla veri aktarmak için Azure IoT Central sürekli veri dışa aktarma özelliğinin nasıl kullanılacağı açıklanır. **Telemetri**, **cihaz**ve **cihaz şablonlarını** JSON biçimindeki dosyalara dışarı aktarabilirsiniz. Bu veriler, Microsoft Power BI 'de Azure Machine Learning veya uzun süreli eğilim analizinde eğitim modelleri gibi soğuk yol analizi için kullanılabilir.

> [!Note]
> Sürekli veri dışarı aktarmayı açtığınızda, yalnızca o andan itibaren verileri alırsınız. Şu anda, sürekli veri dışa aktarma kapalı olduğunda veriler bir saat için alınamaz. Daha fazla geçmiş verileri sürdürmek için sürekli veri dışa aktarmayı erken açın.


## <a name="prerequisites"></a>Önkoşullar

- IoT Central uygulamanızda yönetici olmanız gerekir

## <a name="create-storage-account"></a>Depolama hesabı oluştur
Uygulamasına dışarı aktarmak için mevcut bir depolama alanı yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)oluşturun. Yeni [Azure Blob depolama hesapları](https://aka.ms/blobdocscreatestorageaccount) veya [Azure Data Lake Storage v2 depolama hesapları](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account)oluşturma hakkında daha fazla bilgi edinebilirsiniz.

    > [!Note] 
    > Verileri bir ADLS v2 depolama hesabına aktarmayı seçerseniz, **Blobstorage**olarak **hesap türünü** seçmeniz gerekir. 

    > [!Note] 
    > Kullandıkça Öde IoT Central uygulamanız için olandan farklı aboneliklerdeki depolama hesaplarına veri aktarabilirsiniz. Bu durumda bir bağlantı dizesi kullanarak bağlanacaksınız.

2. Depolama hesabınızda bir kapsayıcı oluşturun. Depolama hesabınıza gidin. **BLOB hizmeti**altında **bloblara gözatamıyorum**' ı seçin. Yeni bir kapsayıcı oluşturmak için üst kısımdaki **+ kapsayıcı** ' yı seçin.


## <a name="set-up-continuous-data-export"></a>Sürekli veri dışarı aktarma ayarlama

Verilerin dışarı aktarılacağı bir depolama hedefine sahip olduğunuza göre, sürekli veri dışa aktarma ayarlamak için aşağıdaki adımları izleyin. 

1. IoT Central uygulamanızda oturum açın.

2. Sol taraftaki menüden **veri dışarı aktarma**' yı seçin.

    > [!Note]
    > Sol menüde veri dışa aktarma görmüyorsanız, uygulamanızda yönetici değilsiniz demektir. Verilerin dışarı aktarılmasını ayarlamak için bir yöneticiye danışın.

3. Sağ üst köşedeki **+ Yeni** düğmesini seçin. Dışarı aktarma işlemi hedefi olarak **Azure Blob depolama** ' yı seçin. 

    > [!NOTE] 
    > Uygulama başına en fazla dışarı aktarma sayısı beştir. 

    ![Yeni sürekli veri dışa aktarma oluştur](media/howto-export-data-pnp/export-new2.png)

4. Aşağı açılan liste kutusunda **depolama hesabı ad**alanınızı seçin. Ayrıca, listede **bir bağlantı dizesi girerek**son seçeneği de seçebilirsiniz. 

    > [!NOTE] 
    > Depolama hesabı ad alanlarını yalnızca **IoT Central uygulamanızla aynı abonelikte**görürsünüz. Bu aboneliğin dışında bir hedefe aktarmak istiyorsanız, **bir bağlantı dizesi girin** ' i seçin ve 5. adıma bakın.

    > [!NOTE] 
    > 7 günlük deneme uygulamaları için sürekli veri vermeyi yapılandırmanın tek yolu bir bağlantı dizesidir. Bunun nedeni 7 günlük deneme uygulamalarının ilişkili bir Azure aboneliğine sahip olmaması olabilir.

    ![Bloba yeni dışarı aktarma oluştur](media/howto-export-data-pnp/export-create-blob2.png)

5. Seçim **Bir bağlantı dizesi girin**' i seçerseniz, Bağlantı dizenizi yapıştırmanız için yeni bir kutu belirir. Depolama hesabınızın bağlantı dizesini almak için Azure portal depolama hesabına gidin:
    - **Ayarlar**altında **erişim anahtarları** ' nı seçin.
    - KEY1 bağlantı dizesini veya key2 bağlantı dizesini kopyalayın
 
6. Açılan liste kutusundan bir kapsayıcı seçin. Bir Kapsayıcınız yoksa Azure portal depolama hesabınıza gidin:
    - **BLOB hizmeti**altında **Bloblar**' ı seçin. **+ Kapsayıcı** ' ya tıklayın ve kapsayıcınıza bir ad verin. Verileriniz için bir genel erişim düzeyi seçin (herhangi bir sürekli veri dışa aktarma ile çalışır). [Azure Storage belgelerinden](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container)daha fazla bilgi edinin.

7.  **Dışarı aktarılacak veriler**' in altında, türü **üzerine**ayarlayarak dışarı aktarılacak her bir veri türünü belirtin.
   
    > [!NOTE] 
    > Veriler JSON biçiminde verilir.

8. Sürekli veri dışa aktarmayı açmak için, **veri dışa aktarma** geçişi 'nin açık olduğundan emin olun. **Kaydet**’i seçin.

   ![Sürekli veri vermeyi yapılandırma](media/howto-export-data-pnp/export-list-blob2.png)

9. Birkaç dakika sonra verileriniz depolama hesabınızda görüntülenir.


## <a name="path-structure"></a>Yol yapısı

Telemetriyi, cihazları ve cihaz şablonları verileri, depolama hesabınıza dakikada bir kez, son dışarıya aktarılmış dosyadan bu yana yapılan değişiklikleri içeren her bir dosyayla birlikte verilir. Verilen veriler JSON biçiminde üç klasöre yerleştirilir. Depolama hesabınızdaki varsayılan yollar şunlardır:
- Telemetri: {Container}/{app-id}/telemetry/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}
- Cihazlar: {Container}/{app-id}/Devices/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}
- Cihaz şablonları: {Container}/{App-ID}/devicetemplates/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}

Dosyaya giderek ve **blobu Düzenle** sekmesini seçerek Azure Portal dışarıya aktarılmış dosyalara gidebilirsiniz.

## <a name="data-format"></a>Veri biçimi
### <a name="telemetry"></a>Telemetri

İçe aktarılmış telemetri verilerinde, bu süre boyunca tüm cihazlardan IoT Central tarafından alınan tüm yeni iletiler bulunur. İçeri aktarılmış dosyalar, blob depolamaya [IoT Hub ileti yönlendirme tarafından içeri](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) aktarılmış ileti dosyalarıyla aynı biçimi kullanır.

> [!NOTE]
> Cihazlarınızın `contentType: application/JSON` ve `contentEncoding:utf-8` (veya `utf-16`, `utf-32`) olan iletiler göndermesini sağlayın. Bir örnek için [IoT Hub belgelerine](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-body) bakın.

> [!NOTE]
> Telemetriyi gönderen cihazlar cihaz kimlikleri tarafından temsil edilir (aşağıdaki bölümlere bakın). Cihazların adlarını almak için cihaz anlık görüntülerini dışarı aktarın. Her ileti kaydını, cihaz kaydının **DeviceID** 'Siyle eşleşen **connectiondeviceıd** kullanarak ilişkilendirin.

Aşağıdaki örnekte JSON biçimindeki bir kayıt gösterilmektedir.

```json
{ 
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{ 

  },
  "SystemProperties":{ 
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{ 
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

### <a name="devices"></a>Cihazlar

Sürekli veri dışa aktarma ilk açıldığında, tüm cihazları içeren tek bir anlık görüntü dışa aktarılabilir. Her cihaz şunları içerir:
- IoT Central cihazın `@id`
- @no__t-cihazın 0
- [cihaz sağlama hizmeti](https://aka.ms/iotcentraldocsdps) 'nden 0 @no__t
- Cihaz şablonu bilgileri
- Özellik değerleri

Dakikada yeni bir anlık görüntü yazılır. Anlık görüntü şunları içerir:

- Son anlık görüntüden bu yana eklenen yeni cihazlar.
- Son anlık görüntüden bu yana değiştirilen özellik değerleri olan cihazlar.

> [!NOTE]
> Son anlık görüntü aktarılmadığından cihazlar silindi. Şu anda anlık görüntülerde silinen cihazlar için göstergeler yok.
>
> Her cihazın ait olduğu cihaz şablonu `instanceOf` alanı tarafından temsil edilir. Cihaz şablonunun adını almak için cihaz şablonu anlık görüntülerini dışarı aktarın.

İçe aktarılmış dosyalar kayıt başına tek bir satır içerir. Aşağıdaki örnekte JSON biçimindeki bir kayıt gösterilmektedir.

```json
{ 
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{ 
    "$cloudProperties":{ 
        "Color":"blue"
    },
    "EnvironmentalSensor":{ 
      "thsensormodel":{ 
        "reported":{ 
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{ 
        "reported":{ 
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{ 
      "totalStorage":{ 
        "reported":{ 
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{ 
        "reported":{ 
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates"></a>Cihaz şablonları

Sürekli veri dışa aktarma ilk açıldığında, tüm cihaz şablonlarının bulunduğu tek bir anlık görüntü dışa aktarılabilir. Her cihaz şablonu şunları içerir:
- cihaz şablonunun `@id`
- cihaz şablonunun `name`
- cihaz şablonunun `version`
- @No__t-0 ' ı `interfaces` ' i ve telemetri, Özellikler ve komut tanımlarını içeren cihaz
- `cloudProperties` tanımları
- @No__t-0 ile satır içi olarak geçersiz kılmalar ve başlangıç değerleri

Dakikada yeni bir anlık görüntü yazılır. Anlık görüntü şunları içerir:

- Son anlık görüntüden bu yana yeni cihaz şablonları eklendi. Bu, cihaz şablonlarının yeni sürümlerini içerir.
- Son anlık görüntüden bu yana değiştirilen geçersiz kılmalar, ilk değerler ve bulut özellikleri tanımlarını içeren cihaz şablonları.

> [!NOTE]
> Son anlık görüntü aktarılmadığından, cihaz şablonları silinir. Şu anda anlık görüntülerde Silinen cihaz şablonları için göstergeler yok.

İçe aktarılmış dosyalar kayıt başına tek bir satır içerir. Aşağıdaki örnekte JSON biçimindeki bir kayıt gösterilmektedir.

```json
{ 
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{ 
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[ 
      { 
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{ 
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[ 
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{ 
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{ 
                  "@value":"50"
                }
              },
              "visualizationDetail":{ 
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      { 
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{ 
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[ 
            { 
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{ 
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[ 
      { 
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{ 
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{ 
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık verilerinizi dışarı aktarmayı öğrenmiş olduğunuza göre, sonraki adıma geçin:

> [!div class="nextstepaction"]
> [IoT Central cihaz Köprüsü kullanarak diğer IoT bulutlarını bağlama](howto-build-iotc-device-bridge.md)
