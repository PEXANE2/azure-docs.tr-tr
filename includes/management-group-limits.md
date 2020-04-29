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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80335117"
---
| Kaynak | Sınır |
| --- | --- |
| Dizin başına yönetim grupları | 10,000 |
| Yönetim grubu başına abonelikler | Sayısız. |
| Yönetim grubu hiyerarşisi düzeyleri | Kök düzeyi Plus 6 düzey<sup>1</sup> |
| Yönetim grubu başına doğrudan üst yönetim grubu | Bir |
| Konum başına [Yönetim grubu düzeyinde dağıtımlar](../articles/azure-resource-manager/templates/deploy-to-management-group.md) | 800<sup>2</sup> |

<sup>1</sup> 6 düzey abonelik düzeyini içermez.

<sup>2</sup> 800 dağıtım sınırına ulaşırsanız, artık gerekli olmayan geçmişten dağıtımları silin. Yönetim grubu düzeyi dağıtımlarını silmek için [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) veya [az Deployment mg Delete](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-delete)komutunu kullanın.
