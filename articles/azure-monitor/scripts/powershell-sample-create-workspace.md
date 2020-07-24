---
title: Log Analytics çalışma alanı oluştur-Azure PowerShell
description: Betik örneği Azure PowerShell-için Log Analytics çalışma alanı oluşturun
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 09/07/2017
ms.openlocfilehash: f6bfb3a244874f6160d34c174b6d10b9a03ca437
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87024138"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>PowerShell ile Log Analytics çalışma alanı oluşturma

Bu betik, verileri toplamaya, çözümlemeye ve eyleme başlamak istediğinizde gerekli olan bir Azure Log Analytics çalışma alanıyla hızlıca çalışmaya başlamanızı sağlar.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, aboneliğinizde yeni bir Log Analytics çalışma alanı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-Azoperationalınsightsworkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace) | Mevcut bir çalışma alanı hakkında bilgi alır. |
| [New-Azoperationalınsightsworkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) | Belirtilen kaynak grubunda ve konumda bir çalışma alanı oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

