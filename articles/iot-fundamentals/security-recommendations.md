---
title: Azure IoT için güvenlik önerileri | Microsoft Docs
description: Bu makalede, Azure IoT Hub çözümünüzde güvenliği sağlamak için ek adımlar özetlenmektedir.
author: dsk-2015
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dkshir
ms.custom: security-recommendations
ms.openlocfilehash: 55ca189d5f4622a395ffe603d7f0d6764db82f3d
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877216"
---
# <a name="security-recommendations-for-azure-internet-of-things-iot-deployment"></a>Azure Nesnelerin İnterneti (IoT) dağıtımı için güvenlik önerileri

Bu makale, Azure IoT hizmetlerine yönelik güvenlik önerilerini içerir. Bu önerilerin uygulanması, Azure IoT 'nin bir müşterisi olarak güvenlik yükümlülüklerinizi karşılamanız ve IoT çözümlerinizin genel güvenliğini iyileştirmenize yardımcı olur. Azure IoT tarafından sunulan içsel güvenlik özellikleri hakkında daha fazla bilgi için, [sıfırdan IoT güvenliği](iot-security-ground-up.md)makalesini okuyun.

## <a name="general"></a>Genel

| Öneri | Açıklamalar |
|-|-|
| Güncel kalın | Desteklenen platformlar, programlama dilleri, protokoller ve çerçeveler için en son sürümleri kullanın. |
| Kimlik doğrulama anahtarlarını güvenli tut | Dağıtımdan sonra cihaz kimliklerini ve kimlik doğrulama anahtarlarını fiziksel olarak güvende tutun. Bu, kayıtlı bir cihaz olarak kötü amaçlı cihaz maskeli olarak bir maske oluşmasını önler. |
| Mümkün olduğunda cihaz SDK 'larını kullan | Cihaz SDK 'Ları, güçlü ve güvenli bir cihaz uygulaması geliştirmeye yardımcı olmak için, şifreleme, kimlik doğrulama vb. gibi çeşitli güvenlik özellikleri uygular. Daha fazla bilgi için bkz. [Azure IoT Hub SDK 'Larını anlama ve kullanma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) . |


## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Açıklamalar |
|-|-|
| Hub için erişim denetimi tanımlama | İşlevlere göre her bir bileşenin IoT Hub çözümünüzde sahip olacağı [erişim türünü anlayın ve tanımlayın](iot-security-deployment.md#securing-the-cloud) . İzin verilen izinler *kayıt defteri okuma*, *registryreadwrite*, *serviceconnect*ve *deviceconnect*' dir. [IoT Hub 'ınızdaki varsayılan paylaşılan erişim ilkeleri](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#access-control-and-permissions) , rolüne göre her bir bileşen için izinleri tanımlamaya da yardımcı olabilir. |
| Arka uç hizmetleri için erişim denetimi tanımlama | IoT Hub çözümünüz tarafından alınan veriler [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/), [Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/), [App Service](https://docs.microsoft.com/azure/app-service/), [Logic Apps](https://docs.microsoft.com/azure/logic-apps/)ve [BLOB depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)gibi diğer Azure hizmetleri tarafından tüketilebilir. Bu hizmetler için belgelendiği şekilde, uygun erişim izinlerini anladığınızdan ve bunlara izin verdiğinizden emin olun. |


## <a name="data-protection"></a>Veri koruma

| Öneri | Açıklamalar |
|-|-|
| Güvenli cihaz kimlik doğrulaması | Her bir cihaz için [benzersiz bir kimlik anahtarı veya güvenlik belirteci](iot-security-deployment.md#iot-hub-security-tokens)ya da bir [cihaz içi X. 509.440 Sertifikası](iot-security-deployment.md#x509-certificate-based-device-authentication) kullanarak cihazlarınız ve IoT Hub 'ınız arasında güvenli iletişimin sağlanmasına emin olun. [Seçilen protokole (MQTT, AMQP veya https) göre güvenlik belirteçlerini kullanmak](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security)için uygun yöntemi kullanın. |
| Güvenli cihaz iletişimi | IoT Hub, 1,2 ve 1,0 sürümlerini destekleyen Aktarım Katmanı Güvenliği (TLS) standardını kullanarak cihazlara bağlantı sağlar. En yüksek güvenliği sağlamak için [TLS 1,2](https://tools.ietf.org/html/rfc5246) kullanın. |
| Güvenli hizmet iletişimi | IoT Hub, [Azure depolama](/azure/storage/) veya yalnızca TLS protokolünü kullanarak [Event Hubs](/azure/event-hubs) arka uç hizmetlerine bağlanmak için uç noktalar sağlar ve şifrelenmemiş bir kanalda hiçbir uç nokta gösterilmez. Bu veriler depolama veya analiz için bu arka uç hizmetlerine ulaştıktan sonra, bu hizmet için uygun güvenlik ve şifreleme yöntemleri ve arka uçta hassas bilgileri koruyun. |


## <a name="networking"></a>Ağ

| Öneri | Açıklamalar |
|-|-|
| Cihazlarınıza erişimi koruma | İstenmeyen erişimleri önlemek için cihazlarınızda donanım bağlantı noktalarını en düşük düzeyde tutun. Ayrıca, cihazın fiziksel olarak değiştirilmesini önlemeye veya algılamaya yönelik mekanizmalar oluşturun. Ayrıntılar için [IoT güvenlik en iyi yöntemlerini](iot-security-best-practices.md) okuyun. |
| Güvenli donanım oluşturma | Cihazları ve altyapıyı daha güvenli tutmak için şifrelenmiş depolama veya Güvenilir Platform Modülü (TPM) gibi güvenlik özellikleri ekleyin. Cihaz işletim sistemi ve sürücülerini en son sürümlere yükseltmez ve alan izin veriyorsa virüsten koruma ve kötü amaçlı yazılımdan koruma özellikleri yükler. Bunun çeşitli güvenlik tehditlerini azaltmaya nasıl yardımcı olduğunu anlamak için [IoT güvenlik mimarisini](iot-security-architecture.md) okuyun. |


## <a name="monitoring"></a>İzleme

| Öneri | Açıklamalar |
|-|-|
| Cihazlarınıza yetkisiz erişimi izleme |  Cihazın veya bağlantı noktalarından herhangi bir güvenlik ihlallerinin veya fiziksel olarak değiştirilmesini izlemek için cihazınızın işletim sisteminizin günlüğe kaydetme özelliğini kullanın. |
| IoT çözümünüzü buluttan izleyin | [Azure izleyici 'de ölçümleri](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics)kullanarak IoT Hub çözümünüzün genel durumunu izleyin. |
| Tanılamayı ayarla | Çözümünüzdeki olayları günlüğe kaydederek ve daha sonra performansı Izlemek için tanılama günlüklerini Azure Izleyici 'ye göndererek işlemlerinizi yakından izleyin. Daha fazla bilgi için [, IoT Hub 'ınızdaki sorunları okuyun ve tanılayın](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health) . |

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT ile ilgili gelişmiş senaryolar için ek güvenlik gereksinimlerini dikkate almanız gerekebilir. Daha fazla bilgi için bkz. [IoT güvenlik mimarisi](iot-security-architecture.md) .

