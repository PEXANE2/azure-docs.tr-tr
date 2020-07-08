---
title: Load Balancer için Azure CLı örnekleri
titleSuffix: Azure Load Balancer
description: Azure CLI Örnekleri
services: load-balancer
documentationcenter: load-balancer
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 06/14/2018
ms.author: allensu
ms.openlocfilehash: f35007bc3c47aeec9bcdd8a418983b95f6f20ad6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74225430"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Load Balancer için Azure CLı örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|-|-|
| [Yüksek kullanılabilirlik için VM 'lere Yük Dengeleme trafiği](./scripts/load-balancer-linux-cli-sample-nlb.md) | Yüksek oranda kullanılabilir ve yük dengeli bir yapılandırmada birkaç sanal makine oluşturur. |
| [Farklı kullanılabilirlik bölgelerindeki VM’lerin yükünü dengeleme](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md) | Bölge içinde farklı kullanılabilirlik bölgelerinde üç VM oluşturur ve bölgesel olarak yedekli bir ön uç IP adresiyle Standart Load Balancer. Bu yük dengeleyici yapılandırması, uygulamalarınızın ve verilerinizin, bir veri merkezinin tamamındaki bir hatadan veya kaybolmasından korunmasına yardımcı olur. |
|[Belirli bir kullanılabilirlik bölgesi içinde VM yüklerini dengeleme](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)|Belirli bir bölge için tek bir bölgedeki veri yolunu ve kaynakları hizalamaya yardımcı olan, bölgesel ön uç IP 'si ile birlikte standart Load Balancer üç VM oluşturur.|
| [VM 'lerde birden çok Web sitesinin yükünü dengeleme](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md) | Bir Azure Load Balancer aracılığıyla erişilebilen, Azure kullanılabilirlik kümesine katılmış birden çok IP yapılandırmasına sahip iki VM oluşturur. |
| | |

