---
title: Azure VMware çözümleri (AVS)-ağ denetim listesi
description: AVS tarafından Azure VMware çözümünde ağ CıDR 'nin ayrılmasına yönelik denetim listesi
author: sharaths-cs
ms.author: dikamath
ms.date: 09/25/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5cbb0fc0514c17fe34f8e57806e6620cfa8b3f68
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025019"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-avs"></a>AVS tarafından Azure VMware çözümü için ağ önkoşulları

Azure VMware çözümleri (AVS), şirket içi ortamlardan, kurumsal olarak yönetilen cihazlardan ve Azure kaynaklarından kullanıcılar ve uygulamalar için erişilebilen bir VMware özel bulut ortamı sunar. Bağlantı, VPN 'Ler ve Azure ExpressRoute bağlantıları gibi ağ hizmetleri aracılığıyla dağıtılır. Bu ağ hizmetlerinden bazıları, hizmetleri etkinleştirmek için ağ adresi aralıklarını belirtmenizi gerektirir. 

Bu makaledeki tablolarda, belirtilen adresleri kullanan adres aralıkları ve ilgili hizmetler kümesi açıklanır. Adreslerin bazıları zorunludur ve bazıları dağıtmak istediğiniz hizmetlere bağımlıdır. Bu adres alanları, şirket içi alt ağlarınız, Azure sanal ağ alt ağları veya planlı AVS iş yükü alt ağları ile çakışmamalıdır.

## <a name="network-address-ranges-required-for-creating-an-avs-private-cloud"></a>AVS özel bulutu oluşturmak için gereken ağ adresi aralıkları

Bir AVS hizmeti ve bir AVS özel bulutu oluştururken aşağıdaki gibi, belirtilen ağ sınıfsız etki alanları arası yönlendirme (CıDR) aralıklarına uymanız gerekir.

| Ad/kullanım     | Açıklama                                                                                                                            | Adres aralığı            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| Ağ Geçidi CıDR      | Edge Hizmetleri (VPN ağ geçitleri) için gereklidir. Bu CıDR, AVS hizmeti oluşturma sırasında gereklidir ve RFC 1918 alanından olmalıdır. | /28                      |
| vSphere/vSAN CıDR | VMware yönetim ağları için gereklidir. Bu CıDR, AVS özel bulutu oluşturma sırasında belirtilmelidir.                                    | /24 veya/23 veya/22 veya/21 |

## <a name="network-address-range-required-for-azure-network-connection-to-an-on-premises-network"></a>Şirket içi ağa Azure ağ bağlantısı için gereken ağ adresi aralığı

[ExpressRoute aracılığıyla bir şirket içi ağdan AVS özel bulut ağına](on-premises-connection.md) bağlanmak Global Reach bir bağlantı kurar. Bağlantı, şirket içi ağınız, AVS özel bulut ağınız ve Azure ağlarınız arasındaki rotaları Exchange için Sınır Ağ Geçidi Protokolü (BGP) kullanır.

| Ad/kullanım             | Açıklama                                                                                                                                                                             | Adres aralığı |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| ExpressRoute eşleme CıDR | Şirket içi bağlantı için ExpressRoute Global Reach kullandığınızda gereklidir. Bir destek bileti aracılığıyla Global Reach bağlantı isteği yapıldığında bu CıDR sağlanmalıdır. | /29           |

## <a name="network-address-range-required-for-using-a-site-to-site-vpn-connection-to-an-on-premises-network"></a>Şirket içi ağa siteden siteye VPN bağlantısı kullanmak için gereken ağ adresi aralığı

[Siteden sıteye VPN kullanarak şirket içi bir ağdan AVS özel bulut ağına](vpn-gateway.md) bağlanmak IÇIN aşağıdaki IP adresleri, şirket içi ağ ve tanımlayıcılar gerekir. 

| Adres/adres aralığı | Açıklama                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eş IP               | Şirket içi VPN ağ geçidi genel IP adresi. Şirket içi veri merkezi ve AVS hizmet bölgesi arasında siteden siteye VPN bağlantısı kurmak için gereklidir. Siteden siteye VPN ağ geçidi oluşturma işlemi sırasında bu IP adresi gereklidir.                                         |
| Eş tanımlayıcı       | Şirket içi VPN ağ geçidinin eş tanımlayıcısı. Bu genellikle **eş IP**ile aynıdır.  Şirket içi VPN ağ geçidiniz üzerinde benzersiz bir tanımlayıcı belirtilmişse, tanımlayıcı belirtilmelidir.  Siteden siteye VPN ağ geçidi oluşturma sırasında eş KIMLIĞI gereklidir.   |
| Şirket içi ağlar   | Bölgedeki AVS ağlarına erişmesi gereken şirket içi ön ekler.  Kullanıcıların ağa erişebileceği istemci ağı da dahil olmak üzere AVS ağına erişecek şirket içi bir ağdan tüm ön ekleri dahil edin.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Noktadan siteye VPN bağlantıları kullanmak için gereken ağ adresi aralığı

Noktadan siteye VPN bağlantısı, bir istemci makinesinden AVS ağına erişim sağlar. [Noktadan sıteye VPN ayarlamak için](vpn-gateway.md)aşağıdaki ağ adresi aralığını belirtmeniz gerekir.

| Adres/adres aralığı | Açıklama                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İstemci alt ağı         | Noktadan siteye VPN kullanarak bağlandığınızda, DHCP adresleri istemci alt ağı tarafından sağlanır. Bu alt ağ, bir AVS portalında Noktadan siteye VPN ağ geçidi oluştururken gereklidir. Ağ, biri UDP bağlantısı ve diğeri de TCP bağlantıları için olmak üzere iki alt ağa bölünmüştür. |

## <a name="next-steps"></a>Sonraki adımlar

* [AVS özel bulutuna erişim için şirket içi güvenlik duvarı kurulumu](on-premises-firewall-configuration.md)
* [Hızlı başlangıç-bir AVS hizmeti oluşturma](quickstart-create-cloudsimple-service.md)
* [Hızlı başlangıç-AVS özel bulutu yapılandırma](quickstart-create-private-cloud.md)
* [Azure ağ bağlantıları](cloudsimple-azure-network-connection.md) hakkında daha fazla bilgi
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md) hakkında daha fazla bilgi
