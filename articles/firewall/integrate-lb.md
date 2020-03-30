---
title: Azure Güvenlik Duvarı’nı Azure Standart Load Balancer ile tümleştirme
description: Azure Güvenlik Duvar'ı Azure Standart Yük Dengeleyicisi (genel veya dahili) içeren sanal ağa entegre edebilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 02/28/2020
ms.author: victorh
ms.openlocfilehash: ab9a500d9535b55702b8baff15f8cc47e6ac2c86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196745"
---
# <a name="integrate-azure-firewall-with-azure-standard-load-balancer"></a>Azure Güvenlik Duvarı’nı Azure Standart Load Balancer ile tümleştirme

Azure Güvenlik Duvar'ı Azure Standart Yük Dengeleyicisi (genel veya dahili) içeren sanal ağa entegre edebilirsiniz. 

Tercih edilen tasarım, çok daha basit bir tasarım olduğu için bir dahili yük dengeleyicisini Azure güvenlik duvarınızla entegre etmektir. Zaten bir tane dağıttıysanız ve bunu yerinde tutmak istiyorsanız, bir ortak yük dengeleyicisi kullanabilirsiniz. Ancak, genel yük dengeleyici senaryosu ile işlevselliği bozabilecek asimetrik bir yönlendirme sorunu farkında olmanız gerekir.

Azure Yük Dengeleyicisi hakkında daha fazla bilgi için azure [yük bakiyesi nedir?](../load-balancer/load-balancer-overview.md)

## <a name="public-load-balancer"></a>Genel yük dengeleyici

Bir ortak yük dengeleyicisi ile yük dengeleyicisi, genel bir ön uç IP adresiyle dağıtılır.

### <a name="asymmetric-routing"></a>Asimetrik yönlendirme

Asimetrik yönlendirme, bir paketin hedefe giden bir yolu aldığı ve kaynağa dönerken başka bir yol izlediği yerdir. Bu sorun, bir alt ağ güvenlik duvarının özel IP adresine giden varsayılan bir rotaya sahipse ve siz ortak yük dengeleyicisi kullanıyorsanız oluşur. Bu durumda, gelen yük dengeleyici trafiği genel IP adresi üzerinden alınır, ancak dönüş yolu güvenlik duvarının özel IP adresinden geçer. Güvenlik duvarı durum lu olduğundan, güvenlik duvarı böyle bir oturumdan haberdar olmadığından dönen paketi düşürür.

### <a name="fix-the-routing-issue"></a>Yönlendirme sorununu düzeltme

Bir Alt ağa Azure Güvenlik Duvarı dağıttığınızda, bir adım, alt ağ için paketleri güvenlik duvarının AzureWallSubnet'te bulunan özel IP adresiüzerinden yönlendiren varsayılan bir rota oluşturmaktır. Daha fazla bilgi için [Bkz. Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarını dağıtın ve yapılandırın.](tutorial-firewall-deploy-portal.md#create-a-default-route)

Güvenlik duvarını yük dengeleyici senaryonuza kattığında, Internet trafiğinizin güvenlik duvarınızın genel IP adresi üzerinden gelmesini istersiniz. Buradan, güvenlik duvarı güvenlik duvarı kurallarını uygular ve paketleri yük bakiyenizin genel IP adresine uygular. Sorun burada oluşur. Paketler güvenlik duvarının genel IP adresine gelir, ancak özel IP adresi (varsayılan rotayı kullanarak) aracılığıyla güvenlik duvarına geri döner.
Bu sorunu önlemek için, güvenlik duvarının genel IP adresi için ek bir ana bilgisayar rotası oluşturun. Güvenlik duvarının genel IP adresine giden paketler Internet üzerinden yönlendirilir. Bu, varsayılan rotayı güvenlik duvarının özel IP adresine götürmeyi önler.

![Asimetrik yönlendirme](media/integrate-lb/Firewall-LB-asymmetric.png)

### <a name="route-table-example"></a>Rota tablosu örneği

Örneğin, aşağıdaki yollar genel IP adresi 20.185.97.136 ve özel IP adresi 10.0.1.4 bir güvenlik duvarı içindir.

> [!div class="mx-imgBorder"]
> ![Yol tablosu](media/integrate-lb/route-table.png)

### <a name="nat-rule-example"></a>NAT kural örneği

Aşağıdaki örnekte, Bir NAT kuralı 20.185.97.136'da RDP trafiğini güvenlik duvarına 20.42.98.220'de yük dengeleyicisine çevirir:

> [!div class="mx-imgBorder"]
> ![NAT kuralı](media/integrate-lb/nat-rule-02.png)

### <a name="health-probes"></a>Sistem durumu araştırmaları

TCP sistem sondalarını 80 bağlantı noktasına veya HTTP/HTTPS sondalarına kullanıyorsanız, yük dengeleyici havuzundaki ana bilgisayarlarda çalışan bir web hizmetine sahip olmanız gerektiğini unutmayın.

## <a name="internal-load-balancer"></a>İç yük dengeleyici

Dahili yük dengeleyicisi ile yük dengeleyicisi özel bir ön uç IP adresiyle dağıtılır.

Bu senaryoda asimetrik yönlendirme sorunu yok. Gelen paketler güvenlik duvarının genel IP adresine ulaşır, yük bakiyesinin özel IP adresine çevrilir ve aynı dönüş yolunu kullanarak güvenlik duvarının özel IP adresine döner.

Bu nedenle, bu senaryoyu genel yük dengeleyici senaryosuna benzer şekilde, ancak güvenlik duvarı genel IP adresi ana bilgisayar rotasına gerek kalmadan dağıtabilirsiniz.

## <a name="additional-security"></a>Ek güvenlik

Yük dengeli senaryonuzun güvenliğini daha da artırmak için ağ güvenlik gruplarını (NSGs) kullanabilirsiniz.

Örneğin, yük dengeli sanal makinelerin bulunduğu arka uç alt netinde bir NSG oluşturabilirsiniz. Güvenlik duvarı IP adresinden/bağlantı noktasından gelen trafiğe izin verin.

![Ağ güvenlik grubu](media/integrate-lb/nsg-01.png)

NSG'ler hakkında daha fazla bilgi için [Güvenlik gruplarına](../virtual-network/security-overview.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı'nı nasıl dağıtıp yapılandırılamayı](tutorial-firewall-deploy-portal.md)öğrenin.