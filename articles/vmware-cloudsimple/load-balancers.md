---
title: Azure VMware çözümleri (AVS)-AVS özel bulutları için bir yük dengeleme çözümü seçin
description: Bir AVS özel bulutu 'nda uygulama dağıtma Yük Dengeleme seçeneklerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 405bc9d95b8d82e2181e2fb828d6bcc00c8c4639
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014887"
---
# <a name="choose-a-load-balancing-solution-for-avs-private-clouds"></a>AVS özel bulutları için bir yük dengeleme çözümü seçin

Bir uygulama bir AVS özel bulutunda dağıtıldığında, yük dengeleme için birkaç seçenekten birini seçebilirsiniz.

AVS özel bulutunuzda bir sanal veya yazılım tabanlı yük dengeleyici seçebilir ya da Azure aboneliğinizde çalışan Azure L7 yük dengeleyiciyi, AVS özel bulutu 'nda çalışan Web katmanı VM 'lerinizi ön sona erdirmek için kullanabilirsiniz. Burada birkaç seçenek listeliyoruz:

## <a name="virtual-load-balancers"></a>Sanal yük dengeleyiciler

VMware ortamınızda sanal yük dengeleyici gereçlerini vCenter arabiriminden dağıtabilir ve uygulama trafiğinizi ön uca yapılandırabilirsiniz.

Popüler satıcıların bazıları şunlardır: NGINX: http://nginx.org/en/docs/http/load_balancing.html F5-BigIP-Traffic Manager: https://www.f5.com/products/big-ip-services/virtual-editions Citrix ADC: https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Azure L7 yük dengeleyici

Azure Application Gateway ' yi bir AVS özel bulutu 'nda çalıştırılan uygulamanız için bir L7 yük dengeleyici olarak kullandığınızda, yük dengeleyici yazılımını yönetmeniz gerekmez. Yük dengeleyici yazılımı Azure tarafından yönetilir. AVS özel bulutundaki tüm Web katmanı VM 'Leri özel IP adresleri kullanır ve adları çözümlemek için ek NAT kuralları ya da genel IP adresleri gerektirmez. Web katmanı VM 'Leri, Azure Application Gateway ile özel, düşük gecikmeli ve yüksek bant genişliğine sahip bir bağlantı üzerinden iletişim kurar.

Bu çözümü yapılandırma hakkında daha fazla bilgi edinmek için Azure Application Gateway 'yi bir L7 yük dengeleyici olarak kullanmaya yönelik çözüm kılavuzuna bakın.

## <a name="azure-internal-load-balancer"></a>Azure iç yük dengeleyici

Uygulamanızı, Web ön uç katmanının Azure aboneliğinizdeki bir Azure sanal ağı içinde çalıştığı bir karma dağıtımda çalıştırmayı tercih ederseniz ve uygulamanın DB katmanı, AVS özel bulutu 'ndaki VMware VM 'lerinde çalışıyorsa, Azure iç yük dengeleyici (L) kullanabilirsiniz. 4 yük dengeleyici), trafik yönetimi için VERITABANı katmanı sanal makinelerinizin önünde.

Daha fazla bilgi için bkz. Azure [iç Load Balancer](../load-balancer/concepts-limitations.md#internalloadbalancer) belgeleri.

## <a name="global-server-load-balancer"></a>Küresel sunucu yük dengeleyici

DNS tabanlı yük dengeleyici arıyorsanız, Azure Marketi 'nde bulunan üçüncü taraf çözümlerini kullanabilir ya da yerel Azure çözümüne gidebilirsiniz.

Azure Traffic Manager, genel Azure bölgelerinde ve şirket içinde bulunan hizmetlere en uygun trafiği dağıtmanıza olanak tanıyan, yüksek kullanılabilirlik ve yanıt verme olanağı sunan bir DNS tabanlı trafik yük dengeleyicidir. Daha fazla bilgi için bkz. Azure [Traffic Manager](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) belgeleri.
