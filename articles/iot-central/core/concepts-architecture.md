---
title: Azure IoT Central'da mimari kavramlar | Microsoft Dokümanlar
description: Bu makale, Azure IoT Central mimarisiyle ilgili temel kavramları tanıtır
author: dominicbetts
ms.author: dobett
ms.date: 11/27/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 12ad231d81b6c134ebb8d4902b3f95c978e9622d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271648"
---
# <a name="azure-iot-central-architecture"></a>Azure IoT Central mimarisi



Bu makalede, Microsoft Azure IoT Merkezi mimarisine genel bir bakış sağlanmaktadır.

![Üst düzey mimari](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Cihazlar

Cihazlar Azure IoT Merkezi uygulamanızla veri alışverişinde bulunur. Bir aygıt şunları yapabilir:

- Telemetri gibi ölçümler gönderin.
- Ayarları uygulamanızla senkronize edin.

Azure IoT Central'da, bir aygıtın uygulamanızla değiş tokuş edebileceği veriler bir aygıt şablonunda belirtilir. Aygıt şablonları hakkında daha fazla bilgi için [Meta veri yönetimine](#metadata-management)bakın.

Aygıtların Azure IoT Merkezi uygulamanıza nasıl bağlanışları hakkında daha fazla bilgi edinmek için [Aygıt bağlantısına](concepts-get-connected.md)bakın.

## <a name="azure-iot-edge-devices"></a>Azure IoT Edge cihazları

[Azure IoT SDK'ları](https://github.com/Azure/azure-iot-sdks)kullanılarak oluşturulan aygıtların yanı sıra, [Azure IoT Edge aygıtlarını](../../iot-edge/about-iot-edge.md) bir IoT Merkezi uygulamasına da bağlayabilirsiniz. IoT Edge, bulut zekası ve özel mantığı doğrudan IoT Central tarafından yönetilen IoT aygıtlarında çalıştırmanızı sağlar. IoT Edge çalışma süresi şunları yapmanızı sağlar:

- Aygıttaki iş yüklerini yükleyin ve güncelleyin.
- Aygıtta IoT Edge güvenlik standartlarını koruyun.
- IoT Edge modüllerinin her zaman çalışır durumda olduğundan emin olun.
- Uzaktan izleme için modül durumunu buluta bildirin.
- Akış aşağı yaprak aygıtları ile IoT Edge aygıtı arasındaki iletişimi, bir IoT Edge aygıtındaki modüller arasındaki ve bir IoT Edge aygıtı ile bulut arasındaki iletişimi yönetin.

![Azure IoT Edge ile Azure IoT Merkezi](./media/concepts-architecture/iotedge.png)

IoT Central, IoT Edge aygıtları için aşağıdaki özellikleri sağlar:

- Bir IoT Edge aygıtının özelliklerini açıklamak için aygıt şablonları aşağıdakiler gibi:
  - Bir aygıt filosu için bir manifesto yönetmenize yardımcı olan dağıtım bildirimi yükleme özelliği.
  - IoT Edge aygıtında çalışan modüller.
  - Her modülün gönderdiği telemetri.
  - Her modülün bildirdiği özellikler.
  - Her modülün yanıt vermesi gereken komutlar.
  - IoT Edge ağ geçidi aygıt özelliği modeli ile akış aşağı aygıt yetenek modeli arasındaki ilişkiler.
  - IoT Edge aygıtında depolanan bulut özellikleri.
  - IoT Merkezi uygulamanızın bir parçası olan özelleştirmeler, panolar ve formlar.

  Daha fazla bilgi için [Azure IoT Edge aygıtlarını Azure IoT Merkezi uygulama makalesine bağlayın.](./concepts-iot-edge.md)

- Azure IoT aygıt sağlama hizmetini kullanarak IoT Edge aygıtlarını ölçekte sağlama olanağı
- Kurallar ve eylemler.
- Özel panolar ve analizler.
- IoT Edge aygıtlarından sürekli veri aktarım.

### <a name="iot-edge-device-types"></a>IoT Edge cihaz türleri

IoT Central, IoT Edge cihaz türlerini aşağıdaki gibi sınıfa uygular:

- Yaprak cihazları. Bir IoT Edge aygıtında akış aşağı yaprak aygıtları olabilir, ancak bu aygıtlar IoT Central'da bulunmamaktadır.
- Akış aşağı aygıtları olan ağ geçidi aygıtları. Hem ağ geçidi aygıtı hem de akış aşağı aygıtları IoT Central'da

![IoT Edge'e Genel Bakış ile IoT Merkezi](./media/concepts-architecture/gatewayedge.png)

### <a name="iot-edge-patterns"></a>IoT Edge desenleri

IoT Central aşağıdaki IoT Edge aygıt desenlerini destekler:

#### <a name="iot-edge-as-leaf-device"></a>Yaprak cihaz olarak IoT Edge

![Yaprak cihaz olarak IoT Edge](./media/concepts-architecture/edgeasleafdevice.png)

IoT Edge cihazı IoT Central'da ve herhangi bir alt akım aygıtında ve telemetrileri IoT Edge aygıtından geldiği şeklinde temsil edilir. IoT Edge aygıtına bağlı alt akım aygıtları IoT Central'da sağlanmıyor.

#### <a name="iot-edge-gateway-device-connected-to-downstream-devices-with-identity"></a>Kimlikle alt akım aygıtlarına bağlı IoT Edge ağ geçidi aygıtı

![Akış aşağı aygıt kimliğine sahip IoT Edge](./media/concepts-architecture/edgewithdownstreamdeviceidentity.png)

IoT Edge cihazı, IoT Edge aygıtına bağlı aşağı akım aygıtlarıyla birlikte IoT Central'da bulunur. Ağ geçidi üzerinden akış aşağı aygıtları sağlama için çalışma zamanı desteği şu anda desteklenmemektedir.

#### <a name="iot-edge-gateway-device-connected-to-downstream-devices-with-identity-provided-by-the-iot-edge-gateway"></a>IoT Edge ağ geçidi tarafından sağlanan kimliği olan alt aygıtlara bağlı IoT Edge ağ geçidi aygıtı

![Kimlik olmadan downstream cihazı ile IoT Edge](./media/concepts-architecture/edgewithoutdownstreamdeviceidentity.png)

IoT Edge cihazı, IoT Edge aygıtına bağlı aşağı akım aygıtlarıyla birlikte IoT Central'da bulunur. Akış aşağı aygıtlara kimlik sağlayan ağ geçidinin çalışma zamanı desteği ve akış aşağı aygıtların sağlanması şu anda desteklenmemektedir. Kendi kimlik çeviri modülünüzü getirirseniz, IoT Central bu deseni destekleyebilir.

## <a name="cloud-gateway"></a>Bulut ağ geçidi

Azure IoT Central, Azure IoT Hub'ı aygıt bağlantısı sağlayan bir bulut ağ geçidi olarak kullanır. IoT Hub şunları sağlar:

- Bulutta ölçekte veri alımı.
- Cihaz yönetimi.
- Aygıt bağlantısını güvenli hale getirin.

IoT Hub hakkında daha fazla bilgi edinmek için [Azure IoT Hub'ına](https://docs.microsoft.com/azure/iot-hub/)bakın.

Azure IoT Central'da aygıt bağlantısı hakkında daha fazla bilgi edinmek için [Bkz. Aygıt bağlantısı.](concepts-get-connected.md)

## <a name="data-stores"></a>Veri depoları

Azure IoT Central uygulama verilerini bulutta depolar. Depolanan uygulama verileri şunları içerir:

- Aygıt şablonları.
- Cihaz kimlikleri.
- Aygıt meta verileri.
- Kullanıcı ve rol verileri.

Azure IoT Central, aygıtlarınızdan gönderilen ölçüm verileri için bir zaman serisi deposu kullanır. Analiz hizmeti tarafından kullanılan cihazlardan gelen zaman serisi verileri.

## <a name="analytics"></a>Analiz

Analiz hizmeti, uygulamanın görüntülenebilen özel raporlama verilerini oluşturmakla yükümlüdür. Bir operatör uygulamada görüntülenen [analitiği özelleştirebilir.](howto-create-analytics.md) Analiz hizmeti, Azure Time [Series Öngörüleri'nin](https://azure.microsoft.com/services/time-series-insights/) üzerine kuruludur ve cihazlarınızdan gönderilen ölçüm verilerini işler.

## <a name="rules-and-actions"></a>Kurallar ve eylemler

[Kurallar ve eylemler,](tutorial-create-telemetry-rules.md) uygulama içindeki görevleri otomatikleştirmek için yakın bir şekilde çalışır. Oluşturucu, tanımlanan eşiği aşan sıcaklık gibi aygıt telemetrisini temel alan kuralları tanımlayabilir. Azure IoT Central, kural koşullarının ne zaman karşılandığını belirlemek için bir akış işlemcisi kullanır. Bir kural koşulu karşılandığında, oluşturucu tarafından tanımlanan bir eylemi tetikler. Örneğin, bir eylem, mühendise aygıttaki sıcaklığın çok yüksek olduğunu bildiren bir e-posta gönderebilir.

## <a name="metadata-management"></a>Meta veri yönetimi

Bir Azure IoT Merkezi uygulamasında, aygıt şablonları aygıt türlerinin davranışını ve yeteneğini tanımlar. Örneğin, buzdolabı aygıtı şablonu, buzdolabının uygulamanıza gönderdiği telemetriyi belirtir.

![Şablon mimarisi](media/concepts-architecture/template-architecture.png)

IoT Merkezi uygulama aygıtı şablonunda şunlar yer alıyor:

- **Aygıt özelliği modelleri,** gönderdiği telemetri, aygıt durumunu tanımlayan özellikler ve aygıtın yanıt verdiği komutlar gibi bir aygıtın yeteneklerini belirtir. Aygıt yetenekleri bir veya daha fazla arabirim halinde düzenlenir. Aygıt özelliği modelleri hakkında daha fazla bilgi için [IoT Tak ve Çalıştır (önizleme)](../../iot-pnp/overview-iot-plug-and-play.md) belgelerine bakın.
- **Bulut özellikleri,** bir aygıtın IoT Central depolarını belirtir. Bu özellikler yalnızca IoT Central'da depolanır ve hiçbir zaman bir aygıta gönderilmez.
- **Görünümler,** operatörün aygıtları izlemesine ve yönetmesine izin vermek için oluşturucunun oluşturduğu panoları ve formları belirtir.
- **Özelleştirmeler,** oluşturucunun aygıt özelliği modelindeki bazı tanımları geçersiz kArak, bunları IoT Merkezi uygulamasıyla daha alakalı hale getirmelerine izin vermez.

Bir uygulama, her aygıt şablonuna dayalı bir veya daha fazla simüle edilmiş ve gerçek aygıta sahip olabilir.

## <a name="data-export"></a>Veri dışarı aktarma

Azure IoT Merkezi uygulamasında, [verilerinizi sürekli olarak](howto-export-data.md) kendi Azure Etkinlik Hub'larınıza ve Azure Hizmet Veri Örnekleri örneklerine aktarabilirsiniz. Verilerinizi azure blob depolama hesabınıza düzenli aralıklarla dışa aktarabilirsiniz. IoT Central ölçümleri, aygıtları ve aygıt şablonlarını dışa aktarabilir.

## <a name="batch-device-updates"></a>Toplu aygıt güncellemeleri

Azure IoT Merkezi uygulamasında, bağlı aygıtları yönetmek için [iş oluşturabilir ve çalıştırabilirsiniz.](howto-run-a-job.md) Bu işler, aygıt özelliklerinde veya ayarlarında toplu güncelleştirmeler yapmanıza veya komutçalıştırmanıza izin verir. Örneğin, birden çok soğutulmuş otomat için fan hızını artırmak için bir iş oluşturabilirsiniz.

## <a name="role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC)

Yönetici, önceden tanımlanmış rollerden birini kullanarak veya özel bir rol oluşturarak Azure IoT Merkezi uygulamasının [erişim kurallarını tanımlayabilir.](howto-manage-users-roles.md) Roller, bir kullanıcının uygulamanın hangi alanlarına erişebileceğini ve hangi eylemleri gerçekleştirebileceğini belirler.

## <a name="security"></a>Güvenlik

Azure IoT Central'daki güvenlik özellikleri şunlardır:

- Veriler aktarım sırasında ve istirahatte şifrelenir.
- Kimlik doğrulama, Azure Active Directory veya Microsoft Account tarafından sağlanır. İki faktörlü kimlik doğrulama desteklenir.
- Tam kiracı yalıtımı.
- Aygıt düzeyi güvenliği.

## <a name="ui-shell"></a>UI mermisi

Kullanıcı Bira Birimi kabuğu modern, duyarlı, HTML5 tarayıcı tabanlı bir uygulamadır.
Yönetici, özel temalar uygulayarak ve yardım bağlantılarını kendi özel yardım kaynaklarınızı işaret etmek üzere değiştirerek uygulamanın kullanıcı arasını özelleştirebilir. Kullanıcı Bira Birimi özelleştirmesi hakkında daha fazla bilgi edinmek için Azure [IoT Merkezi Kullanıcı Bira Birimi makalesini özelleştir'e](howto-customize-ui.md) bakın.

Bir operatör kişiselleştirilmiş uygulama panoları oluşturabilir. Farklı verileri görüntüleyen ve aralarında geçiş yapan birkaç panonuz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central mimarisini öğrendiğiniz için önerilen bir sonraki adım, Azure IoT Central'da [aygıt bağlantısı](concepts-get-connected.md) hakkında bilgi edinmektir.
