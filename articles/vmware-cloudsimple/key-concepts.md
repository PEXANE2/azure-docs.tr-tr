---
title: Azure VMware çözümlerini (AVS) yönetmeye yönelik temel kavramlar
description: Azure VMware çözümlerini (AVS) yönetmeye yönelik temel kavramları açıklar
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c581ffbba7fbaefc8fbd0ffc7e92350bd69802b8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025240"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solutions-avs"></a>Azure VMware çözümlerini (AVS) yönetmek için temel kavramlar

Azure VMware çözümlerini (AVS) yönetmek için aşağıdaki kavramların anlaşılmasına gerek vardır:

* Azure VMware Solutions (AVS)-Service olarak görünen AVS hizmeti
* Azure VMware çözümleri (AVS) olarak görünen AVS düğümü-düğüm
* AVS özel bulutu
* Hizmet ağı
* Azure VMware çözümleri (AVS)-sanal makine olarak görünen AVS sanal makinesi

## <a name="avs-service"></a>AVS hizmeti

AVS hizmeti ile Azure portal VMware çözümleri (AVS) ile ilişkili tüm kaynakları oluşturabilir ve yönetebilirsiniz. Hizmeti kullanmayı düşündüğünüz her bölgede bir hizmet kaynağı oluşturun.

[AVS hizmeti](cloudsimple-service.md)hakkında daha fazla bilgi edinin.

## <a name="avs-node"></a>AVS düğümü

AVS düğümü, VMware ESXi hiper yöneticinin dağıtıldığı adanmış, çıplak ve hiper yakınsanmış bir işlem ve depolama ana bilgisayarı. Daha sonra bu düğüm VMware vSphere, vCenter, vSAN ve NSX platformlarına birleştirilir. AVS ağ hizmetleri ve uç ağ hizmetleri de etkinleştirilir. Her düğüm, [AVS özel bulutları](cloudsimple-private-cloud.md)oluşturmak için sağlayabileceğiniz bir işlem ve depolama kapasitesi birimi görevi görür. AVS hizmetinin kullanılabildiği bir bölgede düğümleri temin edersiniz veya ayırtın.

[AVS düğümleri](cloudsimple-node.md)hakkında daha fazla bilgi edinin.

## <a name="avs-private-cloud"></a>AVS özel bulutu

Bir AVS özel bulutu, kendi yönetim etki alanında bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware Stack ortamıdır. VMware yığını ESXi Konakları, vSphere, vCenter, vSAN ve NSX içerir. Yığın adanmış düğümlerde çalışır (ayrılmış ve yalıtılmış çıplak donanım) ve Kullanıcı tarafından vCenter ve NSX Yöneticisi içeren yerel VMware araçları aracılığıyla kullanılır. Adanmış düğümler Azure konumlarında dağıtılır ve Azure tarafından yönetilir. Her AVS özel bulutu, VLAN 'Lar ve alt ağlar ve güvenlik duvarı tabloları gibi ağ hizmetleri kullanılarak kesimlere ve güvenli hale getirilir. Şirket içi ortamınıza ve Azure ağına bağlantılar, güvenli, özel VPN ve Azure ExpressRoute bağlantıları kullanılarak oluşturulur.

[AVS özel bulutu](cloudsimple-private-cloud.md)hakkında daha fazla bilgi edinin.

## <a name="service-networking"></a>Hizmet ağı

AVS hizmeti, AVS hizmetinizin dağıtıldığı bölge başına bir ağ sağlar. Ağ, yönlendirme özelliği varsayılan olarak etkin olan tek bir TCP katman 3 adres alanıdır. Bu bölgede oluşturulan tüm AVS özel bulutları ve alt ağları ek bir yapılandırma olmadan birbirleriyle iletişim kurar. VLAN 'Ları kullanarak, vCenter üzerinde dağıtılmış bağlantı noktası grupları oluşturursunuz. Aşağıdaki ağ özelliklerini kullanarak, AVS özel bulutunuzda iş yükü kaynaklarınızı yapılandırabilir ve güvenli hale getirebilirsiniz:

* [VLAN 'Lar ve alt ağlar](cloudsimple-vlans-subnets.md)
* [Güvenlik Duvarı tabloları](cloudsimple-firewall-tables.md)
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md)
* [Genel IP](cloudsimple-public-ip-address.md)
* [Azure ağ bağlantısı](cloudsimple-azure-network-connection.md)

## <a name="avs-virtual-machine"></a>AVS sanal makinesi

AVS hizmetinde, VMware sanal makinelerini Azure portal yönetebilirsiniz. VSphere ortamınızdan bir veya daha fazla küme veya kaynak havuzu, hizmetin oluşturulduğu abonelikle eşleştirilebilir.

Aşağıdakiler hakkında daha fazla bilgi edinin:

* [AVS sanal makineleri](cloudsimple-virtual-machines.md)
* [Azure abonelik eşleme](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
