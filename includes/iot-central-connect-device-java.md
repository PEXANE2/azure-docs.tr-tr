---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 11/24/2020
ms.openlocfilehash: ad305202daf09258bbd439e824fc07f1eed1c5af
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97033886"
---
## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](../articles/iot-central/core/quick-deploy-iot-central.md). Uygulamanın 14 Temmuz 2020 tarihinde veya sonrasında oluşturulmuş olması gerekir.
* Java SE Development Kit 8 olan bir geliştirme makinesi. [Azure ve Azure Stack Için Java uzun süreli destek](/java/azure/jdk/?preserve-view=true&view=azure-java-stable), **uzun süreli destek** altında **Java 8**' i seçin.
* [Apache Maven 3](https://maven.apache.org/download.cgi).
* Örnek kodu içeren Java GitHub deposu [için Microsoft Azure ıOT SDK](https://github.com/Azure/azure-iot-sdk-java) 'nın yerel bir kopyası. Deponun bir kopyasını indirmek için bu bağlantıyı kullanın: ZIP 'i [indirin](https://github.com/Azure/azure-iot-sdk-java/archive/master.zip). Sonra dosyayı yerel makinenizde uygun bir konuma ayıklayın.

## <a name="review-the-code"></a>Kodu gözden geçirin

Java için Microsoft Azure IoT SDK 'nın kopyasında daha önce indirdiğiniz *Azure-iot-SDK-Java/Device/iot-Device-Samples/PNP-Device-Sample/Thermostat-Device-Sample/src/Main/Java/Samples/com/Microsoft/Azure/SDK/IoT/Device/termostat. Java* dosyasını bir metin düzenleyicisinde açın.

IoT Central bağlanmak için örneği çalıştırdığınızda, cihazı kaydetmek ve bir bağlantı dizesi oluşturmak için cihaz sağlama hizmeti 'ni (DPS) kullanır. Örnek, gereken DPS bağlantı bilgilerini komut satırı ortamından alır.

`main`Yöntemi:

* `initializeAndProvisionDevice`Model kimliğini ayarlamak için çağrılar `dtmi:com:example:Thermostat;1` yapın, cihaz sağlamak ve kaydetmek için DPS kullanın, bir **deviceclient** örneği oluşturun ve IoT Central uygulamanıza bağlanın. IoT Central, bu cihazın cihaz şablonunu tanımlamak veya oluşturmak için model KIMLIĞINI kullanır. Daha fazla bilgi için bkz. cihaz [şablonuyla bir cihazı ilişkilendirme](../articles/iot-central/core/concepts-get-connected.md#associate-a-device-with-a-device-template).
* Komut için bir komut işleyicisi oluşturur `getMaxMinReport` .
* Yazılabilir özellik için bir özellik güncelleştirme işleyicisi oluşturur `targetTemperature` .
* Sıcaklık telemetrisini göndermek ve beş saniyede bir özelliği güncelleştirmek için bir iş parçacığı başlatır `maxTempSinceLastReboot` .

```java
async function main() {

public static void main(String[] args) throws URISyntaxException, IOException, ProvisioningDeviceClientException, InterruptedException {

    // ...

    switch (deviceSecurityType.toLowerCase())
    {
        case "dps":
        {
            if (validateArgsForDpsFlow())
            {
                initializeAndProvisionDevice();
                break;
            }
            throw new IllegalArgumentException("Required environment variables are not set for DPS flow, please recheck your environment.");
        }
        case "connectionstring":
        {
            // ...
        }
        default:
        {
            // ...
        }
    }

    deviceClient.startDeviceTwin(new TwinIotHubEventCallback(), null, new TargetTemperatureUpdateCallback(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> desiredPropertyUpdateCallback =
            Collections.singletonMap(
                    new Property("targetTemperature", null),
                    new Pair<>(new TargetTemperatureUpdateCallback(), null));
    deviceClient.subscribeToTwinDesiredProperties(desiredPropertyUpdateCallback);

    String methodName = "getMaxMinReport";
    deviceClient.subscribeToDeviceMethod(new GetMaxMinReportMethodCallback(), methodName, new MethodIotHubEventCallback(), methodName);

    new Thread(new Runnable() {
        @SneakyThrows({InterruptedException.class, IOException.class})
        @Override
        public void run() {
            while (true) {
                if (temperatureReset) {
                    // Generate a random value between 5.0°C and 45.0°C for the current temperature reading.
                    temperature = BigDecimal.valueOf(random.nextDouble() * 40 + 5).setScale(1, RoundingMode.HALF_UP).doubleValue();
                    temperatureReset = false;
                }

                sendTemperatureReading();
                Thread.sleep(5 * 1000);
            }
        }
    }).start();
}
```

`initializeAndProvisionDevice`Yöntemi, cihazın IoT Central kaydetmek ve bağlanmak IÇIN DPS nasıl kullandığını gösterir. Yük, IoT Central [cihazı bir cihaz şablonuyla ilişkilendirmek](../articles/iot-central/core/concepts-get-connected.md#associate-a-device-with-a-device-template)için KULLANDıĞı model kimliğini içerir.:

```java
private static void initializeAndProvisionDevice() throws ProvisioningDeviceClientException, IOException, URISyntaxException, InterruptedException {
    SecurityProviderSymmetricKey securityClientSymmetricKey = new SecurityProviderSymmetricKey(deviceSymmetricKey.getBytes(), registrationId);
    ProvisioningDeviceClient provisioningDeviceClient = null;
    ProvisioningStatus provisioningStatus = new ProvisioningStatus();

    provisioningDeviceClient = ProvisioningDeviceClient.create(globalEndpoint, scopeId, provisioningProtocol, securityClientSymmetricKey);

    AdditionalData additionalData = new AdditionalData();
    additionalData.setProvisioningPayload(String.format("{\"modelId\": \"%s\"}", MODEL_ID));

    provisioningDeviceClient.registerDevice(new ProvisioningDeviceClientRegistrationCallbackImpl(), provisioningStatus, additionalData);

    while (provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getProvisioningDeviceClientStatus() != ProvisioningDeviceClientStatus.PROVISIONING_DEVICE_STATUS_ASSIGNED)
    {
        if (provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getProvisioningDeviceClientStatus() == ProvisioningDeviceClientStatus.PROVISIONING_DEVICE_STATUS_ERROR ||
                provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getProvisioningDeviceClientStatus() == ProvisioningDeviceClientStatus.PROVISIONING_DEVICE_STATUS_DISABLED ||
                provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getProvisioningDeviceClientStatus() == ProvisioningDeviceClientStatus.PROVISIONING_DEVICE_STATUS_FAILED)
        {
            provisioningStatus.exception.printStackTrace();
            System.out.println("Registration error, bailing out");
            break;
        }
        System.out.println("Waiting for Provisioning Service to register");
        Thread.sleep(MAX_TIME_TO_WAIT_FOR_REGISTRATION);
    }

    ClientOptions options = new ClientOptions();
    options.setModelId(MODEL_ID);

    if (provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getProvisioningDeviceClientStatus() == ProvisioningDeviceClientStatus.PROVISIONING_DEVICE_STATUS_ASSIGNED) {
        // ...

        String iotHubUri = provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getIothubUri();
        String deviceId = provisioningStatus.provisioningDeviceClientRegistrationInfoClient.getDeviceId();

        deviceClient = DeviceClient.createFromSecurityProvider(iotHubUri, deviceId, securityClientSymmetricKey, IotHubClientProtocol.MQTT, options);
        deviceClient.open();
    }
}
```

`sendTemperatureTelemetry`Yöntemi, cihazın sıcaklık telemetrisini IoT Central nasıl göndereceğini gösterir:

```java
private static void sendTemperatureTelemetry() {
    String telemetryName = "temperature";
    String telemetryPayload = String.format("{\"%s\": %f}", telemetryName, temperature);

    Message message = new Message(telemetryPayload);
    message.setContentEncoding(StandardCharsets.UTF_8.name());
    message.setContentTypeFinal("application/json");

    deviceClient.sendEventAsync(message, new MessageIotHubEventCallback(), message);

    temperatureReadings.put(new Date(), temperature);
}
```

`updateMaxTemperatureSinceLastReboot`Yöntemi `maxTempSinceLastReboot` IoT Central bir özellik güncelleştirmesi gönderir:

```java
private static void updateMaxTemperatureSinceLastReboot() throws IOException {
    String propertyName = "maxTempSinceLastReboot";
    Property reportedProperty = new Property(propertyName, maxTemperature);

    deviceClient.sendReportedProperties(Collections.singleton(reportedProperty));
}
```

`TargetTemperatureUpdateCallback`Sınıfı, `TwinPropertyCallBack` IoT Central yazılabilir Özellik güncelleştirmelerini işlemeye yönelik yöntemi içerir:

```java
String propertyName = "targetTemperature";

public void TwinPropertyCallBack(Property property, Object context) {
    if (property.getKey().equalsIgnoreCase(propertyName)) {
        double targetTemperature = ((Number)property.getValue()).doubleValue();

        EmbeddedPropertyUpdate pendingUpdate = new EmbeddedPropertyUpdate(targetTemperature, StatusCode.IN_PROGRESS.value, property.getVersion(), null);
        Property reportedPropertyPending = new Property(propertyName, pendingUpdate);
        try {
            deviceClient.sendReportedProperties(Collections.singleton(reportedPropertyPending));
        } catch (IOException e) {
            throw new RuntimeException("IOException when sending reported property update: ", e);
        }

        // Update temperature in 2 steps
        double step = (targetTemperature - temperature) / 2;
        for (int i = 1; i <=2; i++) {
            temperature = BigDecimal.valueOf(temperature + step).setScale(1, RoundingMode.HALF_UP).doubleValue();
            Thread.sleep(5 * 1000);
        }

        EmbeddedPropertyUpdate completedUpdate = new EmbeddedPropertyUpdate(temperature, StatusCode.COMPLETED.value, property.getVersion(), "Successfully updated target temperature");
        Property reportedPropertyCompleted = new Property(propertyName, completedUpdate);
        deviceClient.sendReportedProperties(Collections.singleton(reportedPropertyCompleted));
    } else {
        log.debug("Property: Received an unrecognized property update from service.");
    }
}
```

`GetMaxMinReportMethodCallback`Sınıfı, `call` IoT Central çağrılan komutları işlemek için bir yöntem içerir:

```java
String commandName = "getMaxMinReport";

public DeviceMethodData call(String methodName, Object methodData, Object context) {
    if (methodName.equalsIgnoreCase(commandName)) {

        String jsonRequest = new String((byte[]) methodData, StandardCharsets.UTF_8);
        Date since = getCommandRequestValue(jsonRequest, Date.class);

        double runningTotal = 0;
        Map<Date, Double> filteredReadings = new HashMap<>();
        for (Map.Entry<Date, Double> entry : temperatureReadings.entrySet()) {
            if (entry.getKey().after(since)) {
                filteredReadings.put(entry.getKey(), entry.getValue());
                runningTotal += entry.getValue();
            }
        }

        if (filteredReadings.size() > 1) {
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss'Z'");
            double maxTemp = Collections.max(filteredReadings.values());
            double minTemp = Collections.min(filteredReadings.values());
            double avgTemp = runningTotal / filteredReadings.size();
            String startTime = sdf.format(Collections.min(filteredReadings.keySet()));
            String endTime = sdf.format(Collections.max(filteredReadings.keySet()));

            String responsePayload = String.format(
                    "{\"maxTemp\": %.1f, \"minTemp\": %.1f, \"avgTemp\": %.1f, \"startTime\": \"%s\", \"endTime\": \"%s\"}",
                    maxTemp,
                    minTemp,
                    avgTemp,
                    startTime,
                    endTime);

            return new DeviceMethodData(StatusCode.COMPLETED.value, responsePayload);
        }

        return new DeviceMethodData(StatusCode.NOT_FOUND.value, null);
    }

    log.error("Command: Unknown command {} invoked from service.", methodName);
    return new DeviceMethodData(StatusCode.NOT_FOUND.value, null);
}
```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

[!INCLUDE [iot-central-connection-configuration](iot-central-connection-configuration.md)]

Windows 'ta, indirdiğiniz Java SDK deposunun kök klasörüne gidin.

Örnek uygulamayı derlemek için aşağıdaki komutu çalıştırın:

```cmd/sh
mvn install -T 2C -DskipTests
```

## <a name="run-the-code"></a>Kodu çalıştırma

Örnek uygulamayı çalıştırmak için, bir komut satırı ortamı açın ve *Azure-IoT-SDK-Java/Device/IoT-Device-Samples/PNP-Device-Sample/termostat-* Device-Sample klasörü, *termostat. Java* örnek dosyasını içeren klasöre gidin.

[!INCLUDE [iot-central-connection-environment](iot-central-connection-environment.md)]

Örneği çalıştırın:

```cmd/sh
mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.sdk.iot.device.Thermostat"
```

Aşağıdaki çıktı, IoT Central kaydetme ve bağlanma için cihazı gösterir. Örnek, telemetri göndermeye başlar:

```output
Waiting for Provisioning Service to register
Waiting for Provisioning Service to register
IotHUb Uri : iotc-...azure-devices.net
Device ID : sample-device-01
2020-11-05 14:38:27.218 DEBUG Thermostat:208 - Opening the device client.
2020-11-05 14:38:27.253 DEBUG MqttIotHubConnection:122 - Opening MQTT connection...
2020-11-05 14:38:27.405 DEBUG Mqtt:117 - Sending MQTT CONNECT packet...
2020-11-05 14:38:28.782 DEBUG Mqtt:121 - Sent MQTT CONNECT packet was acknowledged
2020-11-05 14:38:28.786 DEBUG Mqtt:291 - Sending MQTT SUBSCRIBE packet for topic devices/sample-device-01/messages/devicebound/#
2020-11-05 14:38:28.965 DEBUG Mqtt:297 - Sent MQTT SUBSCRIBE packet for topic devices/sample-device-01/messages/devicebound/# was acknowledged
2020-11-05 14:38:28.967 DEBUG MqttIotHubConnection:205 - MQTT connection opened successfully
2020-11-05 14:38:28.968 INFO  IotHubTransport:270 - The connection to the IoT Hub has been established
2020-11-05 14:38:28.970 INFO  IotHubTransport:1133 - Updating transport status to new status CONNECTED with reason CONNECTION_OK
2020-11-05 14:38:28.972 DEBUG IotHubTransport:1143 - Invoking connection status callbacks with new status details
2020-11-05 14:38:28.976 INFO  IotHubTransport:327 - Client connection opened successfully
2020-11-05 14:38:28.978 INFO  DeviceClient:398 - Device client opened successfully
2020-11-05 14:38:28.979 DEBUG Thermostat:142 - Start twin and set handler to receive "targetTemperature" updates.
2020-11-05 14:38:28.993 INFO  IotHubTransport:421 - Message was queued to be sent later ( Message details: Correlation Id [85f8f25b-924e-47a1-b7f0-e7bd227cbb9f] Message Id [df65a02b-6d75-40fe-8636-a571ad3ae970] Device Operation Type [DEVICE_OPERATION_TWIN_SUBSCRIBE_DESIRED_PROPERTIES_REQUEST] )
2020-11-05 14:38:28.994 INFO  IotHubTransport:1040 - Sending message ( Message details: Correlation Id [85f8f25b-924e-47a1-b7f0-e7bd227cbb9f] Message Id [df65a02b-6d75-40fe-8636-a571ad3ae970] Device Operation Type [DEVICE_OPERATION_TWIN_SUBSCRIBE_DESIRED_PROPERTIES_REQUEST] )
2020-11-05 14:38:29.000 INFO  IotHubTransport:421 - Message was queued to be sent later ( Message details: Correlation Id [d3692283-63d6-446a-aeab-5a4e4aa9e948] Message Id [d53139e3-acb4-458b-8c03-119d8fc04d5a] Request Id [0] Device Operation Type [DEVICE_OPERATION_TWIN_GET_REQUEST] )
2020-11-05 14:38:29.018 DEBUG Mqtt:291 - Sending MQTT SUBSCRIBE packet for topic $iothub/twin/res/#
2020-11-05 14:38:29.080 INFO  IotHubTransport:421 - Message was queued to be sent later ( Message details: Correlation Id [f8c2180d-7289-4bf8-afa5-0ad6565a27eb] Message Id [c553e8bd-3c7e-4830-a3e7-12964cf3d936] Device Operation Type [DEVICE_OPERATION_TWIN_SUBSCRIBE_DESIRED_PROPERTIES_REQUEST] )
2020-11-05 14:38:29.103 DEBUG Thermostat:150 - Set handler to receive "getMaxMinReport" command.
2020-11-05 14:38:29.127 INFO  IotHubTransport:421 - Message was queued to be sent later ( Message details: Correlation Id [c6f3c2e6-68da-4d52-b429-9e62418a2fd7] Message Id [7f17f191-7ab0-46e7-897e-a635c0519287] Device Operation Type [DEVICE_OPERATION_METHOD_SUBSCRIBE_REQUEST] )
2020-11-05 14:38:29.167 INFO  IotHubTransport:421 - Message was queued to be sent later ( Message details: Correlation Id [e1dc7b20-1783-462a-87db-a63042a583c4] Message Id [752f3b01-6c28-4f76-a314-97c75f37bc64] )
2020-11-05 14:38:29.169 DEBUG Thermostat:419 - Telemetry: Sent - {"temperature": 39.6 C} with message Id 752f3b01-6c28-4f76-a314-97c75f37bc64.
```

[!INCLUDE [iot-central-monitor-thermostat](iot-central-monitor-thermostat.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verdiğini görebilirsiniz:

```output
2020-11-05 14:43:03.179 DEBUG Thermostat:318 - Command: Received - Generating min, max, avg temperature report since Thu Nov 05 06:30:00 GMT 2020.
2020-11-05 14:43:03.180 DEBUG Thermostat:345 - Command: MaxMinReport since Thu Nov 05 06:30:00 GMT 2020: "maxTemp": 33.1 C, "minTemp": 33.1 C, "avgTemp": 33.1 C, "startTime": 2020-11-05T14:42:28Z, "endTime": 2020-11-05T14:42:58Z

...

2020-11-05 14:49:08.182 DEBUG Thermostat:261 - Property: Received - {"targetTemperature": 56.0 C}.
2020-11-05 14:49:08.185 DEBUG Thermostat:270 - Property: Update - {"targetTemperature": 56.0 C} is IN_PROGRESS
2020-11-05 14:49:18.206 DEBUG Thermostat:282 - Property: Update - {"targetTemperature": 55.9 C} is COMPLETED
```
