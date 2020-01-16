---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 684b212ca771af6c336cf6239e18ea367f2da5ce
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045107"
---
Bu makalede PowerShell cmdlet 'leri kullanılmaktadır. Cmdlet 'leri çalıştırmak için, Azure 'da barındırılan ve tarayıcı aracılığıyla kullanılan etkileşimli bir kabuk ortamı Azure Cloud Shell kullanabilirsiniz. Azure Cloud Shell, Azure PowerShell cmdlet 'lerinin önceden yüklenmiş olarak gelir.

Bu makalede yer alan herhangi bir kodu Azure Cloud Shell çalıştırmak için, bir Cloud Shell oturumu açın, kodu kopyalamak için bir kod bloğundaki **Kopyala** düğmesini kullanın ve Windows ve Linux 'ta __CTRL + SHIFT + v__ veya MacOS 'ta __cmd + SHIFT + v__ ile Cloud Shell oturumuna yapıştırın. Yapıştırılan metin otomatik olarak yürütülmez, bu nedenle kodu çalıştırmak için **ENTER** tuşuna basın.

Azure Cloud Shell şunları ile başlatabilirsiniz:

|  |   |
|-----------------------------------------------|---|
| Kod bloğunun sağ üst köşesindeki **Deneyin**’i seçin. Bu, metni Cloud Shell otomatik __olarak kopyalamaz.__ | ![Azure Cloud Shell için deneme örneği](./media/cloud-shell-try-it/hdi-azure-cli-try-it.png) |
| Tarayıcınızda [Shell.Azure.com](https://shell.azure.com) açın. | [![Azure Cloud Shell düğme Başlat](./media/cloud-shell-try-it/hdi-launch-cloud-shell.png)](https://shell.azure.com) |
| [Azure portalının](https://portal.azure.com) sağ üst köşesindeki menüde yer alan **Cloud Shell** düğmesini seçin. | ![Azure portaldaki Cloud Shell düğmesi](./media/cloud-shell-try-it/hdi-cloud-shell-menu.png) |

**PowerShell 'i yerel olarak çalıştırma**

Ayrıca Azure PowerShell cmdlet 'lerini bilgisayarınıza yerel olarak yükleyip çalıştırabilirsiniz. PowerShell cmdlet 'leri sıklıkla güncelleştirilir. En son sürümü çalıştırmıyorsanız, yönergelerde belirtilen değerler başarısız olabilir. Bilgisayarınızda yüklü Azure PowerShell sürümlerini bulmak için `Get-Module -ListAvailable Az` cmdlet 'ini kullanın. Yüklemek veya güncelleştirmek için bkz. [Azure PowerShell modülünü yüklemek](/powershell/azure/install-az-ps).

PowerShell 'i yerel olarak çalıştırıyorsanız, Azure bağlantınızı oluşturmak için ' Connect-AzAccount ' komutunu çalıştırdığınızdan emin olun.