---
title: Azure IoT Hub AMQP desteğini anlayın | Microsoft Dokümanlar
description: Geliştirici kılavuzu - AMQP Protokolü'nü kullanarak IoT Hub aygıta ve hizmete bakan uç noktalara bağlanan aygıtlar için destek. Azure IoT aygıt SDK'larında yerleşik AMQP desteği hakkında bilgi içerir.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: robinsh
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 7b3dcfc51df7f0fe4291e9c5babccc1444ad32e9
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730745"
---
# <a name="communicate-with-your-iot-hub-by-using-the-amqp-protocol"></a>AMQP Protokolü'nü kullanarak IoT hub'ınızla iletişim kurun

Azure IoT Hub, cihaza bakan ve hizmete bakan uç noktalar aracılığıyla çeşitli işlevler sunmak için [OASIS Advanced Message Queuing Protocol (AMQP) sürüm 1.0'ı](https://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) destekler. Bu belge, IoT Hub işlevini kullanmak için bir IoT hub'ına bağlanmak için AMQP istemcilerinin kullanımını açıklar.

## <a name="service-client"></a>Servis istemcisi

### <a name="connect-and-authenticate-to-an-iot-hub-service-client"></a>Bir IoT hub'ına (servis istemcisi) bağlanma ve kimlik doğrulaması yapma

Bir IoT hub'ına AMQP kullanarak bağlanmak için, istemci [talep tabanlı güvenlik (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) veya [Basit Kimlik Doğrulama ve Güvenlik Katmanı (SASL) kimlik doğrulamasını](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer)kullanabilir.

Hizmet istemcisi için aşağıdaki bilgiler gereklidir:

| Bilgi | Değer |
|-------------|--------------|
| IoT hub ana bilgisayar adı | `<iot-hub-name>.azure-devices.net` |
| Anahtar adı | `service` |
| Erişim anahtarı | Hizmetle ilişkili birincil veya ikincil anahtar |
| Paylaşılan erişim imzası | Aşağıdaki biçimde kısa ömürlü paylaşılan erişim `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`imzası: . Bu imzayı oluşturmak için kodu almak [için IoT Hub'ına denetim erişimine](./iot-hub-devguide-security.md#security-token-structure)bakın.

Aşağıdaki kod parçacığı, gönderen bağlantısı aracılığıyla bir IoT hub'ına bağlanmak için [Python'daki uAMQP kitaplığını](https://github.com/Azure/azure-uamqp-python) kullanır.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here:
# https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>'  # example: '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(
    iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username),
                                  urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoke-cloud-to-device-messages-service-client"></a>Buluttan aygıta iletileri çağırma (servis istemcisi)

Hizmet ile IoT hub'ı ile aygıt ve IoT hub'ı arasındaki buluttan cihaza ileti alışverişi hakkında bilgi edinmek için, [IoT hub'ınızdan buluttan aygıta ileti gönder'e](iot-hub-devguide-messages-c2d.md)bakın. Hizmet istemcisi, aşağıdaki tabloda açıklandığı gibi, ileti göndermek ve aygıtlardan daha önce gönderilen iletiler için geri bildirim almak için iki bağlantı kullanır:

| Oluşturan | Bağlantı türü | Bağlantı yolu | Açıklama |
|------------|-----------|-----------|-------------|
| Hizmet | Gönderen bağlantısı | `/messages/devicebound` | Aygıtlar için mukadder olan buluttan cihaza iletiler hizmet tarafından bu bağlantıya gönderilir. Bu bağlantı üzerinden gönderilen `To` iletilerin özelliği hedef aygıtın alıcı `/devices/<deviceID>/messages/devicebound`bağlantı yoluna ayarlanmıştır. |
| Hizmet | Alıcı bağlantısı | `/messages/serviceBound/feedback` | Hizmet tarafından bu bağlantıdan alınan aygıtlardan gelen tamamlama, reddetme ve terk etme geri bildirim iletileri. Geri bildirim iletileri hakkında daha fazla bilgi için [bkz.](./iot-hub-devguide-messages-c2d.md#message-feedback) |

Aşağıdaki kod snippet python [uAMQP kitaplığı](https://github.com/Azure/azure-uamqp-python)kullanarak bir bulut-to-aygıt iletisi oluşturmak ve bir aygıta göndermek nasıl gösterir.

```python
import uuid
# Create a message and set message property 'To' to the device-bound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full'  # Alternative values are 'positive', 'negative', and 'none'
app_props = {'iothub-ack': ack}
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props,
                    application_properties=app_props)

# Send the message by using the send client that you created and connected to the IoT hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if it's not needed
send_client.close()
```

Geri bildirim almak için hizmet istemcisi bir alıcı bağlantısı oluşturur. Aşağıdaki kod snippet [Python uAMQP kitaplığı](https://github.com/Azure/azure-uamqp-python)kullanarak bir bağlantı oluşturmak için nasıl gösterir:

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Re-create the URI by using the preceding feedback path and authenticate it
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username),
                                  urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
    print('received a message')
    # Check content_type in message property to identify feedback messages coming from device
    if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
        msg_body_raw = msg.get_data()
        msg_body_str = ''.join(msg_body_raw)
        msg_body = json.loads(msg_body_str)
        print(json.dumps(msg_body, indent=2))
        print('******************')
        for feedback in msg_body:
            print('feedback received')
            print('\tstatusCode: ' + str(feedback['statusCode']))
            print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
            print('\tdeviceId: ' + str(feedback['deviceId']))
            print
    else:
        print('unknown message:', msg.properties.content_type)
```

Önceki kodda gösterildiği gibi, buluttan cihaza geri bildirim iletisinde bir içerik türü *vardır/vnd.microsoft.iothub.feedback.json*. Orijinal iletinin teslim durumunu çıkarmak için iletinin JSON gövdesindeki özellikleri kullanabilirsiniz:

* Geri `statusCode` bildirim gövdesindeki anahtar aşağıdaki değerlerden birine sahiptir: *Başarı*, *Süresi Dolmuş*, *TeslimatCountExceeded*, *Reddedildi*, veya *Tasfiye*.

* Geri `deviceId` bildirim gövdesindeki anahtar, hedef aygıtın kimliğine sahiptir.

* Geri `originalMessageId` bildirim gövdesindeki anahtar, hizmet tarafından gönderilen orijinal buluttan aygıta iletinin kimliğine sahiptir. Bu teslim durumunu, geri bildirimleri buluttan aygıta iletilerle ilişkilendirmek için kullanabilirsiniz.

### <a name="receive-telemetry-messages-service-client"></a>Telemetri iletileri alma (servis istemcisi)

Varsayılan olarak, IoT hub'ınız yutulan aygıt telemetri iletilerini yerleşik bir olay hub'ında depolar. Hizmet istemciniz, depolanan olayları almak için AMQP Protokolünü kullanabilir.

Bu amaçla, hizmet istemcisinin öncelikle IoT hub bitiş noktasına bağlanması ve yerleşik olay hub'larına bir yeniden yönlendirme adresi alması gerekir. Hizmet istemcisi daha sonra yerleşik olay hub'ına bağlanmak için sağlanan adresi kullanır.

Her adımda, istemci aşağıdaki bilgi parçalarını sunması gerekir:

* Geçerli hizmet kimlik bilgileri (hizmet paylaşılan erişim imza belirteci).

* İletileri almak istediği tüketici grubu bölümüne iyi biçimlendirilmiş bir yol. Belirli bir tüketici grubu ve bölüm kimliği için `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` yol aşağıdaki biçime `$Default`sahiptir: (varsayılan tüketici grubu).

* Bölümdeki bir başlangıç noktasını belirlemek için isteğe bağlı bir filtreleme yüklemi. Bu yüklem bir sıra numarası, ofset veya enqueued zaman damgası şeklinde olabilir.

Aşağıdaki kod snippet önceki adımları göstermek için [Python uAMQP kitaplığını](https://github.com/Azure/azure-uamqp-python) kullanır:

```python
import json
import uamqp
import urllib
import time

# Use the generate_sas_token implementation that's available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(
    consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(
    policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username),
                                  urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified by using endpoint_filter
# Valid predicates include:
# - amqp.annotation.x-opt-sequence-number
# - amqp.annotation.x-opt-offset
# - amqp.annotation.x-opt-enqueued-time
# Set endpoint_filter variable to None if no filter is needed
endpoint_filter = b'amqp.annotation.x-opt-sequence-number > 2995'

# Helper function to set the filtering predicate on the source URI


def set_endpoint_filter(uri, endpoint_filter=''):
    source_uri = uamqp.address.Source(uri)
    source_uri.set_filter(endpoint_filter)
    return source_uri


receive_client = uamqp.ReceiveClient(
    set_endpoint_filter(uri, endpoint_filter), debug=True)
try:
    batch = receive_client.receive_message_batch(max_batch_size=5)
except uamqp.errors.LinkRedirect as redirect:
    # Once a redirect error is received, close the original client and recreate a new one to the re-directed address
    receive_client.close()

    sas_auth = uamqp.authentication.SASTokenAuth.from_shared_access_key(
        redirect.address, policy_name, access_key)
    receive_client = uamqp.ReceiveClient(set_endpoint_filter(
        redirect.address, endpoint_filter), auth=sas_auth, debug=True)

# Start receiving messages in batches
batch = receive_client.receive_message_batch(max_batch_size=5)
for msg in batch:
    print('*** received a message ***')
    print(''.join(msg.get_data()))
    print('\t: ' + str(msg.annotations['x-opt-sequence-number']))
    print('\t: ' + str(msg.annotations['x-opt-offset']))
    print('\t: ' + str(msg.annotations['x-opt-enqueued-time']))
```

Belirli bir aygıt kimliği için, IoT hub iletilerini depolamak için hangi bölümü belirlemek için aygıt kimliği karma kullanır. Önceki kod snippet olayların böyle tek bir bölümden nasıl alındığını gösterir. Ancak, tipik bir uygulamanın genellikle tüm olay hub bölmelerinde depolanan olayları alması gerektiğini unutmayın.

## <a name="device-client"></a>Cihaz istemcisi

### <a name="connect-and-authenticate-to-an-iot-hub-device-client"></a>Bir IoT hub'ına (aygıt istemcisi) bağlanma ve kimlik doğrulaması yapma

Bir aygıt AMQP kullanarak bir IoT hub'ına bağlanmak için [talep tabanlı güvenlik (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) veya [Basit Kimlik Doğrulama ve Güvenlik Katmanı (SASL)](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) kimlik doğrulamasını kullanabilir.

Aygıt istemcisi için aşağıdaki bilgiler gereklidir:

| Bilgi | Değer |
|-------------|--------------|
| IoT hub ana bilgisayar adı | `<iot-hub-name>.azure-devices.net` |
| Erişim anahtarı | Aygıtla ilişkili birincil veya ikincil anahtar |
| Paylaşılan erişim imzası | Aşağıdaki biçimde kısa ömürlü paylaşılan erişim `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`imzası: . Bu imzayı oluşturmak için kodu almak [için IoT Hub'ına denetim erişimine](./iot-hub-devguide-security.md#security-token-structure)bakın.

Aşağıdaki kod parçacığı, gönderen bağlantısı aracılığıyla bir IoT hub'ına bağlanmak için [Python'daki uAMQP kitaplığını](https://github.com/Azure/azure-uamqp-python) kullanır.

```python
import uamqp
import urllib
import uuid

# Use generate_sas_token implementation available here:
# https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
device_id = '<device-id>'
access_key = '<primary-or-secondary-key>'
username = '{device_id}@sas.{iot_hub_name}'.format(
    device_id=device_id, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token('{hostname}/devices/{device_id}'.format(
    hostname=hostname, device_id=device_id), access_key, None)

# e.g., '/devices/{device_id}/messages/devicebound'
operation = '<operation-link-name>'
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username),
                                  urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
send_client = uamqp.SendClient(uri, debug=True)
```

Aşağıdaki bağlantı yolları aygıt işlemleri olarak desteklenir:

| Oluşturan | Bağlantı türü | Bağlantı yolu | Açıklama |
|------------|-----------|-----------|-------------|
| Cihazlar | Alıcı bağlantısı | `/devices/<deviceID>/messages/devicebound` | Aygıtlar için mukadder olan buluttan cihaza iletiler her hedef aygıt tarafından bu bağlantıdan alınır. |
| Cihazlar | Gönderen bağlantısı | `/devices/<deviceID>/messages/events` | Aygıttan gönderilen aygıttan buluta iletiler bu bağlantı üzerinden gönderilir. |
| Cihazlar | Gönderen bağlantısı | `/messages/serviceBound/feedback` | Cihazlar tarafından bu bağlantı üzerinden hizmete gönderilen buluttan cihaza ileti geri bildirimi. |

### <a name="receive-cloud-to-device-commands-device-client"></a>Bulut-aygıt komutları alma (aygıt istemcisi)

Aygıtlara gönderilen bulut-aygıt komutları bir `/devices/<deviceID>/messages/devicebound` bağlantıyla gelir. Aygıtlar bu iletileri toplu olarak alabilir ve ileti veri yükünü, ileti özelliklerini, ek açıklamaları veya uygulama özelliklerini gerektiği gibi iletide kullanabilir.

Aşağıdaki kod parçacığı, bir aygıt tarafından buluttan aygıta iletileri almak için [Python'daki uAMQP kitaplığını](https://github.com/Azure/azure-uamqp-python)kullanır.

```python
# ...
# Create a receive client for the cloud-to-device receive link on the device
operation = '/devices/{device_id}/messages/devicebound'.format(
    device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username),
                                  urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
while True:
    batch = receive_client.receive_message_batch(max_batch_size=5)
    for msg in batch:
        print('*** received a message ***')
        print(''.join(msg.get_data()))

        # Property 'to' is set to: '/devices/device1/messages/devicebound',
        print('\tto:                     ' + str(msg.properties.to))

        # Property 'message_id' is set to value provided by the service
        print('\tmessage_id:             ' + str(msg.properties.message_id))

        # Other properties are present if they were provided by the service
        print('\tcreation_time:          ' + str(msg.properties.creation_time))
        print('\tcorrelation_id:         ' +
              str(msg.properties.correlation_id))
        print('\tcontent_type:           ' + str(msg.properties.content_type))
        print('\treply_to_group_id:      ' +
              str(msg.properties.reply_to_group_id))
        print('\tsubject:                ' + str(msg.properties.subject))
        print('\tuser_id:                ' + str(msg.properties.user_id))
        print('\tgroup_sequence:         ' +
              str(msg.properties.group_sequence))
        print('\tcontent_encoding:       ' +
              str(msg.properties.content_encoding))
        print('\treply_to:               ' + str(msg.properties.reply_to))
        print('\tabsolute_expiry_time:   ' +
              str(msg.properties.absolute_expiry_time))
        print('\tgroup_id:               ' + str(msg.properties.group_id))

        # Message sequence number in the built-in event hub
        print('\tx-opt-sequence-number:  ' +
              str(msg.annotations['x-opt-sequence-number']))
```

### <a name="send-telemetry-messages-device-client"></a>Telemetri iletileri gönderme (aygıt istemcisi)

Ayrıca, AMQP kullanarak bir cihazdan telemetri iletileri gönderebilirsiniz. Aygıt isteğe bağlı olarak uygulama özellikleri sözlüğü veya ileti kimliği gibi çeşitli ileti özellikleri sağlayabilir.

Aşağıdaki kod parçacığı, bir aygıttan aygıttan buluta iletigöndermek için [Python'daki uAMQP kitaplığını](https://github.com/Azure/azure-uamqp-python) kullanır.

```python
# ...
# Create a send client for the device-to-cloud send link on the device
operation = '/devices/{device_id}/messages/events'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

send_client = uamqp.SendClient(uri, debug=True)

# Set any of the applicable message properties
msg_props = uamqp.message.MessageProperties()
msg_props.message_id = str(uuid.uuid4())
msg_props.creation_time = None
msg_props.correlation_id = None
msg_props.content_type = None
msg_props.reply_to_group_id = None
msg_props.subject = None
msg_props.user_id = None
msg_props.group_sequence = None
msg_props.to = None
msg_props.content_encoding = None
msg_props.reply_to = None
msg_props.absolute_expiry_time = None
msg_props.group_id = None

# Application properties in the message (if any)
application_properties = { "app_property_key": "app_property_value" }

# Create message
msg_data = b"Your message payload goes here"
message = uamqp.Message(msg_data, properties=msg_props, application_properties=application_properties)

send_client.queue_message(message)
results = send_client.send_all_messages()

for result in results:
    if result == uamqp.constants.MessageState.SendFailed:
        print result
```

## <a name="additional-notes"></a>Ek notlar

* AmQP bağlantıları, bir ağ hatası veya kimlik doğrulama belirteci (kodda oluşturulan) sona ermesi nedeniyle kesintiye uğrayabilir. Hizmet istemcisi bu koşulları ele almalıdır ve gerekirse bağlantıyı ve bağlantıları yeniden kurmalıdır. Kimlik doğrulama belirteci süresi dolduğunda, istemci belirteci sona ermeden önce proaktif olarak yenileyerek bir bağlantı düşüşünü önleyebilir.

* Müşteriniz bazen bağlantı yönlendirmelerini doğru şekilde işleyebilir. Böyle bir işlemi anlamak için AMQP istemci belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

AMQP Protokolü hakkında daha fazla bilgi edinmek için [AMQP v1.0 belirtimine](https://www.amqp.org/sites/amqp.org/files/amqp.pdf)bakın.

IoT Hub mesajlaşması hakkında daha fazla bilgi edinmek için bkz:

* [Buluttan cihaza iletiler](./iot-hub-devguide-messages-c2d.md)
* [Ek protokoller için destek](iot-hub-protocol-gateway.md)
* [İleti Telemetri Taşıma (MQTT) Protokolü için Destek](./iot-hub-mqtt-support.md)
