---
title: IoT uzaktan izleme ile cihaz simülasyonu - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılulu kılavuzu, uzaktan izleme çözüm hızlandırıcısı ile aygıt simülatörünü nasıl kullanacağınızı gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 8babacfede6e13fde629492e1cd9f80af7f0e53f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943062"
---
# <a name="create-and-test-a-new-simulated-device"></a>Yeni bir simüle aygıt oluşturma ve test edin

Uzaktan İzleme çözüm hızlandırıcısı, kendi simüle edilmiş cihazlarınızı tanımlamanızı sağlar. Bu makalede, yeni bir simüle ampul aygıtı tanımlamak ve daha sonra yerel olarak test nasıl gösterir. Çözüm hızlandırıcı soğutucu ve kamyon gibi simüle cihazları içerir. Ancak, gerçek aygıtları dağıtmadan önce IoT çözümlerinizi sınamak için kendi simüle edilmiş aygıtlarınızı tanımlayabilirsiniz.

> [!NOTE]
> Bu makalede, aygıt simülasyon hizmetinde barındırılan simüle edilmiş aygıtların nasıl kullanılacağı açıklanmaktadır. Gerçek bir aygıt oluşturmak istiyorsanız, [cihazınızı Uzaktan İzleme çözüm hızlandırıcısına bağlayın'](iot-accelerators-connecting-devices.md)a bakın.

Bu nasıl yapılmalı kılavuzu, aygıt simülasyonu microservice'i nasıl özelleştirdiğinizi gösterir. Bu microservice Uzaktan İzleme çözüm hızlandırıcısının bir parçasıdır. Aygıt simülasyonu yeteneklerini göstermek için bu nasıl yapılan lar kılavuzu Contoso IoT uygulamasında iki senaryo kullanır:

İlk senaryoda, Contoso'nun mevcut **Chiller** aygıt türüne yeni bir telemetri türü eklersiniz.

İkinci senaryoda, Contoso yeni bir akıllı ampul cihazı test etmek istiyor. Testleri çalıştırmak için, aşağıdaki özelliklere sahip yeni bir benzetimli aygıt oluşturursunuz:

*Özellikler*

| Adı                     | Değerler                      |
| ------------------------ | --------------------------- |
| Renk                    | Beyaz, Kırmızı, Mavi            |
| Parlaklık               | 0 ile 100 arasında                    |
| Tahmini kalan yaşam | 10.000 saatten geri sayım |

*Telemetri*

Aşağıdaki tablo, ampulun buluta bildirdiği verileri veri akışı olarak gösterir:

| Adı   | Değerler      |
| ------ | ----------- |
| Durum | "on", "kapalı" |
| Sıcaklık | Derece F |
| çevrimiçi | doğru, yanlış |

> [!NOTE]
> **Çevrimiçi** telemetri değeri tüm benzetimtürleri için zorunludur.

*Yöntemler*

Aşağıdaki tablo, yeni aygıtın desteklediği eylemleri gösterir:

| Adı        |
| ----------- |
| Aç   |
| Kapatın  |

*İlk durum*

Aşağıdaki tablo aygıtın başlangıç durumunu gösterir:

| Adı                     | Değerler |
| ------------------------ | -------|
| İlk renk            | Beyaz  |
| İlk parlaklık       | 75     |
| İlk kalan yaşam   | 10,000 |
| İlk telemetri durumu | "üzerinde"   |
| İlk telemetri sıcaklığı | 200   |

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliğine ihtiyacınız var.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunu takip etmek için şunları yapmanız gerekir:

* Visual Studio Code. [Mac, Linux ve Windows için Visual Studio Kodu indirebilirsiniz.](https://code.visualstudio.com/download)
* .NET Çekirdek. [Mac, Linux ve Windows için .NET Core'u](https://www.microsoft.com/net/download)indirebilirsiniz.
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* Postacı. [Mac, Windows veya Linux için Postacı'yı](https://www.getpostman.com/apps)indirebilirsiniz.
* Azure aboneliğinize dağıtılan bir [IoT hub'ı.](../../articles/iot-hub/iot-hub-create-through-portal.md) Bu kılavuzdaki adımları tamamlamak için IoT hub'ın bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portalından alabilirsiniz.
* SQL API kullanan ve [güçlü tutarlılık](../../articles/cosmos-db/how-to-manage-database-account.md)için yapılandırılan bir Cosmos DB veritabanı. Bu kılavuzdaki adımları tamamlamak için Cosmos DB veritabanının bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portalından alabilirsiniz.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Geliştirme ortamınızı hazırlamak için aşağıdaki görevleri tamamlayın:

* Cihaz simülasyonu microservice için kaynak indirin.
* Depolama bağdaştırıcısı microservice için kaynağı indirin.
* Depolama bağdaştırıcısını microservice'i yerel olarak çalıştırın.

Bu makaledeki yönergeler Windows kullandığınızı varsayar. Başka bir işletim sistemi kullanıyorsanız, bazı dosya yollarını ve komutlarını ortamınıza uyacak şekilde ayarlamanız gerekebilir.

### <a name="download-the-microservices"></a>Mikro hizmetleri indirin

GitHub'dan [yerel](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) makinenizde uygun bir konuma uzaktan izleme mikro hizmetlerini indirin ve indirin. Makale, bu klasörün adını **uzaktan izleme-services-dotnet-master**olduğunu varsayar.

Cihaz [simülasyonu microservice'i](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) GitHub'dan yerel makinenizde uygun bir konuma indirin ve indirin. Makale, bu klasörün adını **aygıt-simülasyon-dotnet-master**varsayar.

### <a name="run-the-storage-adapter-microservice"></a>Depolama adaptörmikroservice çalıştırın

Visual Studio Code'da **uzaktan izleme-hizmetler-dotnet-master\storage-adaptör** klasörünü açın. Çözülmemiş bağımlılıkları düzeltmek için geri **yükleme** düğmelerini tıklatın.

Depolama **bağdaştırıcısı/WebService/appsettings.ini** dosyasını açın ve Cosmos DB bağlantı dizenizi **documentDBConnectionString** değişkenine atayın.

Microservice'i yerel olarak çalıştırmak için **Hata Ayıklama'yı**> Başlat Hata Ayıklama'yı tıklatın.

Visual Studio Code'daki **Terminal** penceresi, web hizmeti sağlık kontrolü için bir [http://127.0.0.1:9022/v1/status](http://127.0.0.1:9022/v1/status)URL de dahil olmak üzere çalışan mikro hizmetten çıktıyı gösterir: . Bu adrese gidince, durum "Tamam: Canlı ve iyi" olmalıdır.

Sonraki adımları tamamlarken depolama bağdaştırıcısı microservice Visual Studio Code bu durumda çalışan bırakın.

## <a name="modify-the-chiller"></a>Soğutucuyu değiştirin

Bu bölümde, varolan **Chiller** aygıt türüne yeni bir **İç Sıcaklık** telemetri türü eklersiniz:

1. Yerel makinenizde yeni bir klasör **C:\temp\devicemodels** oluşturun.

1. Aygıt simülasyonu microservice indirilen kopyasından aşağıdaki dosyaları yeni klasörünüze kopyalayın:

    | Kaynak | Hedef |
    | ------ | ----------- |
    | Hizmetler\data\devicemodels\chiller-01.json | C:\temp\devicemodels\chiller-01.json |
    | Hizmetler\data\devicemodels\scripts\chiller-01-state.js | C:\temp\devicemodels\scripts\chiller-01-state.js |
    | Hizmetler\data\devicemodels\scripts\Reboot-method.js | C:\temp\devicemodels\scripts\Reboot-method.js |
    | Hizmetler\data\devicemodels\scripts\FirmwareUpdate-method.js | C:\temp\devicemodels\scripts\FirmwareUpdate-method.js |
    | Hizmetler\data\devicemodels\scripts\EmergencyValveRelease-method.js | C:\temp\devicemodels\scripts\EmergencyValveRelease-method.js |
    | Hizmetler\data\devicemodels\scripts\IncreasePressure-method.js | C:\temp\devicemodels\scripts\IncreasePressure-method.js |

1. **C:\temp\devicemodels\chiller-01.json** dosyasını açın.

1. **InitialState** bölümünde aşağıdaki iki tanım ekleyin:

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

1. **C:\temp\devicemodels\chiller-01.json** dosyasını kaydedin.

1. **C:\temp\devicemodels\scripts\chiller-01-state.js** dosyasını açın.

1. **Durum** değişkenine aşağıdaki alanları ekleyin:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. **Ana** işlevi aşağıdaki gibi güncelleyin:

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

## <a name="create-the-lightbulb"></a>Ampulü oluşturun

Bu bölümde, yeni bir **Ampul** aygıt türü tanımlarsınız:

1. **C:\temp\devicemodels\ampul-01.json** dosyası oluşturun ve aşağıdaki içeriği ekleyin:

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

    Değişiklikleri **C:\temp\devicemodels\lightbulb-01.json**olarak kaydedin.

1. **C:\temp\devicemodels\scripts\lightbulb-01-state.js** dosyası oluşturun ve aşağıdaki içeriği ekleyin:

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

    Değişiklikleri **C:\temp\devicemodels\scripts\lightbulb-01-state.js**olarak kaydedin.

1. **C:\temp\devicemodels\scripts\SwitchOn-method.js** dosyası oluşturun ve aşağıdaki içeriği ekleyin:

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

    Değişiklikleri **C:\temp\devicemodels\scripts\SwitchOn-method.js'ye**kaydet.

1. **C:\temp\devicemodels\scripts\SwitchOff-method.js** dosyası oluşturun ve aşağıdaki içeriği ekleyin:

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

    Değişiklikleri **C:\temp\devicemodels\scripts\SwitchOff-method.js**olarak kaydedin.

Şimdi **Chiller** aygıt türünün özelleştirilmiş bir sürümünü oluşturduk ve yeni bir **Ampul** aygıt türü oluşturdunuz.

## <a name="test-the-devices"></a>Aygıtları test edin

Bu bölümde, önceki bölümlerde oluşturduğunuz aygıt türlerini yerel olarak sınarsınız.

### <a name="run-the-device-simulation-microservice"></a>Cihaz simülasyonu microservice çalıştırın

GitHub'dan indirdiğiniz **aygıt simülasyonu-dotnet-master** klasörünü Visual Studio Code'un yeni bir örneğinde açın. Çözülmemiş bağımlılıkları düzeltmek için geri **yükleme** düğmelerini tıklatın.

**WebService/appsettings.ini** dosyasını açın ve Cosmos DB bağlantı dizenizi **documentdb_connstring** değişkenine atayın ve ayarları aşağıdaki gibi değiştirin:

```ini
device_models_folder = C:\temp\devicemodels\

device_models_scripts_folder = C:\temp\devicemodels\scripts\
```

Microservice'i yerel olarak çalıştırmak için **Hata Ayıklama'yı**> Başlat Hata Ayıklama'yı tıklatın.

Visual Studio Code'daki **Terminal** penceresi çalışan mikro hizmetten çıktıgösterir.

Sonraki adımları tamamlarken, aygıt simülasyonu microservice Visual Studio Code bu durumda çalışan bırakın.

### <a name="set-up-a-monitor-for-device-events"></a>Aygıt olayları için monitör ayarlama

Bu bölümde, IoT hub'ınıza bağlı aygıtlardan gönderilen telemetriyi görüntülemek için bir olay monitörü ayarlamak için Azure CLI'yi kullanırsınız.

Aşağıdaki komut dosyası, IoT hub'ınızın adının **aygıt simülasyontesti**olduğunu varsayar.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-iot

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Benzetimli aygıtları test ederken olay monitörünü çalışır durumda bırakın.

### <a name="create-a-simulation-with-the-updated-chiller-device-type"></a>Güncelleştirilmiş soğutucu aygıt türüyle simülasyon oluşturma

Bu bölümde, güncelleştirilmiş soğutucu aygıt türünü kullanarak bir simülasyon çalıştırmak için aygıt simülasyonu microservice istemek için Postacı aracını kullanın. Postacı, bir web hizmetine REST istekleri göndermenizi sağlayan bir araçtır. İhtiyacınız olan Postacı yapılandırma **dosyaları, aygıt simülasyonu-dotnet** deposunun yerel kopyasında bulunur.

Postacı kurmak için:

1. Yerel makinenizde Postacı'yı açın.

1. **Dosya > Alma'yı**tıklatın. Ardından **Dosyaları Seç'i**tıklatın.

1. **Aygıt simülasyonu-dotnet-master/docs/postacı** klasörüne gidin. **Azure IoT Aygıt Simülasyonu çözüm hızlandırıcısı.postman_collection** ve **Azure IoT Aygıt Simülasyonu çözüm hızlandırıcısı.postman_environment'yi** seçin ve **Aç'ı**tıklatın.

1. Azure **IoT Aygıt Simülasyonu çözüm hızlandırıcısını** gönderebileceğiniz isteklere genişletin.

1. **Ortam Yok'u** tıklatın ve **Azure IoT Aygıt Simülasyonu çözüm hızlandırıcıyı**seçin.

Artık Postacı çalışma alanınızda cihaz simülasyonu microservice ile etkileşim de kullanabileceğiniz bir koleksiyon ve ortam yüklü.

Simülasyonu yapılandırmak ve çalıştırmak için:

1. Postacı koleksiyonunda, **değiştirilmiş chiller simülasyonu oluştur'u** seçin ve **Gönder'i**tıklatın. Bu istek, benzetilen soğutucu aygıt türünün dört örneğini oluşturur.

1. Azure CLI penceresindeki olay izleme çıkışı, yeni **internal_temperature** değerleri de dahil olmak üzere benzetilen aygıtlardan gelen telemetriyi gösterir.

Simülasyonu durdurmak için Postacı'daki **simülasyonu Durdur** isteğini seçin ve **Gönder'e**tıklayın.

### <a name="create-a-simulation-with-the-lightbulb-device-type"></a>Ampul cihaz türü ile bir simülasyon oluşturma

Bu bölümde, ampul cihaz türünü kullanarak bir simülasyon çalıştırmak için cihaz simülasyonu microservice istemek için Postacı aracını kullanın. Postacı, bir web hizmetine REST istekleri göndermenizi sağlayan bir araçtır.

Simülasyonu yapılandırmak ve çalıştırmak için:

1. Postacı koleksiyonunda **ampul simülasyonu oluştur'u** seçin ve **Gönder'e**tıklayın. Bu istek, benzetilen ampul aygıt türünün iki örneğini oluşturur.

1. Azure CLI penceresindeki olay monitörü çıkışı, simüle edilen ampullerden gelen telemetriyi gösterir.

Simülasyonu durdurmak için Postacı'daki **simülasyonu Durdur** isteğini seçin ve **Gönder'e**tıklayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İki yerel olarak çalışan iki mikro hizmeti Visual Studio Code örneklerinde durdurabilirsiniz **(Hata Ayıklama > Hata Ayıklama'yı Durdurun).**

IoT Hub ve Cosmos DB örneklerini artık istemiyorsanız, gereksiz ücretleri önlemek için bunları Azure aboneliğinizden silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, özel bir simüle aygıt türleri oluşturmak ve yerel cihaz simülasyonmicroservice çalıştırarak bunları test nasıl gösterdi.

Önerilen bir sonraki adım, özel simüle edilmiş aygıt türlerinizi [Uzaktan İzleme çözüm hızlandırıcısına](iot-accelerators-remote-monitoring-deploy-simulated-device.md)nasıl dağıtılacağınızı öğrenmektir.
