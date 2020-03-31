---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: df4735699f71256598bfd418a505ae8d3e5b816c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80296897"
---
Oturum açtıktan sonra Azure hesabınızla ilişkili aboneliklerin listesini görürsünüz. Sahip olunan `isDefault: true` abonelik bilgileri, Azure CLI komutları için şu anda etkinleştirilen aboneliktir. Bu abonelik, Azure Machine Learning çalışma alanınızı içeren abonelikle aynı olmalıdır. Çalışma alanınız için genel bakış sayfasını ziyaret ederek [Azure portalından](https://portal.azure.com) abonelik kimliğini bulabilirsiniz. Çalışma alanı nesnesinden abonelik kimliğini almak için SDK'yı da kullanabilirsiniz. Örneğin, `Workspace.from_config().subscription_id`.
    
Başka bir abonelik seçmek için, abonelik kimliğine geçmek için [az hesap kümesi](https://docs.microsoft.com/cli/azure/account#az-account-set) komutunu kullanın. Abonelik seçimi hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)