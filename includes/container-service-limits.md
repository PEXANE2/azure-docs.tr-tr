---
title: include dosyası
description: include dosyası
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 32edd880e5e455393e60c87f4f963d71a3e59a20
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95555778"
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
[standard-load-balancer]: ../articles/load-balancer/load-balancer-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest