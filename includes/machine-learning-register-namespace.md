---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 12/01/2020
ms.author: larryfr
ms.openlocfilehash: e92d3ac9ed4330cc1680428a426e881538cb0e78
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102623371"
---
* Yeni bir çalışma alanı oluştururken, çalışma alanının gerek duyduğu Hizmetleri otomatik olarak oluşturabilir veya var olan hizmetleri kullanabilirsiniz. __Mevcut hizmetleri farklı bir Azure aboneliğinden çalışma alanından__ kullanmak istiyorsanız, Azure Machine Learning ad alanını bu hizmetleri içeren abonelikte kaydetmeniz gerekir. Örneğin, aboneliği, B aboneliğinden bir depolama hesabı kullanan A aboneliğinde bir çalışma alanı oluştururken, depolama hesabını çalışma alanıyla kullanabilmeniz için önce Azure Machine Learning ad alanı B aboneliğinde kayıtlı olmalıdır.

    Azure Machine Learning için kaynak sağlayıcısı, __Microsoft. MachineLearningServices__. Kayıtlı olup olmadığını ve nasıl kaydedeceğinizi öğrenmek hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../articles/azure-resource-manager/management/resource-providers-and-types.md)  makalesi.

    > [!IMPORTANT]
    > Bu yalnızca çalışma alanı oluşturma sırasında belirtilen kaynaklar için geçerlidir; Azure depolama hesapları, Azure Container kaydı, Azure Key Vault ve Application Insights.
