---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 3c9679f3d66d58c7a6c847b54c84438c979ecd39
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97936875"
---
[Azure Özel Uç Nokta](../articles/private-link/private-endpoint-overview.md), Azure Özel Bağlantı destekli bir hizmete özel ve güvenli bir şekilde bağlanmanızı sağlayan ağ arabirimidir.  Özel Uç Nokta, sanal ağınızdaki bir özel IP adresini kullanır ve bu sayede hizmeti sanal ağınıza getirir.

[Premium](../articles/azure-functions/functions-premium-plan.md) ve [App Service](../articles/azure-functions/dedicated-plan.md) planlarında barındırılan Işlevleriniz için özel uç nokta kullanabilirsiniz.

İşlevler için gelen özel uç nokta bağlantısı oluştururken, özel adresi çözümlemek için bir DNS kaydına de ihtiyacınız olacaktır.  Varsayılan olarak, Azure portal kullanarak özel bir uç nokta oluştururken sizin için özel bir DNS kaydı oluşturulur.

Daha fazla bilgi için bkz. [Web Apps Için özel uç noktaları kullanma](../articles/app-service/networking/private-endpoint.md).