---
title: Öğretici-genel bir Node.js istemci uygulamasını Azure IoT Central bağlama | Microsoft Docs
description: Bu öğreticide, bir cihaz geliştiricisi olarak, Node.js istemci uygulaması çalıştıran bir cihaza Azure IoT Central uygulamanıza nasıl bağlanacağı gösterilmektedir. Bir işlecin bağlı bir cihazla etkileşime girmesine izin veren görünümler ekleyerek otomatik olarak oluşturulan cihaz şablonunu değiştirirsiniz.
author: dominicbetts
ms.author: dobett
ms.date: 11/03/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom:
- mqtt
- device-developer
- devx-track-js
ms.openlocfilehash: 6175be702f0824ad2cd2b146e96641037407ca0b
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "96018809"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application-nodejs"></a>Öğretici: İstemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama (Node.js)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bu öğreticide, bir cihaz geliştiricisi olarak, Node.js istemci uygulamasını Azure IoT Central uygulamanıza nasıl bağlayabilmeniz gösterilmektedir. Node.js uygulaması, bir termostat cihazının davranışının benzetimini yapar. Uygulama IoT Central bağlandığı zaman, termostat cihaz modelinin model KIMLIĞINI gönderir. IoT Central, cihaz modelini almak ve sizin için bir cihaz şablonu oluşturmak üzere model KIMLIĞINI kullanır. Bir işlecin bir cihazla etkileşime geçmesini sağlamak için cihaz şablonuna özelleştirmeler ve görünümler eklersiniz.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Node.js cihaz kodu oluşturup çalıştırın ve IoT Central uygulamanıza bağlanın.
> * Cihazdan gönderilen sanal Telemetriyi görüntüleyin.
> * Özel görünümleri bir cihaz şablonuna ekleyin.
> * Cihaz şablonunu yayımlayın.
> * Cihaz özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı denetlemek için bir komut çağırın.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md). Uygulamanın 14 Temmuz 2020 tarihinde veya sonrasında oluşturulmuş olması gerekir.
* [Node.js](https://nodejs.org/) sürüm 6 veya üzeri yüklü bir geliştirme makinesi. `node --version`Sürümünüzü denetlemek için komut satırında çalıştırabilirsiniz. Bu öğreticideki yönergeler, Windows komut isteminde **node** komutunu çalıştırmakta olduğunuzu varsayar. Ancak, diğer birçok işletim sisteminde Node.js kullanabilirsiniz.
* Örnek kodu içeren Node.jsGitHub deposu [için Microsoft Azure ıOT SDK 'sının ](https://github.com/Azure/azure-iot-sdk-node) yerel bir kopyası. Deponun bir kopyasını indirmek için bu bağlantıyı kullanın: ZIP 'i [indirin](https://github.com/Azure/azure-iot-sdk-node/archive/master.zip). Sonra dosyayı yerel makinenizde uygun bir konuma ayıklayın.

## <a name="review-the-code"></a>Kodu gözden geçirin

Daha önce indirdiğiniz Node.js Microsoft Azure IoT SDK kopyasında, *Azure-IoT-SDK-node/Device/Samples/PNP/simple_thermostat.js* dosyasını bir metin düzenleyicisinde açın.

IoT Central bağlanmak için örneği çalıştırdığınızda, cihazı kaydetmek ve bir bağlantı dizesi oluşturmak için cihaz sağlama hizmeti 'ni (DPS) kullanır. Örnek, gereken DPS bağlantı bilgilerini komut satırı ortamından alır.

`main`Yöntemi:

* Bir `client` nesne oluşturur ve `dtmi:com:example:Thermostat;1` bağlantıyı açmadan önce model kimliğini ayarlar.
* Bir komut işleyicisi oluşturur.
* Her 10 saniyede bir sıcaklık telemetrisi göndermek için bir döngü başlatır.
* `maxTempSinceLastReboot`Özelliği IoT Central 'e gönderir. IoT Central, `serialNumber` cihaz modelinin parçası olmadığından özelliği yoksayar.
* Yazılabilir Özellikler işleyicisi oluşturur.

```javascript
async function main() {

  // ...

  // fromConnectionString must specify a transport, coming from any transport package.
  const client = Client.fromConnectionString(deviceConnectionString, Protocol);

  let resultTwin;
  try {
    // Add the modelId here
    await client.setOptions(modelIdObject);
    await client.open();

    client.onDeviceMethod(commandMaxMinReport, commandHandler);

    // Send Telemetry every 10 secs
    let index = 0;
    intervalToken = setInterval(() => {
      sendTelemetry(client, index).catch((err) => console.log('error', err.toString()));
      index += 1;
    }, telemetrySendInterval);

    // attach a standard input exit listener
    attachExitHandler(client);

    // Deal with twin
    try {
      resultTwin = await client.getTwin();
      const patchRoot = createReportPropPatch({ serialNumber: deviceSerialNum });
      const patchThermostat = createReportPropPatch({
        maxTempSinceLastReboot: deviceTemperatureSensor.getMaxTemperatureValue()
      });

      // the below things can only happen once the twin is there
      updateComponentReportedProperties(resultTwin, patchRoot);
      updateComponentReportedProperties(resultTwin, patchThermostat);

      // Setup the handler for desired properties
      desiredPropertyPatchHandler(resultTwin);

    } catch (err) {
      console.error('could not retrieve twin or report twin properties\n' + err.toString());
    }
  } catch (err) {
    console.error('could not connect Plug and Play client or could not attach interval function for telemetry\n' + err.toString());
  }
}
```

`provisionDevice`İşlevi, cihazın IoT Central kaydetmek ve bağlanmak IÇIN DPS nasıl kullandığını gösterir. Yük, model KIMLIĞINI içerir:

```javascript
async function provisionDevice(payload) {
  var provSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
  var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvProtocol(), provSecurityClient);

  if (!!(payload)) {
    provisioningClient.setProvisioningPayload(payload);
  }

  try {
    let result = await provisioningClient.register();
    deviceConnectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
  } catch (err) {
    console.error("error registering device: " + err.toString());
  }
}
```

`sendTelemetry`İşlevi, cihazın sıcaklık telemetrisini IoT Central nasıl göndereceğini gösterir. `getCurrentTemperatureObject`Yöntemi şuna benzer bir nesne döndürür `{ temperature: 45.6 }` :

```javascript
async function sendTelemetry(deviceClient, index) {
  console.log('Sending telemetry message %d...', index);
  const msg = new Message(
    JSON.stringify(
      deviceTemperatureSensor.updateSensor().getCurrentTemperatureObject()
    )
  );
  msg.contentType = 'application/json';
  msg.contentEncoding = 'utf-8';
  await deviceClient.sendEvent(msg);
}
```

`main`Yöntemi, özelliği IoT Central göndermek için aşağıdaki iki yöntemi kullanır `maxTempSinceLastReboot` . `main`Yöntemi `createReportPropPatch` Şuna benzer bir nesne ile çağırır `{maxTempSinceLastReboot: 80.9}` :

```javascript
const createReportPropPatch = (propertiesToReport) => {
  let patch;
  patch = { };
  patch = propertiesToReport;
  return patch;
};

const updateComponentReportedProperties = (deviceTwin, patch) => {
  deviceTwin.properties.reported.update(patch, function (err) {
    if (err) throw err;
    console.log('Properties have been reported for component');
  });
};
```

`main`Yöntemi, IoT Central _hedef sıcaklık_ yazılabilir özelliğindeki güncelleştirmeleri işlemek için aşağıdaki iki yöntemi kullanır. `propertyUpdateHandle`Sürüm ve durum kodu ile nasıl yanıt oluşturulduğuna dikkat edin:

```javascript
const desiredPropertyPatchHandler = (deviceTwin) => {
  deviceTwin.on('properties.desired', (delta) => {
    const versionProperty = delta.$version;

    Object.entries(delta).forEach(([propertyName, propertyValue]) => {
      if (propertyName !== '$version') {
        propertyUpdateHandler(deviceTwin, propertyName, null, propertyValue, versionProperty);
      }
    });
  });
};

const propertyUpdateHandler = (deviceTwin, propertyName, reportedValue, desiredValue, version) => {
  console.log('Received an update for property: ' + propertyName + ' with value: ' + JSON.stringify(desiredValue));
  const patch = createReportPropPatch(
    { [propertyName]:
      {
        'value': desiredValue,
        'ac': 200,
        'ad': 'Successfully executed patch for ' + propertyName,
        'av': version
      }
    });
  updateComponentReportedProperties(deviceTwin, patch);
  console.log('updated the property');
};
```

`main`Yöntemi, komuta yapılan çağrıları işlemek için aşağıdaki iki yöntemi kullanır `getMaxMinReport` . `getMaxMinReportObject`Yöntemi, raporu JSON nesnesi olarak oluşturur:

```javascript
const commandHandler = async (request, response) => {
  switch (request.methodName) {
  case commandMaxMinReport: {
    console.log('MaxMinReport ' + request.payload);
    await sendCommandResponse(request, response, 200, deviceTemperatureSensor.getMaxMinReportObject());
    break;
  }
  default:
    await sendCommandResponse(request, response, 404, 'unknown method');
    break;
  }
};

const sendCommandResponse = async (request, response, status, payload) => {
  try {
    await response.send(status, payload);
    console.log('Response to method \'' + request.methodName +
              '\' sent successfully.' );
  } catch (err) {
    console.error('An error ocurred when sending a method response:\n' +
              err.toString());
  }
};
```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

[!INCLUDE [iot-central-connection-configuration](../../../includes/iot-central-connection-configuration.md)]

## <a name="run-the-code"></a>Kodu çalıştırma

Örnek uygulamayı çalıştırmak için bir komut satırı ortamı açın ve *simple_thermostat.js* örnek dosyasını içeren *Azure-IoT-SDK-node/Device/Samples/PNP* klasörüne gidin.

[!INCLUDE [iot-central-connection-environment](../../../includes/iot-central-connection-environment.md)]

Gerekli paketleri yükler:

```cmd/sh
npm install
```

Örneği çalıştırın:

```cmd/sh
node simple_thermostat.js
```

Aşağıdaki çıktı, IoT Central kaydetme ve bağlanma için cihazı gösterir. Örnek daha sonra `maxTempSinceLastReboot` özelliği telemetri göndermeye başlamadan önce gönderir:

```cmd/sh
registration succeeded
assigned hub=iotc-.......azure-devices.net
deviceId=sample-device-01
payload=undefined
Connecting using connection string HostName=iotc-........azure-devices.net;DeviceId=sample-device-01;SharedAccessKey=Ci....=
Enabling the commands on the client
Please enter q or Q to exit sample.
The following properties will be updated for root interface:
{ maxTempSinceLastReboot: 55.20309427428496 }
Properties have been reported for component
Sending telemetry message 0...
Sending telemetry message 1...
Sending telemetry message 2...
Sending telemetry message 3...
```

[!INCLUDE [iot-central-monitor-thermostat](../../../includes/iot-central-monitor-thermostat.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verdiğini görebilirsiniz:

```cmd/sh
MaxMinReport 2020-10-15T12:00:00.000Z
Response to method 'getMaxMinReport' sent successfully.

...

Received an update for property: targetTemperature with value: {"value":86.3}
The following properties will be updated for root interface:
{
  targetTemperature: {
    value: { value: 86.3 },
    ac: 200,
    ad: 'Successfully executed patch for targetTemperature',
    av: 2
  }
}
```

## <a name="view-raw-data"></a>Ham verileri görüntüleme

[!INCLUDE [iot-central-monitor-thermostat-raw-data](../../../includes/iot-central-monitor-thermostat-raw-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

IoT Central öğreticiler kümesine devam etmeyi tercih ediyorsanız ve bir IoT Central çözümü oluşturma hakkında daha fazla bilgi edinmek istiyorsanız, bkz.:

> [!div class="nextstepaction"]
> [Ağ geçidi cihaz şablonu oluşturma](./tutorial-define-gateway-device-type.md)

Bir cihaz geliştiricisi olarak, Node.js kullanarak bir cihazın nasıl oluşturulacağı hakkında temel bilgileri öğrendiğinize göre, önerilen bazı sonraki adımlar şunlardır:

* Cihaz kodunuzu uygularken cihaz şablonlarının rolü hakkında daha fazla bilgi edinmek için cihaz [şablonu nedir?](./concepts-device-templates.md) makalesini okuyun.
* Cihazların IoT Central nasıl kaydedileceği ve cihaz bağlantılarının güvenliğini IoT Central sağlama hakkında daha fazla bilgi edinmek için [Azure IoT Central 'ye](./concepts-get-connected.md) bağlanın.
* IoT Central ile cihaz alışverişi yapılan veriler hakkında daha fazla bilgi edinmek için [telemetri, özellik ve komut yüklerini](concepts-telemetry-properties-commands.md) okuyun.
