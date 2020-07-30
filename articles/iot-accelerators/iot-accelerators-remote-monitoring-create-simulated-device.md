---
title: IoT uzaktan izleme ile cihaz benzetimi-Azure | Microsoft Docs
description: Bu nasıl yapılır Kılavuzu, uzaktan izleme çözüm hızlandırıcısında cihaz simülatörünü nasıl kullanacağınızı gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.custom: mqtt, devx-track-javascript
ms.openlocfilehash: c0aac897d85cd96d537238bc81cbc89d3626a07c
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422768"
---
# <a name="create-and-test-a-new-simulated-device"></a>Yeni bir sanal cihaz oluşturma ve test etme

Uzaktan Izleme çözümü hızlandırıcısı, kendi sanal cihazlarınızı tanımlamanızı sağlar. Bu makalede yeni bir sanal açık ampul cihazının nasıl tanımlanacağı ve ardından yerel olarak nasıl test yapılacağı gösterilir. Çözüm Hızlandırıcısı, chilve structuralks gibi sanal cihazları içerir. Ancak, gerçek cihazları dağıtmadan önce IoT çözümlerinizi test etmek için kendi sanal cihazlarınızı tanımlayabilirsiniz.

> [!NOTE]
> Bu makalede, cihaz benzetimi hizmetinde barındırılan sanal cihazların nasıl kullanılacağı açıklanır. Gerçek bir cihaz oluşturmak istiyorsanız, bkz. [cihazınızı uzaktan izleme çözüm hızlandırıcısına bağlama](iot-accelerators-connecting-devices.md).

Bu nasıl yapılır kılavuzunda, cihaz benzetimi mikro hizmetini nasıl özelleştireceğiniz gösterilmektedir. Bu mikro hizmet, uzaktan Izleme çözümü hızlandırıcının bir parçasıdır. Cihaz benzetimi yeteneklerini göstermek için, bu nasıl yapılır Kılavuzu contoso IoT uygulamasında iki senaryoyu kullanır:

İlk senaryoda, contoso 'nun var olan **Chiller** cihaz türüne yeni bir telemetri türü eklersiniz.

İkinci senaryoda, contoso yeni bir akıllı ampul cihazını test etmek istiyor. Testleri çalıştırmak için, aşağıdaki özelliklere sahip yeni bir sanal cihaz oluşturursunuz:

*Özellikler*

| Name                     | Değerler                      |
| ------------------------ | --------------------------- |
| Color (Renk)                    | Beyaz, kırmızı, mavi            |
| Parlaklık               | 0-100                    |
| Tahmini kalan ömür | 10.000 saatten geri sayım |

*Telemetri*

Aşağıdaki tabloda, açık ampul 'in veri akışı olarak buluta rapor aldığı veriler gösterilmektedir:

| Name   | Değerler      |
| ------ | ----------- |
| Durum | "açık", "kapalı" |
| Sıcaklık | Derece F |
| çevrimiçi | doğru, yanlış |

> [!NOTE]
> **Çevrimiçi** telemetri değeri, tüm benzetimli türler için zorunludur.

*Yöntemler*

Aşağıdaki tabloda yeni cihazın desteklediği eylemler gösterilmektedir:

| Name        |
| ----------- |
| Geçiş yap   |
| Geçiş kapalı  |

*İlk durum*

Aşağıdaki tabloda cihazın ilk durumu gösterilmektedir:

| Name                     | Değerler |
| ------------------------ | -------|
| İlk renk            | Beyaz  |
| İlk parlaklık       | 75     |
| Başlangıçtaki kalan ömür   | 10,000 |
| İlk telemetri durumu | dayanır   |
| İlk telemetri sıcaklığı | 200   |

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için etkin bir Azure aboneliğine ihtiyacınız vardır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu izlemek için şunlar gerekir:

* Visual Studio Code. [Mac, Linux ve Windows için Visual Studio Code indirebilirsiniz](https://code.visualstudio.com/download).
* .NET Core. [Mac, Linux ve Windows için .NET Core 'u](https://www.microsoft.com/net/download)indirebilirsiniz.
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* Postman. [Mac, Windows veya Linux Için Postman](https://www.getpostman.com/apps)indirebilirsiniz.
* [Azure aboneliğinize dağıtılan bir IoT Hub 'ı](../../articles/iot-hub/iot-hub-create-through-portal.md). Bu kılavuzdaki adımları tamamlayabilmeniz için IoT Hub 'ın bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portal alabilirsiniz.
* SQL API 'sini kullanan ve [güçlü tutarlılık](../../articles/cosmos-db/how-to-manage-database-account.md)için yapılandırılmış Cosmos DB veritabanı. Bu kılavuzdaki adımları tamamlayabilmeniz için Cosmos DB veritabanının bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portal alabilirsiniz.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Geliştirme ortamınızı hazırlamak için aşağıdaki görevleri doldurun:

* Cihaz benzetimi mikro hizmeti için kaynağı indirin.
* Depolama bağdaştırıcısı mikro hizmeti için kaynağı indirin.
* Depolama bağdaştırıcısı mikro hizmetini yerel olarak çalıştırın.

Bu makaledeki yönergeler Windows kullandığınızı varsayar. Başka bir işletim sistemi kullanıyorsanız, ortamınıza uyacak şekilde bazı dosya yollarını ve komutları ayarlamanız gerekebilir.

### <a name="download-the-microservices"></a>Mikro hizmetleri indirin

GitHub 'dan [Uzaktan izleme mikro hizmetlerini](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) indirin ve yerel makinenizde uygun bir konuma ayıklayın. Makalede bu klasörün adı **Remote-Monitoring-Services-DotNet-Master**olduğu varsayılır.

[Cihaz benzetimi mikro hizmetini](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) GitHub 'dan yerel makinenizde uygun bir konuma indirip sıkıştırmasını açın. Makale, bu klasörün adının **cihaz-simülasyon-DotNet-Master**olduğunu varsayar.

### <a name="run-the-storage-adapter-microservice"></a>Depolama bağdaştırıcısı mikro hizmetini çalıştırma

Visual Studio Code 'de **Remote-Monitoring-Services-DotNet-master\storage-Adapter** klasörünü açın. Çözümlenmemiş bağımlılıkları onarmak için **geri yükleme** düğmelerine tıklayın.

**Storage-Adapter/WebService/appsettings.ini** dosyasını açın ve Cosmos DB Bağlantı dizenizi **documentdbconnectionstring** değişkenine atayın.

Mikro hizmeti yerel olarak çalıştırmak için hata ayıklama **> hata ayıklamayı Başlat**' a tıklayın.

Visual Studio Code **Terminal** penceresinde, çalışan mikro hizmetin Web hizmeti sistem durumu denetimi URL 'si de dahil olmak üzere bir çıktı gösterilmektedir: [http://127.0.0.1:9022/v1/status](http://127.0.0.1:9022/v1/status) . Bu adrese gittiğinizde, durumun "Tamam: canlı ve iyi" olması gerekir.

Sonraki adımları tamamlayarak bu Visual Studio Code örneğinde depolama bağdaştırıcısı mikro hizmetini çalışır durumda bırakın.

## <a name="modify-the-chiller"></a>Chil'i değiştirme

Bu bölümde, var olan **Chiller** cihaz türüne yeni bir **iç sıcaklık** telemetri türü eklersiniz:

1. Yerel makinenizde **C:\temp\devicemodellerini** yeni bir klasör oluşturun.

1. Aşağıdaki dosyaları, cihaz simülasyon mikro hizmetinin indirilen kopyasından yeni klasörünüze kopyalayın:

    | Kaynak | Hedef |
    | ------ | ----------- |
    | Üzerinde Services\data\devicemodels\chiller-01.js | Üzerinde C:\temp\devicemodels\chiller-01.js |
    | Services\data\devicemodels\scripts\chiller-01-state.js | C:\temp\devicemodels\scripts\chiller-01-state.js |
    | Services\data\devicemodels\scripts\Reboot-method.js | C:\temp\devicemodels\scripts\Reboot-method.js |
    | Services\data\devicemodels\scripts\FirmwareUpdate-method.js | C:\temp\devicemodels\scripts\FirmwareUpdate-method.js |
    | Services\data\devicemodels\scripts\EmergencyValveRelease-method.js | C:\temp\devicemodels\scripts\EmergencyValveRelease-method.js |
    | Services\data\devicemodels\scripts\IncreasePressure-method.js | C:\temp\devicemodels\scripts\IncreasePressure-method.js |

1. C:\temp\devicemodels\chiller-01.jsdosya **üzerinde** açın.

1. **InitialState** bölümünde aşağıdaki iki tanımı ekleyin:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. **Telemetri** dizisinde aşağıdaki tanımı ekleyin:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. **C:\temp\devicemodels\chiller-01.js** dosyaya kaydedin.

1. **C:\temp\devicemodels\scripts\chiller-01-state.js** dosyasını açın.

1. **Durum** değişkenine aşağıdaki alanları ekleyin:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. **Ana** işlevi aşağıdaki gibi güncelleştirin:

    ```js
    function main(context, previousState, previousProperties) {

        // Restore the global state before generating the new telemetry, so that
        // the telemetry can apply changes using the previous function state.
        restoreSimulation(previousState, previousProperties);

        // 75F +/- 5%,  Min 25F, Max 100F
        state.temperature = vary(75, 5, 25, 100);

        // 70% +/- 5%,  Min 2%, Max 99%
        state.humidity = vary(70, 5, 2, 99);

        // 65F +/- 2%,  Min 15F, Max 125F
        state.internal_temperature = vary(65, 2, 15, 125);

        log("Simulation state: " + state.simulation_state);
        if (state.simulation_state === "high_pressure") {
            // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
            state.pressure = vary(250, 25, 50, 300);
        } else {
            // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
            state.pressure = vary(150, 10, 50, 300);
        }

        updateState(state);
        return state;
    }
    ```

1. **C:\temp\devicemodels\scripts\chiller-01-state.js** dosyasını kaydedin.

## <a name="create-the-lightbulb"></a>Ampul oluşturma

Bu bölümde, yeni bir **lightampul** cihaz türü tanımlarsınız:

1. Üzerinde bir dosya **C:\temp\devicemodels\lightbulb-01.js** oluşturun ve aşağıdaki içeriği ekleyin:

    ```json
    {
      "SchemaVersion": "1.0.0",
      "Id": "lightbulb-01",
      "Version": "0.0.1",
      "Name": "Lightbulb",
      "Description": "Smart lightbulb device.",
      "Protocol": "MQTT",
      "Simulation": {
        "InitialState": {
          "online": true,
          "temperature": 200.0,
          "temperature_unit": "F",
          "status": "on"
        },
        "Interval": "00:00:20",
        "Scripts": [
          {
            "Type": "javascript",
            "Path": "lightbulb-01-state.js"
          }
        ]
      },
      "Properties": {
        "Type": "Lightbulb",
        "Color": "White",
        "Brightness": 75,
        "EstimatedRemainingLife": 10000
      },
      "Tags": {
        "Location": "Building 2",
        "Floor": "2",
        "Campus": "Redmond"
      },
      "Telemetry": [
        {
          "Interval": "00:00:20",
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"status\":\"${status}\"}",
          "MessageSchema": {
            "Name": "lightbulb-status;v1",
            "Format": "JSON",
            "Fields": {
              "temperature": "double",
              "temperature_unit": "text",
              "status": "text"
            }
          }
        }
      ],
      "CloudToDeviceMethods": {
        "SwitchOn": {
          "Type": "javascript",
          "Path": "SwitchOn-method.js"
        },
        "SwitchOff": {
          "Type": "javascript",
          "Path": "SwitchOff-method.js"
        }
      }
    }
    ```

    **C:\temp\devicemodels\lightbulb-01.jsüzerindeki**değişiklikleri kaydedin.

1. **C:\temp\devicemodels\scripts\lightbulb-01-state.js** bir dosya oluşturun ve aşağıdaki içeriği ekleyin:

    ```javascript
    "use strict";

    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };

    // Default device properties
    var properties = {};

    /**
     * Restore the global state using data from the previous iteration.
     *
     * @param previousState device state from the previous iteration
     * @param previousProperties device properties from the previous iteration
     */
    function restoreSimulation(previousState, previousProperties) {
      // If the previous state is null, force a default state
      if (previousState) {
        state = previousState;
      } else {
        log("Using default state");
      }

      if (previousProperties) {
        properties = previousProperties;
      } else {
        log("Using default properties");
      }
    }

    /**
     * Simple formula generating a random value around the average
     * in between min and max
     *
     * @returns random value with given parameters
     */
    function vary(avg, percentage, min, max) {
      var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
      value = Math.max(value, min);
      value = Math.min(value, max);
      return value;
    }

    /**
     * Simple formula that sometimes flips the status of the lightbulb
     */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }

    /**
     * Entry point function called by the simulation engine.
     * Returns updated simulation state.
     * Device property updates must call updateProperties() to persist.
     *
     * @param context             The context contains current time, device model and id
     * @param previousState       The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState, previousProperties) {

      // Restore the global device properties and the global state before
      // generating the new telemetry, so that the telemetry can apply changes
      // using the previous function state.
      restoreSimulation(previousState, previousProperties);

      state.temperature = vary(200, 5, 150, 250);

      // Make this flip every so often
      state.status = flip(state.status);

      updateState(state);

      return state;
    }
    ```

    Değişiklikleri **C:\temp\devicemodels\scripts\lightbulb-01-state.js**kaydedin.

1. **C:\temp\devicemodels\scripts\SwitchOn-method.js** bir dosya oluşturun ve aşağıdaki içeriği ekleyin:

    ```javascript
    "use strict";

    // Default state
    var state = {
      status: "on"
    };

    /**
     * Entry point function called by the method.
     *
     * @param context        The context contains current time, device model and id
     * @param previousState  The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState) {
      log("Executing lightbulb Switch On method.");
      state.status = "on";
      updateState(state);
    }
    ```

    Değişiklikleri **C:\temp\devicemodels\scripts\SwitchOn-method.js**kaydedin.

1. **C:\temp\devicemodels\scripts\SwitchOff-method.js** bir dosya oluşturun ve aşağıdaki içeriği ekleyin:

    ```javascript
    "use strict";

    // Default state
    var state = {
      status: "on"
    };

    /**
     * Entry point function called by the method.
     *
     * @param context        The context contains current time, device model and id
     * @param previousState  The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState) {
      log("Executing lightbulb Switch Off method.");
      state.status = "off";
      updateState(state);
    }
    ```

    Değişiklikleri **C:\temp\devicemodels\scripts\SwitchOff-method.js**kaydedin.

Artık, **chilcihaz** türünün özelleştirilmiş bir sürümünü oluşturdunuz ve yeni bir **lightampul** cihaz türü oluşturdunuz.

## <a name="test-the-devices"></a>Cihazları test etme

Bu bölümde, önceki bölümlerde oluşturduğunuz cihaz türlerini yerel olarak test edersiniz.

### <a name="run-the-device-simulation-microservice"></a>Cihaz benzetimi mikro hizmetini çalıştırma

Yeni bir Visual Studio Code örneğinde GitHub 'dan indirdiğiniz **cihaz benzetimi-DotNet-ana** klasörünü açın. Çözümlenmemiş bağımlılıkları onarmak için **geri yükleme** düğmelerine tıklayın.

**Web hizmeti/appsettings.ini** dosyasını açın ve Cosmos DB Bağlantı dizenizi **documentdb_connstring** değişkenine atayın ve ayrıca ayarları aşağıdaki gibi değiştirin:

```ini
device_models_folder = C:\temp\devicemodels\

device_models_scripts_folder = C:\temp\devicemodels\scripts\
```

Mikro hizmeti yerel olarak çalıştırmak için hata ayıklama **> hata ayıklamayı Başlat**' a tıklayın.

Visual Studio Code **Terminal** penceresinde, çalışan mikro hizmetten alınan çıkış gösterilmektedir.

Sonraki adımları tamamladıktan sonra cihaz benzetimi mikro hizmetini bu Visual Studio Code örneğinde çalışır durumda bırakın.

### <a name="set-up-a-monitor-for-device-events"></a>Cihaz olayları için izleyici ayarlama

Bu bölümde, IoT Hub 'ınıza bağlı cihazlardan gönderilen telemetrinin görüntüleneceği bir olay izleyicisi ayarlamak için Azure CLı 'yi kullanırsınız.

Aşağıdaki betik, IoT Hub 'ınızın adının **cihaz benzetimi-test**olduğunu varsayar.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-iot

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Sanal cihazları test ederken olay izleyicisini çalışır durumda bırakın.

### <a name="create-a-simulation-with-the-updated-chiller-device-type"></a>Güncelleştirilmiş chilcihaz türüyle simülasyon oluşturma

Bu bölümde, cihaz benzetimi mikro hizmetinin güncelleştirilmiş chilcihaz türünü kullanarak benzetim çalıştırmasını istemek için Postman aracını kullanırsınız. Postman, bir Web hizmetine REST istekleri göndermenize olanak sağlayan bir araçtır. İhtiyacınız olan Postman yapılandırma dosyaları, **cihaz benzetimi-DotNet** deposunun yerel kopyasıdır.

Postman 'yi ayarlamak için:

1. Yerel makinenizde Postman 'yi açın.

1. **Dosya > Içeri aktar**' a tıklayın. Ardından **dosyaları seç**' e tıklayın.

1. **Cihaz-simülasyon-DotNet-Master/docs/Postman** klasörüne gidin. **Azure IoT cihaz benzetimi Çözüm Hızlandırıcısı. postman_collection** ve **Azure IoT cihaz benzetimi çözüm Hızlandırıcısı. postman_environment** ' nı seçin ve **Aç**' a tıklayın.

1. **Azure IoT cihaz benzetimi çözüm hızlandırıcıyı** , gönderebileceğiniz isteklere genişletin.

1. **Ortam yok** ' a tıklayın ve **Azure IoT cihaz benzetimi Çözüm Hızlandırıcısı**' nı seçin.

Artık Postman çalışma alanınızda yüklü olan ve cihaz benzetimi mikro hizmeti ile etkileşim kurmak için kullanabileceğiniz bir koleksiyon ve ortamınız vardır.

Benzetimi yapılandırmak ve çalıştırmak için:

1. Postman koleksiyonunda, **değiştirilen chilsimülasyonu oluştur** ' u seçin ve **Gönder**' e tıklayın. Bu istek, benzetimli chilcihaz türünün dört örneğini oluşturur.

1. Azure CLı penceresindeki olay izleyici çıktısı, yeni **internal_temperature** değerleri de dahil olmak üzere sanal cihazlardan gelen Telemetriyi gösterir.

Benzetimi durdurmak için Postman 'daki **Benzetim Isteğini durdur** ' u seçin ve **Gönder**' e tıklayın.

### <a name="create-a-simulation-with-the-lightbulb-device-type"></a>Ampul cihaz türü ile simülasyon oluşturma

Bu bölümde, lightampul cihaz türünü kullanarak bir benzetim çalıştırmak üzere cihaz benzetimi mikro hizmetini istemek için Postman aracını kullanırsınız. Postman, bir Web hizmetine REST istekleri göndermenize olanak sağlayan bir araçtır.

Benzetimi yapılandırmak ve çalıştırmak için:

1. Postman koleksiyonunda, **lightampul simülasyonu oluştur** ' u seçin ve **Gönder**' e tıklayın. Bu istek, sanal açık ampul cihaz türünün iki örneğini oluşturur.

1. Azure CLı penceresindeki olay izleyici çıktısı, sanal açık olan sanal bilgisayarlardan Telemetriyi gösterir.

Benzetimi durdurmak için Postman 'daki **Benzetim Isteğini durdur** ' u seçin ve **Gönder**' e tıklayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Visual Studio Code örneklerinde yerel olarak çalışan iki mikro hizmeti durdurabilirsiniz (**hata ayıklama > hata ayıklamayı durdurabilirsiniz**).

Artık IoT Hub ve Cosmos DB örneklerine ihtiyacınız yoksa, gereksiz ücretlerden kaçınmak için Azure aboneliğinizden silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, özel bir sanal cihaz türleri oluşturma ve cihaz benzetimi mikro hizmetini yerel olarak çalıştırarak test etme işlemleri gösterilmektedir.

Önerilen sonraki adım, özel sanal cihaz türlerinizi [Uzaktan izleme çözüm hızlandırıcısına](iot-accelerators-remote-monitoring-deploy-simulated-device.md)dağıtmayı öğrenmaktır.
