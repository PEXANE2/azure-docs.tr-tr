---
title: PowerShell - Azure DevTest Labs'daki bir laboratuvarda özel bir rol oluşturun
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
ms.openlocfilehash: 9605b3c63877335b8f180ae5fd2b06edc1601096
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760478"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarda özel bir rol oluşturmak için PowerShell'i kullanın

Bu örnek PowerShell komut dosyası, Azure DevTest Labs'daki bir laboratuvarda kullanılmak üzere özel bir rol oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir laboratuvar.** Komut dosyası, varolan bir laboratuarınız olmasını gerektiriyor. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) | Azure RBAC kullanarak sorun alabilen bir Azure kaynak sağlayıcısının işlemlerini alır. |
| [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) | Atama için kullanılabilen tüm Azure RBAC rollerini listeler. |
| [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) | Özel bir rol oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Hizmetleri PowerShell komut dosyası örnekleri [Azure Lab Hizmetleri PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
