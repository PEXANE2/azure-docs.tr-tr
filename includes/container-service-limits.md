---
title: dosya dahil etme
description: dosya dahil etme
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 6b4678b381e769993b01bbedd1cb4c0aeefc0cc1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80335064"
---
| Kaynak | Sınır |
| --- | :--- |
| Abonelik başına en fazla küme | 100 |
| Sanal makine kullanılabilirlik kümeleri ve temel Load Balancer SKU 'SU ile küme başına en fazla düğüm  | 100 |
| Sanal Makine Ölçek Kümeleri ve [Standart Load Balancer SKU 'su][standard-load-balancer] olan küme başına en fazla düğüm | 1000 ( [düğüm havuzu][node-pool]başına 100 düğüm) |
| Düğüm başına maksimum Pod: kubenet ile [temel ağ][basic-networking] | 110 |
| Düğüm başına en fazla düğüm sayısı: Azure Container ağ arabirimi ile [Gelişmiş ağ][advanced-networking] | Azure CLI dağıtımı: 30<sup>1</sup><br />Azure Resource Manager şablonu: 30<sup>1</sup><br />Portal dağıtımı: 30 |

<sup>1</sup> Azure CLI veya Kaynak Yöneticisi şablonuyla bir Azure Kubernetes hizmeti (aks) kümesi dağıttığınızda, bu değer düğüm başına 250 Pod 'ye yapılandırılabilir. Zaten bir aks kümesi dağıttıktan sonra veya Azure Portal kullanarak bir küme dağıtırsanız, düğüm başına maksimum Pod yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-standard-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
