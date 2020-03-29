---
title: IoT Tak ve Çalıştır kitaplıkları ve SDK'ları
description: IoT Tak ve Çalıştır özellikli çözümler geliştirmek için kullanılabilen cihaz ve hizmet kitaplıkları hakkında bilgi.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 394edc24cfeefb96cb2f412985cb990a2306bb09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064339"
---
# <a name="iot-plug-and-play-libraries-and-sdks"></a>IoT Tak ve Çalıştır kitaplıkları ve SDK'ları

IoT Tak ve Çalıştır kitaplıkları ve SDK'ları, geliştiricilerin birden fazla platformda çeşitli programlama dillerini kullanarak IoT çözümleri oluşturmasına olanak tanır. Aşağıdaki tablo, örneklere bağlantılar içerir ve başlamanıza yardımcı olmak için hızlı başlangıçlar içerir:

## <a name="microsoft-supported-libraries-and-sdks"></a>Microsoft destekli kitaplıklar ve SDK'lar

| Platform | Kütüphane / Paket | Kaynak kod | Örnek | Hızlı Başlangıç | Başvuru |
| -------- | ------- | ----------- | ------ | ---------- | --------- |
| C/Linux  | [Cihaz SDK apt-get üzerinde](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#aptgetpackage) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Dijital İkiz istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [IoT Hub'a bağlanma](./quickstart-connect-pnp-device-c-linux.md) | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Windows  | [Vcpkg üzerinde Cihaz SDK](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/doc/setting_up_vcpkg.md#setup-c-sdk-vcpkg-for-windows-development-environment) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Dijital İkiz istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [IoT Hub'a bağlanma](./quickstart-connect-pnp-device-c-windows.md) | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/mbed  | [EMBED üzerinde Cihaz SDK](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#mbed) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Dijital İkiz istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Arduino  | [Arduino IDE'de Cihaz SDK](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#arduino) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Dijital İkiz istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/iOS  | [CocoaPod üzerinde Cihaz SDK](https://cocoapods.org/pods/AzureIoTHubClient) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Dijital İkiz istemci örnekleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C#    | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.DigitalTwin.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [Dijital İkiz örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/digitaltwin/Samples) | [IoT Hub'a bağlanma](./quickstart-connect-pnp-device-csharp.md) | [Başvuru](https://docs.microsoft.com/dotnet/api/overview/azure/iot/client?view=azure-dotnet) |
| Java   | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/digital-twin-device-client-preview/1.0.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Dijital İkiz örnekleri](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/digital-twin/Samples) | [IoT Hub'a bağlanma](./quickstart-connect-pnp-device-java.md) | [Başvuru](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device) |
| Node.js | [NPM](https://www.npmjs.com/package/azure-iot-digitaltwins-device) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview) | [Dijital İkiz örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview/digitaltwins/samples) | [IoT Hub'a bağlanma](./quickstart-connect-pnp-device-node.md) | [Başvuru](https://docs.microsoft.com/javascript/api/azure-iot-device/) |

## <a name="iot-hub-support"></a>IoT Hub desteği

IoT Tak ve Çalıştır cihazı özellikleri yalnızca [ücretsiz ve standart seviye IoT hub'ları](../iot-hub/iot-hub-scaling.md)tarafından desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Aygıt SDK'larına ve kitaplıklarına ek olarak, model depolarıyla etkileşimkurmak için REST API'lerini kullanabilirsiniz.