---
title: Azure Güvenlik Duvarı Zorlamalı tünel
description: Daha fazla işleme için Internet 'e ait trafiği ek bir güvenlik duvarı veya ağ sanal gerecine yönlendirmek üzere Zorlamalı tünel yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 08/19/2020
ms.author: victorh
ms.openlocfilehash: da2b206bf24cb33180305e32e270b989eb64dfa3
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88612613"
---
# <a name="azure-firewall-forced-tunneling"></a>Azure Güvenlik Duvarı Zorlamalı tünel

Yeni bir Azure Güvenlik Duvarı yapılandırırken internete giden tüm trafiği doğrudan internete gitmek yerine belirlenmiş bir atlamaya gidecek şekilde yönlendirebilirsiniz. Örneğin, Internet 'e geçirilmeden önce ağ trafiğini işlemek için şirket içi bir uç güvenlik duvarına veya diğer ağ sanal gerecine (NVA) sahip olabilirsiniz. Ancak, Zorlamalı tünel için mevcut bir güvenlik duvarını yapılandıramazsınız.

Varsayılan olarak, tüm giden Azure bağımlılıklarının karşılanmasını sağlamak için Azure Güvenlik duvarında zorlamalı tünelye izin verilmez. Varsayılan bir yolu olan *AzureFirewallSubnet* üzerinde Kullanıcı tanımlı yol (UDR), doğrudan Internet 'e gitmez.

## <a name="forced-tunneling-configuration"></a>Zorlamalı tünel yapılandırması

Zorlamalı tüneli desteklemek için, hizmet yönetimi trafiği müşteri trafiğinden ayrılır. Kendi ilişkili genel IP adresi ile *AzureFirewallManagementSubnet* adlı ek bir ayrılmış alt ağ (minimum alt ağ boyutu/26) gereklidir. Bu alt ağda izin verilen tek yol Internet 'e yönelik varsayılan bir yoldur ve BGP yolu yayılması devre dışı bırakılmalıdır.

Şirket içi trafiği zorlamak için BGP aracılığıyla tanıtılan bir varsayılan yolunuz varsa, güvenlik duvarınızı dağıtmadan önce *AzureFirewallSubnet* ve *AzureFirewallManagementSubnet* oluşturmanız ve Internet 'e varsayılan bır yol içeren bir UDR uygulamanız ve **ağ geçidi yollarının** devre dışı bırakılması gerekir.

Bu yapılandırmada, *AzureFirewallSubnet* artık, Internet 'e geçirilmeden önce trafiği işlemek için herhangi bir şirket içi güvenlik duvarı veya NVA için yollar içerebilir. Bu alt ağda **ağ geçidi yollarının yayılması** etkinse, bu yolları BGP üzerinden de yayımlayabilirsiniz *AzureFirewallSubnet* .

Örneğin, *AzureFirewallSubnet* üzerinde VPN ağ geçidiniz ile şirket içi cihazınıza ulaşmak için bir sonraki atlama olarak varsayılan bir yol oluşturabilirsiniz. Ya da şirket içi ağa uygun yolları almak için **ağ geçidi yollarını yay** ' i de etkinleştirebilirsiniz.

:::image type="content" source="media/forced-tunneling/route-propagation.png" alt-text="Sanal ağ geçidi yol yayma":::

Zorlamalı tüneli etkinleştirirseniz, Internet 'e bağlı trafik, AzureFirewallSubnet 'deki güvenlik duvarı özel IP adreslerinden birine, kaynak şirket içi güvenlik duvarınızdan gizleniyor.

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı AzureFirewallSubnet 'deki güvenlik duvarı özel IP adreslerinden birine giden trafiği yeniden çıkarır. Ancak, Azure Güvenlik duvarını genel IP adresi **aralığınızı SNAT olarak** yapılandırmak için yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SNAT özel IP adresi aralıkları](snat-private-range.md).

Azure Güvenlik duvarını Zorlamalı tünel oluşturmayı destekleyecek şekilde yapılandırdıktan sonra, yapılandırmayı geri alamazsınız. Güvenlik duvarınızdaki diğer tüm IP yapılandırmalarını kaldırırsanız, yönetim IP yapılandırması da kaldırılır ve güvenlik duvarı serbest bırakılır. Yönetim IP yapılandırmasına atanan genel IP adresi kaldırılamaz, ancak farklı bir genel IP adresi atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portal kullanarak Azure Güvenlik duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)
