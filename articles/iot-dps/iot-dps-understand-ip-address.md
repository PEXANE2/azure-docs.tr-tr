---
title: IoT Cihaz Sağlama Hizmeti (DPS) örneğinizin IP adresini anlama | Microsoft Dokümanlar
description: IoT Aygıt Sağlama Hizmeti (DPS) adresinizi ve özelliklerini nasıl sorgulayacağınızı öğrenin. DPS örneğinizin IP adresi, olağanüstü durum kurtarma veya bölgesel arıza gibi belirli senaryolar sırasında değişebilir.
author: wesmc7777
ms.author: wesmc
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: f6afd5c4cc5aa0215f943979ae91389b39d449f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284934"
---
# <a name="iot-hub-dps-ip-addresses"></a>IoT Hub DPS IP adresleri

Bir IoT Hub Aygıt Sağlama Hizmetinin (DPS) ortak uç noktalarına ait IP adresi önekleri _AzureIoTHub_ [hizmet etiketi](../virtual-network/service-tags-overview.md)altında düzenli aralıklarla yayımlanır. Çeşitli ağ yalıtım hedefleri uygulamak için bu IP adresi önekleri bir IoT DPS örneği ile aygıtlar veya ağ varlıkları arasındaki bağlantıyı denetlemek için kullanabilirsiniz:

| Hedef | Yaklaşım |
|------|----------|
| Aygıtlarınızın ve hizmetlerinizin yalnızca IoT Hub DPS uç noktalarıyla iletişim kurmasını sağlayın | IoT Hub DPS örneklerini keşfetmek için _AzureIoTHub_ hizmet etiketini kullanın. Bu IP adresi önekleri için aygıtlarınızın ve hizmetlerinizin güvenlik duvarı ayarınındaki ALLOW kurallarını yapılandırın. Aygıtların veya hizmetlerin iletişim kurmasını istemediğiniz diğer hedef IP adreslerine trafiği bırakacak kuralları yapılandırın. |
| IoT Hub DPS uç noktanızın yalnızca aygıtlarınızdan ve ağ varlıklarınızdan bağlantı aldığından emin olun | Aygıt ve DPS servis API'leri için filtre kuralları oluşturmak için IoT DPS [IP filtresi özelliğini](iot-dps-ip-filtering.md) kullanın. Bu filtre kuralları yalnızca aygıtlarınızdan ve ağ varlık IP adreslerinden gelen bağlantılara izin vermek için kullanılabilir [(bkz. sınırlamalar](#limitations-and-workarounds) bölümü). | 




## <a name="best-practices"></a>En iyi uygulamalar

* Aygıtlarınızın güvenlik duvarı yapılandırmasına ALLOW kuralları eklerken, [ilgili protokoller tarafından kullanılan](../iot-hub/iot-hub-devguide-protocols.md#port-numbers)belirli bağlantı noktalarını sağlamak en iyisidir.

* IoT DPS örneklerinin IP adresi önekleri değişebilir. Bu değişiklikler, yürürlüğe girmeden önce hizmet etiketleri aracılığıyla periyodik olarak yayınlanır. Bu nedenle, en son hizmet etiketlerini düzenli olarak almak ve kullanmak için süreçler geliştirmeniz önemlidir. Bu [işlem, servis etiketleri bulma API](../virtual-network/service-tags-overview.md#service-tags-on-premises)üzerinden otomatikolabilir. Hizmet etiketleri bulma API hala önizleme ve bazı durumlarda etiketleri ve IP adreslerinin tam listesini üretmek olmayabilir. Bulma API'si genellikle kullanılabilir olana kadar, [hizmet etiketlerini indirilebilir JSON formatında](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)kullanmayı düşünün. 

* *AzureIoTHub'ı kullanın.[ bölge adı]* etiketi, dps uç noktaları tarafından belirli bir bölgede kullanılan IP önekleri tanımlamak için. Veri merkezi olağanüstü durum kurtarma veya [bölgesel arıza](../iot-hub/iot-hub-ha-dr.md)için hesap vermek için, DPS örneğinizin coğrafi çifti bölgesinin IP öneklerine bağlantının da etkin olduğundan emin olun.

* BIR DPS örneği için güvenlik duvarı kurallarının ayarlanması, Azure CLI ve PowerShell komutlarını buna karşı çalıştırmak için gereken bağlantıyı engelleyebilir. Bu bağlantı sorunlarını önlemek için, CLI veya PowerShell istemcilerinin DPS örneğinizle iletişim kurmasını yeniden etkinleştirmek için müşterilerinizin IP adresi önekleri için ALLOW kuralları ekleyebilirsiniz.  


## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici geçici işler

* DPS IP filtre özelliğinin 100 kuralı vardır. Bu sınır, Azure Müşteri Desteği aracılığıyla istekler yoluyla yükseltilebilir. 

* Yapılandırılmış [IP filtreleme kurallarınız](iot-dps-ip-filtering.md) yalnızca BAĞLı IoT Hub uç noktalarınızda değil, yalnızca DPS uç noktalarınızda uygulanır. Bağlantılı IoT Hub'ları için IP filtreleme ayrı olarak yapılandırılmalıdır. Daha fazla bilgi için bkz: [IoT Hub IP filtreleme kuralları.](../iot-hub/iot-hub-ip-filtering.md)

## <a name="support-for-ipv6"></a>IPv6 için destek 

IPv6 şu anda IoT Hub veya DPS'de desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

DPS ile IP adresi yapılandırmaları hakkında daha fazla bilgi edinmek için bkz:

* [IP filtrelemeyi yapılandırma](iot-dps-ip-filtering.md)
