---
title: Azure IoT Central mimari kavramları | Microsoft Docs
description: Bu makalede, Azure IoT Central mimarisiyle ilgili temel kavramlar tanıtılmaktadır
author: dominicbetts
ms.author: dobett
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: fab65828228bdea8f0367f8928b83934bf0a7a4f
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995885"
---
# <a name="azure-iot-central-architecture-preview-features"></a>Azure IoT Central mimarisi (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu makalede Microsoft Azure IoT Central mimarisine genel bakış sunulmaktadır.

![Üst düzey mimari](media/concepts-architecture-pnp/architecture.png)

## <a name="devices"></a>Cihazlar

Cihazlar, Azure IoT Central uygulamanızla verileri değiş tokuş. Bir cihaz şunları yapabilir:

- Telemetri gibi ölçümleri gönderin.
- Ayarları uygulamanızla eşitler.

Azure IoT Central, bir cihazın uygulamanızla değiş tokuş edebilir bir cihaz şablonunda belirtilir. Cihaz şablonları hakkında daha fazla bilgi için bkz. [metadata Management](#metadata-management).

Cihazların Azure IoT Central uygulamanıza nasıl bağlanabileceği hakkında daha fazla bilgi edinmek için bkz. [cihaz bağlantısı](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="cloud-gateway"></a>Bulut ağ geçidi

Azure IoT Central, Azure IoT Hub cihaz bağlantısı sağlayan bir bulut ağ geçidi olarak kullanır. IoT Hub şunları sunar:

- Bulutta ölçekte veri alımı.
- Cihaz yönetimi.
- Güvenli cihaz bağlantısı.

IoT Hub hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

Azure IoT Central cihaz bağlantısı hakkında daha fazla bilgi için bkz. [cihaz bağlantısı](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="data-stores"></a>Veri depolama alanları

Azure IoT Central, uygulama verilerini bulutta depolar. Depolanan uygulama verileri şunları içerir:

- Cihaz şablonları.
- Cihaz kimlikleri.
- Cihaz meta verileri.
- Kullanıcı ve rol verileri.

Azure IoT Central, cihazlarınızdan gönderilen ölçüm verileri için bir zaman serisi deposu kullanır. Analiz hizmeti tarafından kullanılan cihazlardan gelen zaman serisi verileri.

## <a name="analytics"></a>Analiz

Analiz hizmeti, uygulamanın görüntülediği özel raporlama verilerini oluşturmaktan sorumludur. Bir işleç, uygulamada görünen [Analizi özelleştirebilir](howto-create-analytics.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) . Analytics hizmeti [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) üzerine kurulmuştur ve cihazlarınızdan gönderilen ölçüm verilerini işler.

## <a name="rules-and-actions"></a>Kurallar ve eylemler

[Kurallar ve eylemler](howto-create-telemetry-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) , uygulama içindeki görevleri otomatikleştirmek için birlikte çalışır. Bir Oluşturucu, tanımlı bir eşiği aşan sıcaklık gibi cihaz telemetrisine dayalı kurallar tanımlayabilir. Azure IoT Central, kural koşullarının ne zaman karşılandığını anlamak için bir akış işlemcisi kullanır. Bir kural koşulu karşılandığında, Oluşturucu tarafından tanımlanan bir eylemi tetikler. Örneğin, bir eylem bir mühendisin bir cihazdaki sıcaklığın çok yüksek olduğunu bildiren bir e-posta gönderebilir.

## <a name="metadata-management"></a>Meta veri yönetimi

Azure IoT Central uygulamasında cihaz şablonları cihaz türlerinin davranışını ve yeteneklerini tanımlar. Örneğin, bir soğutma cihaz şablonu, bir soğutma 'nin uygulamanıza gönderdiği Telemetriyi belirtir.

![Şablon mimarisi](media/concepts-architecture-pnp/template-architecture.png)

IoT Central önizleme uygulaması cihaz şablonunda:

- **Cihaz yetenek modelleri** , gönderdiği telemetri, cihaz durumunu tanımlayan Özellikler ve cihazın yanıt verdiği komutları gibi bir cihazın yeteneklerini belirtir. Cihaz özellikleri bir veya daha fazla arabirimde düzenlenir. Cihaz yetenek modelleri hakkında daha fazla bilgi için bkz. [ıot Tak ve kullan](../iot-pnp/overview-iot-plug-and-play.md) belgeleri.
- **Bulut özellikleri** bir cihaz için IoT Central depolar özelliklerini belirtir. Bu özellikler yalnızca IoT Central depolanır ve hiçbir şekilde cihaza gönderilmez.
- **Görünümler** , bir işlecin cihazları izlemesine ve yönetmesine izin vermek için oluşturucunun oluşturduğu panoları ve formları belirler.
- **Özelleştirmeler** , oluşturucunun IoT Central uygulamayla daha uygun olması için cihaz yetenek modelindeki bazı tanımları geçersiz kılmasını sağlar.

Bir uygulama, her bir cihaz şablonuna dayalı bir veya daha fazla sanal ve gerçek cihaza sahip olabilir.

## <a name="data-export"></a>Veri dışarı aktarma

Azure IoT Central uygulamasında verilerinizi kendi Azure Event Hubs ve Azure Service Bus örneklerine [sürekli olarak dışarı aktarabilirsiniz](howto-export-data-event-hubs-service-bus-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) . Ayrıca verilerinizi Azure Blob depolama hesabınıza düzenli olarak dışarı aktarabilirsiniz. IoT Central ölçümleri, cihazları ve cihaz şablonlarını dışarı aktarabilir.

## <a name="batch-device-updates"></a>Batch cihaz güncelleştirmeleri

Azure IoT Central uygulamasında, bağlı cihazları yönetmek için [işler oluşturabilir ve çalıştırabilirsiniz](howto-run-a-job.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) . Bu işler, cihaz özellikleri veya ayarları için toplu güncelleştirmeler yapmanızı veya komutları çalıştırmanızı sağlar. Örneğin, birden çok soğutma Başlatan makinenin fan hızını artırmak için bir iş oluşturabilirsiniz.

## <a name="role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC)

Yönetici, önceden tanımlanmış rolleri kullanarak bir Azure IoT Central uygulaması için [erişim kuralları tanımlayabilir](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) . Bir yönetici, kullanıcının uygulamaya erişimi olan hangi bölgelere sahip olduğunu belirlemek için rollere Kullanıcı atayabilir.

## <a name="security"></a>Güvenlik

Azure IoT Central içindeki güvenlik özellikleri şunlardır:

- Veriler aktarım sırasında ve bekleyen sırada şifrelenir.
- Kimlik doğrulaması Azure Active Directory veya Microsoft hesabı tarafından sağlanır. İki öğeli kimlik doğrulama desteklenir.
- Tam kiracı yalıtımı.
- Cihaz düzeyi güvenliği.

## <a name="ui-shell"></a>UI kabuğu

UI kabuğu modern, yanıt veren ve HTML5 tarayıcı tabanlı bir uygulamadır.
Yönetici, özel temalar uygulayarak ve kendi özel yardım kaynaklarınızı işaret etmek üzere yardım bağlantılarını değiştirerek uygulamanın kullanıcı arabirimini özelleştirebilir. UI özelleştirmesi hakkında daha fazla bilgi için bkz. [Azure IoT Central kullanıcı arabirimini özelleştirme](howto-customize-ui.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesi.

Bir işleç, kişiselleştirilmiş uygulama panoları oluşturabilir. Farklı verileri görüntüleyen ve aralarında geçiş yapan çeşitli panolarınız olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central mimarisini öğrendiğinize göre, önerilen sonraki adım Azure IoT Central [cihaz bağlantısı](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında bilgi edineceksiniz.