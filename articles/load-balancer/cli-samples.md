---
title: Yük Dengeleyici için Azure CLI örnekleri
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
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225430"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Yük Dengeleyici için Azure CLI örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|-|-|
| [Yüksek kullanılabilirlik için VM 'lere Yük Dengeleme trafiği](./scripts/load-balancer-linux-cli-sample-nlb.md) | Birden çok yüksek oranda kullanılabilir sanal makineler ve yük dengeli küme yapılandırması oluşturur. |
| [Kullanılabilirlik bölgelerindeki VM’lerde yük dengeleme](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md) | Bölgesel olarak yedekli ön uç IP adresine sahip bir bölge ve Standard Load Balancer içinde farklı kullanılabilirlik alanlarında üç VM oluşturur. Bu yük dengeleyici yapılandırması bir KAYBEDİLMESİNDEN veya veri merkezinin tamamı kaybı uygulamalarınızın ve verilerinizin korunmasına yardımcı olur. |
|[Belirli bir kullanılabilirlik bölgesi içinde VM yüklerini dengeleme](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)|Üç sanal makineleri, veri yolu ve belirli bir bölgedeki tek bir bölgedeki kaynakları hizalamaya yardımcı bölgesel ön uç IP'si ile standart yük dengeleyici oluşturur.|
| [VM 'lerde birden çok Web sitesinin yükünü dengeleme](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md) | Azure kullanılabilirlik kümesi, Azure Load Balancer erişilebilen katılmış birden fazla IP yapılandırması ile iki VM oluşturur. |
| | |

