---
title: Yük Dengeleyicisi için Azure CLI Örnekleri
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225430"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Yük Dengeleyicisi için Azure CLI Örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|-|-|
| [Yüksek kullanılabilirlik için VM'lere yük dengesi trafiği](./scripts/load-balancer-linux-cli-sample-nlb.md) | Yüksek kullanılabilir ve yük dengeli yapılandırmabirkaç sanal makineler oluşturur. |
| [Farklı kullanılabilirlik bölgelerindeki VM’lerin yükünü dengeleme](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md) | Bölge içinde farklı kullanılabilirlik bölgelerinde üç VM ve bölge yedekli ön uç IP adresiyle Standart Yük Dengeleyicisi oluşturur. Bu yük dengeleyici yapılandırması, uygulamalarınızı ve verilerinizi olası bir hatadan veya tüm veri merkezinin kaybına karşı korumaya yardımcı olur. |
|[Belirli bir kullanılabilirlik bölgesi içinde VM yüklerini dengeleme](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)|Belirli bir bölge için veri yolunu ve kaynakları tek bir bölgede hizalamanıza yardımcı olan, zonal frontend IP'li standart yük dengeleyicisi olmak üzere üç VM oluşturur.|
| [VM'lerde birden fazla web sitesini yükleme dengesi](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md) | Birden çok IP yapılandırmasına sahip, Azure Kullanılabilirlik Seti'ne katılan ve Azure Yük Dengeleyicisi aracılığıyla erişilebilen iki VM oluşturur. |
| | |

