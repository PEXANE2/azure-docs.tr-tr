---
title: Azure Güvenlik Duvarı zorunlu tünel
description: Daha fazla işlem için Internet'e bağlı trafiği ek bir güvenlik duvarına veya ağ sanal cihazına yönlendirmek için zorunlu tünel leme işlemini yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 02/24/2020
ms.author: victorh
ms.openlocfilehash: e51f6de370a5340082f64a0ca15c61583f75962b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597294"
---
# <a name="azure-firewall-forced-tunneling-preview"></a>Azure Güvenlik Duvarı zorunlu tünelleme (önizleme)

Azure Güvenlik Duvarı'nı, doğrudan Internet'e gitmek yerine Internet'e bağlı tüm trafiği atanmış bir sonraki atlamaya yönlendirecek şekilde yapılandırabilirsiniz. Örneğin, Internet'e geçmeden önce ağ trafiğini işlemek için şirket içi bir güvenlik duvarınız veya başka bir ağ sanal cihazınız (NVA) olabilir.

> [!IMPORTANT]
> Azure Güvenlik Duvarı zorunlu tünelleme şu anda genel önizlemede.
>
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Varsayılan olarak, tüm giden Azure bağımlılıklarının karşılandığından emin olmak için Azure Güvenlik Duvarı'nda zorunlu tünel lemelere izin verilmez. *AzureFirewallSubnet'te* doğrudan Internet'e gitmeyen varsayılan bir rotaya sahip Kullanıcı Tanımlı Rota (UDR) yapılandırmaları devre dışı bırakılır.

## <a name="forced-tunneling-configuration"></a>Zorunlu tünel yapılandırması

Zorunlu tünel itimini desteklemek için, servis yönetimi trafiği müşteri trafiğinden ayrılır. *AzureFirewallManagementSubnet* (minimum alt net boyutu /26) adlı ek bir özel alt ağ, kendi ilişkili genel IP adresiyle birlikte gereklidir. Bu alt ağda izin verilen tek yol Internet'e varsayılan bir yoldur ve BGP rota yayılımı devre dışı bırakılmalı.

Trafiği şirket içinde zorlamak için BGP üzerinden reklamı yapılan varsayılan bir rotanız varsa, güvenlik duvarınızı dağıtmadan önce *AzureFireWallSubnet* ve *AzureFireWallManagementSubnet'i* oluşturmanız ve Internet'e varsayılan rotaya sahip bir UDR'ye sahip olması ve **Sanal ağ ağ geçidi rotası nın** devre dışı bırakıldığını.

Bu yapılandırma da *AzureFirewallSubnet* artık Internet'e geçmeden önce trafiği işlemek için şirket içinde herhangi bir güvenlik duvarına veya NVA'ya giden yolları içerebilir. Bu alt ağ da Sanal ağ ağ geçidi **rotası yayılması** etkinse, bu yolları BGP üzerinden *AzureFirewallSubnet'e* de yayımlayabilirsiniz.

Örneğin, *AzureFirewallSubnet'te* vpn ağ geçidiniz ile şirket içi aygıtınıza ulaşmak için bir sonraki atlama olarak varsayılan bir rota oluşturabilirsiniz. Veya şirket içi ağa uygun yolları almak için **Sanal ağ ağ geçidi rotasını** etkinleştirebilirsiniz.

![Sanal ağ ağ geçidi rotası yayılımı](media/forced-tunneling/route-propagation.png)

Azure Güvenlik Duvarı'nı zorunlu tünel düzenlemeyi destekleyecek şekilde yapılandırdıktan sonra yapılandırmayı geri alamazsınız. Güvenlik duvarınızdaki diğer tüm IP yapılandırmalarını kaldırırsanız, yönetim IP yapılandırması da kaldırılır ve güvenlik duvarı kaldırılır. Yönetim IP yapılandırmasına atanan genel IP adresi kaldırılamaz, ancak farklı bir genel IP adresi atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)