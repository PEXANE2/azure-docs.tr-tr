---
title: Modelleri üretime dağıtın - Ekip Veri Bilimi Süreci
description: Modellerin üretime nasıl dağıtılanın, böylece iş kararlarının almada etkin rol oynamalarını sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722246"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>İş kararlarında etkin rol oynamak için modelleri üretime dağıtın

Üretim dağıtımı, bir modelin bir işletmede etkin bir rol oynamasını sağlar. Dağıtılmış bir modelden öngörüler iş kararları için kullanılabilir.

## <a name="production-platforms"></a>Üretim platformları

Modelleri üretime sokacak çeşitli yaklaşımlar ve platformlar vardır. Aşağıda birkaç seçenek verilmiştir:

- [Azure Machine Learning ile modelleri dağıtma](../how-to-deploy-and-where.md)
- [SQL sunucusunda bir modelin dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Sunucusu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Dağıtımdan önce, model puanlamanın gecikmesinin üretimde kullanılacak kadar düşük olduğundan emin olmak gerekir.
>

>[!NOTE]
>Azure Machine Learning Studio'su kullanarak dağıtım için [bkz.](../studio/deploy-a-machine-learning-web-service.md)
>

## <a name="ab-testing"></a>A/B testi

Birden çok model üretimdeyken, model performansını karşılaştırmak için [A/B testi](https://en.wikipedia.org/wiki/A/B_testing) kullanılabilir. 
 
## <a name="next-steps"></a>Sonraki adımlar

**Belirli senaryolar** için işlemdeki tüm adımları gösteren İzler de sağlanır. Bunlar, [Örnek walkthroughs](walkthroughs.md) makalesinde küçük resim açıklamaları ile listelenir ve bunlarla bağlantılıdır. Bunlar, akıllı bir uygulama oluşturmak için bulut, şirket içi araçlar ve hizmetleri iş akışı veya ardışık ardışık yollarla nasıl birleştirebileceklerini gösterir. 
