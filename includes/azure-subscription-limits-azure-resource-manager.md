---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 02/10/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 32d39493e526a4b95369e2998d3f9ade9f2964dc
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133698"
---
| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Abonelik başına [kaynak grupları](../articles/azure-resource-manager/management/overview.md) |980 |980 |
| Azure Resource Manager API isteği boyutu |4\.194.304 bayt. |4\.194.304 bayt. |
| Abonelik başına Etiketler<sup>1</sup> |Sınırsız. |Sınırsız. |
| Abonelik başına benzersiz etiket hesaplamaları<sup>1</sup> | 10,000 | 10,000 |
| Konum başına [abonelik düzeyi dağıtımlar](../articles/azure-resource-manager/templates/deploy-to-subscription.md) | 800<sup>2</sup> | 800 |
| Azure Active Directory kiracı başına abonelik sayısı | Sınırsız. | Sınırsız. |
| Abonelik başına [Coadministrators](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) |Sınırsız. |Sınırsız. |

<sup>1</sup> Abonelik başına sınırsız sayıda etiket uygulayabilirsiniz. Kaynak veya kaynak grubu başına etiket sayısı 50 ile sınırlıdır. Kaynak Yöneticisi, yalnızca etiket sayısı 10.000 veya daha az olduğunda, abonelikteki [benzersiz etiket adı ve değerlerinin listesini](/rest/api/resources/tags) döndürür. Sayı 10.000 ' i aştığında yine de etikete göre bir kaynak bulabilirsiniz.  

<sup>2</sup> 800 dağıtım sınırına ulaşırsanız, artık gerekli olmayan geçmişten dağıtımları silin. Abonelik düzeyi dağıtımlarını silmek için [Remove-azdeployment](/powershell/module/az.resources/Remove-AzDeployment) veya [az Deployment Delete](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete)komutunu kullanın.
