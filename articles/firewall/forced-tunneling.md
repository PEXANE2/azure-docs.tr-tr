---
title: Azure Güvenlik Duvarı Zorlamalı tünel
description: Daha fazla işleme için Internet 'e ait trafiği ek bir güvenlik duvarı veya ağ sanal gerecine yönlendirmek üzere Zorlamalı tünel yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 4093f91e55272a32ce7df4a78e2ee8b3ebed5fde
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444478"
---
# <a name="azure-firewall-forced-tunneling-preview"></a>Azure Güvenlik Duvarı Zorlamalı tünel (Önizleme)

Azure Güvenlik Duvarı 'nı, Internet 'e yönelik tüm trafiği doğrudan Internet 'e gitmek yerine belirlenen bir sonraki atlamaya yönlendirmek üzere yapılandırabilirsiniz. Örneğin, Internet 'e geçirilmeden önce ağ trafiğini işlemek için şirket içi bir uç güvenlik duvarına veya diğer ağ sanal gerecine (NVA) sahip olabilirsiniz.

> [!IMPORTANT]
> Azure Güvenlik Duvarı Zorlamalı tünel Şu anda genel önizleme aşamasındadır.
>
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Varsayılan olarak, tüm giden Azure bağımlılıklarının karşılanmasını sağlamak için Azure Güvenlik duvarında zorlamalı tünelye izin verilmez. Varsayılan bir yolu olan *AzureFirewallSubnet* üzerinde Kullanıcı tanımlı yol (UDR), doğrudan Internet 'e gitmez.

## <a name="forced-tunneling-configuration"></a>Zorlamalı tünel yapılandırması

Zorlamalı tüneli desteklemek için, hizmet yönetimi trafiği müşteri trafiğinden ayrılır. Kendi ilişkili genel IP adresi ile *AzureFirewallManagementSubnet* adlı ek bir ayrılmış alt ağ gerekir. Bu alt ağda izin verilen tek yol Internet 'e yönelik varsayılan bir yoldur ve BGP yolu yayılması devre dışı bırakılmalıdır.

Şirket içi trafiği zorlamak için BGP aracılığıyla tanıtılan bir varsayılan yönlendirmenize sahipseniz, güvenlik duvarınızı dağıtmadan önce *AzureFirewallSubnet* ve *AzureFirewallManagementSubnet* oluşturmanız ve Internet 'e varsayılan bir rota ile bir UDR oluşturmanız ve sanal ağ geçidi yol yayılması devre dışı bırakılması gerekir.

Bu yapılandırmada, *AzureFirewallSubnet* artık trafiği Internet 'e geçirilmeden önce işlemek üzere herhangi bir şirket içi güvenlik duvarının veya NVA 'nın yollarını içerebilir. Bu alt ağda sanal ağ geçidi yol yayılması etkinse, bu yolları BGP üzerinden *AzureFirewallSubnet* 'e yayımlayabilirsiniz.

Azure Güvenlik duvarını Zorlamalı tünel oluşturmayı destekleyecek şekilde yapılandırdıktan sonra, yapılandırmayı geri alamazsınız. Güvenlik duvarınızdaki diğer tüm IP yapılandırmalarını kaldırırsanız, yönetim IP yapılandırması da kaldırılır ve güvenlik duvarı serbest bırakılır. Yönetim IP yapılandırmasına atanan genel IP adresi kaldırılamaz, ancak farklı bir genel IP adresi atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portal kullanarak Azure Güvenlik duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)