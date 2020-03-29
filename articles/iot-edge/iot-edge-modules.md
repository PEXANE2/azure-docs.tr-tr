---
title: Aygıtlarınızda modüllerin mantığı nasıl çalıştırdığını öğrenin - Azure IoT Edge | Microsoft Dokümanlar
description: Azure IoT Edge modülleri, IoT Edge aygıtlarında iş mantığı çalıştırabilmeniz için uzaktan dağıtılabilen ve yönetilebilen kapsayıcılaştırılmış mantık birimleridir
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 1c625e628f53d156ad56a1c69df1c23aec9120ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76548722"
---
# <a name="understand-azure-iot-edge-modules"></a>Azure IoT Edge modüllerini anlama

Azure IoT Edge, *modüller*biçiminde kenarda iş mantığı dağıtmanızı ve yönetmenize olanak tanır. Azure IoT Edge modülleri, IoT Edge tarafından yönetilen en küçük hesaplama birimidir ve Azure hizmetlerini (Azure Akış Analizi gibi) veya kendi çözüme özel kodunuzu içerebilir. Modüllerin nasıl geliştirildiğini, dağıtılabildiğini ve sürdürüldünü anlamak için, bir modülün dört kavramsal öğesini göz önünde bulundurun:

* **Modül görüntüsü,** bir modülü tanımlayan yazılımı içeren bir pakettir.
* **Modül örneği,** modül görüntüsünü bir IoT Edge aygıtında çalıştıran belirli bir hesaplama birimidir. Modül örneği IoT Edge çalışma zamanı tarafından başlatılır.
* **Modül kimliği,** IoT Hub'ında depolanan ve her modül örneğiyle ilişkili bir bilgi dir (güvenlik kimlik bilgileri dahil).
* **Modül ikizi,** IoT Hub'da depolanan ve meta veriler, yapılandırmalar ve koşullar da dahil olmak üzere bir modül örneği için durum bilgilerini içeren bir JSON belgesidir.

## <a name="module-images-and-instances"></a>Modül görüntüleri ve örnekleri

IoT Edge modül görüntüleri, IoT Edge çalışma zamanının yönetim, güvenlik ve iletişim özelliklerinden yararlanan uygulamalar içerir. Kendi modül resimlerinizi geliştirebilir veya Azure Akış Analizi gibi desteklenen bir Azure hizmetinden bir tane dışa aktarabilirsiniz.
Görüntüler bulutta bulunur ve farklı çözümlerde güncellenebilir, değiştirilebilir ve dağıtılabilir. Örneğin, üretim hattı çıktısını tahmin etmek için makine öğrenimini kullanan bir modül, bir drone'u kontrol etmek için bilgisayar görüşünü kullanan bir modülden ayrı bir görüntü olarak bulunur.

Bir aygıta her modül görüntüsü dağıtıldığında ve IoT Edge çalışma zamanı tarafından başlatıldığında, bu modülün yeni bir örneği oluşturulur. Dünyanın farklı yerlerinde iki cihaz aynı modül görüntüsünü kullanabilir. Ancak, modül cihazda başlatıldığında her aygıtın kendi modül örneği olacaktır.

![Diyagram - Buluttaki modül görüntüleri, cihazlardaki modül örnekleri](./media/iot-edge-modules/image_instance.png)

Uygulamada, modül görüntüleri bir depoda kapsayıcı görüntüleri olarak bulunur ve modül örnekleri aygıtlarda kapsayıcıdır.

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Modül kimlikleri

IoT Edge çalışma zamanı tarafından yeni bir modül örneği oluşturulduğunda, karşılık gelen bir modül kimliği alır. Modül kimliği IoT Hub'da depolanır ve bu modül örneği için tüm yerel ve bulut iletişimleri için adresleme ve güvenlik kapsamı olarak kullanılır.

Modül örneğiyle ilişkili kimlik, örneğin çalıştırıldığı aygıtın kimliğine ve çözümünüzde bu modüle sağladığınız ada bağlıdır. Örneğin, Azure Akış `insight` Analizi kullanan bir modülü çağırır sanız ve bunu bir `Hannover01`aygıta dağıtırsanız, IoT Edge `/devices/Hannover01/modules/insight`çalışma zamanı , ilgili modül kimliği oluşturur.

Açıkçası, senaryolarda bir modül görüntüsünü aynı aygıta birden çok kez dağıtmanız gerektiğinde, aynı görüntüyü farklı adlarla birden çok kez dağıtabilirsiniz.

![Diyagram - Modül kimlikleri aygıtlar içinde ve cihazlar arasında benzersizdir](./media/iot-edge-modules/identity.png)

## <a name="module-twins"></a>Modül ikizleri

Her modül örneğinde, modül örneğini yapılandırmak için kullanabileceğiniz karşılık gelen bir modül ikizi de vardır. Örnek ve ikiz modül kimliği ile birbirleri ile ilişkilidir.

Modül ikizi, modül bilgilerini ve yapılandırma özelliklerini depolayan bir JSON belgesidir. Bu kavram, IoT [Hub'daki aygıt ikiz](../iot-hub/iot-hub-devguide-device-twins.md) konseptine paraleldir. Bir modül ikiz yapısı bir cihaz ikiz aynıdır. Her iki ikiz tipiyle etkileşimde kullanılan API'ler de aynıdır. İkisi arasındaki tek fark, istemci SDK'yı anında taklit etmek için kullanılan kimliktir.

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

Azure IoT Edge modülleri, IoT Hub ile en az bir kez eşitledikten sonra süresiz olarak çevrimdışı çalışabilir. IoT Edge aygıtları bu çevrimdışı özelliği diğer IoT aygıtlarına da genişletebilir. Daha fazla bilgi için bkz. [IoT Edge aygıtları, modülleri ve alt aygıtları için genişletilmiş çevrimdışı özellikleri anlayın.](offline-capabilities.md)

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Edge modülleri geliştirmek için gereksinimleri ve araçları anlama](module-development.md)
* [Azure IoT Edge çalışma süresini ve mimarisini anlama](iot-edge-runtime.md)
