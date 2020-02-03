---
title: Üretimde - Team Data Science Process modelleri dağıtma
description: Üretim iş kararları etkin bir rol oynamaya dönemlik modelleri dağıtma
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: e3e2ef3340ca836f56176c21cf3d221f0bf172b9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722246"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>Modelleri, iş kararları etkin bir rol oynamaya üretime dağıtın.

Üretim dağıtımı, etkin bir rol bir iş yürütmek bir model sağlar. Dağıtılan bir modelde tahminleri iş kararları için kullanılabilir.

## <a name="production-platforms"></a>Üretim platformları

Çeşitli yaklaşımlar ve modelleri üretime koymak için Platform vardır. Bazı seçenekler şunlardır:

- [Modelleri Azure Machine Learning ile dağıtma](../how-to-deploy-and-where.md)
- [SQL Server 'da bir modelin dağıtılması](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Dağıtımdan önce bir üretim ortamında kullanmak için düşük Puanlama modeli, gecikme süresi sağlamak üzere vardır.
>

>[!NOTE]
>Azure Machine Learning Studio kullanarak dağıtım için bkz. [Azure Machine Learning Web hizmeti dağıtma](../studio/deploy-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B testi

Üretimde birden çok model olduğunda, model performansını karşılaştırmak için [A/B testi](https://en.wikipedia.org/wiki/A/B_testing) kullanılabilir. 
 
## <a name="next-steps"></a>Sonraki adımlar

**Belirli senaryolar** için işlemdeki adımların tümünü gösteren yönergeler de sağlanır. Bunlar, [örnek yönergeler](walkthroughs.md) makalesinde listelenmiş ve küçük resim açıklamalarıyla bağlantılandırılır. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir. 
