---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 428a3ad17c81b465635207de622398e814289d87
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616842"
---
> [!TIP]
> Oturum açtıktan sonra Azure hesabınızla ilişkili aboneliklerin listesini görürsünüz. Sahip olunan `isDefault: true` abonelik bilgileri, Azure CLI komutları için şu anda etkinleştirilen aboneliktir. Bu abonelik, Azure Machine Learning çalışma alanınızı içeren abonelikle aynı olmalıdır. Çalışma alanınız için genel bakış sayfasını ziyaret ederek [Azure portalından](https://portal.azure.com) abonelik kimliğini bulabilirsiniz. Çalışma alanı nesnesinden abonelik kimliğini almak için SDK'yı da kullanabilirsiniz. Örneğin, `Workspace.from_config().subscription_id`.
> 
> Başka bir abonelik seçmek `az account set -s <subscription name or ID>` için komutu kullanın ve geçiş yapmak için abonelik adını veya kimliğini belirtin. Abonelik seçimi hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)