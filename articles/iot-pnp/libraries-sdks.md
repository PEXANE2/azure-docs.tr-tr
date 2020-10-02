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
ms.openlocfilehash: 1b534c6ccb3bfca6c7dddaec82d8794deae72ff2
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91651318"
---
# <a name="microsoft-sdks-for-iot-plug-and-play"></a>IoT için Microsoft SDK Tak ve Kullan

IoT Tak ve Kullan kitaplıkları ve SDK 'Ları, geliştiricilerin birden çok platformda çeşitli programlama dillerini kullanarak IoT çözümleri oluşturmalarına olanak tanır. Aşağıdaki tabloda, başlamanıza yardımcı olacak örnekler ve hızlı başlangıçlar yer almaktadır:

## <a name="device-sdks"></a>Cihaz SDK 'Ları

| Dil | Paket | Kod deposu | Örnekler | Hızlı Başlangıç | Başvuru |
|---|---|---|---|---|---|
| C-cihaz | [vcpkg 1.3.9](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/setting_up_vcpkg.md) | [GitHub](https://github.com/Azure/azure-iot-sdk-c) | [Örnekler](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/pnp) | [IoT Hub'a bağlanma](quickstart-connect-device-c.md) | [Başvuru](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| .NET-cihaz | [NuGet 1.31.0](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/) | [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/device/PnpDeviceSamples) | [IoT Hub'a bağlanma](quickstart-connect-device-csharp.md) | [Başvuru](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client?view=azure-dotnet&preserve-view=true) |
| Java-cihaz | [Maven 1.26.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-device-client) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/pnp-device-sample) | [IoT Hub'a bağlanma](quickstart-connect-device-java.md) | [Başvuru](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device?view=azure-java-stable&preserve-view=true) |
| Python-cihaz | [PIP 2.3.0](https://pypi.org/project/azure-iot-device/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/pnp) | [IoT Hub'a bağlanma](quickstart-connect-device-python.md) | [Başvuru](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device?view=azure-python&preserve-view=true) |
| Düğüm-cihaz | [NPM 1.17.2](https://www.npmjs.com/package/azure-iot-device)  | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/master/) | [Örnekler](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples/pnp) | [IoT Hub'a bağlanma](quickstart-connect-device-node.md) | [Başvuru](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-node-latest&preserve-view=true) |
| Gömülü C-cihaz | Yok | [GitHub](https://github.com/Azure/azure-sdk-for-c/)| [Örnekler](howto-use-embedded-c.md#samples) | [Katıştırılmış C kullanma](howto-use-embedded-c.md) | Yok

## <a name="service-sdks"></a>Hizmet SDK'ları

| Platform  | Paket | Kod deposu | Örnekler | Hızlı Başlangıç | Başvuru |
|---|---|---|---|---|---|
| .NET-IoT Hub hizmeti | [NuGet 1.31.0](https://www.nuget.org/packages/Microsoft.Azure.Devices ) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/PnpServiceSamples) | Yok | [Başvuru](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet&preserve-view=true) |
| Java-IoT Hub hizmeti | [Maven 1.26.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-service-client/1.26.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Örnekler](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples/pnp-service-sample) | Yok | [Başvuru](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service?view=azure-java-stable&preserve-view=true) |
| Node-IoT Hub hizmeti | [NPM 1.13.0](https://www.npmjs.com/package/azure-iothub) | [GitHub](https://github.com/Azure/azure-iot-sdk-node) | [Örnekler](https://github.com/Azure/azure-iot-sdk-node/tree/master/service/samples) | Yok | [Başvuru](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-node-latest&preserve-view=true) |
| Python-dijital TWINS hizmeti | [PIP 2.2.3](https://pypi.org/project/azure-iot-hub) | [GitHub](https://github.com/Azure/azure-iot-sdk-python) | [Örnekler](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-hub/samples) | [IoT Hub Digital TWINS API 'siyle etkileşim kurma](quickstart-service-python.md) | Yok |
| Düğüm-dijital TWINS hizmeti | [NPM 1.13.0](https://www.npmjs.com/package/azure-iot-digitaltwins-service) | [GitHub](https://github.com/Azure/azure-iot-sdk-node) | [Örnekler](https://github.com/Azure/azure-iot-sdk-node/tree/master/service/samples/javascript) | [IoT Hub Digital TWINS API 'siyle etkileşim kurma](quickstart-service-node.md) | Yok |

## <a name="next-steps"></a>Sonraki adımlar

SDK 'Ları ve kitaplıkları denemek için bkz.  [Geliştirici Kılavuzu](concepts-developer-guide-device-csharp.md) ve [cihaz hızlı başlangıç](quickstart-connect-device-c.md) ve [hizmet hızlı](quickstart-service-node.md)başlangıçlarını inceleyin.
