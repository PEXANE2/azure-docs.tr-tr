---
title: IoT Tak ve Kullan kitaplıkları ve SDK 'Ları
description: IoT Tak ve Kullan özellikli çözümler geliştirmek için kullanılabilen cihaz ve hizmet kitaplıkları hakkında bilgiler.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: b328d7fe4cf3a3487614ed93dcf130aa7a233efd
ms.sourcegitcommit: 8b37091efe8c575467e56ece4d3f805ea2707a64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75830577"
---
# <a name="iot-plug-and-play-libraries-and-sdks"></a>IoT Tak ve Kullan kitaplıkları ve SDK 'Ları

IoT Tak ve Kullan kitaplıkları ve SDK 'Ları, geliştiricilerin birden çok platformda çeşitli programlama dillerini kullanarak IoT çözümleri oluşturmalarına olanak tanır. Aşağıdaki tabloda, başlamanıza yardımcı olacak örnekler ve hızlı başlangıçlar yer almaktadır:

## <a name="microsoft-supported-libraries-and-sdks"></a>Microsoft tarafından desteklenen kitaplıklar ve SDK 'lar

| Platform | Kitaplık/paket | Kaynak kodu | Örnek | Hızlı Başlangıç | Başvuru |
| -------- | ------- | ----------- | ------ | ---------- | --------- |
| C/Linux  | [Apt 'de cihaz SDK-Get](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#aptgetpackage) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Digital Ikizi istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [IoT Hub Bağlan](./quickstart-connect-pnp-device-c-linux.md) | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Windows  | [Vcpkg 'da cihaz SDK 'Sı](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/doc/setting_up_vcpkg.md#setup-c-sdk-vcpkg-for-windows-development-environment) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Digital Ikizi istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [IoT Hub Bağlan](./quickstart-connect-pnp-device-c-windows.md) | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/mbed  | [EKLEME sırasında cihaz SDK 'Sı](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#mbed) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Digital Ikizi istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Arduino  | [Arduino IDE 'de cihaz SDK 'Sı](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#arduino) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Digital Ikizi istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/iOS  | [CocoaPod üzerinde cihaz SDK 'Sı](https://cocoapods.org/pods/AzureIoTHubClient) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Digital Ikizi istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C#    | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.DigitalTwin.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [Dijital Ikizi örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/digitaltwin/Samples) | [IoT Hub Bağlan](./quickstart-connect-pnp-device-csharp.md) | [Başvuru](https://docs.microsoft.com/dotnet/api/overview/azure/iot/client?view=azure-dotnet) |
| Java   | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/digital-twin-device-client-preview/1.0.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Dijital Ikizi örnekleri](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/digital-twin/Samples) | [IoT Hub Bağlan](./quickstart-connect-pnp-device-java.md) | [Başvuru](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device) |
| Node.js | [NPM](https://www.npmjs.com/package/azure-iot-digitaltwins-device) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview) | [Dijital Ikizi örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview/digitaltwins/samples) | [IoT Hub Bağlan](./quickstart-connect-pnp-device-node.md) | [Başvuru](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device) |

## <a name="iot-hub-support"></a>IoT Hub desteği

IoT Tak ve Kullan cihaz özellikleri yalnızca [ücretsiz ve Standart katman IoT Hub 'ları](../iot-hub/iot-hub-scaling.md)tarafından desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz SDK 'larına ve kitaplıklarına ek olarak, model depolarıyla etkileşim kurmak için REST API 'Lerini kullanabilirsiniz.