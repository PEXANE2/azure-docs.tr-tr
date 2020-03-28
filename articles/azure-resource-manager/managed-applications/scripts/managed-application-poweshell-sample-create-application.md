---
title: Azure PowerShell komut dosyası örneği - Yönetilen bir uygulamayı dağıtma
description: Aboneye yönetilen bir uygulama tanımı dağıtan Azure PowerShell örnek komut dosyası örneği sağlar.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: e076128a5be453028635b1657bf3f90980863148
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75650173"
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
| [Yeni-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplication) | Yönetilen uygulama oluşturur. Şablonun tanım kimliğini ve parametrelerini sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
