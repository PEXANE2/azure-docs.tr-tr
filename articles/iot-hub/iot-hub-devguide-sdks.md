---
title: Azure IoT SK'ları anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - aygıt uygulamaları ve arka uç uygulamaları oluşturmak için kullanabileceğiniz çeşitli Azure IoT aygıtı ve hizmet SDK'ları hakkında bilgi ve bağlantılar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: be5fae45ee513dddf002995ce9c37c6b6565f50c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258397"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Azure IoT Hub SDK'larını anlama ve kullanma

IoT Hub ile çalışmak için iki yazılım geliştirme kitleri (SDK) kategorisi vardır:

* **IoT Hub Aygıt SDK'ları,** aygıt istemcisi veya modül istemcisini kullanarak IoT aygıtlarınızda çalışan uygulamalar oluşturmanıza olanak tanır. Bu uygulamalar IoT hub’ınıza telemetri gönderir ve isteğe bağlı olarak iletileri, işleri, yöntemleri veya ikiz güncelleştirmeleri IoT hub’ınızdan alır.  [Azure IoT Edge çalışma zamanı](../iot-edge/about-iot-edge.md)için [modülleri](../iot-edge/iot-edge-modules.md) yazar yapmak için modül istemcisini de kullanabilirsiniz.

* **IoT Hub Hizmeti SDK'ları,** IoT hub'ınızı yönetmek için arka uç uygulamaları oluşturmanıza ve isteğe bağlı olarak ileti göndermenize, iş zamanlamanıza, doğrudan yöntemler çağırmanıza veya IoT aygıtlarınıza veya modüllerinize istenen özellik güncelleştirmelerini göndermenize olanak tanır.

Buna ek olarak, [Cihaz Sağlama Hizmeti](../iot-dps/about-iot-dps.md)ile çalışmak için bir dizi SDK da sediyoruz.
* **Aygıt SDK'larını sağlama,** Aygıt Sağlama Hizmeti ile iletişim kurmak için IoT aygıtlarınızda çalışan uygulamalar oluşturmanıza olanak tanır.

* **Sağlama Hizmeti SDK'ları,** Aygıt Sağlama Hizmeti'ndeki kayıtlarınızı yönetmek için arka uç uygulamaları oluşturmanıza olanak tanır.

[Azure IoT SDK'larını kullanmanın avantajları](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/)hakkında bilgi edinin.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]


### <a name="os-platform-and-hardware-compatibility"></a>İşletim sistemi platformu ve donanım uyumluluğu

SDK'lar için desteklenen platformlar Azure [IoT SDK Platform Desteği'nde](iot-hub-device-sdk-platform-support.md)bulunabilir.

Belirli donanım aygıtlarıyla SDK uyumluluğu hakkında daha fazla bilgi [için, IoT aygıt kataloğu veya](https://catalog.azureiotsolutions.com/) tek tek depo için Azure Sertifikalı'na bakın.

## <a name="azure-iot-hub-device-sdks"></a>Azure IoT Hub Aygıtı SDK'ları

Microsoft Azure IoT aygıt SDK'ları, Azure IoT Hub hizmetlerine bağlanan ve bunlar tarafından yönetilen uygulamalar oluşturmayı kolaylaştıran kod lar içerir.

.NET için Azure IoT Hub cihazı SDK: 

* [NuGet'den](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)indirin.  Ad alanı, IoT Hub Aygıt İstemlerini (DeviceClient, ModuleClient) içeren Microsoft.Azure.Devices.Clients.Clients'tir.
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Modül referansı](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

C için Azure IoT Hub cihazı SDK (ANSI C - C99):

* [apt-get, MBED, Arduino IDE veya iOS'tan yükleyin](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c)
* [C Cihazını Derleme SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compiling-the-c-device-sdk)
* [API başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Modül referansı](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h)
* [C SDK'yı diğer platformlara taşıma](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
* Çapraz derleme, farklı platformlarda başlamak vb. hakkında bilgi için [geliştirici belgeleri](https://github.com/Azure/azure-iot-sdk-c/tree/master/doc)
* [Azure IoT Hub C SDK kaynak tüketim bilgileri](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/c_sdk_resource_information.md)

Java için Azure IoT Hub cihazı SDK: 

* [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) projesine ekle
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Modül referansı](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)

Node.js için Azure IoT Hub cihazı SDK: 

* [npm'den yükleyin](https://www.npmjs.com/package/azure-iot-device)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-node)
* [API başvurusu](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Modül referansı](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

Python için Azure IoT Hub aygıtı SDK: 

* [pip'ten yükleyin](https://pypi.org/project/azure-iot-device/)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-python)
* [API başvurusu](https://docs.microsoft.com/python/api/azure-iot-device)

iOS için Azure IoT Hub cihazı SDK: 

* [CocoaPod'dan yükleyin](https://cocoapods.org/pods/AzureIoTHubClient)
* [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-ios)
* API başvurusu: [Bkz. C API başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

## <a name="azure-iot-hub-service-sdks"></a>Azure IoT Hub Hizmeti SDK'ları

Azure IoT hizmeti SDK'ları, aygıtları ve güvenliği yönetmek için Doğrudan IoT Hub ile etkileşimedebilen uygulama oluşturmayı kolaylaştıran kod lar içerir.

.NET için Azure IoT Hub hizmeti SDK:

* [NuGet'den](https://www.nuget.org/packages/Microsoft.Azure.Devices/)indirin.  Ad alanı, IoT Hub Hizmet İstemcilerini (RegistryManager, Service Clients) içeren Microsoft.Azure.Devices'tir.
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Java için Azure IoT Hub hizmeti SDK: 

* [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) projesine ekle
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

Node.js için Azure IoT Hub hizmeti SDK: 

* [NPM'den indirin](https://www.npmjs.com/package/azure-iothub)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-node)
* [API başvurusu](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Python için Azure IoT Hub hizmeti SDK: 

* [Pip'ten indirin](https://pypi.python.org/pypi/azure-iot-hub/)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-python/tree/master)

C için Azure IoT Hub hizmeti SDK: 

* [apt-get, MBED, Arduino IDE veya NuGet'den](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md) indirin
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c)

iOS için Azure IoT Hub hizmeti SDK: 

* [CocoaPod'dan yükleyin](https://cocoapods.org/pods/AzureIoTHubServiceClient)
* [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> Geliştirme makinenize ikili ve bağımlılıklar yüklemek için dil ve platforma özel paket yöneticilerini kullanma hakkında bilgi almak için GitHub depolarında okuma dosyalarına bakın.

## <a name="microsoft-azure-provisioning-sdks"></a>Microsoft Azure Sağlama SDK'ları

**Microsoft Azure Sağlama SDK'ları,** Aygıt Sağlama [Hizmeti'ni](../iot-dps/about-iot-dps.md)kullanarak AygıtLar'ı IoT Hub'ınıza aygıt sağlamanızı sağlar.

C#için Azure Sağlama cihazı ve hizmet SDK'ları:

* Cihaz [SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Client/) ve [Hizmet SDK'dan](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet'den indirin.
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp/)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.provisioning.client?view=azure-dotnet)

C için Azure Sağlama cihazı ve hizmet SDK'ları:

* [apt-get, MBED, Arduino IDE veya iOS'tan yükleyin](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [API başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

Java için Azure Sağlama cihazı ve hizmet SDK'ları:

* [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) projesine ekle
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.provisioning.device?view=azure-java-stable)

Node.js için Azure Sağlama cihazı ve hizmet SDK'ları:

* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning)
* [API başvurusu](https://docs.microsoft.com/javascript/api/overview/azure/iothubdeviceprovisioning?view=azure-node-latest)
* [NPM'den Cihaz SDK](https://badge.fury.io/js/azure-iot-provisioning-device) ve [Servis SDK'yı](https://badge.fury.io/js/azure-iot-provisioning-service) İndirin

Python için Azure Sağlama cihazı ve hizmet SDK'ları:

* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-python)
* [Cihaz SDK](https://pypi.org/project/azure-iot-device/) ve [Servis SDK'yı](https://pypi.org/project/azure-iothub-provisioningserviceclient/) pip'ten indirin

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT SDK'ları da geliştirmeye yardımcı olacak bir takım araçlar sağlar:
* [iothub-diagnostik:](https://github.com/Azure/iothub-diagnostics)IoT Hub ile bağlantı ile ilgili sorunları tanılamaya yardımcı olan bir çapraz platform komut satırı aracı.
* [azure-iot-explorer](https://github.com/Azure/azure-iot-explorer): IoT Hub'ınıza bağlanmak ve IoT aygıtları ile eklemek/yönetmek/iletişim kurmak için platformlar arası bir masaüstü uygulamasıdır.

Azure IoT SDK'larını kullanarak geliştirmeyle ilgili dokümanlar:
* IoT Hub SDK'larını kullanarak [bağlantı ve güvenilir mesajlaşmayı nasıl yöneteceğimiz](iot-hub-reliability-features-in-sdks.md) hakkında bilgi edinin.
* iOS ve Android gibi [mobil platformlar için](iot-hub-how-to-develop-for-mobile-devices.md) nasıl geliştirileceğimiz hakkında bilgi edinin.
* [Azure IoT SDK platform desteği](iot-hub-device-sdk-platform-support.md)


Bu IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)
* [Aygıt ikizleri, işleri ve ileti yönlendirmesi için IoT Hub sorgu dili](iot-hub-devguide-query-language.md)
* [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub MQTT desteği](iot-hub-mqtt-support.md)
* [IoT Hub REST API başvurusu](/rest/api/iothub/)
