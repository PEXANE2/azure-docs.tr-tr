---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.openlocfilehash: 82d122ed236dc72ced7ebafe2301ef5f1143897f
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963673"
---
Performansı en üst düzeye çıkarmak için, her bir işlev uygulaması için ayrı bir depolama hesabı kullanın. Bu, her ikisi de yüksek hacimli depolama işlemleri üreten Dayanıklı İşlevler veya Olay Hub 'ı tetiklenen işlevleriniz olduğunda özellikle önemlidir. Uygulama mantığınızın, doğrudan (depolama SDK 'sını kullanarak) veya depolama bağlamalarından biri aracılığıyla Azure depolama ile etkileşime geçtiğinde, ayrılmış bir depolama hesabı kullanmanız gerekir. Örneğin, blob depolamaya bazı veriler yazan bir olay hub 'ı tetiklemeli işleviniz varsa, işlev uygulaması için bir tane&mdash;ve işlev tarafından saklanan Bloblar için başka bir depolama hesabı kullanın.