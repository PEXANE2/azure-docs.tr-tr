---
title: CloudSimple tarafından Azure VMware çözümü-CloudSimple özel bulutları için bir yük dengeleme çözümü seçin
description: Özel bir bulutta uygulama dağıtma Yük Dengeleme seçeneklerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2fec54c19b9040e7cb44f1a5d2b7909510ab7313
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82734612"
---
# <a name="choose-a-load-balancing-solution-for-cloudsimple-private-clouds"></a>CloudSimple özel bulutları için bir yük dengeleme çözümü seçin

Bir CloudSimple özel bulutu 'nda bir uygulama dağıttığınızda, yük dengeleme için birkaç seçenekten birini seçebilirsiniz.

CloudSimple özel bulutunuzda bir sanal veya yazılım tabanlı yük dengeleyici seçebilir veya Azure aboneliğinizde çalışan Azure L7 yük dengeleyiciyi kullanarak CloudSimple özel bulutu 'nda çalışan Web katmanı sanal makinelerinizi ön uca ekleyebilirsiniz. Burada birkaç seçenek listeliyoruz:

## <a name="virtual-load-balancers"></a>Sanal yük dengeleyiciler

VMware ortamınızda sanal yük dengeleyici gereçlerini vCenter arabiriminden dağıtabilir ve uygulama trafiğinizi ön uca yapılandırabilirsiniz.

Popüler satıcıların bazıları şunlardır: NGINX: http://nginx.org/en/docs/http/load_balancing.html F5-BigIP-Traffic Manager: https://www.f5.com/products/big-ip-services/virtual-editions Citrix ADC:https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Azure L7 yük dengeleyici

Azure Application Gateway 'i özel bir bulutta çalışan uygulamanız için bir L7 yük dengeleyici olarak kullandığınızda, yük dengeleyici yazılımını yönetmeniz gerekmez. Yük dengeleyici yazılımı Azure tarafından yönetilir. Özel buluttaki tüm Web katmanı VM 'Leri özel IP adresleri kullanır ve adları çözümlemek için ek NAT kuralları ya da genel IP adresleri gerektirmez. Web katmanı VM 'Leri, Azure Application Gateway ile özel, düşük gecikmeli ve yüksek bant genişliğine sahip bir bağlantı üzerinden iletişim kurar.

Bu çözümü yapılandırma hakkında daha fazla bilgi edinmek için Azure Application Gateway 'yi bir L7 yük dengeleyici olarak kullanmaya yönelik çözüm kılavuzuna bakın.

## <a name="azure-internal-load-balancer"></a>Azure iç yük dengeleyici

Uygulamanızı, Web ön uç katmanının Azure aboneliğinizdeki bir Azure sanal ağı içinde çalıştığı bir karma dağıtımda çalıştırmayı tercih ederseniz ve uygulamanın DB katmanı CloudSimple özel bulutundaki VMware VM 'lerinde çalışıyorsa, trafik yönetimi için VERITABANı katmanı sanal makinelerinizin önünde Azure iç yük dengeleyici (L4 yük dengeleyici) kullanabilirsiniz.

Daha fazla bilgi için bkz. Azure [iç Load Balancer](../load-balancer/components.md#frontend-ip-configurations) belgeleri.

## <a name="global-server-load-balancer"></a>Küresel sunucu yük dengeleyici

DNS tabanlı yük dengeleyici arıyorsanız, Azure Marketi 'nde bulunan üçüncü taraf çözümlerini kullanabilir ya da yerel Azure çözümüne gidebilirsiniz.

Azure Traffic Manager, genel Azure bölgelerinde ve şirket içinde bulunan hizmetlere en uygun trafiği dağıtmanıza olanak tanıyan, yüksek kullanılabilirlik ve yanıt verme olanağı sunan bir DNS tabanlı trafik yük dengeleyicidir. Daha fazla bilgi için bkz. Azure [Traffic Manager](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) belgeleri.
