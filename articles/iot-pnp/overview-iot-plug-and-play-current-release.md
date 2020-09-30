---
title: IoT Tak ve Kullan geçerli sürümü | Microsoft Docs
description: Geçerli IoT Tak ve Kullan sürümüne nelerin dahil olduğunu öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 09/19/2020
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 4cfc9ca14a4b344423a27d9683114de5275fb20b
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91583581"
---
# <a name="whats-in-the-current-iot-plug-and-play-release"></a>Geçerli IoT Tak ve Kullan sürümünde neler vardır

Bu makalede, geçerli IoT Tak ve Kullan sürümünü destekleyen araçlar, SDK 'Lar ve API 'Ler özetlenmektedir. Gösterilen sürüm numaraları, IoT Tak ve Kullan genel kullanıma sunulmakta olduğu zaman sürüm numarasını yansıtır. Sürüm numaraları sürümden sonra arttırılıp artmasını sağlayabilir.

## <a name="modeling-language"></a>Modelleme dili

[Dijital TWINS tanım dili (DTDL) v2](https://github.com/Azure/opendigitaltwins-dtdl).

IoT Tak ve Kullan cihazlarının DTDL ile nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [ıot Tak ve kullan kuralları](concepts-convention.md).

## <a name="tools-and-utilities"></a>Araçlar ve Yardımcı Programlar

- Azure IoT Explorer 0.12.0.

    Daha fazla bilgi için bkz. [Azure IoT Explorer 'ı yüklemek ve kullanmak](howto-use-iot-explorer.md).

- VS Code uzantısı 1.0.0.

    Daha fazla bilgi için bkz. [DTDL yazma araçlarını yüklemek ve kullanmak](howto-use-dtdl-authoring-tools.md).

- Visual Studio 2019 Extension 1.0.0.

    Daha fazla bilgi için bkz. [DTDL yazma araçlarını yüklemek ve kullanmak](howto-use-dtdl-authoring-tools.md).

- Azure CLı IoT uzantısı 0.10.0.

    Daha fazla bilgi edinmek için bkz. [Azure CLI Için Azure IoT uzantısını yüklemek ve kullanmak](howto-use-iot-pnp-cli.md).

    > [!TIP]
    > Azure IoT uzantısı, cihazların sertifikalandırmaya yardımcı olmak için komutlar içerir. Bkz. `az iot product -h`.



## <a name="libraries-and-sdks"></a>Kitaplıklar ve SDK’lar

Kitaplıklar ve SDK 'lar hakkında daha fazla bilgi edinmek için bkz. [IoT Için Microsoft sdk Tak ve kullan](libraries-sdks.md).

- C cihaz SDK [Vcpkg 2020-07-19](https://github.com/microsoft/vcpkg/tree/master/ports/azure-iot-sdk-c)
- .NET cihaz SDK 'Sı [NuGet Microsoft. Azure. Devices. Client 1.30.0](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)
- Java cihaz SDK 'Sı [Maven IoT-cihaz-istemci 1.25.0](https://search.maven.org/artifact/com.microsoft.azure.sdk.iot/iot-device-client)
- Python cihaz SDK 'Sı [PIP Azure-IoT-Device v 2.2.0](https://pypi.org/project/azure-iot-device/)
- Node.js cihaz SDK [NPM Azure-IoT-Device 1.17.1](https://www.npmjs.com/package/azure-iot-device)
- .NET-IoT Hub Service [NuGet Microsoft. Azure. Devices 1.22.0](https://www.nuget.org/packages/Microsoft.Azure.Devices/)
- Java-IoT Hub Service [Maven IoT-Service-Client 1.25.0](https://search.maven.org/artifact/com.microsoft.azure.sdk.iot/iot-service-client)
- Node.js-IoT Hub hizmeti [NPM Azure-ıothub 1.12.5](https://www.npmjs.com/package/azure-iothub)
- Python-IoT Hub/dijital TWINS hizmeti [PIP Azure-IoT-Hub 2.2.2](https://pypi.org/project/azure-iot-hub/)
- DTDL model ayrıştırıcısı [NuGet](https://www.nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser).

### <a name="preview"></a>Önizleme

- Embedded 1.0.0 için Azure SDK [-Önizleme. 5](https://github.com/Azure/azure-sdk-for-c/releases/tag/1.0.0-preview.5)
- Azure RTOS IoT ara yazılım [V6.0_beta3](https://github.com/azure-rtos/azure-iot-preview/releases/tag/v6.0_beta3)

## <a name="rest-apis"></a>REST API'leri

REST API [2020-09-30](https://docs.microsoft.com/rest/api/iothub).

Daha fazla bilgi için bkz. [ıot Tak ve kullan Geliştirici Kılavuzu](concepts-developer-guide-service.md).

## <a name="iot-hub"></a>IoT Hub

IoT Tak ve Kullan, tüm bölgelerde IoT Hub tarafından desteklenir. IoT Tak ve Kullan yalnızca standart veya ücretsiz katman IoT Hub 'ları tarafından desteklenir.

## <a name="announcements"></a>Duyurular

Geçerli ve önceki IoT Tak ve Kullan duyuruları için aşağıdaki blog gönderilerine bakın:

- [Genel Önizleme yenileme (29 Ağustos 2020 tarihinde gönderilir)](https://techcommunity.microsoft.com/t5/internet-of-things/add-quot-plug-and-play-quot-to-your-iot-solutions/ba-p/1548531)
- [Cihazlarınızı IoT Tak ve Kullan hazırlayın ve onaylayın (26 Ağustos 2020 tarihinde gönderilir)](https://azure.microsoft.com/blog/prepare-and-certify-your-devices-for-iot-plug-and-play/)
- [IoT Tak ve Kullan artık önizleme sürümünde sunulmaktadır (22 Ağustos 2019 tarihinde gönderilir)](https://azure.microsoft.com/blog/iot-plug-and-play-is-now-available-in-preview/)
- [Azure IoT Central ve IoT Tak ve Kullan oluşturun (7 Mayıs 2019 tarihinde gönderilir)](https://azure.microsoft.com/blog/build-with-azure-iot-central-and-iot-plug-and-play/)

