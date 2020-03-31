---
title: CloudSimple tarafından Azure VMware Çözümü - CloudSimple Private Clouds için bir yük dengeleme çözümü seçin
description: Özel Bulut'ta bir uygulama dağıtan yük dengeleme seçeneklerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6c98b699b1d3aba15ce69c519d35d7ce3e90d123
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77014887"
---
# <a name="choose-a-load-balancing-solution-for-cloudsimple-private-clouds"></a>CloudSimple Private Clouds için bir yük dengeleme çözümü seçin

Bir uygulamayı CloudSimple Private Cloud'da dağıtırken, yük dengeleme için birkaç seçenekarasından birini seçebilirsiniz.

CloudSimple özel bulutunuzda sanal veya yazılım tabanlı bir yük dengeleyicisi seçebilir veya CloudSimple Private Cloud'da çalışan web katmanı VM'lerinizi ön uçta kullanmak için Azure aboneliğinizde çalışan Azure L7 yük bakiyesini bile kullanabilirsiniz. Burada, birkaç seçenek listeleyin:

## <a name="virtual-load-balancers"></a>Sanal yük dengeleyicileri

VCenter arabirimi aracılığıyla Sanal yük dengeleyici cihazlarını VMware ortamınızda dağıtabilir ve uygulama trafiğinizi ön uca göre yapılandırabilirsiniz.

Bazı popüler satıcılar şunlardır: http://nginx.org/en/docs/http/load_balancing.html NginX: F5- https://www.f5.com/products/big-ip-services/virtual-editions BigIP - Trafik Müdürü: Citrix ADC:https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Azure L7 yük dengeleyicisi

Azure Application Gateway'i Özel Bulut'ta çalışan uygulamanız için L7 yük dengeleyicisi olarak kullandığınızda, yük dengeleyici yazılımını yönetmeniz gerekmez. Yük dengeleyici yazılımı Azure tarafından yönetilir. Özel Bulut'taki tüm web katmanı VM'leri özel IP adresleri kullanır ve adları çözmek için ek NAT kuralları veya genel IP adresleri gerektirmez. Web katmanı VM'ler, özel, düşük gecikmeli, yüksek bant genişliğibağlantısı üzerinden Azure Uygulama Ağ Geçidi ile iletişim kurar.

Bu çözümü nasıl yapılandıracağımız hakkında daha fazla bilgi edinmek için Azure Uygulama Ağ Geçidi'ni L7 yük dengeleyicisi olarak kullanma yla ilgili çözüm kılavuzuna bakın.

## <a name="azure-internal-load-balancer"></a>Azure dahili yük dengeleyicisi

Uygulamanızı, Azure aboneliğinizde bir Azure vNet'te web ön uç katmanının çalıştığı ve uygulamanın DB katmanının CloudSimple Private Cloud'daki VMware VM'lerde çalıştığı karma bir dağıtımda çalıştırmayı seçerseniz, Azure dahili yükünü kullanabilirsiniz trafik yönetimi için DB kademe VM'lerinizin önünde dengeleyici (L4 yük dengeleyici).

Daha fazla bilgi için Azure [Dahili Yük Dengeleyici](../load-balancer/concepts-limitations.md#internalloadbalancer) belgelerine bakın.

## <a name="global-server-load-balancer"></a>Genel sunucu yük dengeleyicisi

DNS tabanlı bir yük dengeleyicisi arıyorsanız, Azure Marketi'nde kullanılabilen üçüncü taraf çözümlerini kullanabilir veya yerel Azure çözümlerini kullanmaya bilirsiniz.

Azure Trafik Yöneticisi, trafiği küresel Azure bölgeleri ve şirket içi hizmetlere en iyi şekilde dağıtmanızı sağlarken yüksek kullanılabilirlik ve yanıt verme olanağı sağlayan DNS tabanlı bir trafik yük dengeleyicisidir. Daha fazla bilgi için Azure [Trafik Yöneticisi](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) belgelerine bakın.
