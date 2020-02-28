---
title: Azure PowerShell betik örneği - bir Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure PowerShell betik örneği - bir Log Analytics çalışma alanına oluşturma
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 09/07/2017
ms.openlocfilehash: fc60be2364e80c288300d78cc5dd23eed4ea9e62
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658158"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>PowerShell ile bir Log Analytics çalışma alanı oluşturma

Bu betik, çalışmaya hızlıca üzerinde veri toplama, çözümleme ve alma eylemi başlatmak istiyorsanız, gerekli olan Azure Log Analytics çalışma alanıyla alır.  

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
| [New-Azoperationalınsightsworkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) | Belirtilen kaynak grubu ve konumda bir çalışma alanı oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

