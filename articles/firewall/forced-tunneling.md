---
title: Azure Güvenlik Duvarı Zorlamalı tünel
description: Daha fazla işleme için Internet 'e ait trafiği ek bir güvenlik duvarı veya ağ sanal gerecine yönlendirmek üzere Zorlamalı tünel yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 05/11/2020
ms.author: victorh
ms.openlocfilehash: 463bccb29d59f06e7381d7d7123946029223a93a
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83199728"
---
# <a name="azure-firewall-forced-tunneling"></a>Azure Güvenlik Duvarı Zorlamalı tünel

Azure Güvenlik Duvarı 'nı, Internet 'e yönelik tüm trafiği doğrudan Internet 'e gitmek yerine belirlenen bir sonraki atlamaya yönlendirmek üzere yapılandırabilirsiniz. Örneğin, Internet 'e geçirilmeden önce ağ trafiğini işlemek için şirket içi bir uç güvenlik duvarına veya diğer ağ sanal gerecine (NVA) sahip olabilirsiniz.

Varsayılan olarak, tüm giden Azure bağımlılıklarının karşılanmasını sağlamak için Azure Güvenlik duvarında zorlamalı tünelye izin verilmez. Varsayılan bir yolu olan *AzureFirewallSubnet* üzerinde Kullanıcı tanımlı yol (UDR), doğrudan Internet 'e gitmez.

## <a name="forced-tunneling-configuration"></a>Zorlamalı tünel yapılandırması

Zorlamalı tüneli desteklemek için, hizmet yönetimi trafiği müşteri trafiğinden ayrılır. Kendi ilişkili genel IP adresi ile *AzureFirewallManagementSubnet* adlı ek bir ayrılmış alt ağ (minimum alt ağ boyutu/26) gereklidir. Bu alt ağda izin verilen tek yol Internet 'e yönelik varsayılan bir yoldur ve BGP yolu yayılması devre dışı bırakılmalıdır.

Şirket içi trafiği zorlamak için BGP aracılığıyla tanıtılan bir varsayılan yönlendirmenize sahipseniz, güvenlik duvarınızı dağıtmadan önce *AzureFirewallSubnet* ve *AzureFirewallManagementSubnet* oluşturmanız ve Internet 'e varsayılan bir rota ile bir UDR oluşturmanız ve **sanal ağ geçidi yol yayılması** devre dışı bırakılması gerekir.

Bu yapılandırmada, *AzureFirewallSubnet* artık trafiği Internet 'e geçirilmeden önce işlemek üzere herhangi bir şirket içi güvenlik duvarının veya NVA 'nın yollarını içerebilir. Bu alt ağda **sanal ağ geçidi yol yayılması** etkinse, bu yolları BGP üzerinden *AzureFirewallSubnet* 'e yayımlayabilirsiniz.

Örneğin, *AzureFirewallSubnet* üzerinde VPN ağ geçidiniz ile şirket içi cihazınıza ulaşmak için bir sonraki atlama olarak varsayılan bir yol oluşturabilirsiniz. Ya da **sanal ağ geçidi yol yaymayı** etkinleştirerek, şirket içi ağa uygun yolları alabilirsiniz.

![Sanal ağ geçidi yol yayma](media/forced-tunneling/route-propagation.png)

Azure Güvenlik duvarını Zorlamalı tünel oluşturmayı destekleyecek şekilde yapılandırdıktan sonra, yapılandırmayı geri alamazsınız. Güvenlik duvarınızdaki diğer tüm IP yapılandırmalarını kaldırırsanız, yönetim IP yapılandırması da kaldırılır ve güvenlik duvarı serbest bırakılır. Yönetim IP yapılandırmasına atanan genel IP adresi kaldırılamaz, ancak farklı bir genel IP adresi atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portal kullanarak Azure Güvenlik duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)
