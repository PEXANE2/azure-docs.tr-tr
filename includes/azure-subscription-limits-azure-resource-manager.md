---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 94fd7e692be31ba247e3342246d3940ed08ef9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334668"
---
| Kaynak | Sınır |
| --- | --- |
| Azure Etkin Dizin kiracı başına abonelikler | Sınırsız. |
| Abonelik başına [ortak yöneticiler](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) |Sınırsız. |
| Abonelik başına [kaynak grupları](../articles/azure-resource-manager/management/overview.md) |980 |
| Azure Kaynak Yöneticisi API istek boyutu |4,194,304 bayt. |
| Abonelik başına etiketler<sup>1</sup> |50 |
| Abonelik başına benzersiz etiket hesaplamaları<sup>1</sup> | 10,000 |
| Konum başına [abonelik düzeyinde dağıtımlar](../articles/azure-resource-manager/templates/deploy-to-subscription.md) | 800<sup>2</sup> |

<sup>1.1.2</sup> En fazla 50 etiketi doğrudan bir aboneye uygulayabilirsiniz. Ancak, abonelik, abonelik içindeki kaynak gruplarına ve kaynaklara uygulanan sınırsız sayıda etiket içerebilir. Kaynak veya kaynak grubu başına etiket sayısı 50 ile sınırlıdır. Kaynak Yöneticisi, yalnızca etiket sayısı 10.000 veya daha az olduğunda abonelikteki [benzersiz etiket adı ve değerlerin](/rest/api/resources/tags) listesini döndürür. Sayı 10.000'i aştığında etikete göre bir kaynak bulabilirsiniz.  

<sup>2.000</sup> 800 dağıtım sınırına ulaşırsanız, artık gerekmeden dağıtımları geçmişten silin. Abonelik düzeyi dağıtımlarını silmek için [Kaldır-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) veya [az deployment alt silme](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-delete)'yi kullanın.
