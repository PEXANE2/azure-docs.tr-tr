---
title: 'PowerShell komut dosyası: Azure Lab Hizmetlerinde izin verilen VM boyutlarını ayarlama | Microsoft Dokümanlar'
description: Bu makaleler, Azure Lab Hizmetleri'nde izin verilen sanal makine (VM) boyutlarını ayarlayan örnek bir PowerShell komut dosyası içerir.
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
ms.openlocfilehash: a1b0e9a4aed475f04ec8dcffa9bc95b7c7c713e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760479"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde izin verilen VM boyutlarını ayarlamak için PowerShell'i kullanma

Bu örnek PowerShell komut dosyası kümeleri Azure Lab Hizmetlerinde sanal makine (VM) boyutlarına izin verdi.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir laboratuvar.** Komut dosyası, varolan bir laboratuarınız olmasını gerektiriyor. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

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
