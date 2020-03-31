---
title: CloudSimple tarafından Azure VMware Çözümü - Ağ kontrol listesi
description: CloudSimple tarafından Azure VMware Solution'da ağ CIDR'ı ayırmak için kontrol listesi
author: sharaths-cs
ms.author: dikamath
ms.date: 09/25/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bfb170036293dc9f519259dc92737f30380aa84a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025019"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-cloudsimple"></a>CloudSimple tarafından Azure VMware Çözümü için ağ ön koşulları

CloudSimple tarafından Azure VMware Solution, şirket içi ortamlardan, kurumsal yönetimden ve Azure kaynaklarından kullanıcılar ve uygulamalar için erişilebilir bir VMware özel bulut ortamı sunar. Bağlantı VPN'ler ve Azure ExpressRoute bağlantıları gibi ağ hizmetleri aracılığıyla teslim edilir. Bu ağ hizmetlerinden bazıları, hizmetleri etkinleştirmek için ağ adresi aralıklarını belirtmenizi gerektirir. 

Bu makaledeki tablolar, belirtilen adresleri kullanan adres aralıkları ve ilgili hizmetler kümesini açıklar. Adreslerden bazıları zorunludur ve bazıları dağıtmak istediğiniz hizmetlere bağlıdır. Bu adres alanları şirket içi alt ağlarınızın, Azure Sanal Ağ alt ağlarınızın veya planlanan CloudSimple iş yükü alt ağlarının hiçbiriyle örtüşmemelidir.

## <a name="network-address-ranges-required-for-creating-a-private-cloud"></a>Özel bir bulut oluşturmak için gereken ağ adresi aralıkları

Bir CloudSimple hizmeti ve özel bir bulut oluşturulması sırasında, aşağıdaki gibi belirtilen ağ sınıfsız etki alanı yönlendirme (CIDR) aralıklarına uymanız gerekir.

| Ad/kullanılan     | Açıklama                                                                                                                            | Adres aralığı            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| Ağ Geçidi CIDR      | Kenar hizmetleri (VPN ağ geçitleri) için gereklidir.  Bu CIDR, CloudSimple Service oluşturma sırasında gereklidir ve RFC 1918 alanından olmalıdır. | /28                      |
| vSphere/vSAN CIDR | VMware yönetim ağları için gereklidir. Bu CIDR özel bulut oluşturma sırasında belirtilmelidir.                                    | /24 veya /23 veya /22 veya /21 |

## <a name="network-address-range-required-for-azure-network-connection-to-an-on-premises-network"></a>Şirket içi ağa Azure ağ bağlantısı için gereken ağ adresi aralığı

ExpressRoute [üzerinden şirket içi bir ağdan özel bulut ağına](on-premises-connection.md) bağlanmak, Global Reach bağlantısı kurar.  Bağlantı, şirket içi ağınız, özel bulut ağınız ve Azure ağlarınız arasındaki yolları değiştirmek için Kenarlık Ağ Geçidi Protokolü 'nü (BGP) kullanır.

| Ad/kullanılan             | Açıklama                                                                                                                                                                             | Adres aralığı |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| ExpressRoute Bakan CIDR | Şirket içi bağlantı için ExpressRoute Global Reach'i kullandığınızda gereklidir. Bu CIDR, bir destek bileti aracılığıyla Global Reach bağlantı isteği yapıldığında sağlanmalıdır. | /29           |

## <a name="network-address-range-required-for-using-a-site-to-site-vpn-connection-to-an-on-premises-network"></a>Şirket içi ağa siteden siteye VPN bağlantısı kullanmak için gereken ağ adresi aralığı

[Siteden siteye VPN kullanarak şirket içi bir ağdan özel bulut ağına](vpn-gateway.md) bağlanmak için aşağıdaki IP adresleri, şirket içi ağ ve tanımlayıcılar gerekir. 

| Adres/adres aralığı | Açıklama                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Eş IP               | Şirket içi VPN ağ geçidi genel IP adresi. Şirket içi veri merkezi ile CloudSimple Service bölgesi arasında siteden siteye VPN bağlantısı kurmak için gereklidir. Bu IP adresi, siteden siteye VPN ağ geçidi oluşturma sırasında gereklidir.                                         |
| Eş tanımlayıcısı       | Şirket içi VPN ağ geçidinin eş tanımlayıcısı. Bu genellikle eş **IP**ile aynıdır.  Şirket içi VPN ağ geçidinizde benzersiz bir tanımlayıcı belirtilmişse, tanımlayıcı belirtilmelidir.  Eş kimliği, siteden siteye VPN ağ geçidi oluşturma sırasında gereklidir.   |
| Şirket içi ağlar   | Bölgedeki CloudSimple ağlarına erişmeihtiyacı olan şirket içi önekleri.  Kullanıcıların ağa erişeceği istemci ağı da dahil olmak üzere CloudSimple ağına erişecek şirket içi ağdaki tüm önekleri ekleyin.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Noktadan noktaya VPN bağlantılarını kullanmak için gereken ağ adresi aralığı

Bir noktadan siteye VPN bağlantısı, bir istemci makinesinden CloudSimple ağına erişim sağlar.  [Noktadan siteye VPN'i ayarlamak için](vpn-gateway.md)aşağıdaki ağ adresi aralığını belirtmeniz gerekir.

| Adres/adres aralığı | Açıklama                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İstemci alt ağı         | DHCP adresleri, bir noktadan siteye VPN kullanarak bağlandığınızda istemci alt ağı tarafından sağlanır. CloudSimple portalında noktadan siteye VPN ağ geçidi oluştururken bu alt ağ gereklidir.  Ağ iki alt ağa ayrılır; biri UDP bağlantısı, diğeri tcp bağlantıları için. |

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulutunuza erişmek için şirket içi güvenlik duvarı kurulumu](on-premises-firewall-configuration.md)
* [Quickstart - CloudSimple hizmeti oluşturun](quickstart-create-cloudsimple-service.md)
* [Quickstart- Özel bulutu yapılandırma](quickstart-create-private-cloud.md)
* [Azure ağ bağlantıları](cloudsimple-azure-network-connection.md) hakkında daha fazla bilgi edinin
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md) hakkında daha fazla bilgi edinin
