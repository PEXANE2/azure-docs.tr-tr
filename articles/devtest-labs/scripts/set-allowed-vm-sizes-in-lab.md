---
title: 'PowerShell betiği: Azure Lab Services izin verilen VM boyutlarını ayarlama | Microsoft Docs'
description: Bu makaleler, Azure Lab Services izin verilen sanal makine (VM) boyutlarını ayarlayan bir örnek PowerShell betiği içerir.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2020
ms.author: spelluru
ms.openlocfilehash: 5b3dbee7d0ac928c4f18f25348e714aba9c1cd13
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84898062"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Azure Lab Services izin verilen VM boyutlarını ayarlamak için PowerShell 'i kullanma

Bu örnek PowerShell betiği, Azure Lab Services izin verilen sanal makine (VM) boyutlarını ayarlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Laboratuvar**. Betik, mevcut bir laboratuvarınızın olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| Find-AzResource | Belirtilen parametrelere göre kaynak arar. |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Kaynakları alır. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Bir kaynağı değiştirir. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | Kaynak oluşturma. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell betiği örnekleri [Azure Lab Services PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
