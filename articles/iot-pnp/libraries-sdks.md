---
title: IoT Tak ve Kullan kitaplıkları ve SDK 'Ları
description: IoT Tak ve Kullan özellikli çözümler geliştirmek için kullanılabilen cihaz ve hizmet kitaplıkları hakkında bilgiler.
author: rido-min
ms.author: rmpablos
ms.date: 07/22/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 5638cd9973c6a4df809e0b200efe85b067aae026
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407804"
---
# <a name="microsoft-sdks-for-iot-plug-and-play"></a>IoT için Microsoft SDK Tak ve Kullan

IoT Tak ve Kullan kitaplıkları ve SDK 'Ları, geliştiricilerin birden çok platformda çeşitli programlama dillerini kullanarak IoT çözümleri oluşturmalarına olanak tanır. Aşağıdaki tabloda, başlamanıza yardımcı olacak örnekler ve hızlı başlangıçlar yer almaktadır:

## <a name="device-sdks-ga"></a>Cihaz SDK 'Ları (GA)

| Dil | Paket | Kod deposu | Örnekler | Hızlı Başlangıç | Başvuru |
|---|---|---|---|---|---|
| C-cihaz | [vcpkg 1.3.9](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/setting_up_vcpkg.md) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/releases/tag/2020-07-19) | [Örnekler](https://github.com/Azure/azure-iot-sdk-c/tree/2020-07-19/iothub_client/samples/pnp) | [IoT Hub'a bağlanma](quickstart-connect-device-c.md) | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| .NET-cihaz | [NuGet 1.27.0](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/1.27.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device/samples/PnpDeviceSamples) | [IoT Hub'a bağlanma](quickstart-connect-device-csharp.md) | [Başvuru](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client?view=azure-dotnet) |
| Java-cihaz | [Maven 1.24.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-device-client/1.24.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/pnp-device-sample) | [IoT Hub'a bağlanma](quickstart-connect-device-java.md) | [Başvuru](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device?view=azure-java-stable) |
| Python-cihaz | [PIP 2.1.4](https://pypi.org/project/azure-iot-device/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/pnp) | [IoT Hub'a bağlanma](quickstart-connect-device-python.md) | [Başvuru](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device?view=azure-python) |
| Düğüm-cihaz | [NPM 1.17.0](https://www.npmjs.com/package/azure-iot-device)  | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples/pnp) | [IoT Hub'a bağlanma](quickstart-connect-device-node.md) | [Başvuru](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-node-latest) |

## <a name="device-sdks-preview"></a>Cihaz SDK 'Ları (Önizleme)

| Dil | Kod deposu/örnekler |
|---|---|
|Embedded için Azure SDK| [GitHub](https://github.com/Azure/azure-sdk-for-c/#) |
|Azure RTOS IoT ara yazılımı| [GitHub](https://github.com/azure-rtos/azure-iot-preview#) |
|Azure RTOS Başlarken Kılavuzu | [GitHub](https://github.com/azure-rtos/getting-started) |

## <a name="service-sdks-preview"></a>Hizmet SDK 'Ları (Önizleme)

| Dil | Paket | Kod deposu | Örnekler | Hızlı Başlangıç | Başvuru |
|---|---|---|---|---|---|
| .NET-IoT Hub hizmeti önizlemesi | [NuGet 1.27.1-Preview-002](https://www.nuget.org/packages/Microsoft.Azure.Devices/1.27.1-preview-002 ) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/pnp-preview-refresh) | [Örnekler](https://github.com/Azure/azure-iot-sdk-csharp/tree/pnp-preview-refresh/iothub/service/samples/PnpServiceSamples) | Yok | Yok |
| Java-IoT Hub hizmeti önizlemesi | [Maven 1.1.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-service-client-preview/1.1.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/pnp-preview-refresh) | [Örnekler](https://github.com/Azure/azure-iot-sdk-java/tree/pnp-preview-refresh/service/iot-service-samples/pnp-service-sample) | Yok | Yok |
| Node-IoT Hub hizmeti önizlemesi | [NPM 1.12.4-PNP-Yenile. 4](https://www.npmjs.com/package/azure-iothub/v/1.12.4-pnp-refresh.4) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/service/samples) | Yok | Yok |
| Python-IoT Hub/dijital TWINS hizmeti önizlemesi | [PIP 2.2.1 RC1](https://pypi.org/project/azure-iot-hub/2.2.1rc1/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/pnp-preview-refresh) | [Örnekler](https://github.com/Azure/azure-iot-sdk-python/tree/pnp-preview-refresh/azure-iot-hub/samples) | [IoT Hub Digital TWINS API 'siyle etkileşim kurma](quickstart-service-python.md) | Yok |
| Düğüm-dijital TWINS hizmeti önizlemesi | [NPM 1.0.0-PNP-Yenile. 3](https://www.npmjs.com/package/azure-iot-digitaltwins-service/v/1.0.0-pnp-refresh.3) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/digitaltwins/samples/service/javascript) | [IoT Hub Digital TWINS API 'siyle etkileşim kurma](quickstart-service-node.md) | Yok |

## <a name="next-steps"></a>Sonraki adımlar

SDK 'Ları ve kitaplıkları denemek için bkz. [Geliştirici Kılavuzu](concepts-developer-guide.md) ve [cihaz hızlı başlangıç](quickstart-connect-device-c.md) ve [hizmet hızlı](quickstart-service-node.md)başlangıçlarını inceleyin.
