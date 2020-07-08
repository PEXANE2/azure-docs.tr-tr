---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 0b03957178af0578d2c6cd91d7377c93f413cec3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84629526"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Veri depolubirisi aşağıdaki yollarla yapılandırılmışsa, veri deposuna bağlanmak için [Şirket içinde barındırılan bir tümleştirme çalışma zamanı](../articles/data-factory/create-self-hosted-integration-runtime.md) ayarlamanız gerekir:

- Veri deposu, bir Azure sanal ağı içinde ya da Amazon sanal özel bulutu içinde şirket içi ağ içinde bulunur.
- Veri deposu, erişimin güvenlik duvarı kurallarında beyaz olarak listelenen IP 'Ler ile sınırlı olduğu yönetilen bir bulut veri hizmetidir.
