---
title: PowerShell - Azure Lab Hizmetlerinde VHD dosyasından özel görüntü oluşturma
description: Bu PowerShell komut dosyası, Azure Lab Hizmetleri'ndeki bir VHD dosyasından özel bir görüntü oluşturur.
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
ms.openlocfilehash: 38383462a665ced1ccb6c6a2f062fab0492eee9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169978"
---
# <a name="use-powershell-to-create-a-custom-image-from-a-vhd-file-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'ndeki bir VHD dosyasından özel bir görüntü oluşturmak için PowerShell'i kullanın

Bu örnek PowerShell komut dosyası, Azure Lab Hizmetleri'ndeki bir VHD dosyasından özel bir görüntü oluşturur

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir laboratuvar.** Komut dosyası, varolan bir laboratuarınız olmasını gerektiriyor. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-image-from-vhd/create-custom-image-from-vhd.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Al-AzResource](/powershell/module/az.resources/get-azresource) | Kaynakları alır. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Yeni-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) | Kaynak grubuna Azure dağıtımı ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Hizmetleri PowerShell komut dosyası örnekleri [Azure Lab Hizmetleri PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
