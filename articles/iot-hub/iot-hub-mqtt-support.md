---
title: Azure IoT Hub MQTT desteğini anlayın | Microsoft Dokümanlar
description: Geliştirici kılavuzu - MQTT protokolünü kullanarak IoT Hub aygıta bakan bir uç noktasına bağlanan aygıtlar için destek. Azure IoT aygıt SDK'larında yerleşik MQTT desteği hakkında bilgi içerir.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: robinsh
ms.openlocfilehash: 2b200692610302bb135982e5419dcda36d5cfe60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271167"
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>MQTT protokolünü kullanarak IoT hub'ınızla iletişim kurun

IoT Hub, aygıtların Aşağıdakileri kullanarak IoT Hub aygıt uç noktalarıyla iletişim kurmasını sağlar:

* [MQTT v3.1.1](https://mqtt.org/) bağlantı noktası 8883
* MQTT v3.1.1 bağlantı noktası 443 websocket üzerinde.

IoT Hub, tam özellikli bir MQTT aracısı değildir ve MQTT v3.1.1 standart sürümünde belirtilen tüm davranışları desteklemez. Bu makalede, aygıtların IoT Hub ile iletişim kurmak için desteklenen MQTT davranışlarını nasıl kullanabileceği açıklanmaktadır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub ile tüm cihaz iletişimi TLS/SSL kullanılarak güvence altına alınmalıdır. Bu nedenle, IoT Hub bağlantı noktası 1883 üzerinden güvenli olmayan bağlantıları desteklemez.

## <a name="connecting-to-iot-hub"></a>IoT Hub'ına bağlanma

Aygıt, aşağıdaki seçeneklerden herhangi birini kullanarak bir IoT hub'ına bağlanmak için MQTT protokolünü kullanabilir.

* [Azure IoT SDK'lardaki kitaplıklar.](https://github.com/Azure/azure-iot-sdks)
* MQTT protokolü doğrudan.

MQTT bağlantı noktası (8883) birçok kurumsal ve eğitim ağı ortamında engellenir. Güvenlik duvarınızda 8883 bağlantı noktasını açamıyorsanız, Web Soketleri üzerinde MQTT kullanmanızı öneririz. Web Soketleri üzerinden MQTT, ağ ortamlarında hemen hemen her zaman açık olan 443 bağlantı noktası üzerinden iletişim kurar. Azure IoT SDK'larını kullanırken Web Soketleri protokolleri üzerinden MQTT ve MQTT'yi nasıl belirteceğimiz [hakkında](#using-the-device-sdks)bilgi edinmek için bkz.

## <a name="using-the-device-sdks"></a>Aygıt SDK'larını kullanma

MQTT protokolünü destekleyen [aygıt SDK'ları](https://github.com/Azure/azure-iot-sdks) Java, Node.js, C, C#ve Python için kullanılabilir. Aygıt SDK'ları, bir IoT hub'ına bağlantı kurmak için standart IoT Hub bağlantı dizesini kullanır. MQTT protokolünü kullanmak için istemci protokolü parametresi **MQTT**olarak ayarlanmalıdır. İstemci protokolü parametresinde Web Soketleri üzerinde MQTT belirtebilirsiniz. Varsayılan olarak, Aygıt SDK'ları **CleanSession** bayrağı **0** olarak ayarlanmış bir IoT Hub'ına bağlanır ve IoT hub'ı ile ileti alışverişi için **QoS 1'i** kullanır.

Bir aygıt bir IoT hub'ına bağlandığında, aygıt SDK'ları aygıtın bir IoT hub'ı ile ileti alışverişini sağlayan yöntemler sağlar.

Aşağıdaki tablo, desteklenen her dil için kod örneklerine bağlantılar içerir ve Web Soketleri protokolü üzerinden MQTT veya MQTT kullanarak IoT Hub'a bağlantı kurmak için kullanılacak parametreyi belirtir.

| Dil | MQTT protokol parametresi | Web Soketleri protokol parametresi üzerinden MQTT
| --- | --- | --- |
| [Node.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js) | azure-iot-device-mqtt. Mqtt | azure-iot-device-mqtt. MqttWs |
| [Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java) |[IotHubClientProtocol](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.iothubclientprotocol?view=azure-java-stable). MQTT | IotHubClientProtocol.MQTT_WS |
| [C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm) | [MQTT_Protocol](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothubtransportmqtt-h/mqtt-protocol) | [MQTT_WebSocket_Protocol](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothubtransportmqtt-websockets-h/mqtt-websocket-protocol) |
| [C #](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device/samples) | [TransportType](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.transporttype?view=azure-dotnet). Mqtt | TransportType.Mqtt, MQTT başarısız olursa Web Soketleri üzerinden MQTT'ye geri döner. Yalnızca Web Soketleri üzerinde MQTT belirtmek için TransportType.Mqtt_WebSocket_Only'ı kullanın |
| [Python](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples) | Varsayılan olarak MQTT'yi destekler | `websockets=True` İstemciyi oluşturmak için çağrıekleme |

Aşağıdaki parça, Azure IoT Düğüm.js SDK'yı kullanırken Web Soketleri protokolü üzerinden MQTT'nin nasıl belirtilen;

```javascript
var Client = require('azure-iot-device').Client;
var Protocol = require('azure-iot-device-mqtt').MqttWs;
var client = Client.fromConnectionString(deviceConnectionString, Protocol);
```

Aşağıdaki parça, Azure IoT Python SDK'yı kullanırken Web Soketleri protokolü üzerinden MQTT'nin nasıl belirtilen;

```python
from azure.iot.device.aio import IoTHubDeviceClient
device_client = IoTHubDeviceClient.create_from_connection_string(deviceConnectionString, websockets=True)
```

### <a name="default-keep-alive-timeout"></a>Varsayılan canlı tutma zaman ayarı

Bir istemci/IoT Hub bağlantısının canlı kalmasını sağlamak için, hem hizmet hem de istemci düzenli olarak birbirlerine *canlı bir* ping gönderir. IoT SDK kullanan istemci aşağıdaki tabloda tanımlanan aralıkta canlı bir şekilde gönderir:

|Dil  |Varsayılan canlı tutma aralığı  |Yapılandırılabilir  |
|---------|---------|---------|
|Node.js     |   180 saniye      |     Hayır    |
|Java     |    230 saniye     |     Hayır    |
|C     | 240 saniye |  [Evet](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md#mqtt-transport)   |
|C#     | 300 saniye |  [Evet](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/src/Transport/Mqtt/MqttTransportSettings.cs#L89)   |
|Piton (V2)   | 60 saniye |  Hayır   |

[MQTT spec'i](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718081)takiben, IoT Hub'ın canlı tutma ping aralığı istemcinin canlı tutma değerinin 1,5 katıdır. Ancak, tüm Azure hizmetleri 29,45 dakika olan Azure yük bakiyesi TCP boşta zaman ayarı'na bağlı olduğundan, IoT Hub sunucu tarafındaki maksimum zaman ayarı 29,45 dakika (1767 saniye) ile sınırlar. 

Örneğin, Java SDK'yı kullanan bir aygıt canlı kalma ping'ini gönderir ve ağ bağlantısını kaybeder. 230 saniye sonra cihaz, çevrimdışı olduğu için canlı tutma pingini kaçırıyor. Ancak, IoT Hub bağlantıyı hemen kapatmaz - `(230 * 1.5) - 230 = 115` [404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)hatası ile aygıtın bağlantısını kesmeden önce bir saniye daha bekler. 

Ayarlayabildiğiniz maksimum istemci tutma `1767 / 1.5 = 1177` değeri saniyedir. Herhangi bir trafik canlı tutmak sıfırlar. Örneğin, başarılı bir SAS belirteci yenilemesi canlı tutmayı sıfırlar.

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Bir aygıt uygulamasını AMQP'dan MQTT'ye geçirme

[SDK'ları](https://github.com/Azure/azure-iot-sdks)kullanıyorsanız, AMQP'dan MQTT'ye geçmek, istemci başlatmada daha önce belirtildiği gibi protokol parametresini değiştirmeyi gerektirir.

Bunu yaparken, aşağıdaki öğeleri kontrol ettiğinizden emin olun:

* AmQP birçok koşuliçin hataları döndürür, MQTT ise bağlantıyı sonlandırır. Sonuç olarak, özel durum işleme mantığınız bazı değişiklikler gerektirebilir.

* MQTT, [buluttan aygıta iletiler](iot-hub-devguide-messaging.md)alırken *reddetme* işlemlerini desteklemez. Arka uç uygulamanızın cihaz uygulamasından yanıt alması gerekiyorsa, [doğrudan yöntemler](iot-hub-devguide-direct-methods.md)kullanmayı düşünün.

* Python SDK'da AMQP desteklenmez

## <a name="using-the-mqtt-protocol-directly-as-a-device"></a>MQTT protokolünü doğrudan kullanma (aygıt olarak)

Bir aygıt Aygıtı SDK'ları kullanamıyorsa, bağlantı noktası 8883'teki MQTT protokolünü kullanarak ortak aygıt uç noktalarına bağlanmaya devam edebilir. **CONNECT** paketinde aygıt aşağıdaki değerleri kullanmalıdır:

* **ClientId** alanı için **deviceId'i**kullanın.

* Kullanıcı **adı** alanı için, `{iothubhostname}` IoT hub'ının tam CName'si nerede yatsın. `{iothubhostname}/{device_id}/?api-version=2018-06-30`

    Örneğin, IoT hub'ınızın adı **contoso.azure-devices.net** ve cihazınızın adı **MyDevice01**ise, tam **Kullanıcı Adı** alanı şunları içermelidir:

    `contoso.azure-devices.net/MyDevice01/?api-version=2018-06-30`

* **Parola** alanı için bir SAS belirteci kullanın. SAS belirteci biçimi hem HTTPS hem de AMQP protokolleri için aynıdır:

  `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`

  > [!NOTE]
  > X.509 sertifika kimlik doğrulaması kullanıyorsanız, SAS belirteç parolaları gerekmez. Daha fazla bilgi için Azure [IoT Hub'ınızda X.509 güvenlik ayarlama'ya](iot-hub-security-x509-get-started.md) bakın ve [aşağıdaki](#tlsssl-configuration)kod yönergelerini izleyin.

  SAS belirteçlerinin nasıl üretilenhakkında daha fazla bilgi [için, IoT Hub güvenlik belirteçlerini kullanmanın](iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app)aygıt bölümüne bakın.

  Test ederken, görsel stüdyo kodu için çapraz platform [Azure IoT Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) veya Kendi kodunuza kopyalayıp yapıştırabileceğiniz bir SAS belirteci oluşturmak için [Aygıt Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) aracını da kullanabilirsiniz:

### <a name="for-azure-iot-tools"></a>Azure IoT Araçları için

1. Visual Studio Code'un sol alt köşesindeki **AZURE IOT HUB AYGıTlar** sekmesini genişletin.
  
2. Cihazınıza sağ tıklayın ve **Aygıt için SAS Belirteci Oluştur'u**seçin.
  
3. **Son kullanma tarihini** ayarlayın ve 'Enter' tuşuna basın.
  
4. SAS belirteci oluşturulur ve panoya kopyalanır.

### <a name="for-device-explorer"></a>Aygıt Gezgini için

1. **Aygıt Gezgini'nde** **Yönetim** sekmesine gidin.

2. **SAS Belirteci'ni** tıklatın (sağ üstte).

3. **SASTokenForm'da** **DeviceID** açılır durumda cihazınızı seçin. **TTL'nizi**ayarlayın.

4. Belirtecinizi oluşturmak için **Oluştur'u** tıklatın.

   Oluşturulan SAS belirteci aşağıdaki yapıya sahiptir:

   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

   MQTT kullanarak bağlanmak için **Parola** alanı olarak kullanılacak bu belirteç parçası:

   `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

MQTT'nin paketleri bağlaması ve bağlantısını kesmesi için, IoT Hub **Operations Monitoring** kanalında bir olay yayınlar. Bu olay, bağlantı sorunlarını gidermenize yardımcı olabilecek ek bilgilere sahiptir.

Cihaz uygulaması **CONNECT** paketinde **bir Will** iletisi belirtebilir. Cihaz uygulaması, `devices/{device_id}/messages/events/` bir `devices/{device_id}/messages/events/{property_bag}` telemetri iletisi olarak iletilecek **Will** iletilerini tanımlamak için **Will** topic adını kullanmalıdır. Bu durumda, ağ bağlantısı kapalıysa, ancak aygıttan daha önce bir **DISCONNECT** paketi alınmamışsa, IoT Hub **CONNECT** paketinde verilen **Will** iletisini telemetri kanalına gönderir. Telemetri kanalı varsayılan **Olaylar** bitiş noktası veya IoT Hub yönlendirmesi tarafından tanımlanan özel bir bitiş noktası olabilir. İleti, kendisine atanan **Will** değerine sahip **iothub-MessageType** özelliğine sahiptir.

### <a name="an-example-of-c-code-using-mqtt-without-azure-iot-c-sdk"></a>Azure IoT C SDK olmadan MQTT kullanarak C kodu örneği
Bu [depoda,](https://github.com/Azure-Samples/IoTMQTTSample)Azure IoT C SDK'yı kullanmadan telemetri iletilerinin nasıl gönderilebildiğini, ioT hub'ı ile etkinlik nasıl alınacağını gösteren birkaç C/C++ demo projesi bulacaksınız. 

Bu örnekler, Eclipse Mosquitto kitaplığını kullanarak IoT hub'ında uygulanan MQTT Broker'a mesaj gönderir.

Bu depo şunları içerir:

**Windows için:**

* TelemetryMQTTWin32: Windows makinesinde oluşturulmuş ve çalıştırılan bir Azure IoT hub'ına telemetri iletisi göndermek için kod içerir.

* SubscribeMQTTWin32: Windows makinesinde belirli bir IoT hub'ındaki olaylara abone olmak için kod içerir.

* DeviceTwinMQTTWin32: Windows makinesindeki Azure IoT hub'ındaki bir aygıtın aygıttaki ikiz olaylarını sorgulamak ve abone olmak için kod içerir.

* PnPMQTTWin32: IoT Plug & Play önizleme Aygıtı özelliklerini bir Windows makinesinde oluşturulmuş ve çalıştıran bir Azure IoT hub'ına içeren bir telemetri iletisi göndermek için kod içerir. Burada IoT Fiş [here](https://docs.microsoft.com/azure/iot-pnp/overview-iot-plug-and-play) & play hakkında daha fazla

**Linux için:**

* MQTTLinux: Linux üzerinde çalıştırmak için kod ve yapı komut dosyası içerir (WSL, Ubuntu ve Raspbian şimdiye kadar test edilmiştir).

* LinuxConsoleVS2019: aynı kodu içerir ancak WSL'yi (Windows Linux alt sistemi) hedefleyen bir VS2019 projesinde. Bu proje Visual Studio'dan Linux'ta adım adım çalışan kodu hata ayıklamanızı sağlar.

**mosquitto_pub için:**

Bu klasör, Mosquitto.org tarafından sağlanan mosquitto_pub yardımcı araçla birlikte kullanılan iki örnek komutu içerir.

* Mosquitto_sendmessage: Aygıt görevi görehareket eden bir Azure IoT hub'ına basit bir kısa mesaj göndermek.

* Mosquitto_subscribe: Bir Azure IoT hub'ında meydana gelen olayları görmek için.

## <a name="using-the-mqtt-protocol-directly-as-a-module"></a>MQTT protokolünü doğrudan kullanma (modül olarak)

Bir modül kimliği kullanarak MQTT üzerinden IoT Hub'a bağlanmak aygıta benzer [(yukarıda](#using-the-mqtt-protocol-directly-as-a-device)açıklanan) ancak aşağıdakileri kullanmanız gerekir:

* İstemci kimliğini `{device_id}/{module_id}`' ye ayarlayın

* Kullanıcı adı ve parolayla kimlik doğrulaması yapacaksanız, kullanıcı adını modül kimliğiyle ilişkili SAS belirteci parolanız olarak ayarlayın `<hubname>.azure-devices.net/{device_id}/{module_id}/?api-version=2018-06-30` ve kullanın.

* Telemetri yayınlamak için konu olarak kullanın. `devices/{device_id}/modules/{module_id}/messages/events/`

* WILL `devices/{device_id}/modules/{module_id}/messages/events/` konusu olarak kullanın.

* İkiz GET ve PATCH konuları modüller ve aygıtlar için aynıdır.

* İkiz durum konusu modüller ve aygıtlar için aynıdır.

## <a name="tlsssl-configuration"></a>TLS/SSL yapılandırması

MQTT protokolünü doğrudan kullanmak için istemcinizin TLS/SSL üzerinden bağlanması *gerekir.* Bu adımı atlama girişimleri bağlantı hataları ile başarısız oldu.

TLS bağlantısı kurmak için DigiCert Baltimore Root Sertifikası'nı indirmeniz ve başvurmanız gerekebilir. Bu sertifika, Azure'un bağlantıyı korumak için kullandığı sertifikadır. Bu sertifikayı [Azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) deposunda bulabilirsiniz. Bu sertifikalar hakkında daha fazla bilgiyi [Digicert'in web sitesinde](https://www.digicert.com/digicert-root-certificates.htm)bulabilirsiniz.

Eclipse Vakfı tarafından [Paho MQTT kitaplığın](https://pypi.python.org/pypi/paho-mqtt) Python sürümünü kullanarak bu nasıl uygulanacağını bir örnek aşağıdaki gibi görünebilir.

İlk olarak, komut satırı ortamınızdan Paho kitaplığını yükleyin:

```cmd/sh
pip install paho-mqtt
```

Ardından, istemciyi Python komut dosyasında uygulayın. Yer tutucuları aşağıdaki gibi değiştirin:

* `<local path to digicert.cer>`DigiCert Baltimore Root sertifikasını içeren yerel bir dosyaya giden yoldur. Bu dosyayı, [Certs.c'deki](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) sertifika bilgilerini C için Azure IoT SDK'dan `-----END CERTIFICATE-----`kopyalayarak `"` oluşturabilir ve satırları `-----BEGIN CERTIFICATE-----` eklemeve , her `\r\n` satırın başındaki ve sonundaki işaretleri kaldırabilir ve her satırın sonundaki karakterleri kaldırabilirsiniz.

* `<device id from device registry>`IoT hub'ınıza eklediğiniz bir aygıtın kimliğidir.

* `<generated SAS token>`bu makalede daha önce açıklandığı gibi oluşturulan aygıt için bir SAS belirtecidir.

* `<iot hub name>`IoT hub'ınızın adı.

```python
from paho.mqtt import client as mqtt
import ssl

path_to_root_cert = "<local path to digicert.cer file>"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
iot_hub_name = "<iot hub name>"


def on_connect(client, userdata, flags, rc):
    print("Device connected with result code: " + str(rc))


def on_disconnect(client, userdata, rc):
    print("Device disconnected with result code: " + str(rc))


def on_publish(client, userdata, mid):
    print("Device sent message")


client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.on_connect = on_connect
client.on_disconnect = on_disconnect
client.on_publish = on_publish

client.username_pw_set(username=iot_hub_name+".azure-devices.net/" +
                       device_id + "/?api-version=2018-06-30", password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None,
               cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1_2, ciphers=None)
client.tls_insecure_set(False)

client.connect(iot_hub_name+".azure-devices.net", port=8883)

client.publish("devices/" + device_id + "/messages/events/", "{id=123}", qos=1)
client.loop_forever()
```

Aygıt sertifikası kullanarak kimlik doğrulaması yapmak için yukarıdaki kod parçacıklarını aşağıdaki değişikliklerle güncelleştirin (sertifika tabanlı kimlik doğrulamasına nasıl hazırlanacağınız hakkında [X.509 CA sertifikası](./iot-hub-x509ca-overview.md#how-to-get-an-x509-ca-certificate) nasıl alınır' a bakın):

```python
# Create the client as before
# ...

# Set the username but not the password on your client
client.username_pw_set(username=iot_hub_name+".azure-devices.net/" +
                       device_id + "/?api-version=2018-06-30", password=None)

# Set the certificate and key paths on your client
cert_file = "<local path to your certificate file>"
key_file = "<local path to your device key file>"
client.tls_set(ca_certs=path_to_root_cert, certfile=cert_file, keyfile=key_file,
               cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1_2, ciphers=None)

# Connect as before
client.connect(iot_hub_name+".azure-devices.net", port=8883)
```

## <a name="sending-device-to-cloud-messages"></a>Aygıttan buluta iletigönderme

Başarılı bir bağlantı yaptıktan sonra, bir aygıt IoT `devices/{device_id}/messages/events/` `devices/{device_id}/messages/events/{property_bag}` Hub'a **Konu Adı**kullanarak veya olarak ileti gönderebilir. Öğe, `{property_bag}` aygıtın url kodlanmış biçimde ek özelliklere sahip iletiler göndermesini sağlar. Örnek:

```text
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Bu `{property_bag}` öğe, HTTPS protokolünde sorgu dizeleri ile aynı kodlamayı kullanır.

Aşağıda, IoT Hub uygulamasına özgü davranışların bir listesi vetir:

* IoT Hub, QoS 2 iletilerini desteklemez. Bir aygıt uygulaması **QoS 2**ile bir ileti yayımlarsa, IoT Hub ağ bağlantısını kapatır.

* IoT Hub iletileri saklamada kalıcı değildir. Bir aygıt **RETAIN** bayrağı kümesi 1'e ayarlanmış bir ileti gönderirse, IoT Hub iletiye **x-opt-retain** uygulama özelliğini ekler. Bu durumda, ioT Hub iletiyi tutma iletisini kalıcı hale getirmek yerine arka uç uygulamasına geçirir.

* IoT Hub, aygıt başına yalnızca bir etkin MQTT bağlantısını destekler. Aynı aygıt kimliği adına yapılan herhangi bir yeni MQTT bağlantısı, IoT Hub'ın varolan bağlantıyı bırakmasına neden olur.

Daha fazla bilgi için [Mesajlaşma geliştiricisi kılavuzuna](iot-hub-devguide-messaging.md)bakın.

## <a name="receiving-cloud-to-device-messages"></a>Buluttan aygıta ileti alma

IoT Hub'dan ileti almak için, `devices/{device_id}/messages/devicebound/#` bir aygıtın **Konu Filtresi**olarak abone olması gerekir. Konu Filtresi'ndeki `#` çok düzeyli joker karakter yalnızca aygıtın konu adında ek özellikler almasına izin vermek için kullanılır. IoT Hub alt konuları filtreleme `?` için joker veya joker kartların kullanımına `#` izin vermez. IoT Hub genel amaçlı bir pub-alt mesajlaşma aracısı olmadığından, yalnızca belgelenen konu adlarını ve konu filtrelerini destekler.

Cihaz, `devices/{device_id}/messages/devicebound/#` konu filtresi tarafından temsil edilen aygıta özgü bitiş noktasına başarıyla abone olana kadar IoT Hub'dan herhangi bir ileti almaz. Bir abonelik kurulduktan sonra, aygıt abonelik saatinden sonra gönderilen buluttan aygıta iletileri alır. Aygıt **CleanSession** bayrağına 0 olarak **0**ayarlanmış olarak bağlanırsa, abonelik farklı oturumlarda kalıcıdır. Bu durumda, aygıt **CleanSession 0'a** bir sonraki bağlanında bağlantısı kesilirken ona gönderilen olağanüstü iletileri alır. Aygıt **CleanSession** bayrak kümesini **1** olarak kullanıyorsa, aygıt bitiş noktasına abone olana kadar IoT Hub'dan herhangi bir ileti almaz.

IoT **Hub, Konu Adı** `devices/{device_id}/messages/devicebound/`veya `devices/{device_id}/messages/devicebound/{property_bag}` ileti özellikleri olduğunda iletiler sunar. `{property_bag}`ileti özelliklerinin url kodlanmış anahtar/değer çiftleri içerir. Özellik çantasına yalnızca uygulama özellikleri ve kullanıcı ayarlanamasa sistem özellikleri **(messageId** veya **correlationId**gibi) dahildir. Sistem özelliği adları önek **$** var, uygulama özellikleri önek ile özgün özellik adını kullanın.

Bir cihaz uygulaması **QoS 2**ile bir konuya abone olduğunda, IoT Hub **SUBACK** paketinde maksimum QoS düzeyi 1 verir. Bundan sonra, IoT Hub QoS 1 kullanarak cihaza iletiler sunar.

## <a name="retrieving-a-device-twins-properties"></a>Aygıt ikizinin özelliklerini alma

İlk olarak, bir `$iothub/twin/res/#`aygıt, işlemin yanıtlarını almak için abone olunur. Daha sonra, **istek kimliği** `$iothub/twin/GET/?$rid={request id}`için doldurulan bir değere sahip konuya boş bir ileti gönderir. Hizmet daha sonra, istekle aynı istek `$iothub/twin/res/{status}/?$rid={request id}` **kimliğini** kullanarak aygıt konusuyla ilgili ikiz verileri içeren bir yanıt iletisi gönderir.

İstek Kimliği, [IoT Hub mesajlaşma geliştiricisi kılavuzuna](iot-hub-devguide-messaging.md)göre ileti özelliği değeri için geçerli bir değer olabilir ve durum tamsayı olarak doğrulanır.

Yanıt gövdesi, aşağıdaki yanıt örneğinde gösterildiği gibi aygıt ikizinin özellikleri bölümünü içerir:

```json
{
    "desired": {
        "telemetrySendFrequency": "5m",
        "$version": 12
    },
    "reported": {
        "telemetrySendFrequency": "5m",
        "batteryLevel": 55,
        "$version": 123
    }
}
```

Olası durum kodları şunlardır:

|Durum | Açıklama |
| ----- | ----------- |
| 200 | Başarılı |
| 429 | [IoT Hub azaltma](iot-hub-devguide-quotas-throttling.md) başına çok fazla istek (daraltılmış) |
| 5** | Sunucu hataları |

Daha fazla bilgi için [Device ikizleri geliştirici kılavuzuna](iot-hub-devguide-device-twins.md)bakın.

## <a name="update-device-twins-reported-properties"></a>Aygıt ikizinin bildirilen özelliklerini güncelleştirme

Bildirilen özellikleri güncelleştirmek için aygıt, belirlenen bir MQTT konusu üzerinden bir yayın yoluyla IoT Hub'a bir istek yayınlar. İsteği işledikten sonra, IoT Hub güncelleştirme işleminin başarı veya hata durumunu başka bir konuya yayın yoluyla yanıtlar. Bu konu, ikiz güncelleştirme isteğinin sonucunu bildirmek için cihaz tarafından abone edilebilir. MQTT'de bu tür bir istek/yanıt etkileşimi uygulamak için, başlangıçta aygıtın güncelleştirme isteğinde sağladığı istek kimliği ()`$rid`kavramından yararlanırız. Bu istek kimliği, aygıtın yanıtı belirli önceki isteğiyle ilişkilendirmesine izin vermek için IoT Hub'ın yanıtına da eklenir.

Aşağıdaki sıra, aygıtın IoT Hub'daki aygıt ikizinde bildirilen özellikleri nasıl güncellebildiğini açıklar:

1. Bir aygıtın işlemin `$iothub/twin/res/#` yanıtlarını IoT Hub'dan almak için öncelikle konuya abone olması gerekir.

2. `$iothub/twin/PATCH/properties/reported/?$rid={request id}` Aygıt, konuya aygıt ikiz güncelleştirmesini içeren bir ileti gönderir. Bu ileti bir **istek kimliği** değeri içerir.

3. Hizmet daha sonra, konuyla ilgili `$iothub/twin/res/{status}/?$rid={request id}`olarak bildirilen özellikler koleksiyonu için yeni ETag değerini içeren bir yanıt iletisi gönderir. Bu yanıt iletisi, istekle aynı **istek kimliğini** kullanır.

İstek iletisi gövdesi, bildirilen özellikler için yeni değerler içeren bir JSON belgesi içerir. JSON belgedeki her üye güncellenir veya aygıt ikizinin belgesine ilgili üyeyi ekler. Üye, `null`üyeyi içeren nesneden siler. Örnek:

```json
{
    "telemetrySendFrequency": "35m",
    "batteryLevel": 60
}
```

Olası durum kodları şunlardır:

|Durum | Açıklama |
| ----- | ----------- |
| 204 | Başarı (içerik döndürülür) |
| 400 | Kötü istek. Yanlış Biçimlendirilmiş JSON |
| 429 | [IoT Hub azaltma](iot-hub-devguide-quotas-throttling.md) başına çok fazla istek (daraltılmış) |
| 5** | Sunucu hataları |

Aşağıdaki python kodu snippet, MQTT (Paho MQTT istemcisi kullanarak) üzerinde ikiz bildirilen özellikleri güncelleştirme işlemi gösterir:

```python
from paho.mqtt import client as mqtt

# authenticate the client with IoT Hub (not shown here)

client.subscribe("$iothub/twin/res/#")
rid = "1"
twin_reported_property_patch = "{\"firmware_version\": \"v1.1\"}"
client.publish("$iothub/twin/PATCH/properties/reported/?$rid=" +
               rid, twin_reported_property_patch, qos=0)
```

Yukarıda bildirilen ikiz özellikleri güncelleştirme işleminin başarısı üzerine, IoT Hub'dan gelen yayın iletisi şu konuya sahip olacaktır: `$iothub/twin/res/204/?$rid=1&$version=6`, başarıyı gösteren durum kodu nerededir, `204` `$rid=1` koddaki aygıt tarafından sağlanan istek kimliğine karşılık gelir ve `$version` güncelleştirmeden sonra aygıt ikizlerinin bildirilen özellikleri bölümüne karşılık gelir.

Daha fazla bilgi için [Device ikizleri geliştirici kılavuzuna](iot-hub-devguide-device-twins.md)bakın.

## <a name="receiving-desired-properties-update-notifications"></a>İstenilen özellikleri güncelleme bildirimlerini alma

Bir aygıt bağlandığında, IoT Hub, çözüm `$iothub/twin/PATCH/properties/desired/?$version={new version}`arka uç tarafından gerçekleştirilen güncelleştirmenin içeriğini içeren konuya bildirimler gönderir. Örnek:

```json
{
    "telemetrySendFrequency": "5m",
    "route": null,
    "$version": 8
}
```

Özellik güncelleştirmeleri `null` gelince, değerler JSON nesne üyesi silindiği anlamına gelir. Ayrıca, ikiz `$version` istenilen özellikler bölümünün yeni sürümünü gösterir unutmayın.

> [!IMPORTANT]
> IoT Hub, yalnızca aygıtlar bağlandığında değişiklik bildirimleri oluşturur. IoT Hub ile aygıt uygulaması arasında senkronize olmak için istenen özellikleri korumak için [aygıt yeniden bağlantı akışını](iot-hub-devguide-device-twins.md#device-reconnection-flow) uyguladığından emin olun.

Daha fazla bilgi için [Device ikizleri geliştirici kılavuzuna](iot-hub-devguide-device-twins.md)bakın.

## <a name="respond-to-a-direct-method"></a>Doğrudan bir yönteme yanıt verme

İlk olarak, bir aygıtın `$iothub/methods/POST/#`abone olması gerekir. IoT Hub, geçerli bir `$iothub/methods/POST/{method name}/?$rid={request id}`JSON veya boş bir gövdeyle konuya yöntem istekleri gönderir.

Yanıt vermek için, aygıt konuya `$iothub/methods/res/{status}/?$rid={request id}`geçerli bir JSON veya boş gövdeiçeren bir ileti gönderir. Bu iletide, **istek kimliği** istek iletisindekiyle eşleşmeli ve **durum** bir tamsayı olmalıdır.

Daha fazla bilgi için [Doğrudan yöntem geliştirici kılavuzuna](iot-hub-devguide-direct-methods.md)bakın.

## <a name="additional-considerations"></a>Diğer konular

Son bir düşünce olarak, bulut tarafında MQTT protokol davranışını özelleştirmeniz gerekiyorsa, [Azure IoT iletişim ağ geçidini](iot-hub-protocol-gateway.md)gözden geçirmelisiniz. Bu yazılım, doğrudan IoT Hub ile arabirim yapan yüksek performanslı özel bir ağ geçidi dağıtmanızı sağlar. Azure IoT iletişim ağ geçidi, cihaz protokolünü brownfield MQTT dağıtımlarını veya diğer özel protokolleri barındıracak şekilde özelleştirmenize olanak tanır. Ancak, bu yaklaşım, özel bir iletişim ağ geçidi çalıştırıp çalıştırmanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

MQTT protokolü hakkında daha fazla bilgi edinmek için [MQTT belgelerine](https://mqtt.org/documentation)bakın.

IoT Hub dağıtımınızı planlama hakkında daha fazla bilgi edinmek için bkz:

* [IoT için Azure Sertifikalı cihaz kataloğu](https://catalog.azureiotsolutions.com/)
* [Ek protokolleri destekleyin](iot-hub-protocol-gateway.md)
* [Etkinlik Hub'larıyla karşılaştırın](iot-hub-compare-event-hubs.md)
* [Ölçekleme, HA ve DR](iot-hub-scaling.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
