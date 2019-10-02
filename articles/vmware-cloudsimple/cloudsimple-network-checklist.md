---
title: CloudSimple-Network denetim listesi ile Azure VMware çözümü
description: CloudSimple tarafından Azure VMware çözümünde ağ CıDR ayırma denetim listesi
author: sharaths-cs
ms.author: dikamath
ms.date: 09/25/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c31942b198697ddc913f3732fd41409334ff0ef6
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817481"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-cloudsimple"></a>CloudSimple tarafından Azure VMware çözümü için ağ önkoşulları

CloudSimple tarafından sunulan Azure VMware çözümü, kurumsal Yönetilen cihazlardan ve Azure kaynaklarından ve şirket içi ortamların kullanıcıları ve uygulamaları için erişilebilen bir VMware özel bulut ortamı sunar. Bağlantı VPN ve ExpressRoute bağlantıları gibi ağ hizmetleri kullanılarak dağıtılır.  Bazı ağ hizmetleri, hizmetleri etkinleştirmek için ağ adresi aralıklarını belirtmenizi gerektirir.  Bu makaledeki tablolarda, belirtilen adresleri kullanan adres aralıkları ve ilgili hizmetler kümesi açıklanır.  Bazı değinilmesi zorunludur ve bazıları dağıtmak istediğiniz hizmetlere bağımlıdır.  Bu adres alanları, şirket içi alt ağlarınız, Azure sanal ağ alt ağları veya planlı CloudSimple iş yükü alt ağları ile çakışmamalıdır.

## <a name="network-address-ranges-required-for-creating-a-private-cloud"></a>Özel bulut oluşturmak için gereken ağ adresi aralıkları

CloudSimple hizmeti ve özel bir bulut oluşturma sırasında, ağ CıDR aralığı şu şekilde gereklidir.

| Ad/kullanım     | Açıklama                                                                                                                            | Adres aralığı            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| Ağ Geçidi CıDR      | Edge Hizmetleri (VPN ağ geçitleri) için gereklidir.  Bu CıDR, CloudSimple hizmeti oluşturma sırasında gereklidir ve RFC 1918 alanından gelmelidir. | /28                      |
| vSphere/vSAN CıDR | VMware yönetim ağları için gereklidir. Bu CıDR, özel bulut oluşturma sırasında belirtilmelidir.                                    | /24 veya/23 veya/22 veya/21 |

## <a name="network-address-range-required-for-azure-network-connection-to-on-premises-network"></a>Şirket içi ağa Azure ağ bağlantısı için gereken ağ adresi aralığı

[ExpressRoute kullanarak şirket içi ağdan özel bulut ağına](on-premises-connection.md) bağlanmak Global Reach bir bağlantı kurar.  Bağlantı, şirket içi ağınız, özel bulut ağınız ve Azure ağlarınız arasında BGP aracılığıyla yollar değiş tokuş eder.

| Ad/kullanım             | Açıklama                                                                                                                                                                             | Adres aralığı |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| ExpressRoute eşleme CıDR | ExpressRoute kullanılırken, şirket içi bağlantı için Global Reach kullanılması gerekir. Bir destek bileti aracılığıyla Global Reach bağlantı isteği yapıldığında, bu CıDR 'nin sağlanması gerekir. | /29           |

## <a name="network-address-range-required-for-using-site-to-site-vpn-connection-to-on-premises-network"></a>Şirket içi ağa siteden siteye VPN bağlantısı kullanmak için gereken ağ adresi aralığı

[Siteden sıteye VPN kullanarak şirket içi ağdan özel bulut ağına](vpn-gateway.md) bağlanmak IÇIN aşağıdaki IP adresleri, şirket içi ağ ve tanımlayıcılar gerekir. 

| Adres/adres aralığı | Açıklama                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eş IP               | Şirket içi VPN ağ geçidi genel IP adresi. Şirket içi veri merkezi ve CloudSimple hizmet bölgesi arasında siteden siteye VPN bağlantısı kurmak için gereklidir. Bu IP, siteden siteye VPN ağ geçidi oluşturma işlemi sırasında gereklidir.                                         |
| Eş tanımlayıcı       | Şirket içi VPN ağ geçidinin eş tanımlayıcısı. Bu genellikle **eş IP**ile aynıdır.  Şirket içi VPN ağ geçidiniz üzerinde benzersiz bir tanımlayıcı belirtilmişse, tanımlayıcının belirtilmesi gerekir.  Siteden siteye VPN Gateway oluşturma sırasında eş KIMLIĞI gereklidir.   |
| Şirket içi ağlar   | Bölgede CloudSimple ağlarına erişmesi gereken şirket içi ön ekler.  Şirket içi ağdan, kullanıcıların erişebileceği istemci ağı da dahil olmak üzere CloudSimple ağına erişecek tüm ön ekleri dahil edin.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Noktadan siteye VPN bağlantıları kullanmak için gereken ağ adresi aralığı

Noktadan siteye VPN bağlantısı, bir istemci makinesinden CloudSimple ağına erişime izin verir.  [Noktadan sıteye VPN 'Nin kurulması](vpn-gateway.md) için aşağıdaki ağ adresi aralığının belirtilmesi gerekir.

| Adres/adres aralığı | Açıklama                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İstemci alt ağı         | Noktadan siteye VPN kullanarak bağlandığınızda, istemci alt ağından DHCP adresleri verilecek. Bu alt ağ, CloudSimple portalında Noktadan siteye VPN ağ geçidi oluştururken gereklidir.  Ağ iki alt ağa bölünür ve bir diğeri UDP bağlantısı ve TCP bağlantıları için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç-CloudSimple hizmeti oluşturma](quickstart-create-cloudsimple-service.md)
* [hızlı başlangıç-oluşturma-özel-bulut](quickstart-create-private-cloud.md)
* [Azure ağ bağlantıları](cloudsimple-azure-network-connection.md) hakkında daha fazla bilgi
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md) hakkında daha fazla bilgi
