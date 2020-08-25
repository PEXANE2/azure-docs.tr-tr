---
title: Yönetilen uygulama tanımı oluşturma-Azure PowerShell
description: Azure aboneliğinde yönetilen uygulama tanımı oluşturan bir Azure PowerShell betik örneği sağlar.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: b605d63db09b81bfe73eca8f4abd7a38d2325004
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "86056030"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>PowerShell ile yönetilen uygulama tanımı oluşturma

Bu betik bir hizmet kataloğunda yönetilen bir uygulama tanımını yayımlar.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulama tanımını oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-Azmanagedadpplicationdefinition](/powershell/module/az.resources/new-azmanagedapplicationdefinition) | Yönetilen uygulama tanımı oluşturur. Gerekli dosyaları içeren paketi sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/get-started-azureps).
