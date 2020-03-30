---
title: include dosyası
description: include dosyası
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 9b2bf8763d175bca028be1fcceda921e87eb5b23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335117"
---
| Kaynak | Sınır |
| --- | --- |
| Dizin başına yönetim grupları | 10,000 |
| Yönetim grubuna göre abonelikler | Sınırsız. |
| Yönetim grubu hiyerarşisi düzeyleri | Kök seviyesi artı 6 seviye<sup>1</sup> |
| Yönetim grubu başına doğrudan üst yönetim grubu | Bir |
| Konum başına [yönetim grubu düzeyinde dağıtımlar](../articles/azure-resource-manager/templates/deploy-to-management-group.md) | 800<sup>2</sup> |

<sup>1.1.2</sup> 6 düzeyleri abonelik düzeyini içermez.

<sup>2.000</sup> 800 dağıtım sınırına ulaşırsanız, artık gerekmeden dağıtımları geçmişten silin. Yönetim grubu düzeyindeki dağıtımları silmek için [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) veya [az deployment mg delete](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-delete)'i kullanın.
