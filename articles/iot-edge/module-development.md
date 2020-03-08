---
title: İçin Azure IOT Edge modülleri geliştirme | Microsoft Docs
description: Azure IOT Edge çalışma zamanı ve IOT Hub ile iletişim kurabilen için modüllerinizi geliştirebilir.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 96bd6b461a5374b5f5bc578c5f58dbcd09cd7087
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78358685"
---
# <a name="develop-your-own-iot-edge-modules"></a>Kendi IoT Edge modüllerinizi geliştirin

Azure IoT Edge modüller, diğer Azure hizmetleriyle bağlanabilir ve daha büyük bulut veri işlem hattınızla katkıda bulunabilir. Bu makalede, IoT Edge çalışma zamanı ve IoT Hub ve dolayısıyla Azure bulutunun geri kalanı ile iletişim kurmak için nasıl modül geliştirebileceğiniz açıklanır.

## <a name="iot-edge-runtime-environment"></a>IOT Edge çalışma zamanı ortamı

IOT Edge çalışma zamanı birden çok IOT Edge modülleri işlevlerini tümleştirin ve bunları IOT Edge cihazları dağıtmak için altyapı sağlar. Herhangi bir program IoT Edge modülü olarak paketlenebilir. IoT Edge iletişim ve yönetim işlevlerine tam olarak yararlanmak için, modülde çalışan bir program, yerel IoT Edge hub 'ına bağlanmak için Azure IoT cihaz SDK 'sını kullanabilir.

## <a name="using-the-iot-edge-hub"></a>IOT Edge hub'ı kullanma

IOT Edge hub'ı iki ana işlevleri sağlar: IOT hub'ı ve yerel iletişimler proxy.

### <a name="iot-hub-primitives"></a>IOT hub'ı temelleri

IOT hub'ı öğesine anlamında bir cihaz için bir modül örneğinin görür:

* Bu, farklı bir modül ikizi sahiptir ve [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve bu cihazın diğer modül TWINS 'i ile yalıtılmış.
* [cihazdan buluta iletileri](../iot-hub/iot-hub-devguide-messaging.md)gönderebilir;
* özellikle kendi kimliğine hedeflenmiş [doğrudan Yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) alabilir.

Şu anda modüller, buluttan cihaza iletileri alamıyor veya karşıya dosya yükleme özelliğini kullanamıyor.

Modül yazarken, [Azure IoT cihaz SDK 'sını](../iot-hub/iot-hub-devguide-sdks.md) kullanarak IoT Edge hub 'ına bağlanabilir ve bir cihaz uygulamasıyla IoT Hub kullandığınızda yukarıdaki işlevleri kullanabilirsiniz. IoT Edge modüller ve IoT cihaz uygulamaları arasındaki tek fark, cihaz kimliği yerine modül kimliğine başvurmanız gerekir.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler

Cihazdan buluta iletileri karmaşık şekilde işlemeyi etkinleştirmek için IoT Edge hub, modüller arasında ve modüller ile IoT Hub arasında bildirime dayalı yönlendirme sağlar. Bildirim temelli yönlendirme modülleri izlemesine izin verir ve işlem iletileri diğer modüller tarafından gönderilen ve bunları karmaşık komut zincirlerinde yayar. Daha fazla bilgi için bkz. [IoT Edge modülleri dağıtma ve yolları oluşturma](module-composition.md).

Normal bir IoT Hub cihaz uygulamasının aksine bir IoT Edge modülü, kendi yerel IoT Edge hub 'ı tarafından işlemek için proxy olan cihazdan buluta iletiler alabilir.

IoT Edge hub, [dağıtım bildiriminde](module-composition.md)açıklanan bildirime dayalı yollara göre iletileri modülünüzü yayar. IOT Edge modülü geliştirirken, ileti işleyicileri ayarlayarak bu iletiler alabilir.

Yolların oluşturulmasını basitleştirmek için IoT Edge modül *girişi* ve *Çıkış* uç noktaları kavramını ekler. Bir modül için herhangi bir giriş belirtmeden yönlendirilen tüm CİHAZDAN buluta iletiler alabilir ve herhangi bir çıktı belirtmeden CİHAZDAN buluta iletileri gönderebilir. Açık girişler ve çıkışlar, kullanarak yine de Yönlendirme kuralları anlamak daha basit hale getirir.

Son olarak, cihaz-bulut iletileri Edge hub'ı tarafından işlenen, aşağıdaki sistem özellikleri ile içerdiği:

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemci cihaz kimliği |
| $connectionModuleId | İletiyi gönderen modülün modül kimliği |
| $inputName | Bu ileti alındı. giriş. Boş olabilir. |
| $outputName | İleti göndermek için kullanılan çıkış. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden IOT Edge hub'ına bağlama

Bir modülden yerel IOT Edge hub'ına bağlanan iki adımdan oluşur:

1. Uygulamanızda bir ModuleClient örneği oluşturun.
2. Bu cihazın IOT Edge hub tarafından sunulan sertifika, uygulamanızın kabul ettiğinden emin olun.

Modueclient örneklerinin IoT cihazlarını IoT Hub bağlama biçimine benzer şekilde modülünüzü cihazda çalışan IoT Edge hub 'ına bağlamak için bir ModuleClient örneği oluşturun. Moduleclient sınıfı ve iletişim yöntemleri hakkında daha fazla bilgi için bkz. tercih ettiğiniz SDK dili için API başvurusu [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet):, [C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient?view=azure-python), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)veya [Node. js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

## <a name="language-and-architecture-support"></a>Dil ve mimari desteği

IoT Edge, gereksinimlerinize uyan senaryoyu oluşturabilmeniz için birden çok işletim sistemini, cihaz mimarilerini ve geliştirme dillerini destekler. Özel IoT Edge modülleri geliştirmeye yönelik seçeneklerinizi anlamak için bu bölümü kullanın. [IoT Edge için geliştirme ve test ortamınızı hazırlama](development-environment.md)konusundaki her dilin araç desteği ve gereksinimleri hakkında daha fazla bilgi edinebilirsiniz.

### <a name="linux"></a>Linux

Aşağıdaki tablodaki tüm diller için IoT Edge AMD64 ve ARM32 Linux cihazları için geliştirmeyi destekler.

| Geliştirme dili | Geliştirme araçları |
| -------------------- | ----------------- |
| C | Visual Studio Code<br>Visual Studio 2017/2019 |
| C# | Visual Studio Code<br>Visual Studio 2017/2019 |
| Java | Visual Studio Code |
| Node.js | Visual Studio Code |
| Python | Visual Studio Code |

>[!NOTE]
>ARM64 Linux cihazları için geliştirme ve hata ayıklama desteği [genel önizlemeye](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)sunuldu. Daha fazla bilgi için bkz. [Visual Studio Code IoT Edge modüllerini geliştirme ve hata ayıklama (Önizleme)](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview).

### <a name="windows"></a>Windows

Aşağıdaki tablodaki tüm diller için IoT Edge AMD64 Windows cihazları için geliştirmeyi destekler.

| Geliştirme dili | Geliştirme araçları |
| -------------------- | ----------------- |
| C | Visual Studio 2017/2019 |
| C# | Visual Studio Code (hata ayıklama özellikleri yok)<br>Visual Studio 2017/2019 |

## <a name="next-steps"></a>Sonraki adımlar

[Geliştirme ve test ortamınızı IoT Edge için hazırlama](development-environment.md)

[IoT Edge için modül geliştirmek C# üzere Visual Studio 'yu kullanma](how-to-visual-studio-develop-module.md)

[IoT Edge için modüller geliştirmek üzere Visual Studio Code kullanın](how-to-vs-code-develop-module.md)

[Azure IoT Hub SDK 'larını anlama ve kullanma](../iot-hub/iot-hub-devguide-sdks.md)
