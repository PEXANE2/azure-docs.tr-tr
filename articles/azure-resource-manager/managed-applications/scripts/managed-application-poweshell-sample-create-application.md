---
title: Azure PowerShell betik örneği-yönetilen bir uygulama dağıtma
description: ', Bir yönetilen uygulama tanımını aboneliğe dağıtan Azure PowerShell örnek betik örneği sağlar.'
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: a2687e9c943df8454ff42a17f44866dcdb7f4730
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86055894"
---
# <a name="deploy-a-managed-application-for-a-service-catalog-with-powershell"></a>PowerShell ile bir hizmet kataloğu için yönetilen bir uygulama dağıtma

Bu betik, hizmet kataloğundan yönetilen bir uygulama tanımını dağıtır.


[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-application/create-application.ps1 "Create application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-Azmanagedadpplication](/powershell/module/az.resources/new-azmanagedapplication) | Yönetilen uygulama oluşturur. Şablonun tanım kimliğini ve parametrelerini sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/get-started-azureps).
