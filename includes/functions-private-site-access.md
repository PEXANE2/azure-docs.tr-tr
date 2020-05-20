---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 5e0cff7bde6e80a776d694820ca7b69dafa7c0d9
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648833"
---
Özel site erişimi, uygulamanızı yalnızca bir Azure sanal ağı gibi özel bir ağdan erişilebilir hale getirme anlamına gelir.

* Özel site erişimi, hizmet uç noktaları yapılandırıldığında [Premium](../articles/azure-functions/functions-premium-plan.md), [Tüketim](../articles/azure-functions/functions-scale.md#consumption-plan)ve [App Service](../articles/azure-functions/functions-scale.md#app-service-plan) planlarında kullanılabilir.
    * Hizmet uç noktaları, **platform özellikleri**  >  **ağ oluşturma**  >  **erişim kısıtlamalarını yapılandırma**  >  **Kural Ekle**' nin altında uygulama temelinde yapılandırılabilir. Sanal ağlar artık bir kural türü olarak seçilebilir.
    * Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktaları](../articles/virtual-network/virtual-network-service-endpoints-overview.md).
    * Hizmet uç noktalarında, işlevinizin hala internet 'e yönelik sanal ağ tümleştirmesiyle birlikte tam giden erişimi olduğunu aklınızda bulundurun.
* Özel site erişimi, iç yük dengeleyici (ıLB) ile yapılandırılmış bir App Service Ortamı içinde de kullanılabilir. Daha fazla bilgi için bkz. [bir App Service ortamı iç yük dengeleyici oluşturma ve kullanma](../articles/app-service/environment/create-ilb-ase.md).

Özel site erişimini ayarlamayı öğrenmek için bkz. [Azure işlevleri özel site erişimi oluşturma](../articles/azure-functions/functions-create-private-site-access.md).