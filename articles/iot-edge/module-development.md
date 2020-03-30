---
title: Azure IoT Edge için modüller geliştirme | Microsoft Dokümanlar
description: Azure IoT Edge için çalışma zamanı ve IoT Hub ile iletişim kurabilen özel modüller geliştirin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 96bd6b461a5374b5f5bc578c5f58dbcd09cd7087
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271388"
---
# <a name="develop-your-own-iot-edge-modules"></a>Kendi IoT Edge modüllerinizi geliştirin

Azure IoT Edge modülleri diğer Azure hizmetlerine bağlanabilir ve daha büyük bulut veri ardışık alanınızın katkısı olabilir. Bu makalede, IoT Edge çalışma zamanı ve IoT Hub ile ve dolayısıyla Azure bulutunun geri kalanıyla iletişim kurmak için modüller nasıl geliştirebileceğiniz açıklanmaktadır.

## <a name="iot-edge-runtime-environment"></a>IoT Edge çalışma zamanı ortamı

IoT Edge çalışma süresi, birden çok IoT Edge modülünün işlevselliğini tümleştirmek ve bunları IoT Edge aygıtlarına dağıtmak için altyapı sağlar. Herhangi bir program Bir IoT Edge modülü olarak paketlenebilir. IoT Edge iletişim ve yönetim işlevlerinden tam olarak yararlanmak için, modülde çalışan bir program yerel IoT Edge hub'ına bağlanmak için Azure IoT Device SDK'yı kullanabilir.

## <a name="using-the-iot-edge-hub"></a>IoT Edge hub'ını kullanma

IoT Edge hub'ı iki ana işlev sağlar: IoT Hub'a proxy ve yerel iletişim.

### <a name="iot-hub-primitives"></a>IoT Hub ilkel

IoT Hub, bir aygıta benzer bir şekilde bir modül örneği görür:

* [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve bu cihazın diğer modül ikizlerinden farklı ve izole edilmiş bir modül ikizi vardır;
* [aygıttan buluta iletiler](../iot-hub/iot-hub-devguide-messaging.md)gönderebilir;
* özellikle kimliğine yönelik [doğrudan yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) alabilir.

Şu anda, modüller buluttan cihaza ileti ler alamıyor veya dosya yükleme özelliğini kullanamıyor.

Modül yazarken, IoT Edge hub'ına bağlanmak için [Azure IoT Aygıt SDK'yı](../iot-hub/iot-hub-devguide-sdks.md) kullanabilir ve bir aygıt uygulamasıyla IoT Hub'ı kullanırken olduğu gibi yukarıdaki işlevleri kullanabilirsiniz. IoT Edge modülleri ile IoT aygıt uygulamaları arasındaki tek fark, aygıt kimliği yerine modül kimliğine başvurmanız gerektiğidir.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler

Aygıttan buluta iletilerin karmaşık bir şekilde işlenmesini etkinleştirmek için, IoT Edge hub'ı iletilerin modüller arasında ve modüller ile IoT Hub arasında bildirimsel yönlendirmesini sağlar. Bildirimsel yönlendirme, modüllerin diğer modüller tarafından gönderilen iletileri durdurmasına ve işlemesine ve bunları karmaşık boru hatlarına yaymasına olanak tanır. Daha fazla bilgi için [modülleri dağıtın ve IoT Edge'de rotalar kurun.](module-composition.md)

Bir IoT Edge modülü, normal bir IoT Hub aygıt uygulamasının aksine, yerel IoT Edge hub'ı tarafından işlem için yakınolan aygıttan buluta iletileri alabilir.

IoT Edge hub'ı, [dağıtım bildiriminde](module-composition.md)açıklanan bildirimsel yolları temel alınabilmek için iletileri modülünüze yayır. Bir IoT Edge modülü geliştirirken, ileti işleyicileri ayarlayarak bu iletileri alabilirsiniz.

Yolların oluşturulmasını kolaylaştırmak için, IoT Edge modül *giriş* ve *çıkış* uç noktaları kavramını ekler. Bir modül, herhangi bir giriş belirtmeden ona yönlendirilen tüm aygıttan buluta iletileri alabilir ve herhangi bir çıktı belirtmeden aygıttan buluta iletiler gönderebilir. Ancak, açık giriş ve çıktıları kullanmak yönlendirme kurallarının anlaşılmasını kolaylaştırır.

Son olarak, Edge hub'ı tarafından işlenen aygıttan buluta iletiler aşağıdaki sistem özellikleriyle damgalanır:

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemcinin aygıt kimliği |
| $connectionModuleId | İletiyi gönderen modülün modül kimliği |
| $inputName | Bu iletiyi alan giriş. Boş olabilir. |
| $outputName | İletigöndermek için kullanılan çıktı. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden IoT Edge hub'ına bağlanma

Bir modülden yerel IoT Edge hub'ına bağlanmak iki adım dan oluşur:

1. Uygulamanızda bir ModuleClient örneği oluşturun.
2. Uygulamanızın söz leziz cihazdaki IoT Edge hub'ı tarafından sunulan sertifikayı kabul ettiğinden emin olun.

Cihazınızda çalışan IoT Edge hub'ına modülünüze bağlanmak için DeviceClient örneklerinin IoT Hub'a bağlanmasına benzer bir ModuleClient örneği oluşturun. ModuleClient sınıfı ve iletişim yöntemleri hakkında daha fazla bilgi için tercih ettiğiniz SDK dili için API referansına bakın: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python,](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient?view=azure-python) [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable), veya [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

## <a name="language-and-architecture-support"></a>Dil ve mimari desteği

IoT Edge, gereksinimlerinize uygun senaryoyu oluşturabilmeniz için birden çok işletim sistemi, aygıt mimarisi ve geliştirme dillerini destekler. Özel IoT Edge modülleri geliştirme seçeneklerinizi anlamak için bu bölümü kullanın. IoT Edge için geliştirme ve test [ortamınızı hazırlayın'da](development-environment.md)her dil için araç desteği ve gereksinimleri hakkında daha fazla bilgi edinebilirsiniz.

### <a name="linux"></a>Linux

Aşağıdaki tablodaki tüm diller için IoT Edge, AMD64 ve ARM32 Linux cihazlarının geliştirilmesini destekler.

| Geliştirme dili | Geliştirme araçları |
| -------------------- | ----------------- |
| C | Visual Studio Code<br>Visual Studio 2017/2019 |
| C# | Visual Studio Code<br>Visual Studio 2017/2019 |
| Java | Visual Studio Code |
| Node.js | Visual Studio Code |
| Python | Visual Studio Code |

>[!NOTE]
>ARM64 Linux cihazları için destek geliştirme ve hata ayıklama [genel önizlemede.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Daha fazla bilgi için Visual [Studio Code(önizleme) 'deki ARM64 IoT Edge modüllerini geliştir](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview)in ve hata ayıklama bölümüne bakın.

### <a name="windows"></a>Windows

Aşağıdaki tablodaki tüm diller için IoT Edge, AMD64 Windows aygıtları için geliştirmeyi destekler.

| Geliştirme dili | Geliştirme araçları |
| -------------------- | ----------------- |
| C | Visual Studio 2017/2019 |
| C# | Visual Studio Code (hata ayıklama özelliği yok)<br>Visual Studio 2017/2019 |

## <a name="next-steps"></a>Sonraki adımlar

[Geliştirme ve test ortamınızı IoT Edge için hazırlayın](development-environment.md)

[IoT Edge için C# modülleri geliştirmek için Visual Studio'u kullanın](how-to-visual-studio-develop-module.md)

[IoT Edge için modüller geliştirmek için Visual Studio Code'u kullanın](how-to-vs-code-develop-module.md)

[Azure IoT Hub SDK'larını anlama ve kullanma](../iot-hub/iot-hub-devguide-sdks.md)
