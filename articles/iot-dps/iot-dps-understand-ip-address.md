---
title: IoT cihazı sağlama hizmeti (DPS) örneğinizin IP adresini anlama | Microsoft Docs
description: IoT cihaz sağlama hizmeti (DPS) adresinizi ve özelliklerini sorgulamayı öğrenin. DPS örneğinizin IP adresi, olağanüstü durum kurtarma veya bölgesel yük devretme gibi belirli senaryolar sırasında değişebilir.
author: wesmc7777
ms.author: wesmc
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: f6afd5c4cc5aa0215f943979ae91389b39d449f6
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79284934"
---
# <a name="iot-hub-dps-ip-addresses"></a>IoT Hub DPS IP adresleri

IoT Hub cihaz sağlama hizmeti 'nin (DPS) genel uç noktaları için IP adresi önekleri, _AzureIoTHub_ [hizmet etiketi](../virtual-network/service-tags-overview.md)altında düzenli aralıklarla yayımlanır. Çeşitli ağ yalıtımı hedefleri uygulamak için IoT DPS örneği ile cihazları veya Ağ varlıkları arasındaki bağlantıyı denetlemek üzere bu IP adresi öneklerini kullanabilirsiniz:

| Hedef | Yaklaşım |
|------|----------|
| Cihazlarınızın ve hizmetlerinizin yalnızca IoT Hub DPS uç noktaları ile iletişim kurabildiğinden emin olun | IoT Hub DPS örneklerini saptamak için _AzureIoTHub_ Service etiketini kullanın. Bu IP adresi önekleri için cihazlarınızda ' ve hizmetler ' güvenlik duvarı ayarında kurallara ızın ver ' i yapılandırın. Cihazların veya hizmetlerin iletişim kurmasını istemediğiniz diğer hedef IP adreslerine trafik bırakma kurallarını yapılandırın. |
| IoT Hub DPS uç noktanızın yalnızca cihazlarınızdan ve ağ varlıklarınızdan bağlantı aldığından emin olun | Cihaz ve DPS hizmet API 'Leri için filtre kuralları oluşturmak üzere IoT DPS [IP filtresi özelliğini](iot-dps-ip-filtering.md) kullanın. Bu filtre kuralları yalnızca cihazlarınızdan ve ağ varlığı IP adreslerinden gelen bağlantılara izin vermek için kullanılabilir (bkz. [sınırlamalar](#limitations-and-workarounds) bölümü). | 




## <a name="best-practices"></a>En iyi uygulamalar

* Cihazlarınızda ızın verme kuralları eklerken, [uygun protokoller tarafından kullanılan belirli bağlantı noktalarını](../iot-hub/iot-hub-devguide-protocols.md#port-numbers)sağlamak en iyisidir.

* IoT DPS örneklerinin IP adresi ön ekleri değişikliğe tabidir. Bu değişiklikler, etkin olmadan önce hizmet etiketleri aracılığıyla düzenli olarak yayımlanır. Bu nedenle, en son hizmet etiketlerini düzenli olarak almak ve kullanmak için işlem geliştirmeniz önemlidir. Bu işlem, [hizmet etiketleri bulma API 'si](../virtual-network/service-tags-overview.md#service-tags-on-premises)aracılığıyla otomatikleştirilebilir. Hizmet etiketleri bulma API 'SI hala önizlemededir ve bazı durumlarda etiketlerin ve IP adreslerinin tam listesini oluşturmayabilir. Keşif API 'SI genel kullanıma hazır olana kadar, [INDIRILEBILIR JSON biçiminde hizmet etiketlerini](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)kullanmayı göz önünde bulundurun. 

* AzureIoTHub kullanın *. [ bölge adı]* etiketi, belirli bir BÖLGEDEKI DPS uç noktaları tarafından kullanılan IP öneklerini belirler. Veri merkezi olağanüstü durum kurtarmayı veya [bölgesel yük devretmeyi](../iot-hub/iot-hub-ha-dr.md)hesaba eklemek IÇIN, DPS örneğinizin coğrafi ÇIFT bölgesinin IP öneklerine bağlantısının de etkinleştirildiğinden emin olun.

* Bir DPS örneği için güvenlik duvarı kuralları ayarlamak, Azure CLı ve PowerShell komutlarını çalıştırmak için gereken bağlantıyı engelleyebilir. Bu bağlantı sorunlarından kaçınmak için, CLı veya PowerShell istemcilerinin DPS örneğiniz ile iletişim kurmasını sağlamak üzere istemcilerinizin IP adresi öneklerine ızın verme kuralları ekleyebilirsiniz.  


## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici çözümler

* DPS IP Filtresi özelliği 100 kurallara sınırlıdır. Bu sınır, Azure müşteri desteği aracılığıyla istekler aracılığıyla oluşturulabilir. 

* Yapılandırılmış [IP filtreleme kurallarınız](iot-dps-ip-filtering.md) , bağlı IoT Hub uç noktalarında değil, yalnızca DPS uç noktalarınıza uygulanır. Bağlı IoT Hub 'Ları için IP filtrelemesi ayrı olarak yapılandırılmalıdır. Daha fazla bilgi için bkz. [IP filtreleme kuralları IoT Hub](../iot-hub/iot-hub-ip-filtering.md).

## <a name="support-for-ipv6"></a>IPv6 desteği 

IPv6, IoT Hub veya DPS üzerinde şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

DPS IP adresi konfigürasyonları hakkında daha fazla bilgi edinmek için bkz.:

* [IP filtrelemeyi yapılandırma](iot-dps-ip-filtering.md)
