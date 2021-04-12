---
title: include dosyası
description: include dosyası
services: container-service
author: mlearned
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: mlearned
ms.custom: include file
ms.openlocfilehash: 1c2dec106ae72ddead7bda54792fa74e38eb6660
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106081079"
---
| Kaynak                                                                                                           | Sınır                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Abonelik başına en fazla küme                                                                                  | 1000                                                                                                                                                                                                        |
| Sanal makine kullanılabilirlik kümeleri ve temel Load Balancer SKU 'SU ile küme başına en fazla düğüm                       | 100                                                                                                                                                                                                         |
| Sanal Makine Ölçek Kümeleri ve [Standart Load Balancer SKU 'su][standard-load-balancer] olan küme başına en fazla düğüm | 1000 ( [düğüm havuzu][node-pool]başına 100 düğüm)                                                                                                                                                                 |
| Düğüm başına maksimum Pod: kubenet ile [temel ağ][basic-networking]                                           | 110                                                                                                                                                                                                         |
| Düğüm başına en fazla düğüm sayısı: Azure Container ağ arabirimi ile [Gelişmiş ağ][advanced-networking]        | Azure CLI dağıtımı: 30<sup>1</sup><br />Azure Resource Manager şablonu: 30<sup>1</sup><br />Portal dağıtımı: 30                                                                                        |
| Açık hizmet ağı (OSDK) AKS eklentisi önizlemesi                                                                          | Kubernetes kümesi sürümü: 1.19 +<sup>2</sup><br />Küme başına OSA denetleyicileri: 1<sup>2</sup><br />OSD denetleyicisi başına pods: 500<sup>2</sup><br />OSD tarafından yönetilen Kubernetes hizmet hesapları: 50<sup>2</sup> |

<sup>1</sup> Azure CLI veya Kaynak Yöneticisi şablonuyla bir Azure Kubernetes hizmeti (aks) kümesi dağıttığınızda, bu değer düğüm başına 250 Pod 'ye yapılandırılabilir. Zaten bir aks kümesi dağıttıktan sonra veya Azure Portal kullanarak bir küme dağıtırsanız, düğüm başına maksimum Pod yapılandıramazsınız.<br />

<sup>2</sup> AKS için OSA eklentisi bir önizleme durumundadır ve genel kullanıma (GA) önce ek geliştirmelere sahip olacak. Önizleme aşamasında, gösterilen limitlerin aşılmadığından önerilir.<br />

<!-- LINKS - Internal -->

[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->

[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
