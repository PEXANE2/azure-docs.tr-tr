---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.openlocfilehash: 82d122ed236dc72ced7ebafe2301ef5f1143897f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76963673"
---
Performansı en üst düzeye çıkarmak için her işlev uygulaması için ayrı bir depolama hesabı kullanın. Bu, özellikle Dayanıklı İşlevler veya Olay Hub'ı tetikleyen işlevler olduğunda önemlidir ve bu işlevler her ikisi de yüksek hacimli depolama hareketleri oluşturur. Uygulama mantığınız Azure Depolama ile doğrudan (Depolama SDK'sını kullanarak) veya depolama bağlayıcılarından biri aracılığıyla etkileşimde bulunduğunda, özel bir depolama hesabı kullanmanız gerekir. Örneğin, depolama alanına bazı veriler yazma yığmak için Bir Olay Hub'ı tetikleyen işleviniz varsa, işlev uygulaması için bir, işlev tarafından depolanan blob'lar için başka bir depolama hesabı&mdash;kullanın.