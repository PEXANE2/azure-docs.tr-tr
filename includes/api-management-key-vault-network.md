---
author: dlepow
ms.service: api-management
ms.topic: include
ms.date: 01/26/2021
ms.author: danlep
ms.openlocfilehash: a9dbedd8516f3a3a592c7fd4f4f5563011d6c6db
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99491029"
---
#### <a name="requirements-for-key-vault-firewall"></a>Key Vault güvenlik duvarı gereksinimleri

Anahtar Kasanızda [Key Vault güvenlik duvarı](../articles/key-vault/general/network-security.md) etkinse, ek gereksinimler aşağıda verilmiştir:

* Anahtar kasasına erişmek için API Management örneğinin **sistem tarafından atanan** yönetilen kimliğini kullanmanız gerekir.
* Güvenlik Duvarı Key Vault, **Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver** seçeneğini etkinleştirin.

#### <a name="virtual-network-requirements"></a>Sanal ağ gereksinimleri

API Management örneği bir sanal ağda dağıtılmışsa, aşağıdaki ağ ayarlarını da yapılandırın:

* API Management alt ağında Azure Key Vault için bir [hizmet uç noktası](../articles/key-vault/general/overview-vnet-service-endpoints.md) etkinleştirin.
* AzureKeyVault ve AzureActiveDirectory [hizmet etiketlerine](../articles/virtual-network/service-tags-overview.md)giden trafiğe izin vermek için bir ağ güvenlik grubu (NSG) kuralı yapılandırın. 

Ayrıntılar için bkz. [sanal ağa bağlanma](../articles/api-management/api-management-using-with-vnet.md#-common-network-configuration-issues)içindeki ağ yapılandırması ayrıntıları.