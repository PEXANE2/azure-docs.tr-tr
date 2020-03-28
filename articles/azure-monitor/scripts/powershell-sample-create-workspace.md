---
title: Günlük Analizi çalışma alanı oluşturma - Azure PowerShell
description: Azure PowerShell Script Örneği - Bir Log Analytics çalışma alanı oluşturun
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 09/07/2017
ms.openlocfilehash: 62b02de5d1c08f6047052d71e3be420cceb1c5c0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80054639"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>PowerShell ile Log Analytics çalışma alanı oluşturun

Bu komut dosyası, veri toplamaya, analiz etmeye ve eyleme başlamak istiyorsanız gereken Azure Log Analytics çalışma alanıyla hızlı bir şekilde çalışmaya başlamanızı sağlar.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, aboneliğinizde yeni bir Log Analytics çalışma alanı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace) | Varolan bir çalışma alanı hakkında bilgi alır. |
| [Yeni-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) | Belirtilen kaynak grubunda ve konumda bir çalışma alanı oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

