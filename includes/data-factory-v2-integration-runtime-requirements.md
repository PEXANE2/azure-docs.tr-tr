---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: 6c348b3dd0005eeab154aa2d74abc617cbd1d0cb
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87529406"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Veri deponuzu şirket içi ağ, Azure sanal ağı veya Amazon sanal özel bulutu içinde bulunuyorsa, bu sunucuya bağlanmak için şirket içinde [barındırılan bir tümleştirme çalışma zamanı](../articles/data-factory/create-self-hosted-integration-runtime.md) ayarlamanız gerekir.

Veri deponuzu yönetilen bir bulut veri hizmeti ise, Azure tümleştirme çalışma zamanı 'nı kullanabilirsiniz. Erişim, güvenlik duvarı kurallarında beyaz olarak listelenen IP 'Ler ile kısıtlanmışsa, izin verilenler listesine [Azure Integration Runtime IP 'leri](../articles/data-factory/azure-integration-runtime-ip-addresses.md) eklemeyi seçebilirsiniz. 

Data Factory tarafından desteklenen ağ güvenlik mekanizmaları ve seçenekleri hakkında daha fazla bilgi için bkz. [veri erişim stratejileri](../articles/data-factory/data-access-strategies.md).
