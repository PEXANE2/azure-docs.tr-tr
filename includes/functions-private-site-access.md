---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 2d66e7f497f85141de172c59b67676e1bb93955e
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96021017"
---
[Azure Özel Uç Nokta](../articles/private-link/private-endpoint-overview.md), Azure Özel Bağlantı destekli bir hizmete özel ve güvenli bir şekilde bağlanmanızı sağlayan ağ arabirimidir.  Özel Uç Nokta, sanal ağınızdaki bir özel IP adresini kullanır ve bu sayede hizmeti sanal ağınıza getirir.

[Premium](../articles/azure-functions/functions-premium-plan.md) ve [App Service](../articles/azure-functions/functions-scale.md#app-service-plan) planlarında barındırılan Işlevleriniz için özel uç nokta kullanabilirsiniz.

İşlevler için gelen özel uç nokta bağlantısı oluştururken, özel adresi çözümlemek için bir DNS kaydına de ihtiyacınız olacaktır.  Varsayılan olarak, Azure portal kullanarak özel bir uç nokta oluştururken sizin için özel bir DNS kaydı oluşturulur.

Daha fazla bilgi için bkz. [Web Apps Için özel uç noktaları kullanma](../articles/app-service/networking/private-endpoint.md).