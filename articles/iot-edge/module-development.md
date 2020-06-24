---
title: Azure IoT Edge için modüller geliştirin | Microsoft Docs
description: Çalışma zamanı ve IoT Hub iletişim kurabilen Azure IoT Edge için özel modüller geliştirin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 96bd6b461a5374b5f5bc578c5f58dbcd09cd7087
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84699303"
---
# <a name="develop-your-own-iot-edge-modules"></a>Kendi IoT Edge modüllerinizi geliştirin

Azure IoT Edge modüller, diğer Azure hizmetleriyle bağlanabilir ve daha büyük bulut veri işlem hattınızla katkıda bulunabilir. Bu makalede, IoT Edge çalışma zamanı ve IoT Hub ve dolayısıyla Azure bulutunun geri kalanı ile iletişim kurmak için nasıl modül geliştirebileceğiniz açıklanır.

## <a name="iot-edge-runtime-environment"></a>IoT Edge çalışma zamanı ortamı

IoT Edge çalışma zamanı, birden çok IoT Edge modülünün işlevselliğini tümleştirme ve bunları IoT Edge cihazlara dağıtma altyapısını sağlar. Herhangi bir program IoT Edge modülü olarak paketlenebilir. IoT Edge iletişim ve yönetim işlevlerine tam olarak yararlanmak için, modülde çalışan bir program, yerel IoT Edge hub 'ına bağlanmak için Azure IoT cihaz SDK 'sını kullanabilir.

## <a name="using-the-iot-edge-hub"></a>IoT Edge hub 'ı kullanma

IoT Edge hub 'ı iki ana işlev sağlar: proxy IoT Hub ve yerel iletişimler.

### <a name="iot-hub-primitives"></a>IoT Hub temel elemanlar

IoT Hub bir cihaza bir modül örneğini görür ve bunun anlamı:

* Bu, farklı bir modül ikizi sahiptir ve [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve bu cihazın diğer modül TWINS 'i ile yalıtılmış.
* [cihazdan buluta iletileri](../iot-hub/iot-hub-devguide-messaging.md)gönderebilir;
* özellikle kendi kimliğine hedeflenmiş [doğrudan Yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) alabilir.

Şu anda modüller, buluttan cihaza iletileri alamıyor veya karşıya dosya yükleme özelliğini kullanamıyor.

Modül yazarken, [Azure IoT cihaz SDK 'sını](../iot-hub/iot-hub-devguide-sdks.md) kullanarak IoT Edge hub 'ına bağlanabilir ve bir cihaz uygulamasıyla IoT Hub kullandığınızda yukarıdaki işlevleri kullanabilirsiniz. IoT Edge modüller ve IoT cihaz uygulamaları arasındaki tek fark, cihaz kimliği yerine modül kimliğine başvurmanız gerekir.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler

Cihazdan buluta iletileri karmaşık şekilde işlemeyi etkinleştirmek için IoT Edge hub, modüller arasında ve modüller ile IoT Hub arasında bildirime dayalı yönlendirme sağlar. Bildirime dayalı yönlendirme, modüllerin diğer modüller tarafından gönderilen iletileri kesmesini ve işlemesini ve bunları karmaşık işlem hatlarına yaymasını sağlar. Daha fazla bilgi için bkz. [IoT Edge modülleri dağıtma ve yolları oluşturma](module-composition.md).

Normal bir IoT Hub cihaz uygulamasının aksine bir IoT Edge modülü, kendi yerel IoT Edge hub 'ı tarafından işlemek için proxy olan cihazdan buluta iletiler alabilir.

IoT Edge hub, [dağıtım bildiriminde](module-composition.md)açıklanan bildirime dayalı yollara göre iletileri modülünüzü yayar. IoT Edge modülünü geliştirirken, ileti işleyicilerini ayarlayarak bu iletileri alabilirsiniz.

Yolların oluşturulmasını basitleştirmek için IoT Edge modül *girişi* ve *Çıkış* uç noktaları kavramını ekler. Bir modül herhangi bir giriş belirtmeden, cihaza yönlendirilen tüm cihazdan buluta iletileri alabilir ve herhangi bir çıktı belirtmeden cihazdan buluta iletiler gönderebilirler. Açık girişler ve çıktılar kullanarak yönlendirme kurallarını daha basit hale getirir.

Son olarak, Edge hub 'ı tarafından işlenen cihazdan buluta iletiler aşağıdaki sistem özellikleriyle damgalı.

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemcinin cihaz KIMLIĞI |
| $connectionModuleId | İletiyi gönderen modülün modül KIMLIĞI |
| $inputName | Bu iletiyi almış olan giriş. Boş olabilir. |
| $outputName | İletiyi göndermek için kullanılan çıktı. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden IoT Edge hub 'ına bağlanma

Bir modülden yerel IoT Edge hub 'ına bağlanmak iki adımdan oluşur:

1. Uygulamanızda bir ModuleClient örneği oluşturun.
2. Uygulamanızın, bu cihazdaki IoT Edge hub tarafından sunulan sertifikayı kabul ettiğinden emin olun.

Modueclient örneklerinin IoT cihazlarını IoT Hub bağlama biçimine benzer şekilde modülünüzü cihazda çalışan IoT Edge hub 'ına bağlamak için bir ModuleClient örneği oluşturun. Moduleclient sınıfı ve iletişim yöntemleri hakkında daha fazla bilgi için bkz. tercih ettiğiniz SDK dili için API başvurusu: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient?view=azure-python), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)veya [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

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

[IoT Edge için C# modülleri geliştirmek için Visual Studio 'Yu kullanma](how-to-visual-studio-develop-module.md)

[IoT Edge için modüller geliştirmek üzere Visual Studio Code kullanın](how-to-vs-code-develop-module.md)

[Azure IoT Hub SDK 'larını anlama ve kullanma](../iot-hub/iot-hub-devguide-sdks.md)
