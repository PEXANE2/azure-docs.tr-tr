---
title: PowerShell-Azure DevTest Labs bir laboratuvarda özel rol oluşturma
description: Bu makalede, Azure DevTest Labs bir laboratuvara dış Kullanıcı ekleyen bir Azure PowerShell betiği sunulmaktadır.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: 242e2a2bf743777dd43a4c84194ebd894b5edd3c
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290405"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs 'de laboratuarda özel bir rol oluşturmak için PowerShell kullanın

Bu örnek PowerShell betiği, Azure DevTest Labs laboratuvarında kullanmak üzere özel bir rol oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Laboratuvar**. Betik, mevcut bir laboratuvarınızın olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) | Azure RBAC kullanılarak güvenliği sağlanabilir bir Azure Kaynak sağlayıcısı için işlemleri alır. |
| [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) | Atama için kullanılabilen tüm Azure RBAC rollerini listeler. |
| [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) | Özel bir rol oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).

Ek Azure Lab Services PowerShell betiği örnekleri [Azure Lab Services PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
