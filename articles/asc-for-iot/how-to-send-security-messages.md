---
title: IoT için Azure Güvenlik Merkezi 'ne güvenlik iletilerinizi gönderin | Microsoft Docs
description: IoT için Azure Güvenlik Merkezi 'ni kullanarak güvenlik iletilerinizi gönderme hakkında bilgi edinin.
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
ms.date: 07/27/2019
ms.author: mlottner
ms.openlocfilehash: c780eea15b9f064d3279c75ac2f967e8b6099ecb
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596213"
---
# <a name="send-security-messages-sdk"></a>Güvenlik iletileri SDK 'Sı gönder

Bu nasıl yapılır kılavuzunda, IoT Aracısı için Azure Güvenlik Merkezi 'ni kullanmadan cihaz güvenlik iletilerinizi toplayıp göndermenizi ve bunun nasıl yapılacağını anlatan IoT hizmeti özelliklerine yönelik Azure Güvenlik Merkezi açıklanır.  

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * İçin güvenlik iletisi gönder API 'sini kullanınC#
> * C için güvenlik iletisi gönder API 'sini kullanma

## <a name="azure-security-center-for-iot-capabilities"></a>IoT özellikleri için Azure Güvenlik Merkezi

IoT için Azure Güvenlik Merkezi, gönderilen veriler [IoT şeması Için Azure Güvenlik Merkezi](https://aka.ms/iot-security-schemas) 'ne uygun olduğu ve ileti güvenlik iletisi olarak ayarlandığı sürece her türlü güvenlik iletisi verilerini işleyebilir ve analiz edebilir.

## <a name="security-message"></a>Güvenlik iletisi

IoT için Azure Güvenlik Merkezi, aşağıdaki ölçütleri kullanarak bir güvenlik iletisi tanımlar:
- İleti Azure IoT C/C# SDK ile gönderildiyse
- İleti [güvenlik iletisi şemasına](https://aka.ms/iot-security-schemas) uygunsa
- İleti gönderilmeden önce bir güvenlik iletisi olarak ayarlandıysa

Her güvenlik iletisi `AgentId` `AgentVersion`,göndereninmeta verilerini, vegüvenlikolaylarınınbirlistesiniiçerir.`MessageSchemaVersion`
Şema, olay türleri dahil olmak üzere güvenlik iletisinin geçerli ve gerekli özelliklerini tanımlar.

>[!Note]
> Şemayla uyumlu olmayan gönderilen iletiler yok sayılır. Yoksayılan iletiler Şu anda depolanmadığından, verileri göndermeyi başlatmadan önce şemayı doğrulamaya dikkat edin. 

>[!Note]
> Azure IoT C/C# SDK kullanılarak gönderilen bir güvenlik iletisi olarak ayarlanmamış Iletiler, IoT işlem hattı Için Azure Güvenlik Merkezi 'ne yönlendirilmeyecektir

## <a name="valid-message-example"></a>Geçerli ileti örneği

Aşağıdaki örnekte geçerli bir güvenlik iletisi nesnesi gösterilmektedir. Örnek, ileti meta verilerini ve bir `ProcessCreate` güvenlik olayını içerir.

Bir güvenlik iletisi olarak ayarladıktan ve gönderildikten sonra, bu ileti IoT için Azure Güvenlik Merkezi tarafından işlenir.

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

## <a name="send-security-messages"></a>Güvenlik iletilerini gönder 

[Azure C# IoT cihaz SDK 'Sını](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview) veya [Azure IoT C cihaz SDK 'Sını](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview)kullanarak IoT Aracısı için Azure Güvenlik Merkezi 'ni kullanmadan güvenlik iletileri gönderin.

IoT için Azure Güvenlik Merkezi 'Nde cihazlarınızdan cihaz verilerini işlenmek üzere göndermek için aşağıdaki API 'lerden birini kullanarak IoT işleme işlem hattı için Azure Güvenlik Merkezi 'ne doğru yönlendirme iletileri işaretleyin. 

Doğru üst bilgiyle işaretlenmiş olsa bile gönderilen tüm veriler, [IoT ileti şeması Için Azure Güvenlik Merkezi](https://aka.ms/iot-security-schemas)ile de uyumlu olmalıdır. 

### <a name="send-security-message-api"></a>Güvenlik iletisi API 'SI gönder

**Güvenlik Iletileri gönderme** API 'si Şu anda C ve C#içinde kullanılabilir.  

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

#### <a name="c-api"></a>C API 'SI

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

## <a name="next-steps"></a>Sonraki adımlar
- IoT hizmetine [genel bakış](overview.md) Için Azure Güvenlik Merkezi 'ni okuyun
- IoT [mimarisi](architecture.md) Için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS](resources-frequently-asked-questions.md) 'yi okuyun
- [Ham güvenlik verilerine](how-to-security-data-access.md) erişme hakkında bilgi edinin
- [Önerileri](concept-recommendations.md) anlama
- [Uyarıları](concept-security-alerts.md) anlama
