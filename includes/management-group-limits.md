---
title: include dosyası
description: include dosyası
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: cdcf6215973755444da9e513761de7ac71e479d4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98739010"
---
| Kaynak | Sınır |
| --- | --- |
| Azure AD kiracısı başına yönetim grupları | 10,000 |
| Yönetim grubu başına abonelikler | Sayısız. |
| Yönetim grubu hiyerarşisi düzeyleri | Kök düzeyi Plus 6 düzey<sup>1</sup> |
| Yönetim grubu başına doğrudan üst yönetim grubu | Bir |
| Konum başına [Yönetim grubu düzeyinde dağıtımlar](../articles/azure-resource-manager/templates/deploy-to-management-group.md) | 800<sup>2</sup> |

<sup>1</sup> 6 düzey abonelik düzeyini içermez.

<sup>2</sup> 800 dağıtım sınırına ulaşırsanız, artık gerekli olmayan geçmişten dağıtımları silin. Yönetim grubu düzeyi dağıtımlarını silmek için [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) veya [az Deployment mg Delete](/cli/azure/deployment/mg#az-deployment-mg-delete)komutunu kullanın.
