---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 428a3ad17c81b465635207de622398e814289d87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81616842"
---
> [!TIP]
> Oturum açtıktan sonra, Azure hesabınızla ilişkili Aboneliklerin listesini görürsünüz. Abonelik bilgileri, Azure `isDefault: true` CLI komutları için şu anda etkinleştirilmiş olan abonelikdir. Bu abonelik, Azure Machine Learning çalışma alanınızı içeren bir aynı olmalıdır. Çalışma alanınızın genel bakış sayfasını ziyaret ederek [Azure Portal](https://portal.azure.com) abonelik kimliğini bulabilirsiniz. Çalışma alanı nesnesinden abonelik KIMLIĞINI almak için SDK 'Yı da kullanabilirsiniz. Örneğin, `Workspace.from_config().subscription_id`.
> 
> Başka bir abonelik seçmek için `az account set -s <subscription name or ID>` komutunu kullanın ve geçiş yapılacak abonelik adını veya kimliğini belirtin. Abonelik seçimi hakkında daha fazla bilgi için bkz. [birden çok Azure aboneliği kullanma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).