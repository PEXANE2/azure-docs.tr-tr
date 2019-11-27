---
title: Modüller mantıksal cihazlarınızda - Azure IOT Edge çalışma şeklini öğrenin | Microsoft Docs
description: Azure IOT Edge modüllerini dağıtılabilir ve böylece IOT Edge üzerinde iş mantığını cihazlara çalıştırabilirsiniz yönetilmesine mantığı kapsayıcılı birimlerin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 744a901c6b0260f4fc14a2f06b88dfb36973b0f8
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456572"
---
# <a name="understand-azure-iot-edge-modules"></a>Azure IoT Edge modüllerini anlama

Azure IoT Edge, çalışma mantığını uç üzerinde *modüller*biçiminde dağıtmanıza ve yönetmenize olanak tanır. Azure IOT Edge modülleri, IOT Edge tarafından yönetilen bir hesaplama birimi en küçük olan ve Azure hizmetlerinin (örneğin, Azure Stream Analytics) veya kendi çözümünüze özgü kodla içerebilir. Modüllerin nasıl geliştirildiğini, dağıtıldığını ve yapıldığını anlamak için, bir modülün dört kavramsal öğesini düşünmenize yardımcı olur:

* **Modül görüntüsü** , bir modülü tanımlayan yazılımı içeren bir pakettir.
* **Modül örneği** , bir IoT Edge cihazında modül görüntüsünü çalıştıran belirli bir hesaplama birimidir. Modül örneğinin IOT Edge çalışma zamanı tarafından başlatılır.
* **Modül kimliği** , her modül örneğiyle ilişkili IoT Hub depolanan bir bilgi parçasıdır (güvenlik kimlik bilgileri dahil).
* **Modül ikizi** , meta veriler, konfigürasyonlar ve koşullar dahil olmak üzere bir modül örneği için durum bilgilerini içeren IoT Hub depolanan bir JSON belgesidir. 

## <a name="module-images-and-instances"></a>Modül görüntüleri ve örnekleri

IOT Edge modülü görüntüler, yönetim, güvenlik ve IOT Edge çalışma zamanı iletişim özelliklerini yararlanarak uygulamalar içerir. Kendi modül görüntüleri geliştirin veya bir Azure Stream Analytics gibi desteklenen bir Azure hizmet verin.
Bulutta görüntü var ve bunlar güncelleştirilebilir, değiştirilmiş ve farklı çözümler dağıtılan. Örneğin, bir insansız hava aracı ile denetlemek için görüntü işleme kullanan bir modül daha ayrı bir görüntü olarak üretim satırı çıkışı tahmin etmek için makine öğrenimini kullanıyor bir modül bulunmaktadır. 

Bir modül görüntüsü bir cihaza dağıtılan ve IOT Edge çalışma zamanı tarafından başlatılan her zaman bu modülü yeni bir örneğini oluşturulur. Dünyanın farklı kısımlarındaki iki cihaz aynı modül görüntüsünü kullanabilir. Ancak, modül cihazda başlatıldığında her cihazın kendi modül örneği olur. 

![Diyagram - bulutta modül görüntüleri, cihazlarda modülü örnekleri](./media/iot-edge-modules/image_instance.png)

Uygulamada, modülleri görüntü deposundaki kapsayıcı görüntüleri olarak mevcut ve cihazlarda modülü örnekleri kapsayıcılardır. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Modül kimlikleri

IOT Edge çalışma zamanı tarafından yeni bir modül örneği oluşturulduğunda, karşılık gelen bir modül kimliği ile ilişkili bir örneğidir. Modül kimliği IOT Hub'ında depolanan ve adresleme ve güvenlik kapsamı, tüm yerel ve bulut iletişimleri için belirli bir modül örneğinin olarak kullanılır.

Cihazın kimliğini bağımlı bir modül örneği ile ilişkili kimlik hangi örneğinin çalıştığından ve çözümünüzde bu modül için yazılan adı. Örneğin, bir Azure Stream Analytics kullanan bir modülün `insight` çağırırsanız ve bunu `Hannover01`adlı bir cihaza dağıtırsanız, IoT Edge çalışma zamanı `/devices/Hannover01/modules/insight`adlı karşılık gelen bir modül kimliği oluşturur.

Açıkça görülebileceği gibi senaryolarda, birden çok kez aynı cihazda, bir modül görüntüsünü dağıtmak ihtiyacınız olduğunda farklı adlar aynı görüntü birden çok kez dağıtabilirsiniz.

![Diyagram - modülü kimlikleri benzersiz cihazları içinde ve cihazlar arasında](./media/iot-edge-modules/identity.png)

## <a name="module-twins"></a>Modül ikizlerini

Her bir modül örneğinin modülü örneği yapılandırmak için kullanabileceğiniz karşılık gelen bir modül ikizi de vardır. Örnek ve ikizi birbiriyle modül kimliği ile ilişkilendirilir. 

Modül ikizi modülü bilgilerine ve yapılandırma özelliklerini depolayan bir JSON belgesidir. Bu kavram, IoT Hub [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) kavramını paraleldir. İkizi modülünün yapısı bir Device ikizi ile aynıdır. İkizlerini her iki tür ile etkileşim kurmak için kullanılan API'ler de aynı olur. İkisi arasındaki tek fark, istemci SDK'sı örneği oluşturmak için kullanılan kimliktir. 

```csharp
// Create a ModuleClient object. This ModuleClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(settings); 
await client.OpenAsync(); 
 
// Get the module twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="offline-capabilities"></a>Çevrimdışı özellikler

Azure IoT Edge modüller, en az bir kez IoT Hub eşitlemeden sonra sonsuza kadar çevrimdışı olarak çalışabilir. IoT Edge cihazlar, bu çevrimdışı özelliği diğer IoT cihazlarına da genişletebilir. Daha fazla bilgi için bkz. [IoT Edge cihazları, modülleri ve alt cihazları için genişletilmiş çevrimdışı özellikleri anlama](offline-capabilities.md).

## <a name="next-steps"></a>Sonraki adımlar
 - [IoT Edge modülleri geliştirmeye yönelik gereksinimleri ve araçları anlayın](module-development.md)
 - [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md)

