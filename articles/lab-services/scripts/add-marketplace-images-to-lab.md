---
title: PowerShell - Azure DevTest Labs'daki bir laboratuvara pazar yeri görüntüsü ekleyin
description: Bu PowerShell komut dosyası, Azure DevTest Labs'daki bir laboratuvara pazar görüntüsü ekler.
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
ms.openlocfilehash: 3d699a22ed06022c6d9df12ade7f202fb304648c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76166441"
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara pazar görüntüsü eklemek için PowerShell'i kullanın

Bu örnek PowerShell komut dosyası, Azure DevTest Labs'daki bir laboratuvara pazar görüntüsü ekler. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir laboratuvar.** Komut dosyası, varolan bir laboratuarınız olmasını gerektiriyor. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| Bul-AzResource | Belirtilen parametrelere dayalı kaynakları arar. |
| [Al-AzResource](/powershell/module/az.resources/get-azresource) | Kaynakları alır. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Kaynağı değiştirir. |
| [Yeni Kaynak](/powershell/module/az.resources/new-azresource) | Kaynak oluşturma. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Hizmetleri PowerShell komut dosyası örnekleri [Azure Lab Hizmetleri PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
