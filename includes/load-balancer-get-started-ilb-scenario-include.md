---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: f0e575af51f952a80fe42102b033727713c75cf9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188796"
---
## <a name="configuration-scenario"></a>Yapılandırma senaryosu

Bu senaryoda aşağıdaki şekilde gösterildiği gibi bir sanal ağ içinde iç yük dengeleyici oluşturacağız:

![İç yük dengeleyici senaryosu](./media/load-balancer-get-started-ilb-scenario-include/figure1.png)

Bu senaryo için yapılandırma aşağıdaki şekildedir:

* **DB1** ve **DB2** adlı iki sanal makine
* İç yük dengeleyici için uç noktalar
* Bir iç yük dengeleyici
