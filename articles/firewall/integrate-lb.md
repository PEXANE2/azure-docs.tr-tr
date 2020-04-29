---
title: Azure Güvenlik Duvarı’nı Azure Standart Load Balancer ile tümleştirme
description: Bir Azure Güvenlik duvarını bir Azure Standart Load Balancer (genel veya dahili) ile bir sanal ağ ile tümleştirebilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 02/28/2020
ms.author: victorh
ms.openlocfilehash: ab9a500d9535b55702b8baff15f8cc47e6ac2c86
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78196745"
---
# <a name="integrate-azure-firewall-with-azure-standard-load-balancer"></a>Azure Güvenlik Duvarı’nı Azure Standart Load Balancer ile tümleştirme

Bir Azure Güvenlik duvarını bir Azure Standart Load Balancer (genel veya dahili) ile bir sanal ağ ile tümleştirebilirsiniz. 

Tercih edilen tasarım, çok daha basit bir tasarım olduğundan, Azure Güvenlik duvarınızla iç yük dengeleyiciyi tümleştirmelidir. Zaten bir dağıtılmışsa ve devam etmek istiyorsanız, ortak yük dengeleyici kullanabilirsiniz. Ancak, ortak yük dengeleyici senaryosuyla işlevselliği kesintiye uğramış bir asimetrik yönlendirme sorunuyla haberdar olmanız gerekir.

Azure Load Balancer hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](../load-balancer/load-balancer-overview.md)

## <a name="public-load-balancer"></a>Ortak yük dengeleyici

Ortak yük dengeleyici ile yük dengeleyici, genel ön uç IP adresi ile dağıtılır.

### <a name="asymmetric-routing"></a>Asimetrik yönlendirme

Asimetrik yönlendirme, bir paketin hedefe bir yol aldığı ve kaynağa dönme sırasında başka bir yol aldığı yerdir. Bu sorun, bir alt ağın güvenlik duvarının özel IP adresine giderken bir varsayılan yolu olduğunda ve bir ortak yük dengeleyici kullanıyorsanız oluşur. Bu durumda, gelen yük dengeleyici trafiği genel IP adresi aracılığıyla alınır, ancak döndürülen yol güvenlik duvarının özel IP adresinden geçer. Güvenlik duvarı durum bilgisi olduğundan, güvenlik duvarı, bu tür bir oturumun farkında olmadığından döndürülen paketi bırakır.

### <a name="fix-the-routing-issue"></a>Yönlendirme sorununu çözme

Bir Azure Güvenlik duvarını bir alt ağa dağıttığınızda, tek bir adım, paketleri güvenlik duvarının AzureFirewallSubnet konumundaki özel IP adresi aracılığıyla yönlendiren alt ağ için varsayılan bir yol oluşturmaktır. Daha fazla bilgi için bkz. [öğretici: Azure Güvenlik duvarını Azure Portal kullanarak dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md#create-a-default-route).

Yük dengeleyici senaryonuza güvenlik duvarını tanıtdığınızda, Internet trafiğinizin güvenlik duvarınızın genel IP adresi aracılığıyla gelmesini istersiniz. Buradan güvenlik duvarı, güvenlik duvarı kurallarını ve NAT 'ları yük dengeleyicinin genel IP adresine uygular. Bu, sorunun oluştuğu yerdir. Paketler, güvenlik duvarının genel IP adresine ulaşır, ancak özel IP adresi (varsayılan yol kullanılarak) aracılığıyla güvenlik duvarına geri döner.
Bu sorundan kaçınmak için, güvenlik duvarının genel IP adresi için ek bir ana bilgisayar yolu oluşturun. Güvenlik duvarının genel IP adresine giden paketler Internet üzerinden yönlendirilir. Bu, güvenlik duvarının özel IP adresine varsayılan yolu almayı önler.

![Asimetrik yönlendirme](media/integrate-lb/Firewall-LB-asymmetric.png)

### <a name="route-table-example"></a>Rota tablosu örneği

Örneğin, aşağıdaki rotalar genel IP adresi 20.185.97.136 ve 10.0.1.4 özel IP adresi için bir güvenlik duvarına yöneliktir.

> [!div class="mx-imgBorder"]
> ![Yol tablosu](media/integrate-lb/route-table.png)

### <a name="nat-rule-example"></a>NAT kuralı örneği

Aşağıdaki örnekte, bir NAT kuralı, 20.185.97.136 adresindeki yük dengeleyiciye 20.42.98.220 adresindeki güvenlik duvarındaki RDP trafiğini çevirir:

> [!div class="mx-imgBorder"]
> ![NAT kuralı](media/integrate-lb/nat-rule-02.png)

### <a name="health-probes"></a>Sistem durumu araştırmaları

Bağlantı noktası 80 veya HTTP/HTTPS araştırmaları için TCP sistem durumu araştırmaları kullanıyorsanız yük dengeleyici havuzundaki konaklarda çalışan bir Web hizmetiniz olması gerektiğini unutmayın.

## <a name="internal-load-balancer"></a>İç yük dengeleyici

İç yük dengeleyici ile, yük dengeleyici özel bir ön uç IP adresi ile dağıtılır.

Bu senaryoyla ilgili bir asimetrik yönlendirme sorunu yok. Gelen paketler, güvenlik duvarının genel IP adresine ulaşır, yük dengeleyicinin özel IP adresine çevrilir ve ardından aynı dönüş yolunu kullanarak güvenlik duvarının özel IP adresine geri döner.

Dolayısıyla, bu senaryoyu ortak yük dengeleyici senaryosuna benzer ancak güvenlik duvarı genel IP adresi ana bilgisayar yoluna gerek kalmadan dağıtabilirsiniz.

## <a name="additional-security"></a>Ek güvenlik

Yük dengeli senaryonuzun güvenliği artırmak için ağ güvenlik grupları (NSG 'ler) kullanabilirsiniz.

Örneğin, yük dengeli sanal makinelerin bulunduğu arka uç alt ağında bir NSG oluşturabilirsiniz. Güvenlik Duvarı IP adresinden/bağlantı noktasından gelen trafiğe izin ver.

![Ağ güvenlik grubu](media/integrate-lb/nsg-01.png)

NSG 'ler hakkında daha fazla bilgi için bkz. [güvenlik grupları](../virtual-network/security-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.