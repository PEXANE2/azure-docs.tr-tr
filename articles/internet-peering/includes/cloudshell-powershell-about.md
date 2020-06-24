---
title: dosya dahil etme
titleSuffix: Azure
description: dosya dahil etme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 3a5485475e3d8030bf85734faec53b0ff0747499
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678463"
---
Cmdlet 'leri çalıştırmak için, ücretsiz bir etkileşimli kabuk olan Azure Cloud Shell kullanabilirsiniz. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Kodu kopyalamak için **Kopyala** ' yı seçin ve Cloud Shell yapıştırın. Ardından, çalıştırmak için **ENTER** ' u seçin. Cloud Shell başlamanın birkaç yolu vardır:


|  |   |
|-----------------------------------------------|---|
| Cloud Shell’i tarayıcınızda açın. | [![https://shell.azure.com/powershell](../media/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Azure portal sağ üst köşesindeki araç çubuğunda **Cloud Shell** düğmesini seçin. | [![Portalda Cloud Shell](../media/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |


Azure Cloud Shell kullanmak istemiyorsanız PowerShell 'i yerel olarak yükleyebilirsiniz. PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz, Azure Resource Manager PowerShell cmdlet 'lerinin en son sürümünü yüklediğinizden emin olun. PowerShell cmdlet 'leri sıklıkla güncelleştirilir. En son özellik işlevlerini almak için genellikle PowerShell cmdlet 'lerinizi güncelleştirmeniz gerekir. Bunu yapmazsanız sorunlarla karşılaşabilirsiniz.

Yerel olarak çalıştırdığınız PowerShell sürümünü bulmak için **' Get-Module-ListAvailable az '** cmdlet 'ini kullanın. Güncelleştirmek için, bkz. [Azure PowerShell modülünü yüklemek](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). Daha fazla bilgi için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](https://docs.microsoft.com/powershell/azure/azurerm/overview).

MacOS üzerinde PowerShell kullanıyorsanız, [macOS 'Ta PowerShell yükleme](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6) bölümündeki adımları izleyin.