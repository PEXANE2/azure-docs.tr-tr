---
title: Yönetilen uygulama tanımı oluşturma - Azure PowerShell
description: Azure aboneliğinde yönetilen bir uygulama tanımı oluşturan bir Azure PowerShell komut dosyası örneği sağlar.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 09fe1455f0dba57fd23aeeb66acb1f99fc2513d4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75650208"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>PowerShell ile yönetilen bir uygulama tanımı oluşturma

Bu betik bir hizmet kataloğunda yönetilen bir uygulama tanımını yayımlar.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulama tanımını oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Yeni-AzManagedApplicationDefinition](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplicationdefinition) | Yönetilen uygulama tanımı oluşturur. Gerekli dosyaları içeren paketi sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
