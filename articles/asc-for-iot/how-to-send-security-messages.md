---
title: Güvenlik iletilerinizi IoT için Azure Güvenlik Merkezi'ne gönderin| Microsoft Dokümanlar
description: IoT için Azure Güvenlik Merkezi'ni kullanarak güvenlik iletilerinizi nasıl göndereceğinizi öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2020
ms.author: mlottner
ms.openlocfilehash: 8bbbd8248c7418b667e34389cb47bd3f6b4f06ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76963827"
---
# <a name="send-security-messages-sdk"></a>Güvenlik mesajları Gönder SDK

Bu nasıl yapılacak kılavuzu, ioT aracısı için bir Azure Güvenlik Merkezi kullanmadan cihazınızın güvenlik iletilerini toplamayı ve göndermeyi seçtiğinizde Azure Güvenlik Merkezi IoT hizmet özelliklerini açıklar ve nasıl yapılacağını açıklar.  

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Azure IoT C SDK'yı kullanarak güvenlik iletileri gönderme
> * Azure IoT C# SDK'yı kullanarak güvenlik iletileri gönderme
> * Azure IoT Python SDK'yı kullanarak güvenlik iletileri gönderme
> * Azure IoT Düğümü.js SDK'yı kullanarak güvenlik iletileri gönderme
> * Azure IoT Java SDK'yı kullanarak güvenlik iletileri gönderme


## <a name="azure-security-center-for-iot-capabilities"></a>IoT özellikleri için Azure Güvenlik Merkezi

Azure Güvenlik Merkezi IoT, gönderilen veriler [Azure Güvenlik Merkezi IoT şemasına](https://aka.ms/iot-security-schemas) uygun olduğu ve iletinin bir güvenlik iletisi olarak ayarlanması sürece her türlü güvenlik iletisi verilerini işleyebilir ve çözümleyebilir.

## <a name="security-message"></a>Güvenlik iletisi

Azure Güvenlik Merkezi IoT için aşağıdaki ölçütleri kullanarak bir güvenlik iletisi tanımlar:
- İleti Azure IoT SDK ile gönderildiyse
- İleti [güvenlik iletisi şemasına uyuyorsa](https://aka.ms/iot-security-schemas)
- İleti göndermeden önce bir güvenlik iletisi olarak ayarlanmışsa

Her güvenlik iletisi, gönderenin , `AgentId` `AgentVersion`, `MessageSchemaVersion` ve güvenlik olaylarının listesini içeren meta verileri içerir.
Şema, etkinlik türlerini de içeren güvenlik iletisinin geçerli ve gerekli özelliklerini tanımlar.

>[!Note]
> Şemaya uymayan gönderilen iletiler yoksayılır. Göz ardı edilen iletiler şu anda depolanmadığından veri göndermeişlemini başlatmadan önce şemayı doğruladığından emin olun. 

>[!Note]
> Azure IoT SDK kullanılarak güvenlik iletisi olarak ayarlanamayan iletiler, IoT için Azure Güvenlik Merkezi'ne yönlendirilmez.

## <a name="valid-message-example"></a>Geçerli ileti örneği

Aşağıdaki örnekte geçerli bir güvenlik iletisi nesnesi gösterilmektedir. Örnek, ileti meta verilerini `ProcessCreate` ve bir güvenlik olayını içerir.

Güvenlik iletisi olarak ayarlanıp gönderildikten sonra, bu ileti IoT için Azure Güvenlik Merkezi tarafından işlenir.

```json
"AgentVersion": "0.0.1",
"AgentId": "e89dc5f5-feac-4c3e-87e2-93c16f010c25",
"MessageSchemaVersion": "1.0",
"Events": [
    {
        "EventType": "Security",
        "Category": "Triggered",
        "Name": "ProcessCreate",
        "IsEmpty": false,
        "PayloadSchemaVersion": "1.0",
        "Id": "21a2db0b-44fe-42e9-9cff-bbb2d8fdf874",
        "TimestampLocal": "2019-01-27 15:48:52Z",
        "TimestampUTC": "2019-01-27 13:48:52Z",
        "Payload":
            [
                {
                    "Executable": "/usr/bin/myApp",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "aUser",
                    "CommandLine": "myApp -a -b"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>Güvenlik iletileri gönderme 

Azure [IoT C aygıtı SDK,](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) [Azure IoT C# cihazı SDK,](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview)Azure [IoT Düğümü.js SDK](https://github.com/Azure/azure-iot-sdk-node), [Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python)veya [Azure IoT Java SDK'yı](https://github.com/Azure/azure-iot-sdk-java)kullanarak, IoT aracısı için Azure Güvenlik Merkezi'ni *kullanmadan* güvenlik iletileri gönderin.

IoT için Azure Güvenlik Merkezi tarafından işlenmek üzere aygıtlarınızdan gelen aygıt verilerini göndermek için, ioT işleme ardışık için Azure Güvenlik Merkezi'ne doğru yönlendirme için iletileri işaretlemek için aşağıdaki API'lerden birini kullanın. 

Doğru üstbilgiyle işaretlenmiş olsa bile gönderilen tüm veriler, [IoT ileti](https://aka.ms/iot-security-schemas)şeması için Azure Güvenlik Merkezi'ne de uygun olmalıdır. 

### <a name="send-security-message-api"></a>Güvenlik iletisi API gönder 

**Güvenlik iletileri GÖNDER** API'si şu anda C ve C#, Python, Node.js ve Java'da kullanılabilir.  

#### <a name="c-api"></a>C API

```c
bool SendMessageAsync(IoTHubAdapter* iotHubAdapter, const void* data, size_t dataSize) {
 
    bool success = true;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;
 
    messageHandle = IoTHubMessage_CreateFromByteArray(data, dataSize);
 
    if (messageHandle == NULL) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubMessage_SetAsSecurityMessage(messageHandle) != IOTHUB_MESSAGE_OK) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubModuleClient_SendEventAsync(iotHubAdapter->moduleHandle, messageHandle, SendConfirmCallback, iotHubAdapter) != IOTHUB_CLIENT_OK) {
        success = false;
        goto cleanup;
    }
 
cleanup:
    if (messageHandle != NULL) {
        IoTHubMessage_Destroy(messageHandle);
    }
 
    return success;
}
 
static void SendConfirmCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback) {
    if (userContextCallback == NULL) {
        //error handling
        return;
    }
 
    if (result != IOTHUB_CLIENT_CONFIRMATION_OK){
        //error handling
    }
}
```
#### <a name="c-api"></a>C# API’si

```cs

private static async Task SendSecurityMessageAsync(string messageContent)
{
    ModuleClient client = ModuleClient.CreateFromConnectionString("<connection_string>");
    Message  securityMessage = new Message(Encoding.UTF8.GetBytes(messageContent));
    securityMessage.SetAsSecurityMessage();
    await client.SendEventAsync(securityMessage);
}
```
#### <a name="nodejs-api"></a>Düğüm.js API

```typescript
var Protocol = require('azure-iot-device-mqtt').Mqtt

function SendSecurityMessage(messageContent)
{
  var client = Client.fromConnectionString(connectionString, Protocol);

  var connectCallback = function (err) {
    if (err) {
      console.error('Could not connect: ' + err.message);
    } else {
      var message = new Message(messageContent);
      message.setAsSecurityMessage();
      client.sendEvent(message);
  
      client.on('error', function (err) {
        console.error(err.message);
      });
  
      client.on('disconnect', function () {
        clearInterval(sendInterval);
        client.removeAllListeners();
        client.open(connectCallback);
      });
    }
  };

  client.open(connectCallback);
}
```

#### <a name="python-api"></a>Python API

Python API'sini kullanmak için [azure-iot-device](https://pypi.org/project/azure-iot-device/)paketini yüklemeniz gerekir.

Python API'sını kullanırken, benzersiz aygıtı veya modül bağlantı dizesini kullanarak güvenlik iletisini modül veya aygıt aracılığıyla gönderebilirsiniz. Aşağıdaki Python komut dosyası örneğini bir aygıtla kullanırken, **IoTHubDeviceClient'ı**ve bir modülle **IoTHubModuleClient'ı**kullanın. 

```python
from azure.iot.device.aio import IoTHubDeviceClient, IoTHubModuleClient
from azure.iot.device import Message

async def send_security_message_async(message_content):
    conn_str = os.getenv("<connection_string>")
    device_client = IoTHubDeviceClient.create_from_connection_string(conn_str)
    await device_client.connect()
    security_message = Message(message_content)
    security_message.set_as_security_message()
    await device_client.send_message(security_message)
    await device_client.disconnect()
```

#### <a name="java-api"></a>Java API’si

```java
public void SendSecurityMessage(string message)
{
    ModuleClient client = new ModuleClient("<connection_string>", IotHubClientProtocol.MQTT);
    Message msg = new Message(message);
    msg.setAsSecurityMessage();
    EventCallback callback = new EventCallback();
    string context = "<user_context>";
    client.sendEventAsync(msg, callback, context);
}
```


## <a name="next-steps"></a>Sonraki adımlar
- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS'yi](resources-frequently-asked-questions.md) okuyun
- Ham güvenlik [verilerine](how-to-security-data-access.md) nasıl erişin gerektiğini öğrenin
- [Önerileri](concept-recommendations.md) anlama
- [Uyarıları](concept-security-alerts.md) anlama
