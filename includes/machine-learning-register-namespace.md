---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 12/01/2020
ms.author: larryfr
ms.openlocfilehash: 6e9f349f2fd90fdb12d8d26ca904e504b75d9db2
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96445182"
---
* Yeni bir çalışma alanı oluştururken, çalışma alanının gerek duyduğu Hizmetleri otomatik olarak oluşturabilir veya var olan hizmetleri kullanabilirsiniz. __Mevcut hizmetleri farklı bir Azure aboneliğinden çalışma alanından__ kullanmak istiyorsanız, Azure Machine Learning ad alanını bu hizmetleri içeren abonelikte kaydetmeniz gerekir. Örneğin, aboneliği, B aboneliğinden bir depolama hesabı kullanan A aboneliğinde bir çalışma alanı oluştururken, depolama hesabını çalışma alanıyla kullanabilmeniz için önce Azure Machine Learning ad alanı B aboneliğinde kayıtlı olmalıdır.

    Azure Machine Learning için kaynak sağlayıcısı, __Microsoft. MachineLearningService__' dir. Kayıtlı olup olmadığını ve nasıl kaydedeceğinizi öğrenmek hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../articles/azure-resource-manager/management/resource-providers-and-types.md)  makalesi.

    > [!IMPORTANT]
    > Bu yalnızca çalışma alanı oluşturma sırasında belirtilen kaynaklar için geçerlidir; Azure depolama hesapları, Azure Container kaydı, Azure Key Vault ve Application Insights.