---
title: Yönetilen kaynak grubunu & Yeniden Boyutlandırın - Azure PowerShell
description: Azure Yönetilen Uygulama için yönetilen bir kaynak grubu alan Azure PowerShell örnek komut dosyası sağlar. Komut dosyası VM'leri yeniden boyutlandırıyor.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 695023f4700370cbe5e9b345f513e38d1cf1fc0c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75650187"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Yönetilen bir kaynak grubunda kaynak elde edin ve PowerShell ile VM'leri yeniden boyutlandırın

Bu betik bir yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki VM’leri yeniden boyutlandırır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | Yönetilen uygulamaları listeler. Sonuçları odaklamak için kaynak grubu adı sağlayın. |
| [Al-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Kaynakları listeler. Sonucu odaklamak için bir kaynak grubu ve kaynak türü sağlayın. |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | Sanal makinenin boyutunu güncelleştirir. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
