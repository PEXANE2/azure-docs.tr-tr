---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: 994b8285999bfa52e2aea9a57ad832aefddcfb76
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86545112"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Veri deponuzu şirket içi ağ, Azure sanal ağı veya Amazon sanal özel bulutu içinde bulunuyorsa, bu sunucuya bağlanmak için şirket içinde [barındırılan bir tümleştirme çalışma zamanı](../articles/data-factory/create-self-hosted-integration-runtime.md) ayarlamanız gerekir.

Veri depoluizin, erişimin güvenlik duvarı kurallarında beyaz olarak listelenen IP 'Ler ile sınırlandırıldığı yönetilen bir bulut veri hizmeti ise, Azure tümleştirme çalışma zamanını kullanabilir ve [IP](../articles/data-factory/azure-integration-runtime-ip-addresses.md) 'lerini izin verilenler listesine ekleyebilirsiniz. 

Genel olarak veri depolarına erişmek üzere Data Factory tarafından desteklenen ağ güvenlik mekanizmaları hakkında bilgi için bkz. [veri erişimi stratejileri](../articles/data-factory/data-access-strategies.md) .
