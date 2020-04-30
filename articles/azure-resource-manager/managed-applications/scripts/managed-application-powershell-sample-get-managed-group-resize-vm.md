---
title: Yönetilen kaynak grubunu al & VM 'Leri yeniden boyutlandır-Azure PowerShell
description: Azure yönetilen uygulaması için yönetilen bir kaynak grubunu alan Azure PowerShell örnek betik sağlar. Betik, VM 'Leri yeniden boyutlandırır.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 695023f4700370cbe5e9b345f513e38d1cf1fc0c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75650187"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Yönetilen bir kaynak grubundaki kaynakları edinme ve VM 'Leri PowerShell ile yeniden boyutlandırma

Bu betik bir yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki VM’leri yeniden boyutlandırır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-Azmanagedadpplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | Yönetilen uygulamaları listeler. Sonuçları odaklamak için kaynak grubu adı belirtin. |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Kaynakları listeler. Sonuca odaklanmak için bir kaynak grubu ve kaynak türü sağlayın. |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | Sanal makinenin boyutunu güncelleştirir. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
