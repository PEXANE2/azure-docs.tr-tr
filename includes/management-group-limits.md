---
title: dosya dahil etme
description: dosya dahil etme
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 204ca121045d1bd53fd507acc7ea9691bfeac12e
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88748737"
---
| Kaynak | Sınır |
| --- | --- |
| Azure AD kiracısı başına yönetim grupları | 10,000 |
| Yönetim grubu başına abonelikler | Sayısız. |
| Yönetim grubu hiyerarşisi düzeyleri | Kök düzeyi Plus 6 düzey<sup>1</sup> |
| Yönetim grubu başına doğrudan üst yönetim grubu | Bir |
| Konum başına [Yönetim grubu düzeyinde dağıtımlar](../articles/azure-resource-manager/templates/deploy-to-management-group.md) | 800<sup>2</sup> |

<sup>1</sup> 6 düzey abonelik düzeyini içermez.

<sup>2</sup> 800 dağıtım sınırına ulaşırsanız, artık gerekli olmayan geçmişten dağıtımları silin. Yönetim grubu düzeyi dağıtımlarını silmek için [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) veya [az Deployment mg Delete](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-delete)komutunu kullanın.
