---
title: IoT hub'ınızın IP adresini anlama | Microsoft Dokümanlar
description: IoT hub IP adresinizi ve özelliklerini nasıl sorgulayacağınızı öğrenin. IoT hub'ınızın IP adresi, olağanüstü durum kurtarma veya bölgesel arıza gibi belirli senaryolar sırasında değişebilir.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: c609f2a3843481442e97061739a806de60a680b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367576"
---
# <a name="iot-hub-ip-addresses"></a>IoT Hub IP adresleri

IoT Hub ortak uç noktalarının IP adresi önekleri _AzureIoTHub_ [hizmet etiketi](../virtual-network/service-tags-overview.md)altında düzenli aralıklarla yayımlanır.

> [!NOTE]
> Şirket içi ağlarda dağıtılan aygıtlar için Azure IoT Hub, Özel uç noktalarla VNET bağlantı tümleştirmesini destekler. Daha fazla bilgi [için VNET'ler için IoT Hub desteğine](./virtual-network-support.md#ingress-connectivity-to-iot-hub-using-private-endpoints) bakın.


Çeşitli ağ yalıtım hedefleri uygulamak için bu IP adresi önekleri, IoT Hub ile aygıtlarınız veya ağ varlıklarınız arasındaki bağlantıyı denetlemek için kullanabilirsiniz:

| Hedef | Uygulanabilir senaryolar | Yaklaşım |
|------|-----------|----------|
| Aygıtlarınızın ve hizmetlerinizin yalnızca IoT Hub uç noktalarıyla iletişim kurmasını sağlayın | [Cihazdan buluta](./iot-hub-devguide-messaging.md)ve [bulutdan cihaza](./iot-hub-devguide-messages-c2d.md) mesajlaşma, [doğrudan yöntemler,](./iot-hub-devguide-direct-methods.md) [cihaz ve modül ikizleri ve](./iot-hub-devguide-device-twins.md) cihaz [akışları](./iot-hub-device-streams-overview.md) | IoT Hub'ı keşfetmek için _AzureIoTHub_ ve _EventHub_ hizmet etiketlerini kullanın ve Event Hub IP adresi önekleri ve bu IP adresi önekleri için aygıtlarınızın ve hizmetlerinizin güvenlik duvarı ayarında ALLOW kurallarını yapılandırın; aygıtların veya hizmetlerin iletişim kurmasını istemediğiniz diğer hedef IP adreslerine trafik bırakın. |
| IoT Hub aygıtınızın uç noktasının yalnızca aygıtlarınızdan ve ağ varlıklarınızdan bağlantı aldığından emin olun | [Cihazdan buluta](./iot-hub-devguide-messaging.md)ve [bulutdan cihaza](./iot-hub-devguide-messages-c2d.md) mesajlaşma, [doğrudan yöntemler,](./iot-hub-devguide-direct-methods.md) [cihaz ve modül ikizleri ve](./iot-hub-devguide-device-twins.md) cihaz [akışları](./iot-hub-device-streams-overview.md) | Aygıtlarınızdan ve ağ varlık IP adreslerinden bağlantılara izin vermek için IoT Hub [IP filtresi özelliğini](iot-hub-ip-filtering.md) kullanın [(bkz. sınırlamalar](#limitations-and-workarounds) bölümü). | 
| Rotalarınızın özel uç nokta kaynaklarına (depolama hesapları, servis veri yolu ve etkinlik hub'ları) yalnızca ağ varlıklarınızdan erişilebilmesini sağlayın | [İleti yönlendirme](./iot-hub-devguide-messages-d2c.md) | Bağlantının kısıtlanması konusunda kaynağınızın kılavuzunu izleyin (örneğin [güvenlik duvarı kuralları,](../storage/common/storage-network-security.md) [özel bağlantılar](../private-link/private-endpoint-overview.md)veya hizmet [bitiş noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)yoluyla); IoT Hub IP adresi önekleri keşfetmek ve kaynağınızın güvenlik duvarı yapılandırmasına bu IP önekleri için IZIN kuralları eklemek için _AzureIoTHub_ hizmet etiketlerini kullanın [(bkz. sınırlamalar](#limitations-and-workarounds) bölümü). |



## <a name="best-practices"></a>En iyi uygulamalar

* Aygıtlarınızın güvenlik duvarı yapılandırmasına ALLOW kuralları eklerken, [ilgili protokoller tarafından kullanılan](./iot-hub-devguide-protocols.md#port-numbers)belirli bağlantı noktalarını sağlamak en iyisidir.

* IoT hub'ının IP adresi önekleri değiştirilebilir. Bu değişiklikler, yürürlüğe girmeden önce hizmet etiketleri aracılığıyla periyodik olarak yayınlanır. Bu nedenle, en son hizmet etiketlerini düzenli olarak almak ve kullanmak için süreçler geliştirmeniz önemlidir. Bu [işlem, servis etiketleri bulma API](../virtual-network/service-tags-overview.md#service-tags-on-premises)üzerinden otomatikolabilir. Hizmet etiketleri bulma API'sinin hala önizlemede olduğunu ve bazı durumlarda etiketlerin ve IP adreslerinin tam listesini oluşturmayabileceğini unutmayın. Bulma API'si genellikle kullanılabilir olana kadar, [hizmet etiketlerini indirilebilir JSON formatında](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)kullanmayı düşünün. 

* *AzureIoTHub'ı kullanın.[ bölge adı]* etiketi, belirli bir bölgedeki IoT hub uç noktaları tarafından kullanılan IP önekleri tanımlamak için kullanılır. Veri merkezi olağanüstü durum kurtarma veya [bölgesel arıza](iot-hub-ha-dr.md) için IoT Hub'ınızın coğrafi çifti bölgesinin IP öneklerine bağlantı nın da etkin olmasını sağlayın.

* IoT Hub'da güvenlik duvarı kurallarının ayarlanması, Azure CLI ve PowerShell komutlarını IoT Hub'ınıza karşı çalıştırmak için gereken bağlantıyı engelleyebilir. Bunu önlemek için, CLI veya PowerShell istemcilerinin IoT Hub'ınızla iletişim kurmasını yeniden etkinleştirmek için müşterilerinizin IP adresi önekleri için ALLOW kuralları ekleyebilirsiniz.  


## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici geçici işler

* IoT Hub IP filtre özelliğinin 10 kuralı vardır. Bu sınır, Azure Müşteri Desteği aracılığıyla istekler yoluyla yükseltilebilir. 

* Yapılandırılmış [IP filtreleme kurallarınız](iot-hub-ip-filtering.md) yalnızca IoT Hub IP uç noktalarınızda uygulanır, IoT hub'ınızın yerleşik Event Hub bitiş noktasında değil. İletilerinizin depolandığı Olay Hub'ında IP filtrelemenin de uygulanmasını istiyorsanız, istediğiniz IP filtreleme kurallarını doğrudan yapılandırabileceğiniz kendi Event Hub kaynağınızı getirebilirsiniz. Bunu yapmak için, kendi Olay Hub kaynağınızı sağlamanız ve iletilerinizi IoT Hub'ınızın yerleşik Olay Hub'ı yerine bu kaynağa göndermek için [ileti yönlendirmesini](./iot-hub-devguide-messages-d2c.md) ayarlamanız gerekir. Son olarak, yukarıdaki tabloda anlatıldığı gibi, ileti yönlendirme işlevini etkinleştirmek için, IoT Hub'ın IP adresi öneklerinden sizin sağlanan Olay Hub kaynağınıza bağlantı izni vermeniz gerekir.

* Bir depolama hesabına yönlendirme yaparken, IoT Hub'ın IP adresi önekleri trafiğine izin vermek yalnızca depolama hesabı IoT Hub'ınız la farklı bir bölgede olduğunda mümkündür.

## <a name="support-for-ipv6"></a>IPv6 için destek 

IPv6 şu anda IoT Hub'da desteklenmez.
