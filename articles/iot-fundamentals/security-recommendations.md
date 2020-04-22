---
title: Azure IoT için güvenlik önerileri | Microsoft Dokümanlar
description: Bu makalede, Azure IoT Hub çözümünüzde güvenliği sağlamak için ek adımlar özetlenmiştir.
author: dsk-2015
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: article
ms.date: 11/13/2019
ms.author: dkshir
ms.custom:
- security-recommendations
- amqp
- mqtt
ms.openlocfilehash: 5dd09988d37982c41b761688492bd2dc3642b2db
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81728990"
---
# <a name="security-recommendations-for-azure-internet-of-things-iot-deployment"></a>Azure Nesnelerin İnterneti (IoT) dağıtımı için güvenlik önerileri

Bu makale, IoT için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelimizde açıklandığı gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olacaktır. Microsoft'un hizmet sağlayıcı sorumluluklarını yerine getirmek için ne yaptığı hakkında daha fazla bilgi için [bulut bilgi işlem için Paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)okuyun.

Bu makalede yer alan önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure'daki kaynaklarınızı korumada ilk savunma hattıdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli aralıklarla analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar.

- Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Güvenlik önerilerine](../security-center/security-center-recommendations.md)bakın.
- Azure Güvenlik Merkezi hakkında bilgi için [Azure Güvenlik Merkezi nedir'e bakın?](../security-center/security-center-intro.md)

## <a name="general"></a>Genel

| Öneri | Yorumlar | ASC tarafından desteklendi |
|-|----|--|
| Güncel kalın | Desteklenen platformların, programlama dillerinin, protokollerin ve çerçevelerin en son sürümlerini kullanın. | - |
| Kimlik doğrulama anahtarlarını güvende tutun | Aygıt kimliklerini ve kimlik doğrulama anahtarlarını dağıtımdan sonra fiziksel olarak güvende tutun. Bu, kötü amaçlı bir aygıtın kayıtlı bir aygıt olarak maskelemesi önlenir. | - |
| Mümkün olduğunda cihaz SDK'larını kullanma | Aygıt SDK'ları, sağlam ve güvenli bir aygıt uygulaması geliştirmenize yardımcı olmak için şifreleme, kimlik doğrulama ve benzeri çeşitli güvenlik özellikleri uygular. Bkz. Daha fazla bilgi için [Azure IoT Hub SDK'larını anlayın ve kullanın.](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) | - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi 

| Öneri | Yorumlar | ASC tarafından desteklendi |
|-|----|--|
| Hub için erişim denetimini tanımlama | İşlevselliği temel alan, her bileşenin IoT Hub çözümünde sahip olacağı [erişim türünü anlayın ve tanımlayın.](iot-security-deployment.md#securing-the-cloud) İzin verilen izinler *Kayıt Defteri Okuma, Kayıt* *DefteriReadWrite*, *ServiceConnect*ve *DeviceConnect*vardır. [IoT hub'ınızdaki](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#access-control-and-permissions) varsayılan paylaşılan erişim ilkeleri, rolüne bağlı olarak her bileşeniçin izinlerin tanımlanmasına da yardımcı olabilir. | - |
| Arka uç hizmetleri için erişim denetimini tanımlama | IoT Hub çözümünüz tarafından alınan veriler [Cosmos DB,](https://docs.microsoft.com/azure/cosmos-db/) [Stream Analytics,](https://docs.microsoft.com/azure/stream-analytics/) [App Service,](https://docs.microsoft.com/azure/app-service/) [Logic Apps](https://docs.microsoft.com/azure/logic-apps/)ve [Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)gibi diğer Azure hizmetleri tarafından tüketilebilir. Bu hizmetler için belgelenmiş olarak uygun erişim izinlerini anladığınızdan ve izin verdiğinden emin olun. | - |

## <a name="data-protection"></a>Veri koruma

| Öneri | Yorumlar | ASC tarafından desteklendi |
|-|----|--|
| Güvenli aygıt kimlik doğrulaması | [Benzersiz bir kimlik anahtarı veya güvenlik belirteci veya](iot-security-deployment.md#iot-hub-security-tokens)her aygıt için cihaz [içi X.509 sertifikası](iot-security-deployment.md#x509-certificate-based-device-authentication) kullanarak aygıtlarınız ve IoT hub'ınız arasında güvenli iletişim sağlayın. [Seçilen protokole (MQTT, AMQP veya HTTPS) dayalı güvenlik belirteçlerini kullanmak](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security)için uygun yöntemi kullanın. | - |
| Güvenli cihaz iletişimi | IoT Hub, 1.2 ve 1.0 sürümlerini destekleyerek Aktarım Katmanı Güvenliği (TLS) standardını kullanarak aygıtlara bağlantı sağlar. Maksimum güvenlik sağlamak için [TLS 1.2'yi](https://tools.ietf.org/html/rfc5246) kullanın. | - |
| Güvenli servis iletişimi | IoT Hub, yalnızca TLS iletişim kuralını kullanarak [Azure Depolama](/azure/storage/) veya [Etkinlik Hub'ları](/azure/event-hubs) gibi arka uç hizmetlerine bağlanmak için uç noktalar sağlar ve şifrelenmemiş bir kanalda hiçbir uç nokta açıklanır. Bu veriler depolama veya analiz için bu arka uç hizmetlerine ulaştığında, bu hizmet için uygun güvenlik ve şifreleme yöntemlerini kullandığından ve hassas bilgileri arka uçta koruduğundan emin olun. | - |

## <a name="networking"></a>Ağ

| Öneri | Yorumlar | ASC tarafından desteklendi |
|-|----|--|
| Aygıtlarınıza erişimi koruyun | İstenmeyen erişimi önlemek için aygıtlarınızdaki donanım bağlantı noktalarını en aza indirir. Ayrıca, aygıtın fiziksel kurcalanmasını önlemek veya algılamak için mekanizmalar oluşturun. Ayrıntılar için [IoT güvenlik en iyi uygulamaları](iot-security-best-practices.md) okuyun. | - |
| Güvenli donanım oluşturma | Aygıtları ve altyapıyı daha güvenli tutmak için şifreli depolama veya Güvenilir Platform Modülü (TPM) gibi güvenlik özelliklerini birleştirin. Aygıt işletim sistemini ve sürücülerini en son sürümlere yükseltilmiş tutun ve alan izin verirse virüsten koruma ve kötü amaçlı yazılımdan koruma özelliklerini yükleyin. Bunun çeşitli güvenlik tehditlerini azaltmaya nasıl yardımcı olabileceğini anlamak için [IoT güvenlik mimarisini](iot-security-architecture.md) okuyun. | - |

## <a name="monitoring"></a>İzleme

| Öneri | Yorumlar | ASC tarafından desteklendi |
|-|----|--|
| Aygıtlarınıza yetkisiz erişimi izleme |  Aygıtınızın veya bağlantı noktalarının güvenlik ihlallerini veya fiziksel kurcamasını izlemek için cihazınızın işletim sisteminin günlük özelliğini kullanın. | - |
| IoT çözümünüzü buluttan izleme | [Azure Monitor'daki ölçümleri](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics)kullanarak IoT Hub çözümünüzün genel durumunu izleyin. | - |
| Tanılamayı ayarlama | Çözümünüzdeki olayları günlüğe kaydederek ve performansta görünürlük elde etmek için tanılama günlüklerini Azure Monitor'a göndererek işlemlerinizi yakından izleyin. Daha fazla bilgi için [IoT hub'ınızdaki Monitör'ü](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health) okuyun ve sorunları tanıkoyun. | - |

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT içeren gelişmiş senaryolar için ek güvenlik gereksinimlerini göz önünde bulundurmanız gerekebilir. Daha fazla kılavuz için [IoT güvenlik mimarisine](iot-security-architecture.md) bakın.

