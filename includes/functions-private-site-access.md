---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 2d66e7f497f85141de172c59b67676e1bb93955e
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578838"
---
[Azure Özel Uç Nokta](../articles/private-link/private-endpoint-overview.md), Azure Özel Bağlantı destekli bir hizmete özel ve güvenli bir şekilde bağlanmanızı sağlayan ağ arabirimidir.  Özel Uç Nokta, sanal ağınızdaki bir özel IP adresini kullanır ve bu sayede hizmeti sanal ağınıza getirir.

[Premium](../articles/azure-functions/functions-premium-plan.md) ve [App Service](../articles/azure-functions/functions-scale.md#app-service-plan) planlarında barındırılan Işlevleriniz için özel uç nokta kullanabilirsiniz.

İşlevler için gelen özel uç nokta bağlantısı oluştururken, özel adresi çözümlemek için bir DNS kaydına de ihtiyacınız olacaktır.  Varsayılan olarak, Azure portal kullanarak özel bir uç nokta oluştururken sizin için özel bir DNS kaydı oluşturulur.

Daha fazla bilgi için bkz. [Web Apps Için özel uç noktaları kullanma](../articles/app-service/networking/private-endpoint.md).