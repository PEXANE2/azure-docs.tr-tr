---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 6147dff71ff8c93c8c46b5921b37db459ea73e4a
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027817"
---
> [!TIP]
> Oturum açtıktan sonra, Azure hesabınızla ilişkili Aboneliklerin listesini görürsünüz. Abonelik bilgileri, `isDefault: true` Azure CLI komutları için şu anda etkinleştirilmiş olan abonelikdir. Bu abonelik, Azure Machine Learning çalışma alanınızı içeren bir aynı olmalıdır. Çalışma alanınızın genel bakış sayfasını ziyaret ederek [Azure Portal](https://portal.azure.com) abonelik kimliğini bulabilirsiniz. Çalışma alanı nesnesinden abonelik KIMLIĞINI almak için SDK 'Yı da kullanabilirsiniz. Örneğin, `Workspace.from_config().subscription_id`.
> 
> Başka bir abonelik seçmek için komutunu kullanın `az account set -s <subscription name or ID>` ve geçiş yapılacak abonelik adını veya kimliğini belirtin. Abonelik seçimi hakkında daha fazla bilgi için bkz. [birden çok Azure aboneliği kullanma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).