---
title: Azure PowerShell - kaynak grubuna abone olun
description: Bu makalede, kaynak grubu için Olay Ağı olaylarına nasıl abone olunur ve kaynak için filtre uygulanır' ı gösteren bir azure PowerShell komut dosyası görüntülenir.
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 5d41bfec5978d1baf38eaf1608f50375170d9929
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76720614"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-powershell"></a>PowerShell ile bir kaynak grubu için olaylara abone olma ve kaynağa göre filtreleme

Bu betik, bir kaynak grubu için olaylara bir Event Grid aboneliği oluşturur. Yalnızca kaynak grubundaki belirtilen bir kaynakla ilgili olayları almak için bir filtre kullanır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script---stable"></a>Örnek betik - kararlı

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events/filter-events.ps1 "Filter events")]

## <a name="sample-script---preview-module"></a>Örnek komut dosyası - önizleme modülü

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

Önizleme örnek komut dosyası Olay Izgara modülü gerektirir. Yüklemek için, çalıştırmak`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events-preview/filter-events-preview.ps1 "Filter events")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, olay aboneliğini oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Yeni-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Event Grid aboneliği oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
