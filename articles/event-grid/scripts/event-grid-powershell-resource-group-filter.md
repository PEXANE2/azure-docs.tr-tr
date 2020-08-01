---
title: Azure PowerShell-kaynak grubuna abone olma
description: Bu Azure PowerShell makalede, bir kaynak grubu ve bir kaynak filtresi için Event Grid olaylarına nasıl abone olunacağı gösterilmektedir.
ms.devlang: powershell
ms.topic: sample
ms.date: 07/08/2020
ms.openlocfilehash: f3aeec208ba6b80643e50307b89f590977bfb446
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460771"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-powershell"></a>PowerShell ile bir kaynak grubu için olaylara abone olma ve kaynağa göre filtreleme

Bu betik, bir kaynak grubu için olaylara bir Event Grid aboneliği oluşturur. Yalnızca kaynak grubundaki belirtilen bir kaynakla ilgili olayları almak için bir filtre kullanır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script---stable"></a>Örnek betik - kararlı

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events/filter-events.ps1 "Filter events")]

## <a name="sample-script---preview-module"></a>Örnek betik-önizleme modülü

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

Önizleme örnek betiği Event Grid modülünü gerektirir. Yüklemek için şunu çalıştırın`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events-preview/filter-events-preview.ps1 "Filter events")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, olay aboneliğini oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzEventGridSubscription](/powershell/module/az.eventgrid/new-azeventgridsubscription) | Event Grid aboneliği oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/get-started-azureps).
