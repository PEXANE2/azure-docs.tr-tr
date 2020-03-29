---
title: PowerShell - Azure DevTest Labs'daki bir laboratuvara harici kullanıcı ekleme
description: Bu makale, Azure DevTest Labs'daki bir laboratuvara harici bir kullanıcı ekleyen bir Azure PowerShell komut dosyası sağlar.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: e9efe5476da051d905feff3d6803cb60ea0c4beb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760496"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara harici bir kullanıcı eklemek için PowerShell'i kullanın

Bu örnek PowerShell komut dosyası, Azure DevTest Labs'daki bir laboratuvara harici bir kullanıcı ekler. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir laboratuvar.** Komut dosyası, varolan bir laboratuarınız olmasını gerektiriyor. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzADUser](/powershell/module/az.resources/get-azaduser) | Kullanıcı nesnesini Azure etkin dizininden yeniden dener. |
| [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) | Belirtilen rolü belirtilen kapsamda belirtilen anaparaya atar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Hizmetleri PowerShell komut dosyası örnekleri [Azure Lab Hizmetleri PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
