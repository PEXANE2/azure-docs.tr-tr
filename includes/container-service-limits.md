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
ms.openlocfilehash: 6b4678b381e769993b01bbedd1cb4c0aeefc0cc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335064"
---
| Kaynak | Sınır |
| --- | :--- |
| Abonelik başına maksimum kümeler | 100 |
| Sanal Makine Kullanılabilirlik Setleri ve Temel Yük Dengeleyici SKU ile küme başına maksimum düğüm  | 100 |
| Sanal Makine Ölçek Setleri ve Standart [Yük Dengeleyici SKU][standard-load-balancer] ile küme başına maksimum düğüm | 1000 [(Düğüm havuzu][node-pool]başına 100 düğüm) |
| Düğüm başına maksimum bölme: Kubenet ile [temel ağ][basic-networking] | 110 |
| Düğüm başına maksimum bölme: Azure Kapsayıcı Ağ Arabirimi ile [gelişmiş ağ][advanced-networking] | Azure CLI dağıtımı: 30<sup>1</sup><br />Azure Kaynak Yöneticisi şablonu: 30<sup>1</sup><br />Portal dağıtımı: 30 |

<sup>1.1.2</sup> Azure CLI veya Kaynak Yöneticisi şablonuyla bir Azure Kubernetes Hizmeti (AKS) kümesi dağıttığınızda, bu değer düğüm başına 250 bölmeye kadar yapılandırılabilir. Bir AKS kümesini zaten dağıttıktan veya Azure portalını kullanarak bir küme dağıttıktan sonra düğüm başına maksimum bölmeleri yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-standard-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
