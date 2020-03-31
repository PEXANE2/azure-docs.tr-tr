---
title: Azure IoT Hub Aygıt Sağlama Hizmeti - Aygıt kavramları
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için aygıt yeniden sağlama kavramlarını açıklar
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 2bf369b784cddf307abc59d2b8766fc8a87e0985
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975355"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>IoT Hub Aygıt yeniden sağlama kavramları

Bir IoT çözümünün kullanım ömrü sırasında aygıtları IoT hub'ları arasında taşımak yaygındır. Bu hareketin nedenleri aşağıdaki senaryoları içerebilir:

* **Coğrafi Konum / GeoLatency**: Bir aygıt konumlar arasında hareket ettikçe, aygıtın daha yakın bir IoT hub'ına geçirilmesiyle ağ gecikmesi artar.

* **Çoklu kiralama**: Bir cihaz aynı IoT çözümü içinde kullanılabilir ve yeni bir müşteriye veya müşteri sitesine yeniden atanabilir. Bu yeni müşteriye farklı bir IoT hub'ı kullanılarak hizmet verilebilir.

* **Çözüm değişikliği**: Bir aygıt yeni veya güncelleştirilmiş bir IoT çözümüne taşınabilir. Bu yeniden atama, aygıtın diğer arka uç bileşenlerine bağlı yeni bir IoT hub'ı ile iletişim kurmasını gerektirebilir.

* **Karantina**: Çözelti değişikliğine benzer. Arızalı, gizliliği ihlal edilen veya güncelliğini yiyiyen bir aygıt, yalnızca güncelleştirebilen ve uyumluluğu geri alabilen bir IoT hub'ına yeniden atanabilir. Aygıt düzgün çalıştığında, ana hub'ına geri taşınır.

Cihaz Sağlama Hizmeti içindeki desteğin yeniden sağlanması bu gereksinimleri karşılar. Aygıtlar, aygıtın kayıt girişinde yapılandırılan yeniden sağlama ilkesine dayalı olarak otomatik olarak yeni IoT hub'larına yeniden atanabilir.

## <a name="device-state-data"></a>Aygıt durumu verileri

Aygıt durumu verileri, [aygıt ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve aygıt özelliklerinden oluşur. Bu veriler Aygıt Sağlama Hizmeti örneğinde ve aygıtın atandığı IoT hub'ında depolanır.

![Cihaz Sağlama Hizmeti ile Sağlama](./media/concepts-device-reprovisioning/dps-provisioning.png)

Bir aygıt başlangıçta bir Aygıt Sağlama Hizmeti örneğiyle birlikte sağlandığında, aşağıdaki adımlar yapılır:

1. Aygıt, aygıt sağlama hizmeti örneğine bir sağlama isteği gönderir. Hizmet örneği, bir kayıt girişine göre aygıt kimliğini doğrular ve aygıt durumu verilerinin ilk yapılandırmasını oluşturur. Hizmet örneği, aygıtı kayıt yapılandırmasına göre bir IoT hub'ına atar ve bu IoT hub atamasını aygıta döndürür.

2. Sağlama hizmeti örneği, atanan IoT hub'ına herhangi bir ilk aygıt durumu verilerinin bir kopyasını verir. Aygıt atanan IoT hub'ına bağlanır ve çalışmaya başlar.

Zaman içinde, IoT hub'ındaki aygıt durumu verileri [aygıt işlemleri](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) ve arka [uç işlemleri](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations)yle güncellenebilir. Aygıt Sağlama Hizmeti örneğinde depolanan ilk aygıt durumu bilgilerine dokunulmamış olarak kalır. Bu el değmemiş aygıt durumu verileri ilk yapılandırmadır.

![Cihaz Sağlama Hizmeti ile Sağlama](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

Senaryoya bağlı olarak, aygıt IoT hub'ları arasında hareket ettikçe, önceki IoT hub'ında güncelleştirilen aygıt durumunu yeni IoT hub'ına geçirmek de gerekebilir. Bu geçiş, Aygıt Sağlama Hizmeti'ndeki ilkeleri yeniden sağlayarak desteklenir.

## <a name="reprovisioning-policies"></a>Politikaların yeniden sağlanması

Senaryoya bağlı olarak, aygıt genellikle yeniden başlatılıyorum bir sağlama hizmeti örneğine istek gönderir. Ayrıca, isteğe bağlı olarak sağlamayı el ile tetikleyen bir yöntemi de destekler. Kayıt girişiyle ilgili yeniden sağlama ilkesi, aygıt sağlama hizmeti örneğinin bu sağlama isteklerini nasıl işleyeceğini belirler. İlke, aygıt durumu verilerinin yeniden sağlama sırasında geçirilip geçirilmeyeceğini de belirler. Aynı ilkeler tek tek kayıt ve kayıt grupları için kullanılabilir:

* **Verileri yeniden sağlama ve geçirme**: Bu ilke, yeni kayıt girişleri için varsayılan dır. Bu ilke, kayıt girişiyle ilişkili aygıtlar yeni bir istek gönderdiğinde harekete geçer (1). Kayıt girişi yapılandırmasına bağlı olarak, aygıt başka bir IoT hub'ına yeniden atanabilir. Aygıt IoT hub'larını değiştiriyorsa, ilk IoT hub'ına sahip aygıt kaydı kaldırılır. Bu ilk IoT hub'ından güncelleştirilmiş aygıt durumu bilgileri yeni IoT hub'ına geçirilir (2). Geçiş sırasında, aygıtın durumu **Atama**olarak bildirilecektir.

    ![Cihaz Sağlama Hizmeti ile Sağlama](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)

* **Yeniden sağlama ve ilk config'e sıfırlama**: Bu ilke, kayıt girişiyle ilişkili aygıtlar yeni bir sağlama isteği gönderdiğinde harekete geçer (1). Kayıt girişi yapılandırmasına bağlı olarak, aygıt başka bir IoT hub'ına yeniden atanabilir. Aygıt IoT hub'larını değiştiriyorsa, ilk IoT hub'ına sahip aygıt kaydı kaldırılır. Aygıt sağlandığında sağlama hizmeti örneğinin aldığı ilk yapılandırma verileri yeni IoT hub'ına sağlanır (2). Geçiş sırasında, aygıtın durumu **Atama**olarak bildirilecektir.

    Bu ilke genellikle IoT hub'larını değiştirmeden fabrika sıfırlamaiçin kullanılır.

    ![Cihaz Sağlama Hizmeti ile Sağlama](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)

* **Asla yeniden sağlama**: Aygıt hiçbir zaman farklı bir hub'a yeniden atanmaz. Bu ilke, geriye dönük uyumluluğu yönetmek için sağlanır.

### <a name="managing-backwards-compatibility"></a>Geriye dönük uyumluluğu yönetme

Eylül 2018'den önce, IoT hub'larına yapılan aygıt atamalarında yapışkan bir davranış vardı. Bir aygıt sağlama işleminden geri döndüğünde, yalnızca aynı IoT hub'ına geri atanır.

Bu davranışa bağımlı olan çözümler için, sağlama hizmeti geriye dönük uyumluluğu içerir. Bu davranış şu anda aygıtlar için aşağıdaki ölçütlere göre korunur:

1. Aygıtlar, Aygıt Sağlama Hizmeti'nde yerel yeniden sağlama desteğinin kullanılabilirliğinden önce bir API sürümüne bağlanır. Aşağıdaki API tablosuna bakın.

2. Aygıtların kayıt girişinin üzerinde yeniden sağlama ilkesi yoktur.

Bu uyumluluk, daha önce dağıtılan aygıtların ilk sınama sırasında mevcut olan aynı davranışı yaşamasını sağlar. Önceki davranışı korumak için, bu kayıtlara yeniden sağlama ilkesi kaydetmeyin. Yeniden sağlama ilkesi ayarlanırsa, yeniden sağlama ilkesi davranışa göre önceliklidir. Yeniden sağlama ilkesinin öncelikli olmasına izin vererek, müşteriler aygıtı yeniden görüntülemek zorunda kalmadan aygıt davranışını güncelleyebilir.

Aşağıdaki akış şeması, davranışın ne zaman mevcut olduğunu göstermeye yardımcı olur:

![geriye dönük uyumluluk akış şeması](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

Aşağıdaki tablo, Aygıt Sağlama Hizmeti'nde yerel yeniden sağlama desteğinin kullanılabilirliğinden önce API sürümlerini gösterir:

| REST API | C SDK | Python SDK'sı |  Node SDK | Java SDK | .NET SDK |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 ve öncesi](/rest/api/iot-dps/createorupdateindividualenrollment/createorupdateindividualenrollment#uri-parameters) | [1.2.8 ve daha önceki](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 ve daha önceki](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 veya daha önceki](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 veya daha önceki](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 veya daha önceki](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Bu değerler ve bağlantılar değişebilir. Bu, yalnızca sürümlerin müşteri tarafından nerede belirlenebileceğini ve beklenen sürümlerin ne olacağını belirlemek için bir yer tutucu girişimidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Aygıtları yeniden sağlama](how-to-reprovision.md)
