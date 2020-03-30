---
title: IP adresi nedir 168.63.129.16? | Microsoft Belgeleri
description: IP adresi 168.63.129.16 ve kaynaklarınızla nasıl çalıştığı hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: genli
ms.openlocfilehash: 287f881fb17dd84357f540ee562e21c66c11ab95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77114369"
---
# <a name="what-is-ip-address-1686312916"></a>IP adresi nedir 168.63.129.16?

IP adresi 168.63.129.16, Azure platform kaynaklarına yönelik bir iletişim kanalını kolaylaştırmak için kullanılan sanal bir genel IP adresidir. Müşteriler Azure'daki özel sanal ağları için herhangi bir adres alanı tanımlayabilir. Bu nedenle, Azure platformu kaynakları benzersiz bir genel IP adresi olarak sunulmalıdır. Bu sanal genel IP adresi aşağıdaki şeyleri kolaylaştırır:

- VM Aracısı'nın Azure platformuyla iletişim kurarak "Hazır" durumda olduğunu bildirmesini sağlar.
- Özel bir DNS sunucusu olmayan kaynaklara (VM gibi) filtrelenmiş ad çözünürlüğü sağlamak için DNS sanal sunucusuyla iletişimi sağlar. Bu filtreleme, müşterilerin yalnızca kaynaklarının ana bilgisayar adlarını çözümlemesini sağlar.
- VM'lerin sistem durumunu belirlemek için [Azure yük dengeleyicisinden sistem durumu sondaları](../load-balancer/load-balancer-custom-probe-overview.md) sağlar.
- VM'nin Azure'daki DHCP hizmetinden dinamik bir IP adresi almasını sağlar.
- PaaS rolü için Konuk Aracılı sinyal iletilerini etkinleştirir.

## <a name="scope-of-ip-address-1686312916"></a>IP adresinin kapsamı 168.63.129.16

Halka açık IP adresi 168.63.129.16 tüm bölgelerde ve tüm ulusal bulutlarda kullanılır. Bu özel genel IP adresi Microsoft'a aittir ve değişmez. Bu IP adresine herhangi bir yerel (VM) güvenlik duvarı ilkelerinde (giden yönde) izin vermenizi öneririz. Bu özel IP adresi ve kaynaklar arasındaki iletişim güvenlidir, çünkü bu IP adresinden yalnızca dahili Azure platformu bir ileti kaynak sağlayabilir. Bu adres engellenirse, çeşitli senaryolarda beklenmeyen davranışlar oluşabilir. 168.63.129.16 [ana bilgisayar düğümünün sanal](../virtual-network/security-overview.md#azure-platform-considerations) IP'sidir ve bu nedenle kullanıcı tanımlı rotalara tabi değildir.

- VM Aracısı WireServer (168.63.129.16) ile 80, 443, 32526 bağlantı noktaları üzerinden giden iletişim gerektirir. Bunlar VM'deki yerel güvenlik duvarında açık olmalıdır. 168.63.129.16 ile bu bağlantı noktalarındaki iletişim, yapılandırılan ağ güvenlik gruplarına tabi değildir.
- 168.63.129.16 VM'ye DNS hizmetleri sağlayabilir. Bu istenmiyorsa, bu trafik VM'deki yerel güvenlik duvarında engellenebilir. Varsayılan olarak DNS iletişimi, [AzurePlatformDNS](../virtual-network/service-tags-overview.md#available-service-tags) hizmet etiketinden özellikle yararlanmayı hedeflemedikçe yapılandırılan ağ güvenlik gruplarına tabi değildir.
- VM bir yük dengeleyici arka uç havuzunun bir parçası olduğunda, [sağlık sondası](../load-balancer/load-balancer-custom-probe-overview.md) iletişiminin 168.63.129.16'dan itibaren kaynaklanmasına izin verilmelidir. Varsayılan ağ güvenlik grubu yapılandırması, bu iletişimi sağlayan bir kurala sahiptir. Bu kural, [AzureLoadBalancer](../virtual-network/service-tags-overview.md#available-service-tags) hizmet etiketinden yararlanır. İstenirse, bu trafik ağ güvenlik grubunu yapılandırArak engellenebilir ancak bu durum başarısız olan sondalara neden olur.

Sanal olmayan bir ağ senaryosunda (Klasik), sistem durumu sondası özel bir IP'den elde edilir ve 168.63.129.16 kullanılmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik grupları](security-overview.md)
- [Ağ güvenlik grubu oluşturma, değiştirme veya silme](manage-network-security-group.md)
