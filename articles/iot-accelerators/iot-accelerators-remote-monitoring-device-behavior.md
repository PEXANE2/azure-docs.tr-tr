---
title: Uzaktan İzleme çözümünde simüle edilen aygıt - Azure | Microsoft Dokümanlar
description: Bu makalede, uzaktan izleme çözümünde simüle edilmiş bir aygıtın davranışını tanımlamak için JavaScript'in nasıl kullanılacağı açıklanmaktadır.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: c39ca0a018bd22844cf7e5350e6d3586319aac16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890856"
---
# <a name="implement-the-device-model-behavior"></a>Aygıt modeli davranışını uygulama

Makale [Anlayın aygıt modeli şema](iot-accelerators-remote-monitoring-device-schema.md) benzetilen bir benzetim aygıt modeli tanımlar. Bu makalede, benzetilen bir aygıtın davranışını uygulayan iki javascript dosyası türüne atıfta bulunulan:

- **Devlet** Aygıtın iç durumunu güncelleştirmek için sabit aralıklarla çalışan JavaScript dosyaları.
- **Yöntem** Çözüm aygıtta bir yöntem çağırdığında çalışan JavaScript dosyaları.

> [!NOTE]
> Aygıt modeli davranışları yalnızca aygıt simülasyon hizmetinde barındırılan simüle edilmiş aygıtlar içindir. Gerçek bir aygıt oluşturmak istiyorsanız, [cihazınızı Uzaktan İzleme çözüm hızlandırıcısına bağlayın'](iot-accelerators-connecting-devices.md)a bakın.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Benzetimli aygıtın durumunu denetleme
> * Simüle edilmiş bir aygıtın Uzaktan İzleme çözümünden gelen bir yöntem çağrısına nasıl yanıt verilebildiğini tanımlayın
> * Komut dosyalarınızı hata ayıklama

## <a name="state-behavior"></a>Durum davranışı

Aygıt modeli şemasının [Simülasyon](../../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#simulation) bölümü, simüle edilmiş bir aygıtın dahili durumunu tanımlar:

- `InitialState`aygıt durumu nesnesinin tüm özellikleri için ilk değerleri tanımlar.
- `Script`aygıt durumunu güncelleştirmek için bir zamanlamada çalışan bir JavaScript dosyasını tanımlar.

Aşağıdaki örnek, benzetilen bir soğutucu aygıtı için aygıt durumu nesnesinin tanımını gösterir:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:05",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

`InitialState` Simüle cihazın durumu, bölümde tanımlandığı gibi, simülasyon hizmeti tarafından bellekte tutulur. Durum bilgileri `main` **chiller-01-state.js'de**tanımlanan işleve giriş olarak geçirilir. Bu örnekte, simülasyon hizmeti **chiller-01-state.js** dosyasını her beş saniyede bir çalıştırUr. Komut dosyası, benzetilen aygıtın durumunu değiştirebilir.

Aşağıdaki tipik `main` bir işlevin anahatlarını gösterir:

```javascript
function main(context, previousState, previousProperties) {

  // Use the previous device state to
  // generate the new device state
  // returned by the function.

  return state;
}
```

`context` Parametre aşağıdaki özelliklere sahiptir:

- `currentTime`biçimi ile bir dize olarak`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, örneğin`Simulated.Chiller.123`
- `deviceModel`, örneğin`Chiller`

Parametre, `state` aygıt simülasyon hizmeti tarafından korunan aygıtın durumunu içerir. Bu değer, `state` önceki çağrıtarafından döndürülen `main`nesnedir.

Aşağıdaki örnek, simülasyon hizmeti `main` tarafından tutulan aygıt durumunu işlemek için yöntemin tipik bir uygulamasını gösterir:

```javascript
// Default state
var state = {
  online: true,
  temperature: 75.0,
  temperature_unit: "F",
  humidity: 70.0,
  humidity_unit: "%",
  pressure: 150.0,
  pressure_unit: "psig",
  simulation_state: "normal_pressure"
};

function restoreState(previousState) {
  // If the previous state is null, force a default state
  if (previousState !== undefined && previousState !== null) {
      state = previousState;
  } else {
      log("Using default state");
  }
}

function main(context, previousState, previousProperties) {

  restoreState(previousState);

  // Update state

  return state;
}
```

Aşağıdaki örnek, yöntemin `main` zaman içinde değişen telemetri değerlerini nasıl simüle edebileceğini gösterir:

```javascript
/**
 * Simple formula generating a random value
 * around the average and between min and max
 */
function vary(avg, percentage, min, max) {
  var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
  value = Math.max(value, min);
  value = Math.min(value, max);
  return value;
}


function main(context, previousState, previousProperties) {

    restoreState(previousState);

    // 75F +/- 5%,  Min 25F, Max 100F
    state.temperature = vary(75, 5, 25, 100);

    // 70% +/- 5%,  Min 2%, Max 99%
    state.humidity = vary(70, 5, 2, 99);

    log("Simulation state: " + state.simulation_state);
    if (state.simulation_state === "high_pressure") {
        // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
        state.pressure = vary(250, 25, 50, 300);
    } else {
        // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
        state.pressure = vary(150, 10, 50, 300);
    }

    return state;
}
```

GitHub'da [soğutucu-01-state.js'nin](https://github.com/Azure/device-simulation-dotnet/blob/master/Services/data/devicemodels/scripts/chiller-01-state.js) tamamını görüntüleyebilirsiniz.

## <a name="method-behavior"></a>Yöntem davranışı

Aygıt modeli şemasının [CloudToDeviceMethods](../../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#cloudtodevicemethods) bölümü, simüle edilmiş bir aygıtın yanıt verme yöntemlerini tanımlar.

Aşağıdaki örnek, benzetimli bir soğutucu aygıtı tarafından desteklenen yöntemlerin listesini gösterir:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "FirmwareUpdate": {
    "Type": "javascript",
    "Path": "FirmwareUpdate-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Her yöntem, yöntemin davranışını uygulayan ilişkili bir JavaScript dosyasına sahiptir.

Şema `InitialState` bölümünde tanımlandığı gibi simüle cihazın durumu simülasyon servisi tarafından bellekte tutulur. Durum bilgileri, yöntem çağrıldığında `main` JavaScript dosyasında tanımlanan işleve giriş olarak aktarılır. Komut dosyası, benzetilen aygıtın durumunu değiştirebilir.

Aşağıdaki tipik `main` bir işlevin anahatlarını gösterir:

```javascript
function main(context, previousState, previousProperties) {

}
```

`context` Parametre aşağıdaki özelliklere sahiptir:

- `currentTime`biçimi ile bir dize olarak`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, örneğin`Simulated.Chiller.123`
- `deviceModel`, örneğin`Chiller`

Parametre, `state` aygıt simülasyon hizmeti tarafından korunan aygıtın durumunu içerir.

Parametre, `properties` IoT Hub aygıt ikizine bildirilen özellikler olarak yazılan aygıtın özelliklerini içerir.

Yöntemin davranışını uygulamaya yardımcı olmak için kullanabileceğiniz üç genel işlev vardır:

- `updateState`simülasyon servisi tarafından tutulan durumu güncellemek için.
- `updateProperty`tek bir aygıt özelliğini güncelleştirmek için.
- `sleep`uzun süren bir görevi simüle etmek için yürütmeyi duraklatmak için.

Aşağıdaki örnek, simüle edilmiş soğutucu aygıtları tarafından kullanılan **IncreasePressure-method.js** komut dosyasının kısaltılmış bir sürümünü gösterir:

```javascript
function main(context, previousState, previousProperties) {

    log("Starting 'Increase Pressure' method simulation (5 seconds)");

    // Pause the simulation and change the simulation mode so that the
    // temperature will fluctuate at ~250 when it resumes
    var state = {
        simulation_state: "high_pressure",
        CalculateRandomizedTelemetry: false
    };
    updateState(state);

    // Increase
    state.pressure = 210;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Increase
    state.pressure = 250;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Resume the simulation
    state.CalculateRandomizedTelemetry = true;
    updateState(state);

    log("'Increase Pressure' method simulation completed");
}
```

## <a name="debugging-script-files"></a>Komut dosyası dosyalarını hata ayıklama

Durum ve yöntem komut dosyalarını çalıştırmak için aygıt simülasyon hizmeti tarafından kullanılan Javascript yorumlayıcısına hata ayıklayıcı eklemek mümkün değildir. Ancak, bilgileri hizmet günlüğüne kaydedebilirsiniz. Yerleşik `log()` işlev, işlev yürütmeyi izlemek ve hata ayıklamak için bilgileri kaydetmenizi sağlar.

Sözdizimi hatası varsa, yorumlayıcı başarısız `Jint.Runtime.JavaScriptException` olur ve hizmet günlüğüne bir giriş yazar.

Hizmeti GitHub'daki [yerel olarak](https://github.com/Azure/device-simulation-dotnet#running-the-service-locally-eg-for-development-tasks) çalıştırma makalesi, aygıt simülasyon hizmetini yerel olarak nasıl çalıştırabileceğinizi gösterir. Hizmeti yerel olarak çalıştırmak, simüle edilmiş aygıtlarınızı buluta dağıtmadan önce hata ayıklamanızı kolaylaştırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kendi özel simüle aygıt modelinizin davranışını nasıl tanımlanabilirsiniz. Bu makalede, nasıl gösterin:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Benzetimli aygıtın durumunu denetleme
> * Simüle edilmiş bir aygıtın Uzaktan İzleme çözümünden gelen bir yöntem çağrısına nasıl yanıt verilebildiğini tanımlayın
> * Komut dosyalarınızı hata ayıklama

Artık benzetilen bir aygıtın davranışını nasıl belirtdiğinizi öğrendiğiniz için, önerilen bir sonraki [adım, benzetimli bir aygıtın](iot-accelerators-remote-monitoring-create-simulated-device.md)nasıl oluşturulabildiğini öğrenmektir.

Uzaktan İzleme çözümü hakkında daha fazla geliştirici bilgisi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
