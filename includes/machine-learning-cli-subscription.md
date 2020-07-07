---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 428a3ad17c81b465635207de622398e814289d87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81616842"
---
> [!TIP]
> Oturum açtıktan sonra, Azure hesabınızla ilişkili Aboneliklerin listesini görürsünüz. Abonelik bilgileri, `isDefault: true` Azure CLI komutları için şu anda etkinleştirilmiş olan abonelikdir. Bu abonelik, Azure Machine Learning çalışma alanınızı içeren bir aynı olmalıdır. Çalışma alanınızın genel bakış sayfasını ziyaret ederek [Azure Portal](https://portal.azure.com) abonelik kimliğini bulabilirsiniz. Çalışma alanı nesnesinden abonelik KIMLIĞINI almak için SDK 'Yı da kullanabilirsiniz. Örneğin, `Workspace.from_config().subscription_id`.
> 
> Başka bir abonelik seçmek için komutunu kullanın `az account set -s <subscription name or ID>` ve geçiş yapılacak abonelik adını veya kimliğini belirtin. Abonelik seçimi hakkında daha fazla bilgi için bkz. [birden çok Azure aboneliği kullanma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).