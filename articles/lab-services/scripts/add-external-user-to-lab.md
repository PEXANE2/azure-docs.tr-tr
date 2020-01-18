---
title: PowerShell-Azure DevTest Labs ' de bir laboratuvara dış Kullanıcı ekleme
description: Bu PowerShell betiği, Azure DevTest Labs bir laboratuvara dış Kullanıcı ekler.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: da5cc4a3809b1867e8aa2c3b21e88c2f30ae3d4d
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76169673"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>PowerShell kullanarak Azure DevTest Labs bir laboratuvara dış Kullanıcı ekleme

Bu örnek PowerShell betiği, Azure DevTest Labs bir laboratuvarına dış Kullanıcı ekler. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Laboratuvar**. Betik, mevcut bir laboratuvarınızın olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzADUser](/powershell/module/az.resources/get-azaduser) | Azure Active Directory 'den Kullanıcı nesnesini yeniden dener. |
| [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) | Belirtilen kapsamdaki belirtilen Role belirtilen rolü atar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell betiği örnekleri [Azure Lab Services PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
